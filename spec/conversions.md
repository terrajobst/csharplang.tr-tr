---
ms.openlocfilehash: d082393a00496b948ad4e3ff9e135d94e89d2448
ms.sourcegitcommit: 1a46441156b13db6c845f4bbb886284387d73023
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67047030"
---
# <a name="conversions"></a>Dönüşümler

A ***dönüştürme*** bir ifadenin belirli bir tür olacak şekilde değerlendirilmesi olanak sağlar. Bir dönüştürme, farklı bir türe sahip olarak kabul edilmesi için belirli bir türde bir ifade neden olabilecek ya da bir tür için bir tür olmayan bir ifade neden olabilir. Dönüştürmeleri olabilir ***örtük*** veya ***açık***, ve bu bir açık tür dönüştürme gerekli olup olmadığını belirler. Örneği için tür dönüştürme `int` türüne `long` kapalıdır, bunu ifadeler tür `int` türü örtük olarak davranılıp `long`. Türünden ters dönüştürme `long` türüne `int`, açık olan ve açık bir tür dönüştürme gerekiyor.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Bazı dönüştürmeler dil tarafından tanımlanır. Programları kendi dönüştürmeler de tanımlayabilir ([kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Örtük dönüşümler

Aşağıdaki dönüştürmeler, örtük dönüştürmelerin sınıflandırılır:

*  Kimlik dönüştürme
*  Örtük sayısal dönüşümler
*  Numaralandırma örtük dönüştürme.
*  Boş değer atanabilir örtük dönüşümler
*  Null değişmez değer dönüştürmeleri
*  Örtük bir başvuru dönüşümleri
*  Kutulama dönüştürmeler
*  Örtük dinamik dönüştürmeler
*  Örtük sabit ifade dönüştürmeler
*  Kullanıcı tanımlı örtük dönüşümler
*  Anonim işlev dönüştürme
*  Yöntem grubu dönüştürmeler

Örtük dönüştürmeleri durumlarda işlevi üye çağrıları dahil olmak üzere çeşitli meydana gelebilir ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), cast ifadeleri ([Cast ifadeleri](expressions.md#cast-expressions)), ve atamaları ([atama işleçleri](expressions.md#assignment-operators)).

Önceden tanımlanmış örtük dönüştürmeler her zaman başarılı ve hiçbir zaman özel durum oluşturulmasına neden olur. Kullanıcı tanımlı düzgün bir şekilde tasarlanmış örtük dönüştürmeleri bu özellikleri de göstermesi gerekir.

Türleri dönüştürme amacıyla `object` ve `dynamic` eşdeğer olarak kabul edilir.

Ancak, dinamik dönüştürmeler ([dinamik örtük dönüştürmelerin](conversions.md#implicit-dynamic-conversions) ve [açık dinamik dönüştürmeler](conversions.md#explicit-dynamic-conversions)) yalnızca türündeki ifadeler için uygulama `dynamic` ([dinamik tür](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Kimlik dönüştürme

Bir kimlik dönüştürme herhangi bir türü aynı türe dönüştürür. Gerekli bir tür zaten olan bir varlık, bu türe dönüştürülebilir olmalıdır söylenebilir, bu dönüşümü yok.

*  Çünkü `object` ve `dynamic` eşdeğer bir kimlik dönüştürme arasında değerlendirilir `object` ve `dynamic`, aynı olan tüm oluşumlarını değiştirilirken oluşturulan türler arasında `dynamic` ile`object`.

### <a name="implicit-numeric-conversions"></a>Örtük sayısal dönüşümler

Örtük sayısal dönüşümler verilmiştir:

*  Gelen `sbyte` için `short`, `int`, `long`, `float`, `double`, veya `decimal`.
*  Gelen `byte` için `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, veya `decimal`.
*  Gelen `short` için `int`, `long`, `float`, `double`, veya `decimal`.
*  Gelen `ushort` için `int`, `uint`, `long`, `ulong`, `float`, `double`, veya `decimal`.
*  Gelen `int` için `long`, `float`, `double`, veya `decimal`.
*  Gelen `uint` için `long`, `ulong`, `float`, `double`, veya `decimal`.
*  Gelen `long` için `float`, `double`, veya `decimal`.
*  Gelen `ulong` için `float`, `double`, veya `decimal`.
*  Gelen `char` için `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, veya `decimal`.
*  Gelen `float` için `double`.

Dönüştürmelere `int`, `uint`, `long`, veya `ulong` için `float` ve `long` veya `ulong` için `double` duyarlık kaybına neden ancak hiçbir zaman büyüklük kaybına neden olur. Diğer örtük sayısal dönüştürmeler, hiçbir zaman bilgileri kaybedersiniz.

Herhangi bir örtük dönüştürme vardır `char` türü için değer bir tam sayı türleri için otomatik olarak dönüştürmez `char` türü.

### <a name="implicit-enumeration-conversions"></a>Numaralandırma örtük dönüştürmeleri

Bir numaralandırma örtük dönüştürme verir *decimal_integer_literal* `0` herhangi dönüştürülecek *enum_type* ve herhangi bir *nullable_type* olan temel alınan türü olan bir *enum_type*. İkinci durumda, temel alınan dönüştürerek dönüştürme değerlendirilir *enum_type* ve sonucu sarmalama ([boş değer atanabilir türler](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Örtük ilişkilendirilmiş dize dönüştürme

Örtük ilişkilendirilmiş dize dönüştürme izinleri bir *interpolated_string_expression* ([Ara değerli dizeler](expressions.md#interpolated-strings)) dönüştürülecek `System.IFormattable` veya `System.FormattableString` (uygulayan `System.IFormattable`).

Bu dönüştürme uygulandığında bir dize değeri ilişkilendirilmiş dizeden oluşur değil. Bunun yerine bir örneğini `System.FormattableString` oluşturulmuş, daha ayrıntılı olarak açıklanan [Ara değerli dizeler](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Boş değer atanabilir örtük dönüşümler

NULL olmayan değer türleri üzerinde çalışması önceden tanımlanmış örtük dönüştürmeleri bu türlerin boş değer atanabilir forms ile de kullanılabilir. Her bir NULL olmayan değer türünden dönüştürme sayısal dönüşümler ve önceden tanımlanmış bir örtük kimlik `S` NULL olmayan bir değer türü için `T`, aşağıdaki boş değer atanabilir örtük dönüştürmeleri vardır:

*  Örtük bir dönüştürme `S?` için `T?`.
*  Örtük bir dönüştürme `S` için `T?`.

Temel alınan bir dönüştürme açık boş değer atanabilir bir dönüştürme değerlendirmesini temel `S` için `T` aşağıdaki gibi çalışır:

*  Boş değer atanabilir bir dönüştürme ise `S?` için `T?`:
    * Kaynak değeri null ise (`HasValue` özellik değer: false), sonuç türü null değeri `T?`.
    * Aksi takdirde, dönüştürme bir gelen çözülme olarak değerlendirilir `S?` için `S`, temel alınan dönüştürme işlemi tarafından izlenen `S` için `T`bir sarmalama çizgidir ([boş değer atanabilir türler](types.md#nullable-types)) gelen`T` için `T?`.

*  Boş değer atanabilir bir dönüştürme ise `S` için `T?`, dönüştürme temel alınan dönüştürme işlemini değerlendirmesinde `S` için `T` diğerine kaydırma ardından `T` için `T?`.

### <a name="null-literal-conversions"></a>Null değişmez değer dönüştürmeleri

Öğesinden örtük bir dönüştürme var `null` değişmez değer için boş değer atanabilir bir tür. Bu dönüştürme null değerini üretir ([boş değer atanabilir türler](types.md#nullable-types)) belirtilen boş değer atanabilir türde.

### <a name="implicit-reference-conversions"></a>Örtük bir başvuru dönüşümleri

Örtük bir başvuru dönüşümleri şunlardır:

*  Herhangi *reference_type* için `object` ve `dynamic`.
*  Herhangi *class_type* `S` herhangi *class_type* `T`, sağlanan `S` türetilir `T`.
*  Herhangi *class_type* `S` herhangi *INTERFACE_TYPE* `T`, sağlanan `S` uygulayan `T`.
*  Herhangi *INTERFACE_TYPE* `S` herhangi *INTERFACE_TYPE* `T`, sağlanan `S` türetilir `T`.
*  Gelen bir *array_type* `S` bir öğe türü ile `SE` için bir *array_type* `T` bir öğe türü ile `TE`, aşağıdakilerin tümü doğru şunlardır:
    * `S` ve `T` yalnızca öğe türleri farklı. Diğer bir deyişle, `S` ve `T` aynı sayıda boyuta sahip.
    * Her ikisi de `SE` ve `TE` olan *reference_type*s.
    * Öğesinden örtük bir başvuru dönüştürmesi var `SE` için `TE`.
*  Herhangi *array_type* için `System.Array` ve arabirimlerini uygular.
*  Bir tek boyutlu dizi türünden `S[]` için `System.Collections.Generic.IList<T>` ve açık bir kimlik veya başvuru dönüştürme sağlanan temel arabirimlerini `S` için `T`.
*  Herhangi *delegate_type* için `System.Delegate` ve arabirimlerini uygular.
*  Null değişmez değer herhangi bir gelen *reference_type*.
*  Herhangi *reference_type* için bir *reference_type* `T` örtük kimlik veya başvuru dönüştürmeleri varsa bir *reference_type* `T0` ve `T0` bir kimlik dönüştürmesi bulunmadığında `T`.
*  Herhangi *reference_type* arabirim veya temsilci türünün `T` bir örtük kimlik veya başvuru dönüştürme bir arabirim veya temsilci türüne sahipse `T0` ve `T0` varyansı dönüştürülebilir (olduğu[ Varyans dönüştürme](interfaces.md#variance-conversion)) için `T`.
*  Başvuru türleri olarak bilinen tür parametreleri içeren örtük dönüştürme. Bkz: [tür parametreleri içeren örtük dönüştürmelerin](conversions.md#implicit-conversions-involving-type-parameters) tür parametreleri içeren örtük dönüştürmeleri hakkında daha fazla ayrıntı için.

Bu dönüştürmeler arasında örtük bir başvuru dönüşümleri olan *reference_type*her zaman başarılı olması için kanıtlanmış ve bu nedenle çalışma zamanında hiçbir denetim gerektiren s.

Başvuru dönüşümleri, örtük veya açık, hiçbir zaman dönüştürülen nesne başvuru kimliğini değiştirin. Başvuru türü başvuru dönüştürmesi değişebilir olsa da başka bir deyişle, hiçbir zaman türü veya başvurulan nesnenin değerini değiştirir.

### <a name="boxing-conversions"></a>Kutulama dönüştürmeler

Kutulama dönüştürmesi izin veren bir *value_type* başvuru türüne örtük olarak dönüştürülecek. Herhangi bir paketleme dönüştürmesi var *non_nullable_value_type* için `object` ve `dynamic`, `System.ValueType` ve herhangi bir *INTERFACE_TYPE* tarafından uygulanan *non_ nullable_value_type*. Ayrıca bir *enum_type* türüne dönüştürülebilir `System.Enum`.

Gelen bir paketleme dönüştürmesi mevcut bir *nullable_type* temel bir başvuru türü için bulunuyorsa ve yalnızca bir paketleme dönüştürmesi var *non_nullable_value_type* başvuru türü.

Bir arabirim türüne paketleme dönüşümü bir değer türünde `I` Kutulama dönüştürmesi bir arabirim türüne sahipse `I0` ve `I0` bir kimlik dönüştürmesi bulunmadığında `I`.

Bir arabirim türüne paketleme dönüşümü bir değer türünde `I` bir arabirim veya temsilci türünün paketleme dönüştürmesi varsa `I0` ve `I0` varyansı dönüştürülebilir olduğundan ([varyansı dönüştürme](interfaces.md#variance-conversion)) için`I`.

Değerini kutulama bir *non_nullable_value_type* nesne örneğini tahsis etme ve kopyalama oluşur *value_type* değerini söz konusu örneğine dönüştürür. Bir yapı türü kutulanabilir `System.ValueType`, tümü yapı için temel sınıf olan bu yana ([devralma](structs.md#inheritance)).

Değerini kutulama bir *nullable_type* aşağıdaki gibi çalışır:

*  Kaynak değeri null ise (`HasValue` özellik değer: false), sonucu hedef türünün bir null başvurudur.
*  Aksi takdirde, sonucu bir paketlenmiş bir başvurudur `T` çözülme ve kaynak değeri kutulama tarafından üretilen.

Kutulama dönüştürmeleri daha ayrıntılı açıklanır [kutulama dönüştürmeler](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Örtük dinamik dönüştürmeler

Türündeki bir ifade dinamik örtülü dönüştürme var `dynamic` herhangi bir tür için `T`. Dönüştürme dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)), yani örtük bir dönüştürme, çalışma zamanında çalışma zamanı tür ifade aranır `T`. Hiçbir dönüştürme bulunursa, bir çalışma zamanı özel durum oluşturulur.

Bu örtük dönüştürme görünüşte başında öneriler ihlal Not [örtük dönüştürmelerin](conversions.md#implicit-conversions) örtük bir dönüştürme hiçbir zaman bir özel durum neden olmamalıdır. Ancak bu dönüştürmeyi kendisi değil ancak *bulma* neden olan özel durumun dönüştürme. Çalışma zamanı özel durumlarını riskini dinamik bağlama kullanımını devralınır. Dinamik bağlama dönüştürme istenildiği gibi değilse, ifade ilk dönüştürülebilir `object`ve ardından istenen türde.

Aşağıdaki örnek örtük dinamik dönüştürme gösterilmektedir:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Atamaları `s2` ve `i` hem de bağlama işlemlerinin çalışma zamanı kadar askıya burada dinamik örtülü dönüştürmeler kullanmak istemiyorsunuz. Çalışma zamanında, çalışma zamanı türünden örtük dönüştürmelerin aranır `d`  --  `string` --hedef türü. Dönüştürme için bulunan `string` değil `int`.

### <a name="implicit-constant-expression-conversions"></a>Örtük sabit ifade dönüştürmeler

Örtük sabit ifade dönüştürme, aşağıdaki dönüştürmeleri izin verir:

*  A *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) türü `int` türüne dönüştürülebilir `sbyte`, `byte`, `short`, `ushort`, `uint`, veya `ulong`, değerini sağlanan *constant_expression* hedef türünün aralığı içinde.
*  A *constant_expression* türü `long` türüne dönüştürülebilir `ulong`, değerini sağlanan *constant_expression* negatif değil.

### <a name="implicit-conversions-involving-type-parameters"></a>Tür parametreleri içeren örtük dönüşümler

Belirtilen tür parametresi için aşağıdaki örtük dönüştürmeler mevcut `T`:

*  Gelen `T` etkili temel sınıfı için `C`, gelen `T` tüm taban sınıfı için `C`, gelen ve giden `T` tarafından uygulanan herhangi bir arabirim için `C`. AT çalışma zamanı Eğer `T` bir değer türü dönüştürme bir paketleme dönüştürmesi yürütülür. Aksi takdirde, dönüştürme örtük bir başvuru dönüştürmesi veya kimlik dönüştürme olarak yürütülür.
*  Gelen `T` bir arabirim türüne `I` içinde `T`etkili arabirimi kümesinde ve `T` herhangi temel arabirimine `I`. AT çalışma zamanı Eğer `T` bir değer türü dönüştürme bir paketleme dönüştürmesi yürütülür. Aksi takdirde, dönüştürme örtük bir başvuru dönüştürmesi veya kimlik dönüştürme olarak yürütülür.
*  Gelen `T` bir tür parametresine `U`, sağlanan `T` bağlıdır `U` ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)). AT çalışma zamanı Eğer `U` bir değer türü ise `T` ve `U` mutlaka aynı türdeyse ve hiçbir dönüştürme işlemi gerçekleştirilir. Aksi takdirde `T` bir değer türü dönüştürme bir paketleme dönüştürmesi yürütülür. Aksi takdirde, dönüştürme örtük bir başvuru dönüştürmesi veya kimlik dönüştürme olarak yürütülür.
*  Null değişmez değer gelen `T`, sağlanan `T` bir başvuru türü olarak bilinir.
*  Gelen `T` bir başvuru türü için `I` örtük bir dönüştürme için bir başvuru türü olup olmadığını `S0` ve `S0` bir kimlik dönüştürmesi bulunmadığında `S`. Çalışma zamanında dönüştürme dönüştürme aynı şekilde yürütülür `S0`.
*  Gelen `T` bir arabirim türüne `I` bir arabirim veya temsilci türüne örtük bir dönüştürme varsa `I0` ve `I0` varyansı dönüştürülebilir olduğundan için `I` ([varyansı dönüştürme](interfaces.md#variance-conversion) ). AT çalışma zamanı Eğer `T` bir değer türü dönüştürme bir paketleme dönüştürmesi yürütülür. Aksi takdirde, dönüştürme örtük bir başvuru dönüştürmesi veya kimlik dönüştürme olarak yürütülür.

Varsa `T` bir başvuru türü olarak bilinen ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)), yukarıdaki dönüştürmeleri tüm örtük bir başvuru dönüşümleri sınıflandırılan ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)). Varsa `T` olan bir başvuru türü bilinmiyor, yukarıdaki dönüştürmeler kutulama dönüşümleri sınıflandırılan ([kutulama dönüştürmeler](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Kullanıcı tanımlı örtük dönüşümler

Başka bir isteğe bağlı standart örtük dönüştürme tarafından izlenen bir kullanıcı tanımlı örtük dönüştürme işleci yürütülmesini arkasından isteğe bağlı standart örtülü dönüştürme, kullanıcı tanımlı bir örtük dönüştürme oluşur. Kullanıcı tanımlı örtük dönüştürmelerin değerlendirmek için kesin kurallar açıklanan [kullanıcı tanımlı örtük dönüştürmeler işleme](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Anonim işlev dönüştürmeler veya yöntem grubu dönüştürmeler

Anonim işlevler ve yöntem grupları türleri içinde ve kendilerini yoktur, ancak türleri ya da ifade ağaç türleri temsilci seçmek için örtük olarak dönüştürülebilir. Anonim işlev dönüştürmeleri daha ayrıntılı olarak açıklanmıştır [anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions) ve yöntem grubu Dönüşümlerde [yöntem grubu dönüştürmeler](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Açık dönüştürmeler

Aşağıdaki dönüştürmeleri, açık dönüştürmeler sınıflandırılan:

*  Tüm örtük dönüştürme.
*  Açık sayısal dönüşümler.
*  Açık numaralandırma dönüştürme.
*  Açık dönüştürmeler boş değer atanabilir.
*  Açık bir başvuru dönüşümleri.
*  Açık arabirim dönüşümler.
*  Kutudan çıkarma dönüştürme.
*  Dinamik açık dönüştürmeler
*  Kullanıcı tanımlı açık dönüştürmeler.

Açık dönüştürmeler cast ifadeleri meydana gelebilir ([Cast ifadeleri](expressions.md#cast-expressions)).

Açık dönüştürmeler tüm örtük dönüştürmelerin içermektedir. Başka bir deyişle, yedekli cast ifadeleri izin verilir.

Örtük dönüştürmeleri olmayan açık dönüştürmeler türleri merit açık yeterince farklı etki alanlarında her zaman başarılı olması için kanıtlanmış olamaz dönüştürmeler, büyük olasılıkla bilgileri kaybedersiniz bilinen dönüştürmeler ve dönüştürmeler olan gösterim.

### <a name="explicit-numeric-conversions"></a>Açık sayısal dönüşümler

Açık sayısal dönüşümler dönüşümlerse olan bir *numeric_type* diğerine *numeric_type* kendisi için bir örtülü sayısal dönüştürme ([örtük sayısal dönüşümler](conversions.md#implicit-numeric-conversions)) zaten mevcut değil:

*  Gelen `sbyte` için `byte`, `ushort`, `uint`, `ulong`, veya `char`.
*  Gelen `byte` için `sbyte` ve `char`.
*  Gelen `short` için `sbyte`, `byte`, `ushort`, `uint`, `ulong`, veya `char`.
*  Gelen `ushort` için `sbyte`, `byte`, `short`, veya `char`.
*  Gelen `int` için `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, veya `char`.
*  Gelen `uint` için `sbyte`, `byte`, `short`, `ushort`, `int`, veya `char`.
*  Gelen `long` için `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, veya `char`.
*  Gelen `ulong` için `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, veya `char`.
*  Gelen `char` için `sbyte`, `byte`, veya `short`.
*  Gelen `float` için `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, veya `decimal`.
*  Gelen `double` için `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, veya `decimal`.
*  Gelen `decimal` için `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, veya `double`.

Açık dönüştürmeler tüm örtük ve açık sayısal dönüşümler içerdiğinden, her zaman herhangi bir dönüştürme mümkün *numeric_type* herhangi diğer *numeric_type* atama ifadesi (kullanma [Cast ifadeleri](expressions.md#cast-expressions)).

Açık sayısal dönüştürmeler, büyük olasılıkla bilgileri kaybeder veya büyük olasılıkla özel durum oluşturulmasına neden. Açık sayısal dönüştürme şu şekilde işlenir:

*  İçin bir integral türünden başka bir integral türüne dönüştürme, içerik denetleme overflow'da işleme bağlıdır ([checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)) dönüştürme aldığı yerleştirin:
    * İçinde bir `checked` bağlamı, dönüştürmenin başarılı kaynak işlecinin değeri hedef türün aralığı içinde ancak oluşturur bir `System.OverflowException` kaynak işlecinin değeri hedef türün aralığı dışında ise.
    * İçinde bir `unchecked` bağlamını, dönüştürme her zaman başarılı olur ve aşağıdaki gibi çalışır.
        * Kaynak türü hedef türünden daha büyük ise sonra kaynak değeri, "fazladan" atarak kesilmiş en önemli bitleri. Sonuç, ardından hedef türünde bir değer kabul edilir.
        * Kaynak türü, hedef türünden küçükse, hedef türüyle aynı boyutta olması ardından kaynağı işaret genişletilmiş veya sıfır genişletilmiş değeridir. Kaynak türü açtıysanız oturum uzantısı kullanılır. Kaynak türü imzalanmamış ise sıfır uzantısı kullanılır. Sonuç, ardından hedef türünde bir değer kabul edilir.
        * Hedef türü olarak aynı boyutta kaynak türü ise kaynak değer hedef türünde bir değer kabul edilir.
*  Dönüştürme için `decimal` integral türü için kaynak değeri sıfır en yakın tamsayı değerine yuvarlanır ve dönüştürme sonucu olarak bu tamsayı değeri olur. Sonuçta elde edilen tamsayı değeri hedef türün aralığı dışında ise bir `System.OverflowException` oluşturulur.
*  Dönüştürme için `float` veya `double` integral türü için içerik denetimi overflow'da işleme bağlıdır ([checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)) dönüştürme aldığı yerleştirin:
    * İçinde bir `checked` bağlamı, dönüştürme geçer gibi:
        * İşlenenin NaN veya sonsuz, olması durumunda bir `System.OverflowException` oluşturulur.
        * Aksi takdirde, kaynak işlenen sıfır en yakın tamsayı değerine yuvarlanır. Bu tamsayı değeri hedef türün aralığı içinde ise, ardından bu dönüştürme sonucu olarak değerdir.
        * Aksi takdirde, bir `System.OverflowException` oluşturulur.
    * İçinde bir `unchecked` bağlamını, dönüştürme her zaman başarılı olur ve aşağıdaki gibi çalışır.
        * İşlenenin NaN veya sonsuz ise, dönüştürmenin sonucu hedef türü belirtilmemiş bir değerdir.
        * Aksi takdirde, kaynak işlenen sıfır en yakın tamsayı değerine yuvarlanır. Bu tamsayı değeri hedef türün aralığı içinde ise, ardından bu dönüştürme sonucu olarak değerdir.
        * Aksi takdirde, dönüştürmenin sonucu hedef türü belirtilmemiş bir değerdir.
*  Dönüştürme için `double` için `float`, `double` değer yuvarlanır en yakın `float` değeri. Varsa `double` değeri olarak temsil etmek için çok küçük bir `float`, sonuç pozitif sıfır veya negatif sıfır. Varsa `double` değeri olarak temsil etmek için çok büyük bir `float`, sonuç Pozitif sonsuz veya negatif sonsuz olur. Varsa `double` değeri NaN ise, NaN sonucudur de.
*  Dönüştürme için `float` veya `double` için `decimal`, kaynak değeri dönüştürülür `decimal` gösterimi ve gerekirse 28 ondalık basamağın sonra en yakın sayıya yuvarlanmasını ([decimal türü](types.md#the-decimal-type)). Kaynak değeri olarak temsil etmek için çok küçük ise bir `decimal`, sıfır sonuç olur. Kaynak değeri NaN ise, sonsuz veya olarak göstermek için çok büyük bir `decimal`, `System.OverflowException` oluşturulur.
*  Dönüştürme için `decimal` için `float` veya `double`, `decimal` değer yuvarlanır en yakın `double` veya `float` değeri. Bu dönüştürme duyarlık kaybedebileceği olsa da, hiçbir zaman bir özel durum oluşturulmasına neden olur.

### <a name="explicit-enumeration-conversions"></a>Açık numaralandırma dönüştürmeler

Açık numaralandırma dönüştürmeler şunlardır:

*  Gelen `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, veya `decimal` herhangi için*enum_type*.
*  Herhangi *enum_type* için `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, veya `decimal`.
*  Herhangi *enum_type* herhangi diğer *enum_type*.

İki tür arasında bir açık numaralandırma dönüştürme katılan tüm düşünerek işlenen *enum_type* , temel alınan türü olarak *enum_type*ve ardından kapalı veya açık gerçekleştirme Sonuç türleri arasında sayısal dönüştürme. Örneğin, belirtilen bir *enum_type* `E` ile ve temel alınan türü `int`, dönüştürme `E` için `byte` açık bir sayısal dönüştürme olarak işlenir ([açık sayısal dönüşümler](conversions.md#explicit-numeric-conversions)) öğesinden `int` için `byte`ve dönüştürme `byte` için `E` örtük sayısal dönüştürme olarak işlenir ([örtük sayısal dönüşümler](conversions.md#implicit-numeric-conversions)) gelen `byte` için `int`.

### <a name="explicit-nullable-conversions"></a>Boş değer atanabilir açık dönüştürmeler

***Boş değer atanabilir açık dönüştürmeler*** izin da bu türlerin boş değer atanabilir forms ile kullanılmak üzere atanamayan değer türleri üzerinde çalışan açık dönüştürmeler önceden tanımlanmış. Her bir NULL olmayan değer türünden dönüştürme önceden tanımlanmış açık dönüştürmeler `S` NULL olmayan bir değer türü için `T` ([kimlik dönüştürme](conversions.md#identity-conversion), [örtük sayısal dönüşümler](conversions.md#implicit-numeric-conversions), [Numaralandırma örtük dönüştürmeleri](conversions.md#implicit-enumeration-conversions), [açık sayısal dönüşümler](conversions.md#explicit-numeric-conversions), ve [açık numaralandırma dönüştürmeler](conversions.md#explicit-enumeration-conversions)), aşağıdaki boş değer atanabilir dönüştürmeler mevcuttur:

*  Açık bir dönüştürme `S?` için `T?`.
*  Açık bir dönüştürme `S` için `T?`.
*  Açık bir dönüştürme `S?` için `T`.

Boş değer atanabilir bir dönüştürmenin değerlendirme, temel alınan bir dönüştürme temel `S` için `T` aşağıdaki gibi çalışır:

*  Boş değer atanabilir bir dönüştürme ise `S?` için `T?`:
    * Kaynak değeri null ise (`HasValue` özellik değer: false), sonuç türü null değeri `T?`.
    * Aksi takdirde, dönüştürme bir gelen çözülme olarak değerlendirilir `S?` için `S`, temel alınan dönüştürme işlemi tarafından izlenen `S` için `T`diğerine kaydırma çizgidir `T` için `T?`.
*  Boş değer atanabilir bir dönüştürme ise `S` için `T?`, dönüştürme temel alınan dönüştürme işlemini değerlendirmesinde `S` için `T` diğerine kaydırma ardından `T` için `T?`.
*  Boş değer atanabilir bir dönüştürme ise `S?` için `T`, dönüştürme, bir gelen çözülme olarak değerlendirilir `S?` için `S` temel dönüştürme ardından `S` için `T`.

Değer ise bir boş değer atanabilir değer sarmalamadan çıkarma girişimi, bir özel durum oluşturur Not `null`.

### <a name="explicit-reference-conversions"></a>Açık bir başvuru dönüşümleri

Açık bir başvuru dönüşümleri şunlardır:

*  Gelen `object` ve `dynamic` herhangi diğer *reference_type*.
*  Herhangi *class_type* `S` herhangi *class_type* `T`, sağlanan `S` bir taban sınıfıdır `T`.
*  Herhangi *class_type* `S` herhangi *INTERFACE_TYPE* `T`, sağlanan `S` korumalı sağlanan değilse ve `S` uygulamıyor `T`.
*  Herhangi *INTERFACE_TYPE* `S` herhangi *class_type* `T`, sağlanan `T` korumalı sağlanan ya da `T` uygulayan `S`.
*  Herhangi *INTERFACE_TYPE* `S` herhangi *INTERFACE_TYPE* `T`, sağlanan `S` türünden türetilmediğinden `T`.
*  Gelen bir *array_type* `S` bir öğe türü ile `SE` için bir *array_type* `T` bir öğe türü ile `TE`, aşağıdakilerin tümü doğru şunlardır:
    * `S` ve `T` yalnızca öğe türleri farklı. Diğer bir deyişle, `S` ve `T` aynı sayıda boyuta sahip.
    * Her ikisi de `SE` ve `TE` olan *reference_type*s.
    * Gelen bir açık bir başvuru dönüştürmesi var `SE` için `TE`.
*  Gelen `System.Array` ve arabirimler için uygular *array_type*.
*  Bir tek boyutlu dizi türünden `S[]` için `System.Collections.Generic.IList<T>` ve bir açık başvuru dönüştürme sağlanan temel arabirimlerini `S` için `T`.
*  Gelen `System.Collections.Generic.IList<S>` ve bunun temel arabirimleri için bir tek boyutlu dizi türü `T[]`, bir açık kimlik veya başvuru dönüştürme olmasını sağlanan `S` için `T`.
*  Gelen `System.Delegate` ve arabirimler için uygular *delegate_type*.
*  Bir başvuru türü için bir başvuru türünden `T` bir açık bir başvuru dönüştürmesi için bir başvuru türü olup olmadığını `T0` ve `T0` sahip bir kimlik dönüştürme `T`.
*  Bir arabirim veya temsilci türüne başvuru türünden `T` arabirim veya temsilci türünün bir açık bir başvuru dönüştürmesi varsa `T0` ve her iki `T0` varyansı dönüştürülebilir olduğundan için `T` veya `T` olduğu Varyans dönüştürülebilir `T0` ([varyansı dönüştürme](interfaces.md#variance-conversion)).
*  Gelen `D<S1...Sn>` için `D<T1...Tn>` burada `D<X1...Xn>` bir genel temsilci türünde `D<S1...Sn>` uyumlu ya da aynı `D<T1...Tn>`ve her tür parametresi için `Xi` , `D` aşağıdaki tutar:
    * Varsa `Xi` sabit, ise `Si` aynıdır `Ti`.
    * Varsa `Xi` değişkendir, sonra bir örtük veya açık kimlik veya başvuru dönüştürmesi `Si` için `Ti`.
    * Varsa `Xi` değişken karşıtı, ise `Si` ve `Ti` ötekisi özdeş veya her ikisi de başvuru türleri.
*  Başvuru türleri olarak bilinen tür parametreleri içeren açık dönüştürmeler. Tür parametreleri içeren açık dönüştürmeler hakkında daha fazla bilgi için bkz. [tür parametreleri içeren açık dönüştürmeler](conversions.md#explicit-conversions-involving-type-parameters).

Bu dönüştürmeler doğru olduklarından emin olmak için çalışma zamanı denetimleri gerektiren başvuru türleri arasında açık bir başvuru dönüşümleri var.

Açık bir başvuru dönüştürmesi çalışma zamanında başarılı olması bir kaynak işlecinin değer olmalıdır `null`, ya da gerçek kaynak işlenen tarafından başvurulan nesne türü hedef türe örtük bir başvuru tarafından dönüştürülebilir bir türü olmalıdır dönüştürme ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) veya paketleme dönüştürmesi ([kutulama dönüştürmeler](conversions.md#boxing-conversions)). Bir açık başvuru dönüştürme başarısız olursa bir `System.InvalidCastException` oluşturulur.

Başvuru dönüşümleri, örtük veya açık, hiçbir zaman dönüştürülen nesne başvuru kimliğini değiştirin. Başvuru türü başvuru dönüştürmesi değişebilir olsa da başka bir deyişle, hiçbir zaman türü veya başvurulan nesnenin değerini değiştirir.

### <a name="unboxing-conversions"></a>Kutudan çıkarma dönüştürme

Bir başvuru türünün açıkça dönüştürülmesi için bir paket açma dönüşümünün izin veren bir *value_type*. Bir paketi açma dönüştürmesi var türlerinden `object`, `dynamic` ve `System.ValueType` herhangi *non_nullable_value_type*ve herhangi *INTERFACE_TYPE* herhangi *non_ nullable_value_type* uygulayan *INTERFACE_TYPE*. Ayrıca yazın `System.Enum` herhangi kutulanmamış olabilir *enum_type*.

Bir başvuru türü için gelen bir paketi açma dönüştürmesi mevcut bir *nullable_type* temel alınan bir paket açma dönüşümünün başvuru türü varsa *non_nullable_value_type* ,  *nullable_type*.

Bir değer türü `S` sahip bir paketi açma dönüştürmesi bir arabirim türünden `I` dönüşümünün bir arabirim türünden olup olmadığını `I0` ve `I0` bir kimlik dönüştürmesi bulunmadığında `I`.

Bir değer türü `S` sahip bir paketi açma dönüştürmesi bir arabirim türünden `I` bir arabirim veya temsilci türünden bir paketi açma dönüştürmesi varsa `I0` ve her iki `I0` varyansı dönüştürülebilir olduğundan için `I` veya `I`varyansı dönüştürülebilir olduğundan için `I0` ([varyansı dönüştürme](interfaces.md#variance-conversion)).

Bir kutudan çıkarma işlemi, ilk nesne örneği, kutulanmış bir değer olan denetimini oluşur verilen *value_type*ve ardından değeri örneğinin dışında kopyalama. Bir null başvuru kutudan çıkarma bir *nullable_type* null değerini üretir *nullable_type*. Bir yapı türünden kutulanmamış `System.ValueType`, tümü yapı için temel sınıf olan bu yana ([devralma](structs.md#inheritance)).

Kutudan çıkarma dönüştürme daha ayrıntılı açıklanır [kutudan çıkarma dönüştürme](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Dinamik açık dönüştürmeler

Türündeki bir ifade açık dinamik dönüştürme var `dynamic` herhangi bir tür için `T`. Dönüştürme dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)), yani açık bir dönüştürme, çalışma zamanında çalışma zamanı tür ifade aranır `T`. Hiçbir dönüştürme bulunursa, bir çalışma zamanı özel durum oluşturulur.

Dinamik bağlama dönüştürme istenildiği gibi değilse, ifade ilk dönüştürülebilir `object`ve ardından istenen türde.

Aşağıdaki sınıf tanımlanan varsayın:
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

Aşağıdaki örnekte, dinamik açık dönüştürmeler gösterilmektedir:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

En iyi dönüştürülmesi `o` için `C` derleme olması bir açık bir başvuru dönüştürmesi sırasında bulunamadı. Bu çalışma zamanında, çünkü başarısız `"1"` aslında değil bir `C`. Dönüştürme `d` için `C` ancak, çalışma zamanı, çalışma zamanı tür dönüştürme tanımlandığı bir kullanıcı için açık dinamik bir dönüştürme askıya `d`  --  `string` --çok `C` bulunursa ve başarılı olur.

### <a name="explicit-conversions-involving-type-parameters"></a>Tür parametreleri içeren açık dönüştürmeler

Belirtilen tür parametresi için aşağıdaki açık dönüştürmeler mevcut `T`:

*  Etkili temel sınıftan `C` , `T` için `T` ve herhangi bir taban sınıftan `C` için `T`. AT çalışma zamanı Eğer `T` bir değer türü dönüştürme bir kutudan çıkarma dönüştürme olarak yürütülür. Aksi takdirde, dönüştürme açık bir başvuru dönüştürmesi veya kimlik dönüştürme olarak yürütülür.
*  İçin herhangi bir arabirim türünden `T`. AT çalışma zamanı Eğer `T` bir değer türü dönüştürme bir kutudan çıkarma dönüştürme olarak yürütülür. Aksi takdirde, dönüştürme açık bir başvuru dönüştürmesi veya kimlik dönüştürme olarak yürütülür.
*  Gelen `T` herhangi *INTERFACE_TYPE* `I` örtük bir dönüştürme yok sağlanan `T` için `I`. AT çalışma zamanı Eğer `T` bir değer türü dönüştürme bir açık bir başvuru dönüştürmesi tarafından izlenen bir paketleme dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme açık bir başvuru dönüştürmesi veya kimlik dönüştürme olarak yürütülür.
*  Bir tür parametresi'den `U` için `T`, sağlanan `T` bağlıdır `U` ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)). AT çalışma zamanı Eğer `U` bir değer türü ise `T` ve `U` mutlaka aynı türdeyse ve hiçbir dönüştürme işlemi gerçekleştirilir. Aksi takdirde `T` bir değer türü dönüştürme bir kutudan çıkarma dönüştürme olarak yürütülür. Aksi takdirde, dönüştürme açık bir başvuru dönüştürmesi veya kimlik dönüştürme olarak yürütülür.

Varsa `T` olan bir başvuru türü olarak bilinen, yukarıdaki dönüştürmeleri tüm açık başvuru dönüşümleri sınıflandırılan ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)). Varsa `T` olan bir başvuru türü bilinmiyor, yukarıdaki dönüştürmeler kutudan çıkarma dönüştürme sınıflandırılan ([kutudan çıkarma dönüştürme](conversions.md#unboxing-conversions)).

Yukarıdaki kuralları, şaşırtıcı olabilir bir sınırlandırılmamış tür parametresi doğrudan Açık dönüştürme bir arabirim olmayan türe izin vermez. Bu kural için bir neden, Karışıklığı önlemek ve semantikleri açık tür dönüştürmeleri yapmaktır. Örneğin, aşağıdaki bildirimi ele alalım:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Varsa doğrudan açıkça dönüştürülmesini `t` için `int` bir kolayca beklenebilir izni, `X<int>.F(7)` döndürecekti `7L`. Standart sayısal dönüştürmeler, yalnızca türleri bağlama zamanında sayısal bir değer olduğunda kabul edildiği için ancak bunu istemezsiniz. Semantiği yapmak için Temizle, yukarıdaki örnekte bunun yerine yazılmış olmalıdır:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Bu kod artık derlemez ancak yürütme `X<int>.F(7)` ardından çalışma zamanında, bu yana paketlenmiş bir özel durum `int` doğrudan dönüştürülemeyen bir `long`.

### <a name="user-defined-explicit-conversions"></a>Kullanıcı tanımlı açık dönüştürmeler

Başka bir isteğe bağlı standart açık dönüştürme tarafından izlenen bir kullanıcı tanımlı dönüştürme örtük veya açık işlecinin yürütme sonrasında isteğe bağlı standart açık dönüştürme, kullanıcı tanımlı açık dönüştürme oluşur. Kullanıcı tanımlı açık dönüştürmeler değerlendirmek için kesin kurallar açıklanan [kullanıcı tanımlı açık dönüştürmeler işleme](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Standart dönüşümler

Standart dönüştürmeler bir kullanıcı tanımlı dönüştürme bir parçası olarak ortaya çıkabilecek bu önceden tanımlı dönüşümler ' dir.

### <a name="standard-implicit-conversions"></a>Standart örtük dönüştürmeler

Aşağıdaki örtük dönüştürmeler standart örtük dönüştürmeler sınıflandırılan:

*  Kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion))
*  Örtük sayısal Dönüşümler ([örtük sayısal dönüşümler](conversions.md#implicit-numeric-conversions))
*  Boş değer atanabilir örtülü dönüştürmeler ([boş değer atanabilir örtük dönüştürmelerin](conversions.md#implicit-nullable-conversions))
*  Örtük bir başvuru Dönüşümleri ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions))
*  Kutulama dönüştürmeler ([kutulama dönüştürmeler](conversions.md#boxing-conversions))
*  Örtük sabit ifade dönüştürmeler ([dinamik örtük dönüştürmelerin](conversions.md#implicit-dynamic-conversions))
*  Tür parametreleri içeren örtülü dönüştürmeler ([tür parametreleri içeren örtük dönüştürmelerin](conversions.md#implicit-conversions-involving-type-parameters))

Standart örtük dönüştürmeler, örtük dönüştürmelerin kullanıcı tanımlı özellikle hariç tutun.

### <a name="standard-explicit-conversions"></a>Standart açık dönüştürmeler

Standart açık dönüştürmeler, tüm standart örtük dönüştürmeler yanı sıra alt kümesini bir ters standart örtük dönüştürme bulunduğu açık dönüştürmeler ' dir. Diğer bir deyişle, standart bir örtük dönüştürme bir türden varsa `A` türüne `B`, standart bir açık dönüştürme türünden varsa `A` türüne `B` ve türünden `B` türüne `A`.

## <a name="user-defined-conversions"></a>Kullanıcı tanımlı dönüştürmeler

C# ile genişletilmesi önceden tanımlanmış örtük ve açık dönüştürmeler sağlar ***kullanıcı tanımlı dönüşümler***. Dönüştürme işleçleri bildirerek kullanıcı tanımlı dönüştürmeler sunulmuştur ([dönüştürme işleçleri](classes.md#conversion-operators)) sınıf ile yapı türleri.

### <a name="permitted-user-defined-conversions"></a>Kullanıcı tanımlı Dönüştürmelere izin verilir

C# bildirilmesi için yalnızca belirli kullanıcı tanımlı Dönüştürmelere izin verir. Özellikle, zaten var olan bir örtük veya açık dönüştürme tanımlanacak mümkün değildir.

Belirtilen kaynak türü için `S` ve hedef türü `T`, `S` veya `T` boş değer atanabilir türler izin `S0` ve `T0` Aksi takdirde, temel alınan türler için başvuru `S0` ve `T0` olan eşit `S` ve `T` sırasıyla. Bir kaynak türünden dönüştürme bildirmek için bir sınıf veya yapı izin `S` hedef türüyle `T` yalnızca aşağıdakilerin tümü doğru olduğunda:

*  `S0` ve `T0` farklı tür aşağıda verilmiştir.
*  Ya da `S0` veya `T0` işleç bildirimi yer alan sınıf veya yapı türü.
*  Ne `S0` ya da `T0` olduğu bir *INTERFACE_TYPE*.
*  Kullanıcı tanımlı dönüşümler, bir dönüştürme yok gelen `S` için `T` veya `T` için `S`.

Kullanıcı tanımlı dönüştürmeler için kısıtlamalar açıklanan daha ayrıntılı olarak [dönüştürme işleçleri](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Yükseltilmiş dönüştürme işleçleri

Bir NULL olmayan değer türünden dönüştüren bir kullanıcı tanımlı dönüştürme işleci göz önünde bulundurulduğunda `S` NULL olmayan bir değer türü için `T`, ***dönüştürme işleci yükseltilmiş*** var. Bu dönüştürür gelen `S?` için`T?`. Bu yükseltilmiş dönüştürme işleci bir gelen çözülmesini gerçekleştirir `S?` için `S` kullanıcı tanımlı dönüştürme ardından `S` için `T` diğerine kaydırma ardından `T` için `T?`dışında bir null değerli `S?` doğrudan bir null değerli dönüştürür `T?`.

Yükseltilmiş dönüştürme işleci, temel alınan kullanıcı tanımlı dönüştürme işleci olarak aynı örtük veya açık sınıflandırma vardır. "Kullanıcı tanımlı dönüştürme" her ikisi de kullanımını geçerli dönem, kullanıcı tanımlı ve dönüştürme işleçleri yükseltilmiş.

### <a name="evaluation-of-user-defined-conversions"></a>Kullanıcı tanımlı dönüşümler değerlendirmesi

Bir kullanıcı tanımlı dönüştürme çağrılır, türünden bir değer dönüştürür ***kaynak türünü***, başka bir türe adlı ***hedef türü***. Kullanıcı tanımlı bir dönüştürmenin değerlendirme merkezi bulma ile ilgili ***en belirgin*** belirli kaynak ve hedef türleri için kullanıcı tanımlı dönüştürme işleci. Bu belirleme, birkaç aşamaya ayrılır:

*  Sınıflar ve yapılar kullanıcı tanımlı dönüştürme işleçlerini kabul edilir kümesi bulunuyor. Bu kaynak türü ve temel sınıflarının ve hedef türü ve kendi temel sınıflarla (kullanıcı tanımlı işleçler yalnızca sınıfları ve yapıları bildirebilir ve sınıf olmayan türler temel sınıfa sahip örtük varsayımların) oluşur. Kaynak veya hedef türü ise bu adımı amaçları için bir *nullable_type*, kendi temel alınan türü kullanılan yerine.
*  Bu türleri kümesinden kullanıcı tanımlı ve dönüştürme işleçleri yükseltilmiş belirleme uygulanabilir. Bir dönüştürme operatörünün uygulanabilmesi standart bir dönüştürme gerçekleştirmek mümkün olmalıdır ([standart dönüştürmeler](conversions.md#standard-conversions)) işlenenin kaynak türünden işleci ve standart bir dönüştürme gerçekleştirmek mümkün türünde olmalıdır Hedef türü işlecinin sonucu türünden.
*  Geçerli kullanıcı tanımlı işleçler kümesinden hangi işleci dönüştürmelerle en belirgin değil belirleme. Genel koşullarını en belirgin işleci, işlenenin türü "kaynak türü için en yakın" ve "hedef türüne en yakın" sonuç türü olan işlecidir. Kullanıcı tanımlı dönüştürme işleçleri yükseltilmiş dönüştürme işleçleri tercih edilir. Aşağıdaki bölümlerde en belirgin kullanıcı tanımlı dönüştürme işleci oluşturma tam kurallarını tanımlanır.

En belirli bir kullanıcı tanımlı dönüştürme işleci belirlendikten sonra kullanıcı tanımlı dönüştürme gerçek yürütülmesi en çok üç adımdan oluşur:

*  İlk olarak, gerekli olursa yükseltilmiş veya kullanıcı tanımlı dönüştürme işleci işlenen türü kaynak türü bir standart dönüştürme gerçekleştirme.
*  Ardından, dönüştürmeyi gerçekleştirmek için yükseltilmiş veya kullanıcı tanımlı dönüştürme işleci çağrılıyor.
*  Son olarak, gerekli olursa yükseltilmiş veya kullanıcı tanımlı dönüştürme işleci sonuç türü hedef türüne standart dönüştürme gerçekleştirme.

Bir kullanıcı tanımlı dönüşümü hiçbir zaman birden fazla yükseltilmiş veya kullanıcı tanımlı dönüştürme işleci içeriyor. Diğer bir deyişle, türünden dönüştürme `S` türüne `T` bir kullanıcı tanımlı dönüşümü hiçbir zaman önce yürütülür `S` için `X` ve kullanıcı tanımlı dönüştürme yürüten `X` için `T`.

Kullanıcı tanımlı dönüştürme örtük veya açık değerlendirme tam tanımları aşağıdaki bölümlerde verilmiştir. Tanımları olun aşağıdaki koşulları kullanın:

*  Standart örtük bir dönüştürme ise ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) bir türden var. `A` türüne `B`ve kullanılmazsa `A` ya da `B` olan *INTERFACE_TYPE*s, ardından `A` olduğu söylenir ***ile çevrelenmiş*** `B`, ve `B` edilir ***kapsayacak*** `A`.
*  ***En kapsamlı türü*** türleri kümesi içinde kümedeki diğer tüm türleri kapsayan bir türdür. Tek tür diğer tüm türleri kapsıyorsa, kümenin en kapsamlı tür yok. En kapsamlı türü daha sezgisel bağlamında, "büyük" kümesindeki türüdür — istediğiniz diğer türlerinin her biri örtük olarak dönüştürülebilir bir tür.
*  ***Türü'en çevrelenmiş*** türleri kümesi, kümedeki diğer tüm türleri ile çevrelenmiş bir türdür. Ardından tek tür diğer türleri tarafından kapsadığı, küme türü çevrelenmiş Hayır en. En encompassed türü daha sezgisel bağlamında, "küçük" kümesindeki türüdür; her diğer türleri için örtük olarak dönüştürülebilir bir tür.

### <a name="processing-of-user-defined-implicit-conversions"></a>İşlem kullanıcı tarafından tanımlanan örtük dönüştürme

Türünden kullanıcı tanımlı örtük dönüştürme `S` türüne `T` şu şekilde işlenir:

*  Türlerini belirleyin `S0` ve `T0`. Varsa `S` veya `T` boş değer atanabilir türler `S0` ve `T0` Aksi takdirde, temel alınan türler olan `S0` ve `T0` eşit olan `S` ve `T` sırasıyla.
*  Bulma türleri, `D`, hangi kullanıcı tanımlı dönüştürme işleçlerini kabul edilir. Bu kümesi oluşur `S0` (varsa `S0` bir sınıf veya yapı), temel sınıflarını `S0` (varsa `S0` bir sınıf), ve `T0` (varsa `T0` bir sınıf veya yapı).
*  Geçerli kullanıcı tanımlı ve yükseltilmiş dönüştürme işleçleri kümesini bulmak `U`. Bu sınıflar veya yapılar için tarafından bildirilen kullanıcı tanımlı ve yükseltilmiş örtük dönüşüm işleçleri oluşur `D` gelen bir kapsayan tür dönüştürme `S` ile çevrelenmiş bir türe `T`. Varsa `U` boş dönüştürme tanımlı değildir ve bir derleme zamanı hatası oluşur.
*  En belirgin kaynak türü bulma `SX`, işleçler, `U`:
    * İşleçler, biri geçerliyse `U` dönüştürmek `S`, ardından `SX` olduğu `S`.
    * Aksi takdirde, `SX` en encompassed kaynak türleri, işleçler, birleştirilmiş bir dizi türünde `U`. Tam olarak bir en çevrelenmiş tür bulunamıyor, sonra dönüştürme belirsiz değildir ve bir derleme zamanı hatası oluşur.
*  En belirgin hedef türü, bulma `TX`, işleçler, `U`:
    * İşleçler, biri geçerliyse `U` Dönüştür `T`, ardından `TX` olduğu `T`.
    * Aksi takdirde, `TX` en kapsamlı türü hedef türleri, işleçler, birleştirilmiş bir dizi içinde `U`. En kapsamlı tam olarak bir tür dönüştürme belirsiz olduğu ve bir derleme zamanı hatası oluşur.
*  En belirgin dönüştürme işleci bulun:
    * Varsa `U` dönüştürür tam olarak bir kullanıcı tanımlı dönüştürme işleci içeren `SX` için `TX`, bu ise en belirgin dönüştürme işleci.
    * Aksi takdirde `U` dönüştürür tam olarak bir yükseltilmiş dönüştürme işleci içeren `SX` için `TX`, bu ise en belirgin dönüştürme işleci.
    * Aksi takdirde, dönüştürme belirsiz değildir ve bir derleme zamanı hatası oluşur.
*  Son olarak, bir dönüştürme uygulanır:
    * Varsa `S` değil `SX`, ardından standart örtük dönüştürme `S` için `SX` gerçekleştirilir.
    * En belirgin dönüştürme işleci dönüştürmek için çağrılan `SX` için `TX`.
    * Varsa `TX` değil `T`, ardından standart örtük dönüştürme `TX` için `T` gerçekleştirilir.

### <a name="processing-of-user-defined-explicit-conversions"></a>Kullanıcı tanımlı açık dönüştürmeler işlenmesini

Türünden kullanıcı tanımlı açık dönüştürme `S` türüne `T` şu şekilde işlenir:

*  Türlerini belirleyin `S0` ve `T0`. Varsa `S` veya `T` boş değer atanabilir türler `S0` ve `T0` Aksi takdirde, temel alınan türler olan `S0` ve `T0` eşit olan `S` ve `T` sırasıyla.
*  Bulma türleri, `D`, hangi kullanıcı tanımlı dönüştürme işleçlerini kabul edilir. Bu kümesi oluşur `S0` (varsa `S0` bir sınıf veya yapı), temel sınıflarını `S0` (varsa `S0` bir sınıftır), `T0` (varsa `T0` bir sınıf veya yapı) ve temel sınıfları `T0` (varsa `T0`bir sınıfı).
*  Geçerli kullanıcı tanımlı ve yükseltilmiş dönüştürme işleçleri kümesini bulmak `U`. Bu kullanıcı tarafından tanımlanan ve lifted örtük oluşur veya açık dönüştürme işleçleri bildirilen sınıflar veya yapılar için `D` gelen bir kapsayan tür dönüştürme veya ile çevrelenmiş `S` kapsayan veya ileçevrelenmişbirtürü`T`. Varsa `U` boş dönüştürme tanımlı değildir ve bir derleme zamanı hatası oluşur.
*  En belirgin kaynak türü bulma `SX`, işleçler, `U`:
    * İşleçler, biri geçerliyse `U` dönüştürmek `S`, ardından `SX` olduğu `S`.
    * Aksi durumda, işleçler, biri geçerliyse `U` kapsayacak türlerinden dönüştürme `S`, ardından `SX` kaynağı bu işleçleri tür birleşik kümesindeki en encompassed türüdür. Çevrelenmiş hiçbir çoğu, türü bulunabilir, daha sonra dönüştürme belirsiz ve bir derleme zamanı hatası oluşur.
    * Aksi takdirde, `SX` en kapsamlı kaynak türleri, işleçler, birleştirilmiş bir dizi türünde `U`. En kapsamlı tam olarak bir tür dönüştürme belirsiz olduğu ve bir derleme zamanı hatası oluşur.
*  En belirgin hedef türü, bulma `TX`, işleçler, `U`:
    * İşleçler, biri geçerliyse `U` Dönüştür `T`, ardından `TX` olduğu `T`.
    * Aksi durumda, işleçler, biri geçerliyse `U` tarafından çevrelenen türlere dönüştürmeniz `T`, ardından `TX` hedef türleri bu işleçleri birleştirilmiş bir dizi içinde en kapsamlı bir türdür. En kapsamlı tam olarak bir tür dönüştürme belirsiz olduğu ve bir derleme zamanı hatası oluşur.
    * Aksi takdirde, `TX` en encompassed türü hedef türleri, işleçler, birleştirilmiş bir dizi içinde `U`. Çevrelenmiş hiçbir çoğu, türü bulunabilir, daha sonra dönüştürme belirsiz ve bir derleme zamanı hatası oluşur.
*  En belirgin dönüştürme işleci bulun:
    * Varsa `U` dönüştürür tam olarak bir kullanıcı tanımlı dönüştürme işleci içeren `SX` için `TX`, bu ise en belirgin dönüştürme işleci.
    * Aksi takdirde `U` dönüştürür tam olarak bir yükseltilmiş dönüştürme işleci içeren `SX` için `TX`, bu ise en belirgin dönüştürme işleci.
    * Aksi takdirde, dönüştürme belirsiz değildir ve bir derleme zamanı hatası oluşur.
*  Son olarak, bir dönüştürme uygulanır:
    * Varsa `S` değil `SX`, ardından standart açık dönüştürme `S` için `SX` gerçekleştirilir.
    * En belirgin kullanıcı tanımlı dönüştürme işleci dönüştürmek için çağrılan `SX` için `TX`.
    * Varsa `TX` değil `T`, ardından standart açık dönüştürme `TX` için `T` gerçekleştirilir.

## <a name="anonymous-function-conversions"></a>Anonim işlev dönüştürme

Bir *anonymous_method_expression* veya *lambda_expression* anonim bir işlev sınıflandırılır ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)). İfade, bir tür yoktur ama bir uyumlu temsilci türü ya da ifade ağaç türüne örtük olarak dönüştürülebilir. Özellikle, anonim bir işlev `F` bir temsilci türüyle uyumlu `D` sağlanan:

*  Varsa `F` içeren bir *anonymous_function_signature*, ardından `D` ve `F` parametreleri aynı sayıda sahip.
*  Varsa `F` içermeyen bir *anonymous_function_signature*, ardından `D` hiçbir parametresi olarak uzun, her tür sıfır veya daha fazla parametrelere sahip `D` sahip `out` parametre değiştiricisi.
*  Varsa `F` her parametresinde bir açıkça belirtilmiş bir parametre listesine sahip `D` karşılık gelen parametre olarak aynı türde ve değiştiricilere sahip `F`.
*  Varsa `F` bir örtük olarak yazılan parametre listesine sahip `D` hiçbir `ref` veya `out` parametreleri.
*  Varsa gövdesinin `F` bir ifade ve ya da `D` sahip bir `void` dönüş türü veya `F` zaman uyumsuz olduğunu ve `D` dönüş türüne sahip `Task`, sonra ne zaman her parametresinin `F` türünü verilir karşılık gelen parametre `D`, gövdesi `F` geçerli bir ifade (wrt [ifadeleri](expressions.md)), izin olarak bir *statement_expression* ([İfade deyimleri](statements.md#expression-statements)).
*  Varsa gövdesinin `F` bir deyim bloğunu ve ya da `D` sahip bir `void` dönüş türü veya `F` zaman uyumsuz olduğunu ve `D` dönüş türüne sahip `Task`, sonra ne zaman her parametresinin `F` türünü verilir karşılık gelen parametre `D`, gövdesi `F` geçerli deyim bloğu (wrt [blokları](statements.md#blocks)) hangi Hayır olarak `return` deyimi bir ifade belirtir.
*  Varsa gövdesinin `F` bir ifadedir ve *ya da* `F` olmayan zaman uyumsuz olduğunu ve `D` void olmayan dönüş türü olan `T`, *veya* `F` zaman uyumsuz olduğunu ve `D` dönüş türü olan `Task<T>`, sonra her parametresinin `F` karşılık gelen parametre türünü verilen `D`, gövdesi `F` geçerli bir ifade (wrt [ İfadeleri](expressions.md)), örtük olarak dönüştürülebilir `T`.
*  Varsa gövdesinin `F` bir deyim bloğunu olduğu ve *ya da* `F` olmayan zaman uyumsuz olduğunu ve `D` void olmayan dönüş türü olan `T`, *veya* `F` zaman uyumsuz olduğunu ve `D` dönüş türü olan `Task<T>`, sonra her parametresinin `F` karşılık gelen parametre türünü verilen `D`, gövdesi `F` geçerli deyim bloğu (wrt [blokları ](statements.md#blocks)) her erişilebilir olmayan bir uç noktasıyla `return` ifadesi örtük olarak dönüştürülebilir bir ifade belirtir `T`.

Konuyu uzatmamak amacıyla, bu bölümde kısa formunu görev türleri için kullanılır. `Task` ve `Task<T>` ([zaman uyumsuz işlevleri](classes.md#async-functions)).

Bir lambda ifadesi `F` bir ifade ağacı türü ile uyumlu `Expression<D>` varsa `F` temsilci türüyle uyumlu `D`. Bu anonim yöntem, lambda ifadeleri uygulanmaz unutmayın.

Belirli lambda ifadeleri ifade ağacı türlere dönüştürülemez: Olsa da dönüştürmenin *var*, derleme zamanında başarısız olur. Bu büyük/küçük harf ise lambda ifadesi:

*  Sahip bir *blok* gövdesi
*  Basit ve bileşik atama işleçleri içeriyor
*  Dinamik olarak bağlı bir ifade içeriyor
*  Zaman uyumsuz olduğu

Genel temsilci türünde aşağıdaki örneklerde kullanın `Func<A,R>` türünde bir bağımsız değişken bir işlevi temsil eden `A` ve türünde bir değer döndürür `R`:
```csharp
delegate R Func<A,R>(A arg);
```

Atamaları
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
Her anonim işlev parametre ve dönüş türleri anonim işlev atanmış olduğu değişken türünden belirlenir.

İlk atama, anonim işlev başarıyla temsilci türüne dönüştürür. `Func<int,int>` olduğundan, `x` türü verilen `int`, `x+1` türüne örtük olarak dönüştürülebilir bir geçerli ifade `int`.

Benzer şekilde, ikinci atama başarıyla anonim işlev temsilci türüne dönüştürür `Func<int,double>` çünkü sonucunu `x+1` (tür `int`) türüne açıkça dönüştürülemez `double`.

Ancak, üçüncü bir derleme zamanı hatası nedeniyle atamadır, `x` türü verilen `double`, sonucunu `x+1` (tür `double`) türüne örtük olarak dönüştürülebilir değil `int`.

Dördüncü atama, anonim zaman uyumsuz işlev başarıyla temsilci türüne dönüştürür. `Func<int, Task<int>>` çünkü sonucunu `x+1` (tür `int`) sonuç türüne açıkça dönüştürülemez `int` görev türü `Task<int>`.

Anonim işlevler aşırı yükleme çözünürlüğü etkiler ve içinde tür çıkarımı katılın. Bkz: [işlev üyeleri](expressions.md#function-members) daha ayrıntılı bilgi için.

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Değerlendirme temsilci türleri olarak anonim işlev dönüştürme

Anonim bir işlevin bir temsilci türüne dönüştürme anonim işlev ve değerlendirme sırasındaki etkin olan dış yakalanan değişkenlere (büyük olasılıkla boş) kümesini başvuran bir temsilci örneği oluşturur. Temsilci çağrıldığında, anonim işlev gövdesi yürütülür. Temsilci tarafından başvurulan yakalanan dış değişkenler kümesini kullanarak gövdesindeki kod yürütülür.

Anonim bir işlevden üretilen bir temsilcinin çağırma listesi tek bir giriş içerir. Hedef yöntemin temsilcisi ve tam hedef nesne belirtilmemiş. Özellikle, bu temsilci hedef nesneye olup belirtilmezse `null`, `this` kapsayan işlev üyesi veya başka bir nesnenin değeri.

Anlamsal olarak eşdeğer anonim işlevleri aynı temsilci türleri için dönüştürme yakalanan dış değişken örnekleri aynı (büyük olasılıkla boş) kümesi ile izin verilir (ancak gerekli değildir) aynı temsilci örneği dönün. Anlamsal olarak eşdeğer terimi, burada anonim işlevler yürütülmesini her durumda, aynı bağımsız değişkenleri verilen aynı etkileri üreteceği ifade etmek için kullanılır. Bu kural, en iyi duruma getirilmesi aşağıdaki gibi bir kod izin verir.

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

İki anonim işlev temsilcileri aynı (boş) yakalanan dış değişkenlerin ve anonim işlevler anlamsal olarak aynı olduğundan olduğundan, derleyici başvurmak için aynı hedef yöntemin temsilcilerin izin verilir. Aslında, derleyici hem anonim işlev ifadeleri çok aynı temsilci örneği döndürmek için izin verilir.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>İfade ağacı türlerine dönüştürme işlemi anonim İşlev değerlendirmesi

İfade ağacı anonim bir işlevin bir ifade ağacı türü dönüştürme üretir ([ifade ağacı türleri](types.md#expression-tree-types)). Anonim işlev yapısını temsil eden bir nesne yapısını oluşumu için daha kesin bir şekilde değerlendirme anonim işlev dönüştürme yol açar. Uygulama tanımlı kesin yapısını ifade ağacı yanı sıra, tam, oluşturma işlemi var.

### <a name="implementation-example"></a>Uygulama örneği

Bu bölümde, anonim işlev dönüştürmeler diğer C# yapılarını açısından olası bir uygulaması açıklanmaktadır. Burada açıklanan uygulama Microsoft C# derleyicisi tarafından kullanılan aynı ilkeler temel alır, ancak göre Hayır olmadığı bir mandated uygulamasıdır veya yalnızca bir mümkün değil. Bu belirtim kapsamı dışında tam semantiği olduğu gibi yalnızca kısa bir süreliğine ifade ağaçlarında yapılan dönüştürmeler bahseder.

Bu bölümün geri kalanında, çeşitli farklı özelliklere sahip anonim işlevler içeren kod örneklerini sağlar. Her örnek için yalnızca diğer C# yapılarını kullanan kod için karşılık gelen bir çeviri sağlanır. Örneklerde, tanımlayıcı `D` aşağıdaki temsilci türünü temsil tarafından kabul edilir:
```csharp
public delegate void D();
```

Anonim bir işlevdir en basit biçimi dış değişken yakalayan biridir:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Bu, anonim işlev kodunu yerleştirildiği bir derleyicinin ürettiği statik bir yönteme başvuran bir temsilci örneklemesine çevrilebilir:
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

Aşağıdaki örnekte, örnek üyeleri anonim işlev başvuruyor `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Bu, derleyicinin ürettiği örnek yöntemine anonim işlev kodunu içeren bir çevrilebilir:
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

Bu örnekte, anonim işlev bir yerel değişken yakalar:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

Yerel değişken ömrü artık en az anonim işlev temsilcisi ömrünü genişletilmesi gerekir. Bu, "yerel değişken bir derleyicinin ürettiği sınıfının bir alanı hoisting tarafından" gerçekleştirilebilir. Yerel değişken örneğinin ([yerel değişkenler örneğinin](expressions.md#instantiation-of-local-variables)) derleyicinin ürettiği sınıfı ve yerel değişken karşılık gelen örneğini bir alana erişimi erişen bir örneğini oluşturmak için'karşılık gelir Derleyicinin ürettiği sınıfı. Ayrıca, derleyicinin ürettiği sınıfının bir örneği yöntemi anonim işlev olur:
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

Son olarak, aşağıdaki anonim yakalamaları işlev `this` iki yerel değişkenler farklı ömürleriyle yanı sıra:
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

Burada, derleyicinin ürettiği sınıfı for each deyimi oluşturulur hangi Yereller yakalanan farklı bloklar yerel öğeler, bağımsız yaşam süreleri olabilir, içinde engelleyin. Örneği `__Locals2`, iç deyim bloğu için derleyicinin ürettiği sınıfı içeren yerel değişken `z` örneği başvuran bir alan `__Locals1`.  Örneği `__Locals1`, derleyicinin ürettiği sınıfı için dış bir deyim bloğunu içeren yerel değişken `y` ve başvuran bir alan `this` kapsayan işlevi üyesinin. Bu veri yapıları ulaşmak mümkün olduğu bir örneği üzerinden dış değişkenlere yakalanan `__Local2`, ve anonim işlev kodunu böylece söz konusu sınıfın bir örnek yöntemi olarak uygulanabilir.

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

Anonim işlevler için ifade ağaçları dönüştürülürken burada yerel değişkenler yakalamak için uygulanan aynı yöntemleri de kullanılabilir: Derleyicinin ürettiği nesnelere başvuru ifade ağacında depolanabilir ve bu nesneler üzerinde alan erişir gibi yerel değişkenlere erişim temsil edilebilir. Bu yaklaşımın avantajı, temsilciler ve ifade ağaçları arasında paylaşılmak üzere "lifted" yerel değişkenler olanak sağlamasıdır.

## <a name="method-group-conversions"></a>Yöntem grubu dönüştürmeler

Örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) bir yöntem grubu var ([ifade sınıflandırmaları](expressions.md#expression-classifications)) uyumlu temsilci türü. Bir temsilci türü verilen `D` ve ifade `E` öğesinden örtük bir dönüştürme var, bir yöntem grubu sınıflandırılan `E` için `D` varsa `E` içinde normal bir form (geçerli olan en az bir yöntem içerir [Geçerli işlev üyesi](expressions.md#applicable-function-member)) için bir bağımsız değişken listesi parametre türlerinin kullanımı ve öğesinin değiştiricilerini tarafından oluşturulan `D`aşağıda açıklandığı gibi.

Bir yöntem grubu dönüştürme derleme zamanı uygulamasının `E` temsilci türüne `D` aşağıda açıklanmıştır. Unutmayın örtük bir dönüştürme varlığını `E` için `D` dönüştürme derleme zamanı uygulamasının hata başarılı olduğunu garanti etmez.

*  Tek bir yöntem `M` bir yöntem çağırma için karşılık gelen seçili ([yöntem çağrıları](expressions.md#method-invocations)) biçiminde `E(A)`, aşağıdaki değişiklikler ile:
    * Bağımsız değişken listesi `A` ifadeleri, bir değişken olarak ve tür ve değiştiricisi ile her sınıflandırılmış bir listesi verilmiştir (`ref` veya `out`) karşılık gelen parametre, *formal_parameter_list* ,`D`.
    * Normal formlarına için geçerli olan yöntemlerden kabul aday yöntemler şunlardır ([geçerli işlev üyesi](expressions.md#applicable-function-member)), olanları yalnızca kendi genişletilmiş biçimde uygulanabilir.
*  Varsa algoritmasının [yöntem çağrıları](expressions.md#method-invocations) bir derleme zamanı hatası oluşursa bir hata üretir. Aksi takdirde algoritma tek bir en iyi yöntem üretir `M` aynı sayıda parametrelere sahip `D` ve dönüştürme yok sayılır.
*  Seçilen yöntemi `M` uyumlu olması gerekir ([temsilci Uyumluluk](delegates.md#delegate-compatibility)) temsilci türüyle `D`, veya aksi halde, bir derleme zamanı hatası oluşur.
*  Seçilen yöntemi `M` örnek ifade ile ilişkili bir örnek yöntemi olduğundan `E` hedef nesneye temsilci belirler.
*  Seçili yöntem M örnek bir ifade üzerinde bir üye erişimi ile belirtilir bir genişletme yöntemi ise, hedef nesneye temsilci örneği ifade belirler.
*  Dönüştürme sonucu olarak bir tür değeri `D`, yani seçili yöntemi ve hedef nesneye başvuran bir yeni oluşturulan temsilci.
*  Bu işlem bir genişletme yöntemi için bir temsilci oluşturulmasına yol açabilir Not algoritmasının [yöntem çağrıları](expressions.md#method-invocations) bir örnek yöntemi bulmada başarısız ancak çağırmayı işlenirken başarılı `E(A)` bir uzantısı olarak yöntem çağırma ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)). Bu nedenle oluşturulan bir temsilci, genişletme yönteminin yanı sıra, ilk bağımsız değişkeninin yakalar.

Aşağıdaki örnekte, yöntem grubu dönüştürmeler göstermektedir:
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

Atamayı `d1` örtük olarak yöntem grubu dönüştürür `F` türünde bir değer için `D1`.

Atamayı `d2` nasıl daha az türetilmiş (değişken karşıtı) parametre türleri içeren bir yöntem için temsilci oluşturmak mümkündür ve daha türetilmiş (değişken) dönüş türü gösterir.

Atamayı `d3` nasıl dönüştürme var yöntemi uygulanabilir değilse gösterir.

Atamayı `d4` nasıl yöntemi, normal bir biçimde uygun olmalıdır gösterir.

Atamayı `d5` parametre ve dönüş türlerinin ve temsilci yöntemi yalnızca başvuru türleri için farklı nasıl verildiğini gösterir.

İle tüm diğer örtük ve açık dönüştürmeler gibi atama işleci açıkça bir yöntem grubu dönüştürme gerçekleştirmek için kullanılabilir. Bu nedenle, örneğin
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
Bunun yerine yazılabilir
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Yöntemi gruplarına aşırı yükleme çözünürlüğü etkilemek ve içinde tür çıkarımı katılın. Bkz: [işlev üyeleri](expressions.md#function-members) daha ayrıntılı bilgi için.

Bir yöntem grubu dönüştürmenin çalışma zamanı değerlendirme gibi çalışır:

*  Derleme zamanında seçilen yöntemi bir örnek yöntemi olduğundan veya bir örnek yöntemi erişilebilen bir genişletme yöntemi, temsilci hedef nesnesi ile ilişkili örneği ifadesinden belirlenir `E`:
    * Örnek ifade değerlendirilir. Bu değerlendirme bir özel durum neden olursa, başka bir adım yürütülür.
    * Örnek ifade ise bir *reference_type*, örnek ifade tarafından hesaplanan değer hedef nesne haline gelir. Seçilen yöntemi bir örnek yöntemi olduğundan ve hedef nesnesi `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.
    * Örnek ifade ise bir *value_type*, bir paketleme işlemi ([kutulama dönüştürmeler](types.md#boxing-conversions)) değeri bir nesneye dönüştürmek için gerçekleştirilir ve bu nesne, hedef nesne haline gelir.
*  Aksi takdirde seçilen yöntemi statik yöntem çağrısı bir parçasıdır ve temsilcinin hedef nesnesi `null`.
*  Temsilci türüne yeni bir örneğini `D` ayrılır. Yeni bir örneğini ayırmak yeterli bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülür.
*  Derleme zamanında belirlenen bir yönteme başvuru ile yeni temsilci örneği başlatılır ve hedef nesneye bir başvuru üzerinde hesaplanan.
