---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868010"
---
# <a name="conversions"></a>Dönüşümler

Bir ***dönüştürme*** , bir ifadenin belirli bir türde olması halinde değerlendirilmesine olanak sağlar. Bir dönüştürme, belirli bir türün ifadesinin farklı bir tür varmış gibi işlenmesine neden olabilir veya tür olmayan bir ifadenin tür almasını sağlayabilir. Dönüştürmeler ***örtük*** veya ***Açık***olabilir ve bu, açık bir dönüştürmenin gerekli olup olmadığını belirler. Örneğin, `int` türünden `long` türüne dönüştürme örtük olur, bu nedenle `int` türündeki ifadeler örtük olarak tür `long`olarak kabul edilebilir. `long` türünden `int`türüne ters dönüştürme açık olduğundan açık bir dönüştürme gerekir.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Bazı dönüşümler dil tarafından tanımlanır. Programlar, kendi dönüştürmelerini ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)) de tanımlayabilir.

## <a name="implicit-conversions"></a>Örtük dönüştürmeler

Aşağıdaki dönüşümler örtük dönüştürmeler olarak sınıflandırılmaktadır:

*  Kimlik dönüştürmeleri
*  Örtük Sayısal dönüştürmeler
*  Örtük numaralandırma dönüştürmeleri
*  Örtülü olarak bulunan dize dönüşümleri
*  Örtük null yapılabilir dönüşümler
*  Null değişmez değer dönüştürmeleri
*  Örtük başvuru dönüştürmeleri
*  Paketleme dönüştürmeleri
*  Örtük dinamik dönüştürmeler
*  Örtük sabit ifade dönüştürmeleri
*  Kullanıcı tanımlı örtük dönüştürmeler
*  Anonim işlev dönüştürmeleri
*  Yöntem grubu dönüştürmeleri

Örtük dönüştürmeler işlev üyesi etkinleştirmeleri ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), atama Ifadeleri (atama işleçleri[) ve](expressions.md#cast-expressions)atamalar ([atama işleçleri](expressions.md#assignment-operators)) dahil olmak üzere çeşitli durumlarda gerçekleşebilir.

Önceden tanımlanmış örtük dönüştürmeler her zaman başarılı olur ve hiçbir zaman özel durum oluşturulmasına neden olmaz. Doğru şekilde tasarlanan Kullanıcı tanımlı örtük dönüştürmeler bu özellikleri de göstermelidir.

Dönüştürme amaçları doğrultusunda `object` ve `dynamic` türleri eşdeğer kabul edilir.

Ancak dinamik dönüştürmeler ([örtük dinamik dönüştürmeler](conversions.md#implicit-dynamic-conversions) ve [Açık dinamik dönüştürmeler](conversions.md#explicit-dynamic-conversions)) yalnızca `dynamic` türündeki ifadeler için geçerlidir ([dinamik tür](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Kimlik dönüştürme

Bir kimlik dönüştürmesi herhangi bir türden aynı türe dönüştürür. Bu dönüştürme, zaten gerekli bir türü olan bir varlığın bu türe dönüştürülebilir olduğunu söylebilmelidir.

*  `object` ve `dynamic` eşit kabul edildiğinden, `object` ve `dynamic`arasında bir kimlik dönüştürmesi ve tüm `dynamic` tekrarlarının `object`ile değiştirilmesi sırasında aynı olan oluşturulmuş türler arasında.

### <a name="implicit-numeric-conversions"></a>Örtük Sayısal dönüştürmeler

Örtük Sayısal dönüştürmeler şunlardır:

*  `sbyte` `short`, `int`, `long`, `float`, `double`veya `decimal`.
*  `byte` `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`veya `decimal`.
*  `short` `int`, `long`, `float`, `double`veya `decimal`.
*  `ushort` `int`, `uint`, `long`, `ulong`, `float`, `double`veya `decimal`.
*  `int` `long`, `float`, `double`veya `decimal`.
*  `uint` `long`, `ulong`, `float`, `double`veya `decimal`.
*  `long` `float`, `double`veya `decimal`.
*  `ulong` `float`, `double`veya `decimal`.
*  `char` `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`veya `decimal`.
*  `float` `double`.

`int`, `uint`, `long`ya da `ulong` arasında dönüştürme `float` ve `long` ya da `ulong` `double`, bir duyarlık kaybına neden olabilir, ancak hiçbir şekilde bir büyüklük kaybına neden olmaz. Diğer örtük sayısal dönüştürmeler hiçbir bilgiyi hiçbir şekilde kaybetmez.

`char` türüne örtük dönüştürmeler yoktur, bu nedenle diğer integral türlerin değerleri otomatik olarak `char` türüne dönüştürülemez.

### <a name="implicit-enumeration-conversions"></a>Örtük numaralandırma dönüştürmeleri

Örtük bir numaralandırma dönüştürmesi, *decimal_integer_literal* `0`, temel alınan türü *enum_type*olan herhangi bir *enum_type* ve herhangi bir *nullable_type* dönüştürülmesine izin verir. İkinci durumda, dönüştürme temel *enum_type* dönüştürülürken değerlendirilir ve sonuç ([null yapılabilir türler](types.md#nullable-types)) sarmalanacaktır.

### <a name="implicit-interpolated-string-conversions"></a>Örtülü olarak bulunan dize dönüşümleri

Örtük bir enterpolasyonlu dize dönüştürme *interpolated_string_expression* ([enterpolasyonlu dizeler](expressions.md#interpolated-strings)) `System.IFormattable` veya `System.FormattableString` (`System.IFormattable`uygulayan) biçimine izin verir.

Bu dönüştürme uygulandığında, bir dize değeri, enterpolasyonlu dizeden oluşamaz. Bunun yerine, bir `System.FormattableString` örneği, [enterpolasyonlu dizelerde](expressions.md#interpolated-strings)açıklanacak şekilde oluşturulur.

### <a name="implicit-nullable-conversions"></a>Örtük null yapılabilir dönüşümler

Null olamayan değer türlerinde çalışan önceden tanımlanmış örtük dönüştürmeler, bu türlerin null atanabilir formlarıyla de kullanılabilir. `S` null olmayan bir değer türünden `T`null değeri alamayan bir değer türüne dönüştüren, önceden tanımlanmış örtük kimlik ve sayısal dönüştürmelerin her biri için, aşağıdaki örtük null yapılabilir dönüşümler mevcuttur:

*  `S?` 'den `T?`örtülü dönüşüm.
*  `S` 'den `T?`örtülü dönüşüm.

`S` ile `T` arasında temel alınan dönüştürmeye göre örtük olarak null yapılabilir dönüştürme değerlendirmesi aşağıdaki gibi devam eder:

*  Null yapılabilir dönüştürme `S?` `T?`:
    * Kaynak değeri null ise (`HasValue` özelliği false), sonuç `T?`türünün null değeridir.
    * Aksi takdirde, dönüştürme `S?` `T``S` ' dan `S`' dan bir sarmalama olarak değerlendirilir ve ardından `T` ' dan `T?`' a kadar bir sarmalama ([null yapılabilir türler](types.md#nullable-types)) tarafından yapılır.

*  Null yapılabilir dönüştürme `S` `T?`ise, dönüştürme, temel alınan dönüşüm olarak `T` `S`, `T` ve `T?`arasında bir sarmalama izler olarak değerlendirilir.

### <a name="null-literal-conversions"></a>Null değişmez değer dönüştürmeleri

`null` değişmez değerinden herhangi bir null yapılabilir tür için örtük bir dönüştürme bulunur. Bu dönüştürme, belirtilen null yapılabilir türdeki null değeri ([null yapılabilir türler](types.md#nullable-types)) oluşturur.

### <a name="implicit-reference-conversions"></a>Örtük başvuru dönüştürmeleri

Örtük başvuru dönüştürmeleri şunlardır:

*  Herhangi bir *reference_type* `object` ve `dynamic`.
*  Herhangi bir *class_type* `S` herhangi bir *class_type* `T`, `S` tarafından belirtilen `T`elde edilir.
*  Herhangi bir *class_type* `S` *interface_type* `T``S` uygular.
*  Herhangi bir *interface_type* `S` herhangi bir *interface_type* `T`, `S` tarafından belirtilen `T`elde edilir.
*  Öğe türü `T` bir *array_type* `S` `SE` öğe türü `TE`*array_type* , aşağıdakilerin tümü doğru olarak belirtilmelidir:
    * `S` ve `T` yalnızca öğe türünde farklılık gösterir. Diğer bir deyişle, `S` ve `T` aynı sayıda boyuta sahiptir.
    * `SE` ve `TE` her ikisi de *reference_type*s.
    * `SE` ' den `TE`örtük bir başvuru dönüştürmesi vardır.
*  Herhangi bir *array_type* `System.Array` ve uyguladığı arabirimlere.
*  Tek boyutlu dizi türünden `S[]` `System.Collections.Generic.IList<T>` ve temel arabirimlerine `S`, örtülü bir kimlik veya başvuru dönüştürmesi olması şartıyla `T`.
*  Herhangi bir *delegate_type* `System.Delegate` ve uyguladığı arabirimlere.
*  Null değişmez değerinden herhangi bir *reference_type*.
*  Herhangi bir *reference_type* , bir *reference_type* `T0` örtük bir kimlik veya başvuru dönüştürmesi varsa ve `T0` `T`bir kimlik dönüştürmesi varsa *reference_type* `T`.
*  Herhangi bir *reference_type* bir arabirime veya temsilci türüne dönüştürme, bir arabirim veya temsilci türü `T0` ve `T0` varyans-dönüştürülebilir ([varyans dönüştürme](interfaces.md#variance-conversion)`T`) `T`.
*  Başvuru türleri olarak bilinen tür parametreleri içeren örtük dönüştürmeler. Tür parametreleri içeren örtük dönüştürmeler hakkında daha fazla ayrıntı için bkz. [tür parametreleri Içeren örtük dönüştürmeler](conversions.md#implicit-conversions-involving-type-parameters) .

Örtük başvuru dönüştürmeleri, her zaman başarılı olarak kanıtlanmış *reference_type*s arasındaki Dönüştürmelere sahiptir ve bu nedenle çalışma zamanında hiçbir denetim gerektirmez.

Başvuru dönüştürmeleri, örtük veya açık, dönüştürülmekte olan nesnenin başvuru kimliğini hiçbir şekilde değiştirmeyin. Diğer bir deyişle, bir başvuru dönüştürmesi başvurunun türünü değiştiremeken, başvuruda bulunulan nesnenin türünü veya değerini hiçbir şekilde değiştirmez.

### <a name="boxing-conversions"></a>Paketleme dönüştürmeleri

Kutulama dönüştürme bir *value_type* örtülü olarak bir başvuru türüne dönüştürülmesine izin verir. Herhangi bir *non_nullable_value_type* `object` ve `dynamic`, *interface_type*tarafından uygulanan *non_nullable_value_type* `System.ValueType` için bir paketleme dönüştürmesi vardır. Ayrıca, bir *enum_type* `System.Enum`türüne dönüştürülebilir.

Bir *nullable_type* bir referans türüne, yalnızca temeldeki *non_nullable_value_type* başvuru türüne bir paketleme dönüştürmesi varsa, bir paketleme dönüştürmesi vardır.

Bir değer türü, bir arabirim türüne paketleme dönüştürmesi varsa `I` bir arabirim türüne kutulama dönüştürmesi içerir `I0` ve `I0` bir kimlik dönüştürmesi `I`.

Bir değer türünün, bir arabirime veya temsilci türüne paketleme dönüştürmesi varsa `I` bir arabirim türüne paketleme dönüştürmesi vardır `I0` ve `I0` varyans-dönüştürülebilir ([varyans dönüştürme](interfaces.md#variance-conversion)) `I`.

Bir *non_nullable_value_type* değerini kutulama bir nesne örneği ayırmayı ve *value_type* değerini bu örneğe kopyalamayı içerir. Yapı, tüm yapılar ([Devralma](structs.md#inheritance)) için bir temel sınıf olduğundan `System.ValueType`türüne kutulanabilir.

Bir *nullable_type* değerini kutulama aşağıdaki gibi devam eder:

*  Kaynak değer null ise (`HasValue` özelliği false), sonuç hedef türün null başvurusudur.
*  Aksi takdirde, sonuç, kaynak değeri sarmalanmış ve paketleme tarafından oluşturulan kutulanmış `T` bir başvurudur.

Paketleme dönüştürmeleri, [paketleme dönüştürmelerinde](types.md#boxing-conversions)daha ayrıntılı bir şekilde açıklanmıştır.

### <a name="implicit-dynamic-conversions"></a>Örtük dinamik dönüştürmeler

`T`tür `dynamic` bir ifadeden örtük dinamik dönüştürme bulunur. Dönüştürme, dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)), bu, örtük bir dönüştürmenin `T`, ifadenin çalışma zamanı türünden çalışma zamanında arayacaktır. Dönüştürme bulunamazsa, bir çalışma zamanı özel durumu oluşturulur.

Örtülü dönüştürmenin, örtük [dönüştürmelerin](conversions.md#implicit-conversions) başlangıcında asla bir özel duruma neden olmaması gerektiğine ilişkin önerileri ihlal ettiğini unutmayın. Ancak dönüştürmenin kendisi değil, özel duruma neden olan dönüştürmenin *bulunması* değildir. Çalışma zamanı özel durumlarının riski dinamik bağlama kullanımına sahiptir. Dönüştürmenin dinamik bağlaması istenmiyorsa, ifade önce `object`ve ardından istenen türe dönüştürülebilir.

Aşağıdaki örnekte örtük dinamik dönüştürmeler gösterilmektedir:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

`s2` ve `i` atamaları her ikisi de, çalışma zamanına kadar işlemlerin bağlantısının askıya alındığı örtük dinamik dönüştürmeler kullanmayı sağlar. Çalışma zamanında örtük dönüştürmeler, hedef türe `string` `d` -- çalışma zamanı türünden alınır. `string` bir dönüştürme bulunur, ancak `int`.

### <a name="implicit-constant-expression-conversions"></a>Örtük sabit ifade dönüştürmeleri

Örtük bir sabit ifade dönüştürmesi aşağıdaki Dönüştürmelere izin verir:

*  `int` türündeki *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) `sbyte`, `byte`, `short`, `ushort`, `uint`veya `ulong`türüne dönüştürülebilir. Bu değer, *constant_expression* değeri hedef türünün aralığı dahilinde.
*  `long` türündeki *constant_expression* `ulong`türüne dönüştürülebilir, ancak *constant_expression* değeri negatif değil.

### <a name="implicit-conversions-involving-type-parameters"></a>Tür parametreleri içeren örtük dönüştürmeler

Verilen tür parametresi için aşağıdaki örtük dönüştürmeler mevcuttur `T`:

*  `T`, `T` temel `C`ve `T` tarafından uygulanan bir arabirime `C`geçerli temel sınıfına. Çalışma zamanında, `T` bir değer türü ise, dönüştürme paketleme dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme örtük bir başvuru dönüştürmesi veya kimlik dönüştürmesi olarak yürütülür.
*  `T`, `T`etkin arabirim kümesinde ve `T` `I`herhangi bir taban arabirimine `I` bir arabirim türüne. Çalışma zamanında, `T` bir değer türü ise, dönüştürme paketleme dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme örtük bir başvuru dönüştürmesi veya kimlik dönüştürmesi olarak yürütülür.
*  `T`, `U`bir tür parametresine `T` `U` ([tür parametre kısıtlamalarına](classes.md#type-parameter-constraints)) bağlıdır. Çalışma zamanında, `U` bir değer türü ise, `T` ve `U` aynı türde ve hiçbir dönüştürme gerçekleştirilmeyebilir. Aksi takdirde, `T` bir değer türü ise, dönüştürme paketleme dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme örtük bir başvuru dönüştürmesi veya kimlik dönüştürmesi olarak yürütülür.
*  Null değişmez değerinden `T`, `T` belirtilen bir başvuru türü olduğu bilinmektedir.
*  `T`, bir başvuru türüne örtük bir dönüştürme içeriyorsa ve `S0` `S`'ye bir kimlik dönüştürmesi varsa `I` bir başvuru türüne `S0`. Çalışma zamanında dönüştürme `S0`dönüştürme ile aynı şekilde yürütülür.
*  `T`, bir arabirim veya temsilci türüne örtülü olarak dönüştürme içeriyorsa ve `I0` varyans-`I` dönüştürülebilir ([varyans dönüştürmesi](interfaces.md#variance-conversion)) `I` bir arabirim türüne `I0`. Çalışma zamanında, `T` bir değer türü ise, dönüştürme paketleme dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme örtük bir başvuru dönüştürmesi veya kimlik dönüştürmesi olarak yürütülür.

`T` bir başvuru türü ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) olarak bilindiğinde, yukarıdaki dönüşümler tamamen örtük başvuru dönüştürmeleri ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) olarak sınıflandırılır. `T` bir başvuru türü olarak bilinmiyorsa, yukarıdaki dönüştürmeler paketleme dönüştürmeleri ([kutulama dönüştürmeleri](conversions.md#boxing-conversions)) olarak sınıflandırılır.

### <a name="user-defined-implicit-conversions"></a>Kullanıcı tanımlı örtük dönüştürmeler

Kullanıcı tanımlı örtük dönüştürme, isteğe bağlı standart bir örtük dönüştürme, ardından Kullanıcı tanımlı örtük dönüştürme işlecinin yürütülmesi ve ardından isteğe bağlı bir standart örtük dönüştürme ile oluşur. Kullanıcı tanımlı örtük dönüştürmeleri değerlendirmek için tam kurallar, [Kullanıcı tanımlı örtük dönüştürmelerin işlenmesinde](conversions.md#processing-of-user-defined-implicit-conversions)açıklanmıştır.

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Anonim işlev dönüştürmeleri ve Yöntem grubu dönüştürmeleri

Anonim işlevler ve yöntem grupları, ve içinde türlerine sahip değildir, ancak dolaylı olarak temsilci türlerine veya ifade ağaç türlerine dönüştürülebilir. Anonim işlev dönüştürmeleri, [metot grubu dönüştürmelerinde](conversions.md#method-group-conversions) [Adsız işlev dönüştürmelerinde](conversions.md#anonymous-function-conversions) ve Yöntem grubu dönüştürmelerinde daha ayrıntılı bir şekilde açıklanmıştır.

## <a name="explicit-conversions"></a>Açık dönüşümler

Aşağıdaki dönüşümler açık dönüştürmeler olarak sınıflandırılmaktadır:

*  Tüm örtük dönüştürmeler.
*  Açık sayısal dönüştürmeler.
*  Açık sabit listesi dönüştürmeleri.
*  Açık boş değer atanabilir dönüşümler.
*  Açık başvuru dönüştürmeleri.
*  Açık arabirim dönüştürmeleri.
*  Kutudan çıkarma dönüştürmeleri.
*  Açık dinamik dönüştürmeler
*  Kullanıcı tanımlı açık dönüştürmeler.

Dönüştürme ifadelerinde açık dönüştürmeler oluşabilir ([atama ifadeleri](expressions.md#cast-expressions)).

Açık dönüştürmeler kümesi tüm örtük dönüştürmeleri içerir. Bu, gereksiz atama ifadelerine izin verilen anlamına gelir.

Örtük dönüştürmeler olmayan açık dönüştürmeler, her zaman başarılı olmak üzere kendini kanıtlanamaz dönüşümlerdir, bilinen dönüştürmeler, büyük olasılıkla bilgi kaybedecektir ve türlerin etki alanları arasında dönüştürme imle.

### <a name="explicit-numeric-conversions"></a>Açık sayısal dönüştürmeler

Açık sayısal dönüştürmeler, bir *numeric_type* örtük bir sayısal dönüştürmenin ([örtük sayısal dönüştürmeler](conversions.md#implicit-numeric-conversions)) zaten mevcut olmadığı başka bir *numeric_type* dönüşümlerdir:

*  `sbyte` `byte`, `ushort`, `uint`, `ulong`veya `char`.
*  `byte` `sbyte` ve `char`.
*  `short` `sbyte`, `byte`, `ushort`, `uint`, `ulong`veya `char`.
*  `ushort` `sbyte`, `byte`, `short`veya `char`.
*  `int` `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`veya `char`.
*  `uint` `sbyte`, `byte`, `short`, `ushort`, `int`veya `char`.
*  `long` `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`veya `char`.
*  `ulong` `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`veya `char`.
*  `char` `sbyte`, `byte`veya `short`.
*  `float` `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`veya `decimal`.
*  `double` `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`veya `decimal`.
*  `decimal` `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`veya `double`.

Açık dönüştürmeler tüm örtük ve açık sayısal dönüştürmeleri içerdiğinden, herhangi bir *numeric_type* bir atama Ifadesi ([atama ifadeleri](expressions.md#cast-expressions)) kullanarak herhangi bir numeric_type başka bir dönüştürmek her zaman mümkündür.

Açık sayısal dönüştürmeler büyük olasılıkla bilgileri kaybedebilir veya büyük olasılıkla özel durumların oluşturulmasına neden olabilir. Açık bir sayısal dönüştürme şu şekilde işlenir:

*  İntegral türünden başka bir integral türüne dönüştürme işlemi için işleme, dönüştürmenin gerçekleştiği taşma Denetim bağlamına ([denetlenen ve işaretlenmemiş operatörler](expressions.md#the-checked-and-unchecked-operators)) bağlıdır:
    * `checked` bağlamında, kaynak işlenenin değeri hedef türü Aralık içindeyse, ancak kaynak işlenenin değeri hedef türü aralığının dışında bir `System.OverflowException` oluşturduğunda dönüştürme başarılı olur.
    * `unchecked` bağlamında, dönüştürme her zaman başarılı olur ve aşağıdaki gibi devam eder.
        * Kaynak türü hedef türünden büyükse, "ekstra" en önemli bitleri atarak kaynak değer kesilir. Sonuç daha sonra hedef türünün bir değeri olarak değerlendirilir.
        * Kaynak türü hedef türünden küçükse, hedef türle aynı boyutta olması için, kaynak değeri ya imza veya sıfır genişletilmiş ya da sıfır genişletilmiş olur. Kaynak türü imzalanmışsa, oturum açma uzantısı kullanılır; Kaynak türü işaretsiz ise sıfır uzantı kullanılır. Sonuç daha sonra hedef türünün bir değeri olarak değerlendirilir.
        * Kaynak türü hedef türle aynı boyutta ise, kaynak değer hedef türün bir değeri olarak değerlendirilir.
*  `decimal` bir integral türüne dönüştürme için, kaynak değer en yakın tamsayı değerine sıfır doğru yuvarlanır ve bu tam sayı değeri dönüştürmenin sonucu olur. Elde edilen integral değeri, hedef türü aralığının dışındaysa bir `System.OverflowException` oluşturulur.
*  `float` veya `double` bir integral türüne dönüştürme için, işleme, dönüştürmenin gerçekleştiği taşma Denetim bağlamına ([denetlenen ve işaretlenmemiş operatörler](expressions.md#the-checked-and-unchecked-operators)) bağlıdır:
    * `checked` bağlamında, dönüştürme aşağıdaki gibi devam eder:
        * İşlenenin değeri NaN veya sonsuz ise, bir `System.OverflowException` oluşturulur.
        * Aksi takdirde, kaynak işleneni en yakın tamsayı değerine sıfır doğru yuvarlanır. Bu integral değeri hedef türünün aralığı içindeyse, bu değer dönüştürmenin sonucudur.
        * Aksi takdirde, bir `System.OverflowException` oluşturulur.
    * `unchecked` bağlamında, dönüştürme her zaman başarılı olur ve aşağıdaki gibi devam eder.
        * İşlenenin değeri NaN veya sonsuz ise, dönüştürmenin sonucu hedef türün belirtilmemiş bir değeridir.
        * Aksi takdirde, kaynak işleneni en yakın tamsayı değerine sıfır doğru yuvarlanır. Bu integral değeri hedef türünün aralığı içindeyse, bu değer dönüştürmenin sonucudur.
        * Aksi takdirde, dönüştürmenin sonucu hedef türün belirtilmemiş bir değeridir.
*  `double` `float`bir dönüştürme için `double` değeri en yakın `float` değerine yuvarlanır. `double` değeri `float`olarak temsil etmek için çok küçükse, sonuç pozitif sıfır veya negatif sıfır olur. `double` değeri `float`olarak temsil etmek için çok büyükse, sonuç pozitif sonsuzluk veya negatif sonsuzluk olur. `double` değeri NaN ise, sonuç de NaN olur.
*  `float` veya `double` `decimal`'e dönüştürme için, kaynak değer `decimal` gösterimine dönüştürülür ve gerekirse 28 ondalık konumundan sonra en yakın sayıya yuvarlanır ([ondalık türü](types.md#the-decimal-type)). Kaynak değeri `decimal`olarak temsil etmek için çok küçükse, sonuç sıfır olur. Kaynak değeri NaN, Infinity veya `decimal`olarak temsil edilebilmesi için çok büyükse, bir `System.OverflowException` oluşturulur.
*  `decimal` `float` veya `double`'e dönüştürme için `decimal` değeri en yakın `double` veya `float` değerine yuvarlanır. Bu dönüştürme duyarlığı kaybedebilir, ancak hiçbir durum özel durum oluşturulmasına neden olmaz.

### <a name="explicit-enumeration-conversions"></a>Açık sabit listesi dönüştürmeleri

Açık numaralandırma dönüştürmeleri şunlardır:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`veya `decimal` herhangi bir *enum_type*.
*  Herhangi bir *enum_type* `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`veya `decimal`.
*  Herhangi bir *enum_type* başka bir *enum_type*.

İki tür arasında açık bir sabit listesi dönüştürme işlemi, herhangi bir katılım *enum_type* , bu *enum_type*temel alınan türü olarak davranarak işlenir ve ardından sonuçtaki türler arasında örtük veya açık bir sayısal dönüştürme gerçekleştirerek. Örneğin, *enum_type* `E` ve temel alınan `int`türü verildiğinde, `E` 'den `byte` 'ye dönüştürme, `int` 'den `byte`'e açık bir sayısal dönüştürme ([Açık sayısal dönüştürmeler](conversions.md#explicit-numeric-conversions)) olarak işlenir ve `byte` 'den `E` 'ye dönüştürme işlemi `byte` 'den `int`örtülü bir sayısal dönüştürme ([örtük sayısal dönüştürmeler](conversions.md#implicit-numeric-conversions)) olarak işlenir.

### <a name="explicit-nullable-conversions"></a>Açık boş değer atanabilir dönüşümler

***Açık null yapılabilir dönüşümler*** , null olamayan değer türlerinde çalışan önceden tanımlı açık Dönüştürmelere izin verir ve bu türlerin null yapılabilir formlarıyla de kullanılır. Null yapılamayan bir değer türünden `S`, null olamayan bir değer türü `T` ([kimlik dönüştürme](conversions.md#identity-conversion), [örtük sayısal dönüştürmeler](conversions.md#implicit-numeric-conversions), [örtük numaralandırma dönüştürmeleri](conversions.md#implicit-enumeration-conversions), [Açık sayısal dönüştürmeler](conversions.md#explicit-numeric-conversions)ve [Açık sabit listesi dönüştürmeleri](conversions.md#explicit-enumeration-conversions)) dönüştüren önceden tanımlanmış açık dönüştürmelerin her biri için, aşağıdaki null yapılabilir dönüşümler mevcuttur:

*  `S?` ' den `T?`açık bir dönüştürme.
*  `S` ' den `T?`açık bir dönüştürme.
*  `S?` ' den `T`açık bir dönüştürme.

`S` ile `T` arasında temel alınan dönüştürmeye göre null yapılabilir dönüştürme değerlendirmesi aşağıdaki gibi devam eder:

*  Null yapılabilir dönüştürme `S?` `T?`:
    * Kaynak değeri null ise (`HasValue` özelliği false), sonuç `T?`türünün null değeridir.
    * Aksi takdirde, dönüştürme `S?` `T``S` ' den `S`' dan bir sarmalama olarak değerlendirilir ve ardından `T` ' den `T?`' a bir sarmalama tarafından yapılır.
*  Null yapılabilir dönüştürme `S` `T?`ise, dönüştürme, temel alınan dönüşüm olarak `T` `S`, `T` ve `T?`arasında bir sarmalama izler olarak değerlendirilir.
*  Null yapılabilir dönüştürme `S?` `T`ise, dönüştürme `S?` ' dan bir sarmalama olarak değerlendirilir `S` ve `S` ' den `T`' e doğru dönüştürme olur.

Null yapılabilir bir değeri sarmalama girişimi, değer `null`bir özel durum oluşturur.

### <a name="explicit-reference-conversions"></a>Açık başvuru dönüştürmeleri

Açık başvuru dönüştürmeleri şunlardır:

*  `object` ve `dynamic` diğer *reference_type*.
*  Herhangi bir *class_type* `S` herhangi bir *class_type* `T`, `S` bir `T`taban sınıfıdır.
*  Herhangi bir *class_type* `S` herhangi bir *interface_type* `T`, `S`, ve `S` `T`uygulamaz.
*  Herhangi bir *interface_type* `S` herhangi bir *class_type* `T`için, `T`, `T` `S`uygulayan.
*  Herhangi bir *interface_type* `S` herhangi bir *interface_type* `T``S`, belirtilen `T`türetilmez.
*  Öğe türü `T` bir *array_type* `S` `SE` öğe türü `TE`*array_type* , aşağıdakilerin tümü doğru olarak belirtilmelidir:
    * `S` ve `T` yalnızca öğe türünde farklılık gösterir. Diğer bir deyişle, `S` ve `T` aynı sayıda boyuta sahiptir.
    * `SE` ve `TE` her ikisi de *reference_type*s.
    * `SE` `TE`bir açık başvuru dönüştürmesi vardır.
*  `System.Array` ve uyguladığı arabirimler *array_type*.
*  Tek boyutlu dizi türünden `System.Collections.Generic.IList<T>` ve temel arabirimlerine `S[]`, `S` bir açık başvuru dönüştürmesi olması şartıyla `T`.
*  `System.Collections.Generic.IList<S>` ve temel arabirimlerinden `T[]`, `T``S` bir açık kimlik veya başvuru dönüştürmesi olması şartıyla, tek boyutlu bir dizi türüne.
*  `System.Delegate` ve uyguladığı arabirimler *delegate_type*.
*  Başvuru türü olan bir başvuru türüne `T`, bir başvuru türü `T0` açık başvuru dönüştürmesi varsa ve `T0` bir kimlik dönüştürme `T`sahipse
*  Bir arabirim veya temsilci türüne açık başvuru dönüştürmesi varsa, bir başvuru türünden bir arabirime veya temsilci türüne `T` `T0` ve `T0` fark-`T` dönüştürülebilir ya da `T` ([varyans dönüştürmesi](interfaces.md#variance-conversion)) arasında değişim yapılır.
*  `D<S1...Sn>`, `D<X1...Xn>` genel bir temsilci türü olduğu `D<T1...Tn>`, `D<S1...Sn>` `D<T1...Tn>`ile uyumlu değil ve `Xi` her bir tür parametresi için `D` aşağıdaki tutmalar:
    * `Xi` sabit ise, `Si` `Ti`aynıdır.
    * `Xi` covaryant ise, örtülü veya açık bir kimlik ya da `Si` `Ti`'e başvuru dönüştürmesi vardır.
    * `Xi` değişken karşıtı ise, `Si` ve `Ti` aynı ya da her iki başvuru türü olur.
*  Başvuru türleri olarak bilinen tür parametreleri içeren açık dönüştürmeler. Tür parametreleriyle ilgili açık dönüştürmeler hakkında daha fazla ayrıntı için bkz. [tür parametreleri Içeren açık dönüştürmeler](conversions.md#explicit-conversions-involving-type-parameters).

Açık başvuru dönüştürmeleri, doğru olduklarından emin olmak için çalışma zamanı denetimleri gerektiren başvuru türleri arasındaki dönüşümlerdir.

Açık bir başvuru dönüştürmenin çalışma zamanında başarılı olması için, kaynak işlenenin değeri `null`olmalı veya kaynak işleneni tarafından başvurulan nesnenin gerçek türü, örtük bir başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) veya paketleme dönüştürmesi ([kutulama dönüştürmeleri](conversions.md#boxing-conversions)) tarafından hedef türe dönüştürülebilen bir tür olmalıdır. Açık bir başvuru dönüştürmesi başarısız olursa, bir `System.InvalidCastException` oluşturulur.

Başvuru dönüştürmeleri, örtük veya açık, dönüştürülmekte olan nesnenin başvuru kimliğini hiçbir şekilde değiştirmeyin. Diğer bir deyişle, bir başvuru dönüştürmesi başvurunun türünü değiştiremeken, başvuruda bulunulan nesnenin türünü veya değerini hiçbir şekilde değiştirmez.

### <a name="unboxing-conversions"></a>Kutudan çıkarma dönüştürmeleri

Kutudan çıkarma dönüştürmesi, bir başvuru türünün açıkça bir *value_type*dönüştürülmesine izin verir. Türler `object`, `dynamic` ve `System.ValueType` herhangi bir *non_nullable_value_type* *ve interface_type uygulayan tüm non_nullable_value_type için bir* kutudan çıkarma dönüştürmesi *vardır.* Ayrıca tür `System.Enum` herhangi bir *enum_type*kutulanabilir.

Başvuru türünden *nullable_type*temel *non_nullable_value_type* bir kutudan çıkarma dönüştürmesi varsa, bir başvuru türünden bir paket açma dönüştürmesi *nullable_type* vardır.

`S` değer türü bir arabirim türünden kutudan çıkarma dönüştürmesi varsa `I` bir arabirim türü `I0` ve `I0` `I`bir kimlik dönüştürmesi varsa.

Bir değer türü `S` bir arabirim türünden bir paket açma dönüştürmesi varsa `I`, `I0` bir arabirim veya temsilci türünden bir paket açma dönüştürmesi varsa ve `I0` varyans-dönüştürülebilir veya `I`, `I` ([varyans dönüştürmesi](interfaces.md#variance-conversion)) olarak değiştirildi.

Kutudan çıkarma işlemi, nesne örneğinin verilen *value_type*kutulanmış bir değer olup olmadığını kontrol ederek değeri örnekten dışarı kopyalamaktan oluşur. Bir *nullable_type* null başvurusunun kutudan çıkarma *nullable_type*null değeri üretir. Bir struct, tüm yapılar ([Devralma](structs.md#inheritance)) için bir temel sınıf olduğundan `System.ValueType`türünden kutulanabilir.

Kutudan çıkarma dönüştürmeleri, kutudan çıkarma [dönüştürmelerinde](types.md#unboxing-conversions)açıklanacaktır.

### <a name="explicit-dynamic-conversions"></a>Açık dinamik dönüştürmeler

`T`tür `dynamic` bir ifadeden açık dinamik dönüştürme bulunur. Dönüştürme, dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)), yani `T`için ifadenin çalışma zamanı türünden çalışma zamanında açık bir dönüştürme yapılır. Dönüştürme bulunamazsa, bir çalışma zamanı özel durumu oluşturulur.

Dönüştürmenin dinamik bağlaması istenmiyorsa, ifade önce `object`ve ardından istenen türe dönüştürülebilir.

Aşağıdaki sınıfın tanımlandığını varsayalım:
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

Aşağıdaki örnekte açık dinamik dönüştürmeler gösterilmektedir:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

`C` `o` en iyi dönüşümü, derleme zamanında açık bir başvuru dönüştürmesi olacak şekilde bulunur. `"1"` aslında bir `C`olmadığından, çalışma zamanında bu başarısız olur. `d` `C` ancak açık dinamik dönüştürme olarak dönüştürme işlemi, `d` -- `string`---arasında bir Kullanıcı tanımlı dönüştürme olduğu ve başarılı olduğu zaman çalışma zamanına askıya alınır.

### <a name="explicit-conversions-involving-type-parameters"></a>Tür parametreleri içeren açık dönüştürmeler

Verilen tür parametresi için aşağıdaki açık dönüştürmeler mevcuttur `T`:

*  `T` `C` geçerli taban sınıftan `T` ve `C` herhangi bir taban sınıftan `T`. Çalışma zamanında, `T` bir değer türü ise, dönüştürme bir kutudan çıkarma dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme açık bir başvuru dönüştürmesi veya kimlik dönüştürmesi olarak yürütülür.
*  Herhangi bir arabirim türünden `T`. Çalışma zamanında, `T` bir değer türü ise, dönüştürme bir kutudan çıkarma dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme açık bir başvuru dönüştürmesi veya kimlik dönüştürmesi olarak yürütülür.
*  `T` *interface_type* `I`, `T` ' den `I`'e örtük bir dönüştürme olmadığı için. Çalışma zamanında, `T` bir değer türü ise, dönüştürme bir paketleme dönüştürmesi ve ardından açık bir başvuru dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme açık bir başvuru dönüştürmesi veya kimlik dönüştürmesi olarak yürütülür.
*  Bir tür parametresi `T``U`, `T` belirtilen `U` ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) bağlıdır. Çalışma zamanında, `U` bir değer türü ise, `T` ve `U` aynı türde ve hiçbir dönüştürme gerçekleştirilmeyebilir. Aksi takdirde, `T` bir değer türü ise, dönüştürme bir kutudan çıkarma dönüştürmesi olarak yürütülür. Aksi takdirde, dönüştürme açık bir başvuru dönüştürmesi veya kimlik dönüştürmesi olarak yürütülür.

`T` bir başvuru türü olarak bilindiğinde, yukarıdaki dönüşümler tamamen açık başvuru dönüştürmeleri ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)) olarak sınıflandırılır. `T` bir başvuru türü olarak bilinmiyorsa, yukarıdaki dönüştürmeler kutudan çıkarma dönüştürmeleri ([kutudan çıkarma dönüştürmeleri](conversions.md#unboxing-conversions)) olarak sınıflandırılır.

Yukarıdaki kurallar, kısıtlanmamış bir tür parametresinden doğrudan açık dönüştürmeye, arabirim olmayan bir türe izin vermez, bu da şaşırtıcı olabilir. Bu kuralın nedeni karışıklığa engel olmak ve bu tür dönüştürmelerin semantiğini temizlemek için kullanılır. Örneğin, aşağıdaki bildirimi ele alalım:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

`int` `t` doğrudan açık dönüştürmeye izin verildiğinde, bir `X<int>.F(7)` `7L`döndürmesini kolayca bekleyebilir. Ancak, Standart Sayısal Dönüşümler yalnızca türlerin bağlama sırasında sayısal olarak bilindiğinde kabul edildiği için değildir. Semantiğini açık hale getirmek için yukarıdaki örneğin yazılması gerekir:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Bu kod artık derlenir, ancak `X<int>.F(7)` kutulanmış `int` doğrudan bir `long`dönüştürülemediğinden çalışma zamanında bir özel durum oluşturur.

### <a name="user-defined-explicit-conversions"></a>Kullanıcı tanımlı açık dönüşümler

Kullanıcı tanımlı açık dönüştürme, isteğe bağlı bir standart açık dönüşümden, ardından Kullanıcı tanımlı örtük veya açık bir dönüştürme işlecinin yürütülmesi ve ardından isteğe bağlı bir standart açık dönüştürme ile oluşur. Kullanıcı tanımlı Açık dönüştürmeleri değerlendirmek için tam kurallar, [Kullanıcı tanımlı açık dönüştürmelerin işlenmesinde](conversions.md#processing-of-user-defined-explicit-conversions)açıklanmıştır.

## <a name="standard-conversions"></a>Standart dönüşümler

Standart dönüştürmeler, Kullanıcı tanımlı dönüştürmenin bir parçası olarak gerçekleşebileceğini önceden tanımlanmış dönüşümlerdir.

### <a name="standard-implicit-conversions"></a>Standart örtük dönüştürmeler

Aşağıdaki örtük dönüştürmeler standart örtük dönüştürmeler olarak sınıflandırılmaktadır:

*  Kimlik dönüştürmeleri ([kimlik dönüştürme](conversions.md#identity-conversion))
*  Örtük Sayısal Dönüşümler ([örtük sayısal dönüştürmeler](conversions.md#implicit-numeric-conversions))
*  Örtük null yapılabilir dönüşümler ([örtük Nullable dönüşümler](conversions.md#implicit-nullable-conversions))
*  Örtük başvuru dönüştürmeleri ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions))
*  Paketleme dönüştürmeleri ([kutulama dönüştürmeleri](conversions.md#boxing-conversions))
*  Örtük sabit ifade dönüştürmeleri ([örtük dinamik dönüştürmeler](conversions.md#implicit-dynamic-conversions))
*  Tür parametreleri içeren örtük dönüştürmeler ([tür parametreleri Içeren örtük dönüştürmeler](conversions.md#implicit-conversions-involving-type-parameters))

Standart örtük dönüştürmeler özellikle Kullanıcı tanımlı örtük dönüştürmeleri hariç tutar.

### <a name="standard-explicit-conversions"></a>Standart açık dönüşümler

Standart açık dönüştürmeler tüm standart örtük dönüştürmelerdir ve ters bir standart örtük dönüştürmenin bulunduğu açık dönüştürmelerin alt kümesidir. Diğer bir deyişle, bir tür `B`türüne standart bir örtük `A` dönüştürme varsa, tür `A` türünden `B` türüne ve `B` tür `A`türüne standart bir açık dönüştürme bulunur.

## <a name="user-defined-conversions"></a>Kullanıcı tanımlı dönüştürmeler

C#önceden tanımlanmış örtük ve açık dönüştürmelerin ***Kullanıcı tanımlı dönüşümlerde***artırılmasına izin verir. Kullanıcı tanımlı dönüştürmeler, sınıf ve yapı türlerinde dönüştürme işleçleri ([dönüştürme işleçleri](classes.md#conversion-operators)) bildirerek ortaya çıkartılır.

### <a name="permitted-user-defined-conversions"></a>İzin verilen Kullanıcı tanımlı dönüştürmeler

C#yalnızca belirli kullanıcı tanımlı dönüştürmelerin bildirilmesine izin verir. Özellikle, zaten varolan bir örtük veya açık dönüştürmeyi yeniden tanımlamak mümkün değildir.

Belirli bir kaynak türü `S` ve hedef türü `T`, `S` veya `T` null yapılabilir türlerse, `S0` ve `T0` temel türlerine başvurmasına izin verir, aksi takdirde `S0` ve `T0` sırasıyla `S` ve `T` eşittir. Bir sınıf veya yapının bir kaynak türü `S` bir hedef türüne dönüştürme bildirmesine izin verilir `T` yalnızca aşağıdakilerin tümü doğru ise geçerlidir:

*  `S0` ve `T0` farklı türlerdir.
*  `S0` ya da `T0`, işleç bildiriminin gerçekleştiği sınıf veya yapı türüdür.
*  Ne `S0` ne de `T0` bir *interface_type*.
*  Kullanıcı tanımlı dönüştürmeler hariç olmak üzere, `S` `T` veya `T` ' den `S`' e kadar bir dönüştürme yok.

Kullanıcı tanımlı dönüştürmeler için uygulanan kısıtlamalar, [dönüştürme işleçlerinde](classes.md#conversion-operators)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="lifted-conversion-operators"></a>Yükseltilmemiş dönüştürme işleçleri

Null yapılamayan bir değer türü `S`, null olamayan bir değer türü `T`dönüştüren bir Kullanıcı tanımlı dönüştürme işleci verildiğinde, `S?` ' den `T?`dönüştüren bir ***yükseltilmemiş dönüştürme işleci*** vardır. Bu yükseltilmemiş dönüştürme işleci, `S?` ' den `S` ' den `T` ' a bir kaydırma gerçekleştirerek, null değerli `T?`doğrudan bir null değerli `S?` dönüştürülebilinerek, `T` `S` ' dan `T?`' a bir sarmalama uygular.

Bir yükseltilmemiş dönüştürme işleci, temel aldığı Kullanıcı tanımlı dönüştürme işleci ile aynı örtük veya açık sınıflandırmasına sahiptir. "Kullanıcı tanımlı dönüştürme" terimi hem Kullanıcı tanımlı hem de yükseltilmemiş dönüştürme işleçlerinin kullanımı için geçerlidir.

### <a name="evaluation-of-user-defined-conversions"></a>Kullanıcı tanımlı dönüştürmeler değerlendirmesi

Kullanıcı tanımlı dönüştürme, ***kaynak türü***olarak adlandırılan türünden bir değeri, ***hedef türü***olarak adlandırılan başka bir türe dönüştürür. Belirli bir kaynak ve hedef türleri için ***en özel*** Kullanıcı tanımlı dönüştürme işlecini bulmada Kullanıcı tanımlı dönüştürme merkezlerinin değerlendirmesi. Bu belirleme birkaç adımda bozulur:

*  Kullanıcı tanımlı dönüştürme işleçlerinin kabul edileceği sınıfların ve yapıların kümesini bulma. Bu küme, kaynak türünden ve temel sınıflarından ve hedef türünden ve temel sınıflarından oluşur (yalnızca sınıfların ve yapıların Kullanıcı tanımlı işleçleri bildirebilme ve sınıf olmayan türlerin temel sınıfları olmadığı örtük varsayımlar ile). Bu adımın amaçları doğrultusunda, kaynak veya hedef tür bir *nullable_type*ise bunun yerine temel alınan türü kullanılır.
*  Bu tür kümesinden, hangi kullanıcı tanımlı ve yükseltilmemiş dönüştürme işleçlerinin geçerli olduğunu belirlemek. Bir dönüştürme işlecinin geçerli olması için, kaynak türünden işlecin işlenen türüne Standart dönüştürme ([Standart dönüşümler](conversions.md#standard-conversions)) gerçekleştirmek mümkün olmalıdır ve işlecin sonuç türünden hedef türüne standart bir dönüştürme gerçekleştirmek mümkün olmalıdır.
*  Geçerli Kullanıcı tanımlı işleçler kümesinden, hangi işlecin kesin olarak en belirgin olduğunu belirleme. Genel koşullarda, en özel işleç, işlenen türü kaynak türüne "en yakın" olan ve sonuç türü hedef türüne "en yakın" olan işleçtir. Kullanıcı tanımlı dönüştürme işleçleri yükseltilmemiş dönüştürme işleçleri üzerinden tercih edilir. En belirli kullanıcı tanımlı dönüştürme işlecini oluşturmaya yönelik kuralların tam kuralları aşağıdaki bölümlerde tanımlanmıştır.

En çok belirli bir Kullanıcı tanımlı dönüştürme işleci tanımlandıktan sonra, Kullanıcı tanımlı dönüştürmenin gerçek yürütmesi üç adımdan oluşur:

*  İlk olarak, gerekirse, kaynak türünden Kullanıcı tanımlı veya yükseltilmemiş dönüştürme işlecinin işlenen türüne standart bir dönüştürme işlemi gerçekleştiriliyor.
*  Sonra, dönüştürmeyi gerçekleştirmek için Kullanıcı tanımlı veya yükseltilmemiş dönüştürme işlecini çağırma.
*  Son olarak, gerekirse Kullanıcı tanımlı veya yükseltilmemiş dönüştürme işlecinin sonuç türünden hedef türüne standart bir dönüştürme işlemi gerçekleştiriliyor.

Kullanıcı tanımlı bir dönüştürmenin değerlendirmesi hiçbir şekilde birden fazla Kullanıcı tanımlı veya yükseltilmemiş dönüştürme işleci içerir. Diğer bir deyişle `S` türünden `T` türüne dönüştürme, önce `S` 'den `X` 'e Kullanıcı tanımlı bir dönüştürme yürütmez ve ardından `X` 'den `T`'e Kullanıcı tanımlı bir dönüştürme yürütmeyecektir.

Aşağıdaki bölümlerde Kullanıcı tanımlı örtük veya açık dönüşümler değerlendirmesinin tam tanımları verilmiştir. Tanımlar aşağıdaki terimleri kullanır:

*  Bir tür `B`bir türden standart örtük dönüştürme ([Standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) varsa ve ne `A` ne de `B` *interface_type*`A`, ***`A` `B`olarak*** kabul edilir ve ***`B` `A`.***
*  Bir tür kümesindeki ***en kapsamlı tür*** , küme içindeki diğer tüm türleri kapsayan tek türdür. Tek bir tür diğer tüm türleri kapsadığı takdirde, küme en fazla kapsayan türe sahip değildir. Daha sezgisel koşullarda, en çok kapsayan tür, küme içindeki "en büyük" türdür — diğer türlerin her birinin örtük olarak dönüştürülebileceği bir tür.
*  Bir tür kümesindeki ***en kapsamlı tür*** , küme içindeki tüm diğer türler tarafından dahil edilen tek türdür. Tüm diğer türler tarafından tek bir tür yoksa, küme, en çok kullanılan türe sahip değildir. Daha sezgisel koşullarda, en kapsamlı tür, küme içindeki "en küçük" türüdür ve örtülü olarak diğer türlerin her birine dönüştürülebilirler.

### <a name="processing-of-user-defined-implicit-conversions"></a>Kullanıcı tanımlı örtük dönüştürmeleri işleme

`S` türünden `T` türüne Kullanıcı tanımlı örtük dönüştürme şu şekilde işlenir:

*  `S0` ve `T0`türleri belirlenir. `S` veya `T` null yapılabilir türlerse, `S0` ve `T0` temel alınan türlerdir, aksi takdirde `S0` ve `T0` sırasıyla `S` ve `T` eşittir.
*  Kullanıcı tanımlı dönüştürme işleçlerinin kabul edileceği `D`türü kümesini bulur. Bu küme `S0` (`S0` bir sınıf veya yapı ise), `S0` temel sınıfları (`S0` bir sınıfsa) ve `T0` (`T0` bir sınıf veya yapı ise) oluşur.
*  Geçerli Kullanıcı tanımlı ve yükseltilmemiş dönüştürme işleçleri kümesini bulun `U`. Bu küme, Kullanıcı tanımlı ve yükseltilmemiş örtük dönüştürme işleçlerinden oluşur ve bir tür kapsayan `S` `T`tarafından dahil edilen bir türe dönüşüm olan `D`. `U` boş ise, dönüştürme tanımsız olur ve derleme zamanı hatası oluşur.
*  `U`işleçleri `SX`en çok belirli kaynak türünü bulun:
    * `U` ' deki işleçlerden herhangi biri `S`' den dönüştürürseniz `SX` `S`.
    * Aksi takdirde `SX`, `U`operatörlerinin Birleşik kaynak türleri kümesindeki en kapsamlı türdür. Tam olarak en kapsamlı bir tür bulunamazsa, dönüştürme belirsizdir ve derleme zamanı hatası oluşur.
*  `U`işleçleri `TX`en çok belirli hedef türünü bulun:
    * `U` ' deki işleçlerden herhangi biri `T`' e dönüştürürsünüz `TX` `T`.
    * Aksi takdirde `TX`, `U`operatörlerinin Birleşik hedef türleri kümesindeki en kapsamlı bir türdür. Tam olarak en kapsamlı bir tür bulunamazsa, dönüştürme belirsizdir ve derleme zamanı hatası oluşur.
*  En belirli dönüştürme işlecini bulun:
    * `U`, `SX` ' ten `TX`dönüştürdüğü tam olarak bir Kullanıcı tanımlı dönüştürme işleci içeriyorsa, bu, en belirli dönüştürme işleçtir.
    * Aksi takdirde, `U` `SX` ' ten `TX`dönüştüren tam olarak bir yükseltilmemiş dönüştürme işleci içeriyorsa, bu, en belirli dönüştürme işleçtir.
    * Aksi takdirde, dönüştürme belirsizdir ve bir derleme zamanı hatası oluşur.
*  Son olarak, dönüştürmeyi uygulayın:
    * `S` `SX`değilse, `S` 'den `SX` 'e standart bir örtük dönüşüm yapılır.
    * En özel dönüştürme işleci, `SX` ' den `TX`dönüştürmek için çağrılır.
    * `TX` `T`değilse, `TX` 'den `T` 'e standart bir örtük dönüşüm yapılır.

### <a name="processing-of-user-defined-explicit-conversions"></a>Kullanıcı tanımlı Açık dönüştürmeleri işleme

`S` türünden `T` türüne bir Kullanıcı tanımlı açık dönüştürme aşağıdaki şekilde işlenir:

*  `S0` ve `T0`türleri belirlenir. `S` veya `T` null yapılabilir türlerse, `S0` ve `T0` temel alınan türlerdir, aksi takdirde `S0` ve `T0` sırasıyla `S` ve `T` eşittir.
*  Kullanıcı tanımlı dönüştürme işleçlerinin kabul edileceği `D`türü kümesini bulur. Bu küme `S0` oluşur (`S0` bir sınıf veya yapı ise), `S0` temel sınıfları (`S0` bir sınıfsa), `T0` (`T0` bir sınıf veya yapı ise) ve temel sınıfları (`T0` bir sınıfsa) oluşur.
*  Geçerli Kullanıcı tanımlı ve yükseltilmemiş dönüştürme işleçleri kümesini bulun `U`. Bu küme, `T`tarafından dahil edilen veya dahil edilen bir tür `S` tarafından dahil edilen veya dahil edilen bir türden veya dahil edilen bir tür tarafından tanımlanan bir türden veya dahil olmak üzere `D`, Kullanıcı tanımlı ve yükseltilmemiş örtük veya açık dönüştürme işleçlerinden oluşur. `U` boş ise, dönüştürme tanımsız olur ve derleme zamanı hatası oluşur.
*  `U`işleçleri `SX`en çok belirli kaynak türünü bulun:
    * `U` ' deki işleçlerden herhangi biri `S`' den dönüştürürseniz `SX` `S`.
    * Aksi takdirde, `U` ' deki işleçlerden herhangi biri `S`çevreleyen türlerden dönüşütürse `SX`, bu operatörlerin Birleşik kaynak türleri kümesindeki en kapsamlı türdür. En çok kullanılan tür bulunamıyorsa, dönüştürme belirsizdir ve derleme zamanı hatası oluşur.
    * Aksi takdirde `SX`, `U`operatörlerinin Birleşik kaynak türleri kümesindeki en kapsamlı bir türdür. Tam olarak en kapsamlı bir tür bulunamazsa, dönüştürme belirsizdir ve derleme zamanı hatası oluşur.
*  `U`işleçleri `TX`en çok belirli hedef türünü bulun:
    * `U` ' deki işleçlerden herhangi biri `T`' e dönüştürürsünüz `TX` `T`.
    * Aksi takdirde, `U` içindeki işleçlerden herhangi biri `T`tarafından dahil edilen türlere dönüşütürse `TX`, bu operatörlerin birleştirilmiş hedef türleri kümesindeki en kapsamlı bir tür olur. Tam olarak en kapsamlı bir tür bulunamazsa, dönüştürme belirsizdir ve derleme zamanı hatası oluşur.
    * Aksi takdirde `TX`, `U`operatörlerin Birleşik hedef türleri kümesindeki en kapsamlı türdür. En çok kullanılan tür bulunamıyorsa, dönüştürme belirsizdir ve derleme zamanı hatası oluşur.
*  En belirli dönüştürme işlecini bulun:
    * `U`, `SX` ' ten `TX`dönüştürdüğü tam olarak bir Kullanıcı tanımlı dönüştürme işleci içeriyorsa, bu, en belirli dönüştürme işleçtir.
    * Aksi takdirde, `U` `SX` ' ten `TX`dönüştüren tam olarak bir yükseltilmemiş dönüştürme işleci içeriyorsa, bu, en belirli dönüştürme işleçtir.
    * Aksi takdirde, dönüştürme belirsizdir ve bir derleme zamanı hatası oluşur.
*  Son olarak, dönüştürmeyi uygulayın:
    * `S` `SX`değilse, `S` 'den `SX` 'e standart bir açık dönüştürme yapılır.
    * En özel kullanıcı tanımlı dönüştürme işleci `SX` `TX`' den ' a dönüştürmek için çağrılır.
    * `TX` `T`değilse, `TX` 'den `T` 'e standart bir açık dönüştürme yapılır.

## <a name="anonymous-function-conversions"></a>Anonim işlev dönüştürmeleri

Bir *anonymous_method_expression* veya *lambda_expression* anonim Işlev olarak sınıflandırılır ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)). İfade bir türe sahip değil, ancak örtülü olarak uyumlu bir temsilci türüne veya ifade ağacı türüne dönüştürülebilir. Özellikle, anonim bir işlev `F`, sağlanmış `D` bir temsilci türü ile uyumludur:

*  `F` bir *anonymous_function_signature*içeriyorsa `D` ve `F` aynı sayıda parametreye sahip olmalıdır.
*  `F` bir *anonymous_function_signature*içermiyorsa, `D` hiçbir parametresi `out` parametre değiştiricisine sahip olmadığı sürece `D` herhangi bir türde sıfır veya daha fazla parametreye sahip olabilir.
*  `F` açıkça belirlenmiş bir parametre listesi varsa, `D` her bir parametre, `F`karşılık gelen parametresiyle aynı tür ve değiştiricilere sahiptir.
*  `F` örtük olarak yazılmış bir parametre listesine sahipse, `D` `ref` veya `out` parametreye sahip değildir.
*  `F` gövdesi bir ifadesiyse ve `D` bir `void` dönüş türüne sahipse veya `F` zaman uyumsuz ise ve `D` dönüş türü `Task`ise, *her bir `F`* parametresi `D`karşılık gelen parametrenin türüne verildiğinde, `F` gövdesi statement_expression ([ifade deyimleri](statements.md#expression-statements)) olarak izin verilen geçerli bir Ifadedir (WRT [ifadeleri](expressions.md)).
*  `F` gövdesi bir deyim bloğu ise ve `D` bir `void` dönüş türüne sahipse veya `F` zaman uyumsuz ise ve `D` dönüş türü `Task`ise, her bir `F` parametresi `D`ilgili parametresinin türü verildiğinde, `F` gövdesi, hiçbir `return` deyiminin bir ifadeyi belirttiği geçerli bir deyim bloğu (WRT [blokları](statements.md#blocks)) olur.
*  `F` gövdesi bir ifade ise *ve `F` zaman* uyumsuz ve `D` void olmayan bir dönüş türü `T`, *ya* da `F` zaman uyumsuz ve `D` bir dönüş türüne sahip `Task<T>`, `F` her bir parametresi `D`için örtük olarak dönüştürülebilir geçerli bir Ifadedir (WRT [ifadeleri](expressions.md)).
*  `F` gövdesi bir ifade bloğudur *ve `F` zaman uyumsuz ve `D`* void olmayan bir dönüş türüne sahip `T`, *ya* da `F` zaman uyumsuz ve `D` bir dönüş türüne sahip `Task<T>`, her bir `F` parametresi `D`karşılık gelen parametrenin türü verildiğinde, `F` gövdesi, her `return` deyiminin `T`örtük olarak dönüştürülebilir bir ifadeyi belirttiği, erişilebilir olmayan bir uç noktası olan geçerli bir deyim bloğu (WRT [blokları](statements.md#blocks)).

Bu bölümde, breçekimi için, `Task` ve `Task<T>` ([zaman uyumsuz işlevler](classes.md#async-functions)) görev türleri için kısa form kullanılır.

Bir lambda ifadesi `F`, `F` temsilci `D`türüyle uyumluysa `Expression<D>` bir ifade ağacı türü ile uyumludur. Bu, anonim metotlar için, yalnızca Lambda ifadelerinde uygulanmadığını unutmayın.

Bazı lambda ifadeleri ifade ağacı türlerine dönüştürülemez: dönüştürme *mevcut*olsa bile, derleme zamanında başarısız olur. Lambda ifadesi ise bu durum budur:

*  Bir *blok* gövdesine sahiptir
*  Basit veya bileşik atama işleçleri içerir
*  Dinamik olarak bağlı bir ifade içerir
*  Zaman uyumsuz

Aşağıdaki örnekler, `A` türünde bir bağımsız değişken alan ve `R`türünde bir değer döndüren bir işlevi temsil eden `Func<A,R>` genel bir temsilci türü kullanır.
```csharp
delegate R Func<A,R>(A arg);
```

Atamalarında
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
Her adsız işlevin parametre ve dönüş türleri, adsız işlevin atandığı değişkenin türünden belirlenir.

İlk atama, anonim işlevi `Func<int,int>` temsilci türüne başarıyla dönüştürür çünkü `x` tür `int`verildiğinde, `x+1` örtük olarak `int`türüne dönüştürülebilir geçerli bir ifadedir.

Benzer şekilde, `x+1` (`int`) sonucu `double`türüne örtülü olarak dönüştürülebilir olduğundan, ikinci atama anonim işlevi `Func<int,double>` temsilci türüne başarıyla dönüştürür.

Ancak, üçüncü atama bir derleme zamanı hatasıdır çünkü `x` `double`türü verildiğinde `double``x+1` sonucu, `int`türüne örtülü olarak dönüştürülebilir değildir.

Dördüncü atama, anonim zaman uyumsuz işlevi `Func<int, Task<int>>` temsilci türüne başarıyla dönüştürür çünkü `x+1` (`int`) sonucu, `Task<int>`görev türü `int` sonuç türüne örtük olarak dönüştürülebilir.

Anonim işlevler aşırı yükleme çözünürlüğünü etkileyebilir ve tür çıkarımı içine katılabilir. Daha fazla ayrıntı için bkz. [işlev üyeleri](expressions.md#function-members) .

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Temsilci türlerine adsız işlev dönüştürmelerini değerlendirme

Anonim bir işlevin bir temsilci türüne dönüştürülmesi,, değerlendirme sırasında etkin olan yakalanan dış değişkenlerin adsız işlevine ve (muhtemelen boş) başvuruda bulunduğu bir temsilci örneği oluşturur. Temsilci çağrıldığında, adsız işlevin gövdesi yürütülür. Gövdedeki kod, temsilci tarafından başvurulan yakalanan dış değişkenler kümesi kullanılarak yürütülür.

Anonim bir işlevden üretilen temsilcinin çağırma listesi tek bir giriş içerir. Temsilcinin tam hedef nesnesi ve Target Yöntemi belirtilmemiş. Özellikle, temsilcinin hedef nesnesinin `null`, kapsayan işlev üyesinin `this` değeri veya başka bir nesne olup olmadığı belirlenmediğini belirtir.

Aynı temsilci türlerine aynı (muhtemelen boş) yakalanan dış değişken örnekleri kümesine sahip anlamsal özdeş anonim işlevlerin dönüştürülmesine izin verilir (ancak gerekli değildir). Aynı bağımsız değişkenlerle aynı etkileri sunan, her durumda, anonim işlevlerin yürütülmesi için, burada anlamsal olarak özdeş terimi kullanılır. Bu kural, aşağıdakiler gibi kodun iyileştirilmesine izin verir.

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

İki anonim işlev temsilcisi aynı (boş) yakalanan dış değişken kümesine sahip olduğundan ve anonim işlevler anlam açısından özdeş olduğundan, derleyicinin temsilcilerin aynı hedef yönteme başvurmasına izin verilir. Aslında, derleyicinin hem anonim işlev ifadelerinden çok aynı temsilci örneğini döndürmesine izin verilir.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>İfade ağacı türlerine anonim işlev dönüştürmeleri değerlendirmesi

Anonim bir işlevin ifade ağacı türüne dönüştürülmesi bir ifade ağacı ([ifade ağacı türleri](types.md#expression-tree-types)) oluşturur. Daha kesin olarak, anonim işlev dönüştürmenin değerlendirmesi, anonim işlevin yapısını temsil eden bir nesne yapısının oluşturulmasına yol açar. İfade ağacının kesin yapısı ve bunu oluşturmak için tam işlem, uygulama tanımlı olur.

### <a name="implementation-example"></a>Uygulama örneği

Bu bölümde, anonim işlev dönüştürmelerinden diğer C# yapılar açısından olası bir uygulama açıklanmaktadır. Burada açıklanan uygulama, Microsoft C# derleyicisi tarafından kullanılan ilkelere dayanır, ancak bu, bir uygulanan uygulamasına sahip değildir ve tek mümkün değildir. Bu belirtim, tam semantiği bu şartların kapsamı dışında olduğu için yalnızca Dönüştürmelere ifade ağaçlarına değinmektedir.

Bu bölümün geri kalanında, farklı özelliklere sahip anonim işlevler içeren çeşitli kod örnekleri verilmiştir. Her örnek için, yalnızca diğer C# yapıları kullanan koda karşılık gelen bir çeviri sağlanır. Örneklerde tanımlayıcı `D`, aşağıdaki temsilci türünü temsil eden kabul edilir:
```csharp
public delegate void D();
```

Anonim bir işlevin en basit biçimi, dış değişken içermeyen bir işlevdir:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Bu, anonim işlevin kodunun yerleştirildiği bir derleyicinin ürettiği statik metoda başvuran bir temsilci örneklemesine çevrilebilir:
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

Aşağıdaki örnekte, anonim işlev `this`örnek üyelerine başvurur:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Bu, anonim işlevin kodunu içeren bir derleyici tarafından oluşturulan örnek metoduna çevrilebilir:
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

Bu örnekte, anonim işlev yerel bir değişken yakalar:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

Yerel değişkenin kullanım ömrü artık en azından adsız işlev temsilcisinin kullanım ömrüne kadar genişletilmelidir. Bu, yerel değişken derleyicinin ürettiği bir sınıfın bir alanına "barındırıdırarak" elde edilebilir. Yerel değişkenin ([yerel değişkenlerin örneklenmesi](expressions.md#instantiation-of-local-variables)) örneklenmesi, ardından derleyici tarafından oluşturulan sınıfın bir örneğini oluşturmaya karşılık gelir ve yerel değişkene erişmek, derleyici tarafından oluşturulan sınıfın örneğindeki bir alana erişmeye karşılık gelir. Ayrıca, anonim işlev derleyicinin ürettiği sınıfının örnek yöntemi olur:
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

Son olarak, aşağıdaki anonim işlev `this` ve farklı yaşam süreleri olan iki yerel değişken yakalar:
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

Burada, yerellerin yakalandığı her bir ekstre bloğu için, farklı bloklara ait yerellerin bağımsız yaşam sürelerinin bulunduğu bir derleyici tarafından oluşturulan bir sınıf oluşturulur. `__Locals2`örneği, iç ifade bloğunun derleyici tarafından üretilen sınıfı, `z` yerel değişkeni ve bir `__Locals1`örneğine başvuran bir alanı içerir.  Bir `__Locals1`örneği, dış ifade bloğu için derleyici tarafından oluşturulan sınıf, `y` yerel değişkeni ve kapsayan işlev üyesinin `this` başvuruda bulunan bir alanı içerir. Bu veri yapıları ile, bir `__Local2`örneği aracılığıyla yakalanan tüm dış değişkenlere ulaşmak mümkündür ve anonim işlevin kodu bu sınıfın örnek yöntemi olarak uygulanabilir.

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

Yerel değişkenleri yakalamak için burada uygulanan teknik, anonim işlevler ifade ağaçlarına dönüştürülürken de kullanılabilir: derleyicinin ürettiği nesneler için başvurular ifade ağacında depolanabilir ve yerel değişkenlere erişim şu olabilir: Bu nesnelerde alan erişimi olarak gösterilir. Bu yaklaşımın avantajı, "yükseltilmemiş" yerel değişkenlerinin temsilciler ve ifade ağaçları arasında paylaşılmasını olanaklı kılar.

## <a name="method-group-conversions"></a>Yöntem grubu dönüştürmeleri

Bir yöntem grubundan ([örtük dönüştürmeler](conversions.md#implicit-conversions)), uyumlu bir temsilci türüne ([ifade sınıflandırmaları](expressions.md#expression-classifications)) sahiptir. Bir temsilci türü `D` ve bir yöntem grubu olarak sınıflandırılan bir ifade `E` verildiğinde, `E`, normal biçimde (uygulanabilir işlev üyesi), aşağıdaki gibi `D`parametre türleri ve değiştiriciler kullanılarak oluşturulan bir bağımsız değişken listesine ([geçerli işlev üyesi](expressions.md#applicable-function-member)) göre oluşturulan bir bağımsız değişken listesine sahip olan bir bağımsız değişken listesine `D` `E` bir örtük dönüştürme bulunur.

Bir yöntem grubundan dönüştürmenin derleme zamanı uygulaması, `D` bir temsilci türüne `E`, aşağıda açıklanmaktadır. `E` 'den `D` örtük dönüştürmenin mevcut olması, dönüştürmenin derleme zamanı uygulamasının hata olmadan başarılı olacağını garanti etmez.

*  Tek bir yöntem `M`, aşağıdaki değişikliklerle birlikte, `E(A)`form bir yöntem çağrısına ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) karşılık gelen seçilir:
    * `A` bağımsız değişken listesi, her biri bir değişken olarak sınıflandırılan ve `D`*formal_parameter_list* karşılık gelen parametrenin türü ve değiştiricisi (`ref` veya `out`) olan ifadelerin bir listesidir.
    * Göz önünde bulundurulması gereken aday yöntemleri yalnızca kendi normal biçimlerinde ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) uygulanabilen ve yalnızca genişletilmiş biçimlerinde geçerli olan yöntemlerdir.
*  [Yöntem etkinleştirmeleri](expressions.md#method-invocations) algoritması bir hata üretirse, derleme zamanı hatası oluşur. Aksi takdirde, algoritma `D` aynı sayıda parametreye sahip `M` tek bir en iyi yöntem üretir ve dönüştürme var olarak kabul edilir.
*  Seçilen yöntem `M`, temsilci türü `D`uyumlu olmalıdır ([temsilci uyumluluğu](delegates.md#delegate-compatibility)) veya aksi halde bir derleme zamanı hatası oluşur.
*  Seçilen `M` Yöntem bir örnek yöntemi ise, `E` ile ilişkili örnek ifadesi temsilcinin hedef nesnesini belirler.
*  Seçili Yöntem, bir örnek ifadesinde üye erişimi aracılığıyla belirtilen bir genişletme yöntemi ise, bu örnek ifadesi temsilcinin hedef nesnesini belirler.
*  Dönüştürmenin sonucu `D`türünde bir değerdir, yani seçili yönteme ve hedef nesneye başvuran yeni oluşturulmuş bir temsilcisidir.
*  Bu işlemin bir uzantı yöntemine bir temsilci oluşturulmasına yol açabileceğini unutmayın. [Yöntem etkinleştirmeleri](expressions.md#method-invocations) algoritması bir örnek yöntemi bulamazsa, ancak `E(A)` bir uzantı yöntemi çağırma ([genişletme yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) olarak çağırma işlemini işlerken başarılı olur. Bu nedenle oluşturulan bir temsilci, Uzantı yönteminin yanı sıra ilk bağımsız değişkenini de yakalar.

Aşağıdaki örnekte, Yöntem grubu dönüştürmeleri gösterilmektedir:
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

`d1` atama, `F` Yöntem grubunu `D1`türünde bir değere örtülü olarak dönüştürür.

`d2` atama, daha az türetilmiş (değişken karşıtı) parametre türleri ve daha türetilmiş (covaryant) dönüş türü olan bir yönteme bir temsilci oluşturmak için nasıl mümkün olduğunu gösterir.

`d3` atama, yöntemin geçerli olmadığı durumlarda dönüştürmenin nasıl mevcut olmadığını gösterir.

`d4` atama, yönteminin normal biçiminde nasıl geçerli olması gerektiğini gösterir.

`d5` atama, temsilci ve yöntemin dönüş türlerinin ve yalnızca başvuru türleri için farklı olmasına izin verildiğini gösterir.

Diğer tüm örtük ve açık dönüşümlerde olduğu gibi, atama işleci açıkça bir yöntem grubu dönüştürmesi gerçekleştirmek için kullanılabilir. Bu nedenle örnek
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
Bunun yerine yazılabilir
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Yöntem grupları aşırı yükleme çözümlemesini etkileyebilir ve tür çıkarımı içine katılabilir. Daha fazla ayrıntı için bkz. [işlev üyeleri](expressions.md#function-members) .

Bir yöntem grubu dönüşümünün çalışma zamanı değerlendirmesi aşağıdaki gibi devam eder:

*  Derleme zamanında seçilen yöntem bir örnek yöntemi ise veya bir örnek yöntemi olarak erişilen bir genişletme yöntemi ise, temsilcinin hedef nesnesi `E`ilişkili örnek ifadeden belirlenir:
    * Örnek ifadesi değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
    * Örnek ifadesi bir *reference_type*ise, örnek ifadesi tarafından hesaplanan değer hedef nesne olur. Seçilen yöntem bir örnek yöntemi ise ve hedef nesne `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
    * Örnek ifadesi bir *value_type*ise, değeri bir nesneye dönüştürmek için bir paketleme Işlemi ([kutulama dönüştürmeleri](types.md#boxing-conversions)) gerçekleştirilir ve bu nesne hedef nesne haline gelir.
*  Aksi takdirde, seçilen yöntem bir statik yöntem çağrısının parçasıdır ve temsilcinin hedef nesnesi `null`.
*  `D` temsilci türünün yeni bir örneği ayrılır. Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.
*  Yeni temsilci örneği, derleme zamanında belirlenen yönteme ve yukarıda hesaplanan hedef nesneye bir başvuruya sahip bir başvuru ile başlatılır.
