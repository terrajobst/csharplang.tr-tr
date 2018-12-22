# <a name="arrays"></a>Diziler

Bir dizi hesaplanan dizinlerini erişilen değişken bir sayı içeren bir veri yapısıdır. Dizinin öğeleri olarak da bilinen bir dizi içindeki tüm aynı türdeki değişkenlerdir ve bu tür dizinin öğe türü olarak adlandırılır.

Bir dizinin her dizi öğesi ile ilişkili sayısından belirleyen bir dereceli. Dizi boyut, dizi boyutları da bilinir. Bir boyut ile bir dizi olarak adlandırılan bir ***tek boyutlu dizi***. Daha fazla biri derece olan bir dizi bir ***çok boyutlu dizi***. Belirli bir boyuttaki çok boyutlu diziler, iki boyutlu diziler, üç boyutlu dizileri vb. da anılır.

Bir dizinin her boyutunun sıfır olarak bir tamsayı değerinden büyük veya eşit olan ilişkili bir uzunluğa sahip. Boyut uzunlukları dizinin türü bir parçası değildir, ancak çalışma zamanında dizi türünün bir örneği oluşturulduğunda yerine oluşturulur. Bir boyutun uzunluğu o boyut için dizin geçerli aralık belirler: Boyut uzunluğu `N`, dizinler arasında değişebilir `0` için `N - 1` dahil. Bir dizideki öğelerin sayısı dizideki her boyutun uzunluğu ürünüdür. Sıfır uzunlukta bir veya daha fazla dizi boyutları varsa, dizi, boş olarak kabul edilir.

Bir dizinin öğe türü bir dizi türü de dahil olmak üzere herhangi bir tür olabilir.

## <a name="array-types"></a>Dizi türleri

Bir dizi türü olarak yazılmış bir *non_array_type* ardından bir veya daha fazla *rank_specifier*: %s

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

A *non_array_type* herhangi *türü* diğer bir deyişle kendisi bir *array_type*.

Boyut sayısı bir dizi türü soldaki tarafından verilen *rank_specifier* içinde *array_type*: A *rank_specifier* bir artı sayısı derecesi olan bir dizi dizi belirtir "`,`" içinde belirteçler *rank_specifier*.

Bir dizi türünün öğe türü soldaki silmesi sonuçları türdür *rank_specifier*:

*  Bir dizi türü formun `T[R]` dereceye sahip bir dizi `R` ve dizi olmayan öğe türü `T`.
*  Bir dizi türü formun `T[R][R1]...[Rn]` dereceye sahip bir dizi `R` ve bir öğe türü `T[R1]...[Rn]`.

Aslında, *rank_specifier*s okunur soldan sağa önce son dizi olmayan öğe türü. Türü `int[][,,][,]` üç boyutlu dizi iki boyutlu dizi tek boyutlu bir dizidir `int`.

Çalışma zamanında, bir dizi türünde bir değer olabilir `null` veya dizi türü örneğine başvuru.

### <a name="the-systemarray-type"></a>System.Array türü

Türü `System.Array` tüm dizi türleri soyut taban türü. Örtük bir başvuru dönüştürmesi ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) için herhangi bir dizi türünden var. `System.Array`ve bir açık bir başvuru dönüştürmesi ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) var. gelen `System.Array` için herhangi bir dizi türü. Unutmayın `System.Array` kendisi değil bir *array_type*. Bunun yerine, olan bir *class_type* tüm gelen *array_type*s türetilir.

Çalışma zamanında, türünde bir değer `System.Array` olabilir `null` veya herhangi bir dizi türü örneğine başvuru.

### <a name="arrays-and-the-generic-ilist-interface"></a>Diziler ve genel IList arabirimi

Tek boyutlu dizi `T[]` arabirimi uygulayan `System.Collections.Generic.IList<T>` (`IList<T>` kısaca) ve kendi temel arabirimleri. Buna göre örtük bir dönüştürme yoktur `T[]` için `IList<T>` ve kendi temel arabirimleri. Ayrıca, bir örtük bir başvuru dönüştürme ise `S` için `T` ardından `S[]` uygulayan `IList<T>` ve gelen bir örtük bir başvuru dönüştürmesi yoktur `S[]` için `IList<T>` ve bunun temel arabirimleri ( [Örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)). Bir açık başvuru dönüştürme ise `S` için `T` sonra gelen bir açık bir başvuru dönüştürmesi `S[]` için `IList<T>` ve temel arabirimlerini ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)). Örneğin:
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

Atama `lst2 = oa1` dönüştürme işlemi bu yana bir derleme zamanı hatası oluşturur `object[]` için `IList<string>` , değil örtük açık bir dönüştürme. Cast `(IList<string>)oa1` bir özel çalışma zamanında bu yana durum oluşturulmasına neden olacak `oa1` başvurular bir `object[]` değil bir `string[]`. Ancak cast `(IList<string>)oa2` bir özel bu yana durum neden olmaz `oa2` başvurular bir `string[]`.

Her başvuru örtük veya açık bir dönüştürme yoktur `S[]` için `IList<T>`, aynı zamanda gelen bir açık bir başvuru dönüştürmesi yoktur `IList<T>` ve bunun temel arabirimleri için `S[]` ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)).

Bir dizi türü olduğunda `S[]` uygulayan `IList<T>`, uygulanan arabirimin üyelerini bazı özel durumlar oluşturabilir. Kesin arabiriminin uygulamasını bu belirtim kapsamı dışındadır davranışıdır.

## <a name="array-creation"></a>Dizi oluşturma

Dizi örneği tarafından oluşturulan *array_creation_expression*s ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)) veya alan ya da içeren yerel bir değişken bildirimi bir *array_initializer*([Dizi başlatıcıları](arrays.md#array-initializers)).

Bir dizi örneği oluşturulduğunda, boyut sayısı ve her boyutun uzunluğu oluşturulur ve ardından örneğinin tüm ömrü boyunca sabit kalır. Diğer bir deyişle, var olan bir dizi örneği derecesini değiştirmek mümkün değildir ve boyutlar yeniden boyutlandırmak mümkündür.

Bir dizi örneği her zaman bir dizi türüdür. `System.Array` Türü, soyut bir tür örneği oluşturulamıyor.

Tarafından oluşturulan dizilerin öğelerine *array_creation_expression*s her zaman varsayılan değerlerine başlatıldı ([varsayılan değerler](variables.md#default-values)).

## <a name="array-element-access"></a>Dizi öğesi erişimi

Dizi öğeleri kullanılarak erişilir *element_access* ifadeleri ([dizi erişim](expressions.md#array-access)) biçiminde `A[I1, I2, ..., In]`burada `A` bir dizi türü ve her bir ifade `Ix` olduğu bir türündeki ifade `int`, `uint`, `long`, `ulong`, ya da bir veya daha fazla bu türleri için örtük olarak dönüştürülebilir. Bir değişken, yani dizinleri tarafından seçilen dizi öğesi bir dizi öğe erişimi sonucudur.

Kullanarak bir dizinin öğeleri numaralandırılabilen bir `foreach` deyimi ([foreach deyimi](statements.md#the-foreach-statement)).

## <a name="array-members"></a>Dizi üyeleri

Her dizi türü tarafından bildirilen üyeleri devralan `System.Array` türü.

## <a name="array-covariance"></a>Dizi kovaryansı

İçin herhangi iki *reference_type*s `A` ve `B`örtük bir başvuru dönüştürmesi, ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) ya da açık bir başvuru dönüştürmesi ([ Açık bir başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) öğesinden var. `A` için `B`, aynı başvuru dönüştürmesi da dizi türünden varsa `A[R]` dizi türüne `B[R]`burada `R` herhangi verilen *rank_specifier* (ancak her ikisi için de aynı dizi türleri). Bu ilişki olarak da bilinen ***dizi kovaryansı***. Dizi kovaryansı özellikle anlamına gelir, bir dizi türünde bir değer `A[R]` aslında bir dizi türünde bir örneğe bir başvuru olabilir `B[R]`, gelen bir örtük bir başvuru dönüştürmesi var sağlanan `B` için `A`.

Dizi kovaryansı nedeniyle, çalışma zamanı denetimi, bir dizi öğesine atanan değerin aslında izin verilen bir türde olmasını sağlar, başvuru türü dizilerin öğelerine atamaları dahil ([basit atama](expressions.md#simple-assignment)). Örneğin:
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

Atamayı `array[i]` içinde `Fill` örtük olarak includes yöntemi tarafından başvurulan nesne sağlar bir çalışma zamanı denetimi `value` ya da `null` veya gerçeköğetürüileuyumlubirörnek`array`. İçinde `Main`, ilk iki çağrılarını `Fill` başarılı, ancak üçüncü çağırma neden bir `System.ArrayTypeMismatchException` ilk atama için yürütme sırasında durum `array[i]`. Özel durum oluşur paketlenmiş bir `int` içinde depolanamaz bir `string` dizi.

Dizi kovaryansı özellikle genişletmiyoruz dizileri için *value_type*s. Örneğin, bu izinleri dönüştürme mevcut bir `int[]` olarak kabul edilmesini bir `object[]`.

## <a name="array-initializers"></a>Dizi başlatıcıları

Dizi başlatıcıları, alan bildirimlerinde belirtilebilir ([alanları](classes.md#fields)), yerel değişken bildirimleri ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) ve dizi oluşturma ifadeleri ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)):

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Bir dizi değişken başlatıcılar, kapsadığı bir dizi Başlatıcısı oluşan "`{`"ve"`}`"belirteçler ve ayrılmış"`,`" belirteçleri. Her değişken başlatıcı bir ifade olan veya çok boyutlu bir dizi iç içe geçmiş dizi Başlatıcı söz konusu olduğunda.

Bir dizi Başlatıcısı kullanıldığı bağlamı başlatılmakta dizi türünü belirler. Bir dizi oluşturma ifadesi dizi türü hemen Başlatıcı önündeki veya dizi Başlatıcı ifadelerinde gösterilir. Bir alan veya Değişken bildiriminde dizi türü alan veya bildirilen değişken türüdür. Bir dizi başlatıcısı bir alan ya da değişken bildirimi gibi kullanıldığında:
```csharp
int[] a = {0, 2, 4, 6, 8};
```
Bu, yalnızca bir eşdeğer dizi oluşturma ifadesi için toplu özelliktir:
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

Tek boyutlu bir dizi için bir dizi başlatıcısı, dizinin öğe türü ile uyumlu atama ifadeler bir dizi oluşmalıdır. İfadeler, artan düzende sıfır dizinindeki öğe ile başlayarak, dizi öğelerini başlatmak. Dizi Başlatıcısı ifadelerin sayısı, dizi örneği oluşturulan uzunluğunu belirler. Örneğin, yukarıdaki dizi Başlatıcısı oluşturur bir `int[]` uzunluğu 5 örneğini ve ardından örneği aşağıdaki değerlerle başlatır:
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

Çok boyutlu bir dizi için dizi başlatıcıda dizideki boyutların gibi iç içe kadar düzeyleri olması gerekir. En dıştaki iç içe geçme düzeyi en soldaki boyuta karşılık gelir ve en içteki iç içe geçme düzeyi en sağdaki boyuta karşılık gelir. Dizinin her boyutunun uzunluğu, dizi başlatıcısında karşılık gelen iç içe geçme düzeyindeki öğelerin sayısı tarafından belirlenir. Her iç içe geçmiş dizi Başlatıcısı için öğelerin sayısını bir dizi başlatıcıları aynı düzeyde aynı olmalıdır. Örnek:
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
beş en soldaki boyutun uzunluğu ve iki en sağdaki boyutun uzunluğu ile iki boyutlu bir dizi oluşturur:
```csharp
int[,] b = new int[5, 2];
```
ve ardından başlatır dizi örneği aşağıdaki değerlerle:
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

En sağdaki dışındaki bir boyut sıfır uzunluğunda belirtilmezse, sonraki boyutları sıfır uzunluk de varsayılır. Örnek:
```csharp
int[,] c = {};
```
en soldaki ve sağdaki boyutu için sıfır uzunluğuna sahip iki boyutlu bir dizi oluşturur:
```csharp
int[,] c = new int[0, 0];
```

Bir dizi oluşturma ifadesi, hem açık boyut uzunlukları hem de bir dizi Başlatıcısı içerdiğinde, uzunlukları sabit ifadeler olması gerekir ve her iç içe geçme düzeyindeki öğelerin sayısını ilgili boyut uzunluğu eşleşmesi gerekir. Bazı örnekler şunlardır:
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

Burada, Başlatıcısı `y` boyut uzunluğu ifadesi bir sabit ve Başlatıcısı olmadığı için bir derleme zamanı hatası oluşur `z` sonuçları bir derleme zamanı hatası nedeniyle uzunluğu ve öğe sayısı Başlatıcı kabul.
