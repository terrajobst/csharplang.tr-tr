---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310363"
---
# <a name="unsafe-code"></a>Güvenli olmayan kod

Önceki bölümlerde C# tanımlandığı gibi çekirdek dil, C 'den ve C++ veri türü olarak işaretçilerin atlanından farklıdır. Bunun yerine C# , başvuruları ve çöp toplayıcı tarafından yönetilen nesneler oluşturma özelliğini sağlar. Diğer özelliklerle bağlanmış bu tasarım, C veya C# C++' den çok daha güvenli bir dil sağlar. Çekirdek C# dilde, başlatılmamış bir değişkene, "Dangling" işaretçisine veya bir diziyi, sınırlarının ötesinde dizinleyen bir ifadeye sahip olmak mümkün değildir. C ve programları düzenli olarak oluşturan hataların tüm kategorileri bundan C++ sonra ortadan kaldırılır.

Neredeyse her işaretçi türü yapısını C 'de veya C++ içinde C#bir başvuru türüne sahip olsa da, işaretçi türlerine erişimin bir zorunludur hale geldiği durumlar vardır. Örneğin, temel alınan işletim sistemiyle arabirim oluşturma, bellek eşlemeli cihaza erişme veya zaman açısından kritik bir algoritma uygulama, işaretçilere erişim olmadan mümkün veya pratik olmayabilir. Bu gereksinimi karşılamak için C# ***güvenli olmayan kod***yazma özelliği sağlar.

Güvenli olmayan kodda, işaretçiler ve integral türleri arasında dönüştürmeler gerçekleştirmek, değişkenlerin adresini almak için ve benzeri işlemleri yapmak üzere işaretçiler üzerinde bildirmek ve çalıştırmak mümkündür. Bir anlamda, güvenli olmayan kod yazmak bir C# program içinde C kodu yazma gibidir.

Güvenli olmayan kod, geliştiricilerin ve kullanıcıların perspektifinden "güvenli" bir özelliktir. Güvenli olmayan kod değiştirici `unsafe`açıkça işaretlenmelidir, bu nedenle geliştiriciler yanlışlıkla güvenli olmayan özellikler kullanamaz ve yürütme altyapısı, güvenli olmayan kodun güvenilmeyen bir ortamda yürütülmemesini sağlamak için çalışmaktadır.

## <a name="unsafe-contexts"></a>Güvenli olmayan bağlamlar

Güvenli olmayan özellikleri C# yalnızca güvenli olmayan bağlamlarda kullanılabilir. Güvenli olmayan bir bağlam, bir tür veya üyenin bildiriminde `unsafe` değiştiricisi eklenerek veya bir *unsafe_statement*kullanarak ortaya çıkartılır:

*  Bir sınıf, yapı, arabirim veya temsilcinin bir bildirimi `unsafe` değiştirici içerebilir ve bu durumda, bu tür bildiriminin tüm metinsel uzantısı (sınıf, yapı veya arabirim gövdesi dahil) güvenli olmayan bir bağlam olarak kabul edilir.
*  Bir alan, yöntem, özellik, olay, Dizin Oluşturucu, işleç, örnek Oluşturucu, yıkıcı veya statik oluşturucunun bir bildirimi `unsafe` değiştirici içerebilir, bu durumda bu üye bildiriminin tüm metin kapsamı güvenli olmayan bir bağlam olarak kabul edilir.
*  *Unsafe_statement* bir *blok*içinde güvenli olmayan bir bağlam kullanımını mümkün bir şekilde sunar. İlişkili *bloğun* tüm metinsel uzantısı güvenli olmayan bir bağlam olarak kabul edilir.

İlişkili dilbilgisi üretimleri aşağıda gösterilmiştir.

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

örnekte

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

struct bildiriminde belirtilen `unsafe` değiştirici, struct bildiriminin tüm metinsel kapsamının güvenli olmayan bir bağlam haline gelmesine neden olur. Bu nedenle, `Left` ve `Right` alanlarını bir işaretçi türü olarak bildirmek mümkündür. Yukarıdaki örnek de yazılabilir

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

Burada, alan bildirimlerinde `unsafe` değiştiriciler, bu bildirimlerin güvenli olmayan bağlamlar olarak kabul edilmesine neden olur.

Güvenli olmayan bir bağlam oluşturma dışında, işaretçi türlerinin kullanılmasına izin veren `unsafe` değiştiricinin bir tür veya üye üzerinde hiçbir etkisi yoktur. örnekte

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

`A` `F` yönteminde `unsafe` değiştirici, `F` 'ın metin kapsamının, dilin güvenli olmayan özelliklerinin kullanılabileceği güvenli olmayan bir bağlam haline gelmesine neden olur. `B``F` geçersiz kılmada, `unsafe` değiştiricinin yeniden belirtilmesi gerekmez--kuşkusuz, `B` içindeki `F` yönteminin güvenli olmayan özelliklere erişmesi gerekir.

İşaretçi türü metodun imzasının bir parçası olduğunda durum biraz farklıdır

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

Burada `F`imzası bir işaretçi türü içerdiğinden, yalnızca güvenli olmayan bir bağlamda yazılabilir. Bununla birlikte, güvenli olmayan bağlam, `A`olduğu gibi, tüm sınıfı güvenli hale getirerek veya yöntem bildiriminde bir `unsafe` değiştiricisi ekleyerek (`B`durumda olduğu gibi) gerçekleştirilebilir.

## <a name="pointer-types"></a>İşaretçi türleri

Güvenli olmayan bir bağlamda, bir *tür* ([türler](types.md)) *pointer_type* bir *value_type* ya da *reference_type*olabilir. Ancak bir *pointer_type* , bu kullanım güvenli olmadığı için güvenli olmayan bir bağlam dışında bir `typeof` ifadesinde de kullanılabilir ([anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions)).

```antlr
type_unsafe
    : pointer_type
    ;
```

Bir *pointer_type* *unmanaged_type* veya anahtar sözcüğü `void`, ardından bir `*` belirteci tarafından yazılır:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

Bir işaretçi türündeki `*` önce belirtilen tür, işaretçi türünün ***başvurulan türü*** olarak adlandırılır. İşaretçi türündeki bir değerin işaret ettiği değişkenin türünü temsil eder.

Başvuruların aksine (başvuru türlerinin değerleri), işaretçiler çöp toplayıcı tarafından izlenmez. çöp toplayıcı, hiçbir işaretçi bilgisine ve işaret ettikleri verilere sahip değildir. Bu nedenle, bir işaretçiye başvuruya veya başvuruları içeren bir yapıya işaret eden bir işaretçi için bir *unmanaged_type*olması gerekir.

*Unmanaged_type* , *reference_type* veya oluşturulmuş tür olmayan ve herhangi bir iç içe geçme düzeyinde *reference_type* veya oluşturulmuş tür alanları içermeyen bir türdür. Diğer bir deyişle, *unmanaged_type* aşağıdakilerden biridir:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`veya `bool`.
*  Herhangi bir *enum_type*.
*  Herhangi bir *pointer_type*.
*  Oluşturulmuş bir tür olmayan ve yalnızca *unmanaged_type*s alanlarını içeren kullanıcı tanımlı *struct_type* .

İşaretçilerin ve başvuruların karıştırılmasına yönelik sezgisel kural, başvuru (nesneler) başvurularının, işaretçiler içermesine izin vermelerdir, ancak işaretçilerin başvurularının başvuru içermesine izin verilmez.

Aşağıdaki tabloda işaretçi türlerine bazı örnekler verilmiştir:

| __Örnek__ | __Açıklama__                               |
|-------------|-----------------------------------------------|
| `byte*`     | `byte` işaretçisi                             |
| `char*`     | `char` işaretçisi                             |
| `int**`     | `int` işaretçisine işaretçi                   |
| `int*[]`    | `int` işaretçilerin tek boyutlu dizisi |
| `void*`     | Bilinmeyen tür işaretçisi                       |

Belirli bir uygulama için, tüm işaretçi türleri aynı boyuta ve gösterimine sahip olmalıdır.

C ve C++' nin aksine, aynı bildirimde C# birden çok işaretçi bildirildiğinde, `*` her işaretçi adında ön ek noktalama işareti olarak değil, yalnızca temel alınan türle birlikte yazılır. Örneğin:

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

Türü `T*` olan bir işaretçinin değeri, `T`türünde bir değişkenin adresini temsil eder. İşaretçi yöneltme işleci `*` ([işaretçi yöneltme](unsafe-code.md#pointer-indirection)) bu değişkene erişmek için kullanılabilir. Örneğin, `int*`türünde bir değişken `P` verildiğinde ifade `*P`, `P`bulunan adreste bulunan `int` değişkenini gösterir.

Bir nesne başvurusu gibi bir işaretçi de `null`olabilir. `null` işaretçisine yöneltme işlecini uygulamak, uygulama tanımlı davranışa neden olur. Değer `null` bir işaretçi, All-bit-sıfır ile temsil edilir.

`void*` türü, bilinmeyen bir türe yönelik bir işaretçi temsil eder. Ayrılan tür bilinmediği için, yöneltme işleci `void*`türünde bir işaretçiye uygulanamaz, ne de bu tür bir işaretçi üzerinde herhangi bir aritmetik işlem yapılabilir. Ancak, `void*` bir işaretçi diğer işaretçi türlerine (ve tam tersi) dönüşebilir.

İşaretçi türleri ayrı bir tür kategorisidir. Başvuru türleri ve değer türlerinin aksine, işaretçi türleri `object` 'ten aktarılmaz ve işaretçi türleri ve `object`arasında dönüştürme yok. Özellikle, kutulama ve kutudan çıkarma ([kutulama ve kutudan](types.md#boxing-and-unboxing)çıkarma) işaretçiler için desteklenmez. Ancak, farklı işaretçi türleri arasında ve işaretçi türleri ile integral türleri arasında dönüştürmeye izin verilir. Bu, [işaretçi dönüştürmelerinde](unsafe-code.md#pointer-conversions)açıklanır.

*Pointer_type* , tür bağımsız değişkeni olarak kullanılamaz ([oluşturulmuş türler](types.md#constructed-types)) ve tür çıkarımı ([tür çıkarımı](expressions.md#type-inference)), bir tür bağımsız değişkenini bir işaretçi türü olacak şekilde çıkartılan genel metot çağrılarında başarısız olur.

Bir *pointer_type* geçici bir alanın türü olarak kullanılabilir ([geçici alanlar](classes.md#volatile-fields)).

İşaretçiler `ref` veya `out` parametresi olarak geçirilebilse de, işaretçi, çağrılan yöntem döndürüldüğünde veya işaret etmek için kullanılan sabit nesne artık sabitlenmemiş bir yerel değişkene işaret etmek üzere ayarlanabildiğinden, bu durum tanımsız davranışa neden olabilir. Örneğin:

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

Bir yöntem bir tür değeri döndürebilir ve bu tür bir işaretçi olabilir. Örneğin, `int`s bitişik dizisine işaretçi verildiğinde, bu dizinin öğe sayısı ve başka bir `int` değeri olduğunda, bir eşleşme meydana gelirse, aşağıdaki yöntem o dizideki bu değerin adresini döndürür; Aksi takdirde `null`döndürür:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

Güvenli olmayan bir bağlamda, işaretçilerde çalıştırmak için birkaç yapı mevcuttur:

*  `*` işleci, işaretçi yöneltme ([işaretçi yöneltme](unsafe-code.md#pointer-indirection)) yapmak için kullanılabilir.
*  `->` işleci, bir yapının üyesine işaretçi aracılığıyla erişmek için kullanılabilir ([işaretçi üyesi erişimi](unsafe-code.md#pointer-member-access)).
*  `[]` işleci bir işaretçiye dizin eklemek için kullanılabilir ([işaretçi öğesi erişimi](unsafe-code.md#pointer-element-access)).
*  `&` işleci, bir değişkenin adresini ([Adres operatörü](unsafe-code.md#the-address-of-operator)) almak için kullanılabilir.
*  `++` ve `--` işleçleri, işaretçileri artırmak ve azaltmak için kullanılabilir ([işaretçi artışı ve azalması](unsafe-code.md#pointer-increment-and-decrement)).
*  `+` ve `-` işleçleri, işaretçi aritmetiği ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)) gerçekleştirmek için kullanılabilir.
*  `==`, `!=`, `<`, `>`, `<=`ve `=>` işleçleri, işaretçileri ([işaretçi karşılaştırması](unsafe-code.md#pointer-comparison)) karşılaştırmak için kullanılabilir.
*  `stackalloc` işleci, çağrı yığınından ([sabit boyutlu arabellekler](unsafe-code.md#fixed-size-buffers)) bellek ayırmak için kullanılabilir.
*  `fixed` deyimleri, bir değişkeni geçici olarak çözmek için kullanılabilir ve bu nedenle adresini elde edebilir ([fixed deyimidir](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Sabit ve taşınabilir değişkenler

Address-of işleci ([Address-of işleci](unsafe-code.md#the-address-of-operator)) ve `fixed` deyimin ([fixed deyimin](unsafe-code.md#the-fixed-statement)) değişkenleri Iki kategoriye böler: ***sabit değişkenler*** ve ***Taşınabilir değişkenler***.

Sabit değişkenler çöp toplayıcı 'nin işleminden etkilenmeyen depolama konumlarında bulunur. (Sabit değişkenlere örnek olarak, yerel değişkenler, değer parametreleri ve başvuru işaretçileri tarafından oluşturulan değişkenler verilebilir.) Diğer yandan, taşınabilir değişkenler çöp toplayıcısının yerini değiştirme veya aktiften çıkarma konusunda yer alan depolama konumlarında bulunur. (Taşınamayacak değişkenlerin örnekleri, nesne ve dizi öğelerindeki alanları içerir.)

`&` işleci ([Adres operatörü](unsafe-code.md#the-address-of-operator)), sabit bir değişkenin adresinin kısıtlama olmadan elde edilebilir olmasını sağlar. Ancak, taşınabilir bir değişken çöp toplayıcısının yeniden konumlandırılmasını veya aktiften çıkarılma tabi olduğundan, taşınabilir bir değişkenin adresi yalnızca bir `fixed` ekstresi ([fixed ifadesiyle](unsafe-code.md#the-fixed-statement)) kullanılarak elde edilebilir ve bu adres yalnızca o `fixed` deyimin süresi boyunca geçerli kalır.

Kesin koşullarda, sabit bir değişken aşağıdakilerden biridir:

*  Değişken anonim bir işlev tarafından yakalanmadığı takdirde, bir yerel değişkene veya bir değer parametresine başvuran *simple_name* ([basit adlar](expressions.md#simple-names)) sonucu.
*  `V` sabit *struct_type*bir değişken olduğu, form `V.I`*member_access* ([üye erişimi](expressions.md#member-access)) sonucu olan bir değişken.
*  Form `*P`*pointer_indirection_expression* ([işaretçi yöneltme](unsafe-code.md#pointer-indirection)), form `P->I`*pointer_member_access* (işaretçi[üyesi erişimi](unsafe-code.md#pointer-member-access)) *veya pointer_element_access form `P[E]`(* [işaretçi öğesi erişimi](unsafe-code.md#pointer-element-access)) sonucu.

Diğer tüm değişkenler taşınabilir değişkenler olarak sınıflandırılmaktadır.

Statik bir alanın taşınamayacak değişken olarak sınıflandırıldığını unutmayın. Ayrıca, parametre için verilen bağımsız değişken sabit bir değişken olsa da, bir `ref` veya `out` parametresinin taşınabilir bir değişken olarak sınıflandırıldığını unutmayın. Son olarak, bir işaretçinin başvurusunun kaldırılması tarafından oluşturulan bir değişkenin her zaman sabit bir değişken olarak sınıflandırıldığını unutmayın.

## <a name="pointer-conversions"></a>İşaretçi Dönüştürmeler

Güvenli olmayan bir bağlamda, kullanılabilir örtük dönüştürmeler ([örtük dönüştürmeler](conversions.md#implicit-conversions)) kümesi aşağıdaki örtük işaretçi dönüşümlerini içerecek şekilde genişletilir:

*  *Pointer_type* `void*`türüne yazın.
*  `null` değişmez değerinden *pointer_type*.

Ayrıca, güvenli olmayan bir bağlamda, kullanılabilir açık dönüştürmeler ([Açık dönüştürmeler](conversions.md#explicit-conversions)) kümesi aşağıdaki açık işaretçi dönüşümlerini içerecek şekilde genişletilir:

*  Herhangi bir *pointer_type* başka bir *pointer_type*.
*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`veya `ulong` herhangi bir *pointer_type*.
*  Herhangi bir *pointer_type* `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`veya `ulong`.

Son olarak, güvenli olmayan bir bağlamda, standart örtük dönüştürmeler kümesi ([Standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) aşağıdaki işaretçi dönüşümünü içerir:

*  *Pointer_type* `void*`türüne yazın.

İki işaretçi türü arasındaki dönüşümler hiçbir şekilde gerçek işaretçi değerini değiştirmez. Diğer bir deyişle, bir işaretçi türünden diğerine dönüştürmenin, işaretçi tarafından verilen temel adres üzerinde hiçbir etkisi yoktur.

Bir işaretçi türü diğerine dönüştürüldüğünde, ortaya çıkan işaretçi, işaret türü için doğru hizalanmazsa, sonuç başvuru başvurusu varsa, bu davranış tanımsızdır. Genel olarak, "doğru hizalı" kavram geçişlidir: tür `A` bir işaretçi, `B`türüne yönelik bir işaretçi için doğru hizalanmışsa, bu, sırasıyla `C`türü bir işaretçi için doğru şekilde hizalanmışsa, `A` türüne yönelik bir işaretçi, `C`türünde bir işaretçi için doğru şekilde hizalanmıştır.

Farklı bir türe yönelik bir işaretçi aracılığıyla bir türe sahip bir değişkene erişildiği aşağıdaki durumu göz önünde bulundurun:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Bir işaretçi türü bayta bir işaretçiye dönüştürüldüğünde, sonuç değişkenin en düşük adresli bayta işaret eder. Sonucun ardışık artışlarına kadar, değişkenin boyutuna kadar, bu değişkenin kalan baytlarına kadar dönen işaretçiler. Örneğin, aşağıdaki yöntem bir Double içindeki sekiz baytın her birini onaltılık bir değer olarak görüntüler:

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

Tabii ki, üretilen çıkış, bitime 'ye bağlıdır.

İşaretçiler ve tamsayılar arasındaki eşlemeler uygulama tanımlı. Bununla birlikte, doğrusal bir adres alanı ile 32 * ve 64 bit CPU mimarilerinde, tam sayı türlerine veya bu tür bir değere yapılan işaretçilerin dönüştürmeleri genellikle `uint` ya da `ulong` değerlerinin, sırasıyla, bu integral türlerine veya bu tür dönüştürmeleri gibi davranır.

### <a name="pointer-arrays"></a>İşaretçi dizileri

Güvenli olmayan bir bağlamda, işaretçiler dizileri oluşturulabilir. İşaretçi dizileri üzerinde yalnızca diğer dizi türleri için uygulanan Dönüştürmelere izin verilir:

*  Herhangi bir *array_type* `System.Array` ve onun uyguladığı arayüzlerden örtük başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)), işaretçi dizileri için de geçerlidir. Ancak, dizi öğelerine `System.Array` veya uyguladığı arabirimler aracılığıyla erişmek için herhangi bir girişim, çalışma zamanında bir özel durum oluşmasına neden olur, çünkü işaretçi türleri `object`dönüştürülebilir değildir.
*  Tek boyutlu dizi türünden `System.Collections.Generic.IList<T>` ve genel temel arabirimlerine `S[]` örtük ve açık başvuru dönüştürmeleri ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions), [açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)) hiçbir şekilde hiçbir şekilde uygulanmaz çünkü işaretçi türleri tür bağımsız değişkeni olarak kullanılamaz ve işaretçi türlerinden işaretçi olmayan türlere dönüştürme yok.
*  `System.Array` ve *array_type* uyguladığı arabirimlerin açık başvuru dönüştürmesi ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)), işaretçi dizileri için geçerli olur.
*  `System.Collections.Generic.IList<S>` ve temel arabirimlerinden bir tek boyutlu dizi türüne `T[]` açık başvuru dönüştürmeleri ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)), işaretçi türleri tür bağımsız değişkenleri olarak kullanılamadığından ve işaretçi türlerinden işaretçi olmayan türlere dönüştürme olmadığı için hiçbir şekilde işaretçi dizileri için geçerli değildir.

Bu kısıtlamalar, [foreach ifadesinde](statements.md#the-foreach-statement) açıklanan diziler üzerinde `foreach` bildirimine yönelik genişletmenin işaretçi dizilerine uygulanamadığını ifade ediyor. Bunun yerine, formun foreach ifadesi

```csharp
foreach (V v in x) embedded_statement
```

`x` türü, form `T[,,...,]`bir dizi türü olduğunda, `N` boyut sayısı eksi 1 ve `T` veya `V` bir işaretçi türü, iç içe for-döngüleri kullanılarak aşağıdaki gibi genişletilir:

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

`a`, `i0`, `i1`,..., `iN` değişkenler, `x` veya *embedded_statement* ya da programın herhangi bir kaynak kodu için görünür veya erişilebilir değildir. `v` değişkeni, gömülü ifadede salt okunurdur. `T` (öğe türü) öğesinden `V`bir açık dönüştürme ([işaretçi dönüşümleri](unsafe-code.md#pointer-conversions)) yoksa, bir hata oluşturulur ve başka bir adım alınmaz. `x` `null`değeri varsa, çalışma zamanında bir `System.NullReferenceException` oluşturulur.

## <a name="pointers-in-expressions"></a>İfadelerdeki işaretçiler

Güvenli olmayan bir bağlamda, bir ifade bir işaretçi türü sonucunu verebilir, ancak güvenli olmayan bir bağlam dışında bir ifadenin işaretçi türü olması için derleme zamanı hatası olur. Kesin koşullarda, güvenli olmayan bir bağlam dışında, bir *simple_name* ([basit adlar](expressions.md#simple-names)), *member_access* ([üye erişimi](expressions.md#member-access)), *invocation_expression* ([çağırma ifadeleri](expressions.md#invocation-expressions)) veya *element_access* ([öğe erişimi](expressions.md#element-access)) bir işaretçi türü ise, bir derleme zamanı hatası oluşur.

Güvenli olmayan bir bağlamda *primary_no_array_creation_expression* ([birincil ifadeler](expressions.md#primary-expressions)) ve *unary_expression* ([Birli İşleçler](expressions.md#unary-operators)) üretimleri aşağıdaki ek yapılara izin verir:

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

Bu yapılar aşağıdaki bölümlerde açıklanmıştır. Güvenli olmayan işleçlerin önceliği ve ilişkilendirilebilirliği, dilbilgisi tarafından kapsanıyor.

### <a name="pointer-indirection"></a>İşaretçi yöneltme

*Pointer_indirection_expression* , bir yıldız işareti (`*`) ve ardından bir *unary_expression*oluşur.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

Birli `*` işleci işaretçi yöneltme 'yi gösterir ve bir işaretçinin işaret ettiği değişkeni elde etmek için kullanılır. `*P`değerlendirilme sonucu, `P` işaretçi türü `T*`bir ifadesiyse, `T`türünde bir değişkendir. Birli `*` işlecini `void*` türünde bir ifadeye veya işaretçi türünde olmayan bir ifadeye uygulamak için derleme zamanı hatası.

Birli `*` işlecini bir `null` işaretçisine uygulamanın etkisi, uygulama tanımlı ' dır. Özellikle, bu işlemin bir `System.NullReferenceException`oluşturduğunda garanti yoktur.

İşaretçiye geçersiz bir değer atanmışsa, birli `*` işlecinin davranışı tanımsızdır. Birli `*` işleci tarafından bir işaretçinin başvurusunun kaldırılması için geçersiz değerler arasında, işaret edilen tür için uygun şekilde hizalı bir adrestir (bkz. [işaretçi dönüştürmelerinde](unsafe-code.md#pointer-conversions)örnek) ve yaşam süresinin sonundan sonraki bir değişkenin adresi.

Kesin atama analizinin amaçları doğrultusunda, form `*P` bir ifade hesaplanarak üretilen bir değişken başlangıçta atanan ([Başlangıçta atanan değişkenler](variables.md#initially-assigned-variables)) olarak değerlendirilir.

### <a name="pointer-member-access"></a>İşaretçi üye erişimi

Bir *pointer_member_access* *primary_expression*ve ardından bir "`->`" belirteci ve ardından bir *tanımlayıcı* ve isteğe bağlı bir *type_argument_list*oluşur.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

Bir işaretçi üye `P->I`, `void*`dışında bir işaretçi türünün bir ifadesi olmalıdır ve `I`, `P` işaret eden `P` bir erişilebilir üyeyi belirtmelidir.

`P->I` form işaretçisi üye erişimi, tam olarak `(*P).I`olarak değerlendirilir. İşaretçi yöneltme işlecinin (`*`) bir açıklaması için bkz. [işaretçi yöneltme](unsafe-code.md#pointer-indirection). Üye erişim işlecinin (`.`) bir açıklaması için bkz. [üye erişimi](expressions.md#member-access).

örnekte

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

`->` işleci, alanlara erişmek ve bir yapı yöntemini işaretçi aracılığıyla çağırmak için kullanılır. İşlem `P->I` tam olarak `(*P).I`eşdeğer olduğundan, `Main` yöntemi eşit olarak da yazılabilir:

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>İşaretçi öğesi erişimi

Bir *pointer_element_access* , ardından "`[`" ve "`]`" içinde olan bir ifade tarafından izlenen bir *primary_no_array_creation_expression* oluşur.

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

`P[E]`bir işaretçi öğesinde, `P` `void*`dışında bir işaretçi türünün bir ifadesi olmalıdır ve `E` örtük olarak `int`, `uint`, `long`veya `ulong`olarak dönüştürülebileceği bir ifade olmalıdır.

`P[E]` form işaretçisi öğesi erişimi, tam olarak `*(P + E)`olarak değerlendirilir. İşaretçi yöneltme işlecinin (`*`) bir açıklaması için bkz. [işaretçi yöneltme](unsafe-code.md#pointer-indirection). İşaretçi ek işlecinin (`+`) bir açıklaması için bkz. [işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic).

örnekte

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

bir işaretçi öğesi erişimi, bir `for` döngüsünde karakter arabelleğini başlatmak için kullanılır. İşlem `P[E]` tam olarak `*(P + E)`eşdeğer olduğundan, örnek de eşit olarak yazılabilir:

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

İşaretçi öğesi erişim işleci, sınırların dışı hataları denetlemez ve bir sınır dışı öğeye erişirken bu davranış tanımlı değildir. Bu, C ve ile C++aynıdır.

### <a name="the-address-of-operator"></a>Address-of işleci

*Addressof_expression* , ve sonrasında *unary_expression*gelen bir ve işareti (`&`) oluşur.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Bir tür `T` olan ve sabit bir değişken ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) olarak sınıflandırılan bir ifade `E` verildiğinde, yapı `&E` `E`tarafından verilen değişkenin adresini hesaplar. Sonucun türü `T*` ve bir değer olarak sınıflandırılır. `E` bir değişken olarak sınıflandırılmadığından, `E` salt okunurdur bir yerel değişken olarak sınıflandırıldığında veya taşınabilir bir değişken `E` içeriyorsa derleme zamanı hatası oluşur. Son durumda, bir sabit ifade ([fixed deyimidir](unsafe-code.md#the-fixed-statement)), adresini almadan önce değişkeni geçici olarak "onarmak" için kullanılabilir. [Üye erişimde](expressions.md#member-access)belirtildiği gibi, bir yapı veya bir `readonly` alanı tanımlayan sınıf için bir örnek Oluşturucu veya statik oluşturucu dışında, bu alan değişken değil değer olarak değerlendirilir. Bu nedenle, adresi alınamaz. Benzer şekilde, bir sabit adresi alınamaz.

`&` işleci, bağımsız değişkeninin kesinlikle atanmasını gerektirmez, ancak bir `&` işlemi sonrasında, işlecin uygulandığı değişken, işlemin gerçekleştiği yürütme yolunda kesin olarak atanır. Bu durumda, değişkenin doğru başlatılmasının gerçekten gerçekleşmekte olduğundan emin olmak için programcı sorumluluğundadır.

örnekte

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i`, `p`başlatmak için kullanılan `&i` işleminin ardından kesinlikle atanmış olarak değerlendirilir. Etkin `*p` atama `i`başlatır, ancak bu başlatmanın eklenmesi programcının sorumluluğundadır ve atama kaldırılırsa hiçbir derleme zamanı hatası oluşmaz.

`&` işleci için kesin atamanın kuralları, yerel değişkenlerin gereksiz şekilde başlatılmasına karşı kaçınılabilir. Örneğin, birçok harici API, API tarafından doldurulan bir yapıya bir işaretçi alır. Bu tür API çağrıları genellikle yerel bir yapı değişkeninin adresini geçer ve kural olmadan yapı değişkeninin gereksiz şekilde başlatılmasına gerek olur.

### <a name="pointer-increment-and-decrement"></a>İşaretçi artışı ve azaltma

Güvenli olmayan bir bağlamda, `++` ve `--` işleçleri ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), `void*`dışındaki tüm türlerin işaretçi değişkenlerine uygulanabilir. Bu nedenle, her işaretçi türü için `T*`, aşağıdaki işleçler örtülü olarak tanımlanmıştır:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

İşleçler, sırasıyla `x + 1` ve `x - 1`ile aynı sonuçları üretir ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)). Diğer bir deyişle, `T*`türünde bir işaretçi değişkeni için `++` işleci, değişkende bulunan adrese `sizeof(T)` ekler ve `--` işleci, değişkende bulunan adresten `sizeof(T)` çıkartır.

Bir işaretçi artıma veya azaltma işlemi işaretçi türünün etki alanını taşarsa, sonuç uygulama tanımlı olur, ancak hiçbir özel durum üretilmez.

### <a name="pointer-arithmetic"></a>İşaretçi aritmetiği

Güvenli olmayan bir bağlamda `+` ve `-` işleçleri ([toplama işleci](expressions.md#addition-operator) ve [çıkarma işleci](expressions.md#subtraction-operator)), `void*`dışındaki tüm işaretçi türlerinin değerlerine uygulanabilir. Bu nedenle, her işaretçi türü için `T*`, aşağıdaki işleçler örtülü olarak tanımlanmıştır:

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

Bir işaretçi türü `P` `T*` ve `int`, `uint`, `long`veya `ulong`türünde bir ifade `N`, ifadeler `P + N` ve `N + P` `T*` tarafından verilen adrese eklenmesinin sonucu olan `N * sizeof(T)` türündeki işaretçi değerini hesaplamak `P`. Benzer şekilde, `P - N` ifade, `P`tarafından verilen adresten `N * sizeof(T)` çıkarılmasına neden olan `T*` türündeki işaretçi değerini hesaplar.

`P` ve `Q`bir işaretçi türü `T*`olan iki ifade verildiğinde, ifade `P - Q` `P` ve `Q` tarafından verilen adresler arasındaki farkı hesaplar ve ardından bu farkı `sizeof(T)`böler. Sonucun türü her zaman `long`. Aslında `P - Q` `((long)(P) - (long)(Q)) / sizeof(T)`olarak hesaplanır.

Örneğin:

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

çıktıyı üreten:

```console
p - q = -14
q - p = 14
```

Bir işaretçi aritmetik işlemi işaretçi türünün etki alanını taşarsa, sonuç uygulama tanımlı bir biçimde kesilir, ancak hiçbir özel durum üretilmez.

### <a name="pointer-comparison"></a>İşaretçi karşılaştırması

Güvenli olmayan bir bağlamda `==`, `!=`, `<`, `>`, `<=`ve `=>` işleçleri ([ilişkisel ve tür-test işleçleri](expressions.md#relational-and-type-testing-operators)) tüm işaretçi türlerinin değerlerine uygulanabilir. İşaretçi karşılaştırma işleçleri şunlardır:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Örtük bir dönüştürme herhangi bir işaretçi türünden `void*` türüne sahip olduğundan, herhangi bir işaretçi türünün işlenenleri bu işleçler kullanılarak karşılaştırılabilir. Karşılaştırma işleçleri, iki işlenen tarafından verilen adresleri işaretsiz tamsayılar gibi karşılaştırır.

### <a name="the-sizeof-operator"></a>Sizeof işleci

`sizeof` işleci, verilen türdeki bir değişken tarafından bulunan bayt sayısını döndürür. `sizeof` bir işlenen olarak belirtilen tür bir *unmanaged_type* ([işaretçi türü](unsafe-code.md#pointer-types)) olmalıdır.

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

`sizeof` işlecinin sonucu `int`türünde bir değerdir. Önceden tanımlanmış bazı türler için `sizeof` işleci, aşağıdaki tabloda gösterildiği gibi sabit bir değer verir.


| __İfadesini__   | __Sonuç__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

Tüm diğer türler için `sizeof` işlecinin sonucu uygulama tarafından tanımlanır ve bir değer olarak sınıflandırıldı, sabit değildir.

Üyelerin bir yapıya paketleneme sırası belirtilmemiş.

Hizalama amacıyla, bir yapının başlangıcında, bir yapı içinde ve yapının sonunda adlandırılmamış doldurma olabilir. Doldurma olarak kullanılan bitlerin içeriği belirsiz.

Yapı türüne sahip bir işlenene uygulandığında, sonuç, herhangi bir doldurma dahil olmak üzere bu tür bir değişkende toplam bayt sayısıdır.

## <a name="the-fixed-statement"></a>Fixed ekstresi

Güvenli olmayan bir bağlamda, *embedded_statement* ([deyimler](statements.md)) üretimi ek bir yapı, bir taşınabilir değişkenini "onarmak" için kullanılan `fixed` bildirimine izin verir. Bu, örneğin, adresi deyimin süresi boyunca sabit kalır.

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

Her *fixed_pointer_declarator* , verilen *pointer_type* yerel bir değişkenini bildirir ve bu yerel değişkeni karşılık gelen *fixed_pointer_initializer*tarafından hesaplanan adresle başlatır. `fixed` bildiriminde belirtilen bir yerel değişkene, bu değişkenin bildiriminin sağında oluşan tüm *fixed_pointer_initializer*ve `fixed` ifadesinin *embedded_statement* erişilebilir. `fixed` ifadesiyle belirtilen yerel değişken salt okunurdur. Katıştırılmış deyimin bu yerel değişkeni (atama veya `++` ile `--` işleçleri) değiştirmeye veya `ref` ya da `out` parametresi olarak geçirmeye çalışırsa bir derleme zamanı hatası oluşur.

*Fixed_pointer_initializer* aşağıdakilerden biri olabilir:

*  "`&`" belirteci, yönetilmeyen bir tür `T`taşınabilir bir değişken ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) için bir *variable_reference* ([kesin bir atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) ile birlikte, türü `T*` örtük olarak `fixed` ifadesinde verilen işaretçi türüne dönüştürülebilir. Bu durumda, başlatıcı verilen değişkenin adresini hesaplar ve değişken `fixed` deyimin süresi boyunca sabit bir adreste kalacak şekilde garanti edilir.
*  Yönetilmeyen tür `T`öğeleriyle *array_type* bir ifadesi, `T*` türü `fixed` deyiminde verilen işaretçi türüne örtük olarak dönüştürülebilir. Bu durumda, başlatıcı dizideki ilk öğenin adresini hesaplar ve `fixed` deyimin süresi boyunca sabit bir adreste kaldığı tüm diziyi garanti eder. Dizi ifadesi null ise veya dizide sıfır öğe varsa, başlatıcı bir adresi sıfıra eşit olarak hesaplar.
*  `string`türünde bir ifade, `char*` türü `fixed` deyiminde verilen işaretçi türüne örtük olarak dönüştürülebilir. Bu durumda, başlatıcı dizedeki ilk karakterin adresini hesaplar ve tüm dize, `fixed` deyimin süresi boyunca sabit bir adreste kalacak şekilde garanti edilir. `fixed` deyiminin davranışı, dize ifadesi null ise uygulama tanımlı ' dır.
*  Taşınabilir bir değişkenin sabit boyut arabelleği üyesine başvuran bir *simple_name* veya *member_access* , sabit boyutlu arabellek üyesinin türü, `fixed` bildiriminde verilen işaretçi türüne örtük olarak dönüştürülebilir. Bu durumda, başlatıcı sabit boyutlu arabelleğin ([Ifadelerde sabit boyutlu arabellekler](unsafe-code.md#fixed-size-buffers-in-expressions)) ilk öğesine bir işaretçi hesaplar ve sabit boyutlu arabelleğin `fixed` deyimi süresince sabit bir adreste kalması garanti edilir.

*Fixed_pointer_initializer* tarafından hesaplanan her adres için `fixed` ifade, adres tarafından başvurulan değişkenin, `fixed` deyimin süresi boyunca çöp toplayıcı tarafından yeniden konumlandırma veya elden çıkarılma tabi olmamasını sağlar. Örneğin, bir *fixed_pointer_initializer* tarafından hesaplanan adres bir nesnenin alanına veya bir dizi örneğinin öğesine başvuruyorsa, `fixed` ifade, kapsayan nesne örneğinin deyimin ömrü boyunca yeniden konumlandırılıp çıkarılmadığından emin olur.

`fixed` deyimleri tarafından oluşturulan işaretçilerin Bu deyimlerin yürütülmesinden fazla devam etmez. Örneğin, `fixed` deyimleri tarafından oluşturulan işaretçiler dış API 'lere geçirildiğinde, API 'Lerin bu işaretçilerin herhangi bir bellek içermediğinden emin olmak programcı sorumluluğundadır.

Sabit nesneler yığının parçalanmasına neden olabilir (çünkü taşınamazlar). Bu nedenle, nesneler yalnızca kesinlikle gerekli olduğunda ve yalnızca en kısa sürede mümkün olduğunda düzeltilmelidir.

Örnek

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

`fixed` deyimin çeşitli kullanımlarını gösterir. İlk ifade bir statik alanın adresini düzeltir ve edinir, ikinci ifade bir örnek alanının adresini düzeltir ve edinir ve üçüncü ifade, bir dizi öğesinin adresini düzeltir ve edinir. Her durumda, değişkenlerin tamamen taşınabilir değişkenler olarak sınıflandırıldığından, bu, normal `&` işlecini kullanmanın bir hatası oluyordu.

Yukarıdaki örnekteki dördüncü `fixed` deyimleri, üçüncü için de benzer bir sonuç üretir.

`fixed` deyimin bu örneği `string`kullanır:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

Tek boyutlu dizilerin güvenli olmayan bir bağlam dizisi öğelerinde, Dizin `0` başlayıp Dizin `Length - 1`sona ermek üzere dizin sırasında artan dizin sırasında depolanır. Çok boyutlu diziler için, dizi öğeleri, en sağdaki boyutun dizinlerinin önce, ardından bir sonraki sol boyutun ve bu şekilde sol tarafında arttığı şekilde depolanır. Dizi örneği `a``p` bir işaretçi alan `fixed` bir bildiriminde, `p` ile `p + a.Length - 1` arasındaki işaretçi değerleri dizideki öğelerin adreslerini temsil eder. Benzer şekilde, `p[0]` ile `p[a.Length - 1]` arasındaki değişkenler gerçek dizi öğelerini temsil eder. Dizilerin nerede depolandığına göre, herhangi bir boyutun dizisini doğrusal hale gelse de işleyebiliriz.

Örneğin:

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

çıktıyı üreten:

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

örnekte

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

bir `fixed` ifade, bir diziyi çözmek için kullanılır, bu nedenle adresinin bir işaretçi alan bir yönteme geçirilmesi sağlanır.

Örnekte:

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

sabit bir ifade, bir yapının sabit boyutlu arabelleğini gidermek için kullanılır, bu nedenle adresi bir işaretçi olarak kullanılabilir.

Bir dize örneğini düzelterek üretilen `char*` değeri her zaman null ile sonlandırılmış bir dizeye işaret eder. Bir `s`dize örneğine `p` işaretçi alan bir fixed ifadesinde, `p + s.Length - 1` `p` arasındaki işaretçi değerleri dizedeki karakterlerin adreslerini temsil eder ve işaretçi değeri `p + s.Length` her zaman bir null karakteri gösterir (değeri `'\0'`olan karakter).

Yönetilen türdeki nesneleri sabit işaretçiler aracılığıyla değiştirmek tanımsız davranışa neden olabilir. Örneğin, dizeler sabittir olduğundan, sabit bir dizeye yönelik işaretçinin başvurduğu karakterlerin değiştirilmediğinden emin olmak programcı sorumluluğundadır.

Dizelerin otomatik olarak boş sonlandırması, "C stili" dizelerini bekleyen dış API 'Leri çağırırken özellikle kullanışlıdır. Ancak, bir dize örneğinin null karakter içermesine izin verildiğini unutmayın. Bu null karakterler varsa, null sonlandırılmış `char*`olarak kabul edildiğinde dize kesilebilir.

## <a name="fixed-size-buffers"></a>Sabit Boyutlu Arabellekler

Sabit boyutlu arabellekler, "C stili" satır içi dizileri yapıların üyeleri olarak bildirmek için kullanılır ve öncelikle yönetilmeyen API 'lerle arabirim oluşturma için faydalıdır.

### <a name="fixed-size-buffer-declarations"></a>Sabit boyutlu arabellek bildirimleri

***Sabit boyutlu arabellek*** , belirli bir türdeki değişkenlerin sabit uzunluklu arabelleği için depolamayı temsil eden bir üyedir. Sabit boyutlu bir arabellek bildirimi, belirli bir öğe türünün bir veya daha fazla sabit boyutlu arabelleğini tanıtır. Sabit boyutlu arabelleklere yalnızca struct bildirimlerinde izin verilir ve yalnızca güvenli olmayan bağlamlarda ([güvensiz bağlamlarda](unsafe-code.md#unsafe-contexts)) bulunabilir.

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

Sabit boyutlu bir arabellek bildirimi, bir dizi öznitelik ([öznitelik](attributes.md)), `new` değiştirici ([değiştiriciler](classes.md#modifiers)), dört erişim değiştiricisinin geçerli bir birleşimini ([tür parametreleri ve kısıtlamaları](classes.md#type-parameters-and-constraints)) ve bir `unsafe` değiştirici ([güvenli olmayan bağlamlar](unsafe-code.md#unsafe-contexts)) içerebilir. Öznitelikler ve değiştiriciler, sabit boyutlu arabellek bildirimi tarafından belirtilen tüm Üyeler için geçerlidir. Aynı değiştiricinin sabit boyutlu bir arabellek bildiriminde birden çok kez görünmesi hatadır.

Sabit boyutlu bir arabellek bildiriminin `static` değiştiricisini içerme izni yoktur.

Sabit boyutlu bir arabellek bildiriminin arabellek öğesi türü, bildirim tarafından tanıtılan arabelleğin öğe türünü belirtir. Buffer öğe türü önceden tanımlanmış türlerden biri olmalıdır `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`veya `bool`.

Buffer öğe türü, her biri yeni bir üye tanıtan sabit boyutlu arabellek bildirimcilerinin bir listesi tarafından izlenir. Sabit boyutlu bir arabellek bildirimci, üyeyi belirten bir tanımlayıcıdan, ardından `[` ve `]` belirteçleriyle bir sabit ifadeyle oluşur. Sabit ifade, bu sabit boyutlu arabellek bildirimci tarafından tanıtılan üye içindeki öğe sayısını belirtir. Sabit ifadenin türü `int`türüne örtük olarak dönüştürülebilir olmalıdır ve değer sıfır olmayan pozitif bir tamsayı olmalıdır.

Sabit boyutlu bir arabelleğin öğelerinin ardışık olarak bellekte düzenlenme garantisi vardır.

Birden çok sabit boyut arabelleği bildiren sabit boyutlu bir arabellek bildirimi, aynı özniteliklere ve öğe türlerine sahip tek bir sabit boyutlu arabellek bildiriminin birden çok bildirimine eşdeğerdir. Örneğin:

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

eşdeğerdir

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>İfadelerde sabit boyutlu arabellekler

Sabit boyutlu bir arabellek üyesinin üye araması ([işleçler](expressions.md#operators)), bir alanın üye aramasına tam olarak devam eder.

Sabit boyutlu arabelleğe bir *simple_name* ([tür çıkarımı](expressions.md#type-inference)) veya bir *member_access* ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) kullanılarak bir ifadede başvurulabilir.

Sabit boyutlu bir arabellek üyesine basit ad olarak başvuruluyorsa, efekt `this.I`form üye erişimi ile aynıdır; burada `I` sabit boyut arabelleği üyesidir.

Formun üye erişiminin `E.I`, `E` bir yapı türüdür ve bu yapı türünde `I` üye araması sabit boyutlu bir üyeyi tanımlarsa, `E.I` sınıflandırılan aşağıdaki gibi değerlendirilir:

*  `E.I` ifadesi güvenli olmayan bir bağlamda gerçekleşmezse, derleme zamanı hatası oluşur.
*  `E` bir değer olarak sınıflandırıldığında, bir derleme zamanı hatası oluşur.
*  Aksi takdirde, `E` taşınabilir bir değişkendir ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) ve `E.I` ifadesi bir *fixed_pointer_initializer* ([fixed Deyimi](unsafe-code.md#the-fixed-statement)) değilse, bir derleme zamanı hatası oluşur.
*  Aksi takdirde, `E` sabit bir değişkene başvuruda bulunur ve ifadenin sonucu, `E`içindeki sabit boyutlu arabellek üyesi `I` ilk öğesine yönelik bir işaretçidir. `S*`, `S` `I`öğe türü olduğu ve bir değer olarak sınıflandırılan türüdür.

Sabit boyutlu arabelleğin sonraki öğelerine, ilk öğeden işaretçi işlemleri kullanılarak erişilebilir. Dizilere erişimin aksine, sabit boyutlu bir arabelleğin öğelerine erişimi güvenli olmayan bir işlemdir ve Aralık işaretli değildir.

Aşağıdaki örnek, sabit boyutlu bir arabellek üyesine sahip bir struct bildirir ve kullanır.

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>Kesin atama denetimi

Sabit boyutlu arabellekler kesin atama denetimine ([kesin atama](variables.md#definite-assignment)) tabi değildir ve yapı türü değişkenlerinin kesin atama denetimi amacıyla sabit boyutlu arabellek üyeleri göz ardı edilir.

Sabit boyutlu bir arabellek üyesinin yapı değişkenini içeren en dıştaki, bir statik değişken, bir sınıf örneğinin örnek değişkeni veya bir dizi öğesi olduğunda, sabit boyutlu arabelleğin öğeleri otomatik olarak varsayılan değerlerine ([varsayılan değerler](variables.md#default-values)) başlatılır. Diğer tüm durumlarda, sabit boyutlu bir arabelleğin ilk içeriği tanımsızdır.

## <a name="stack-allocation"></a>Yığın ayırma

Güvenli olmayan bir bağlamda, yerel değişken bildirimi ([yerel değişken bildirimleri](statements.md#local-variable-declarations)), çağrı yığınından bellek ayıran bir yığın ayırma başlatıcısı içerebilir.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type* , yeni ayrılan konumda depolanacak öğelerin türünü gösterir ve *ifade* bu öğelerin sayısını gösterir. Birlikte getirildiğinde, gerekli ayırma boyutunu belirtir. Yığın ayırma boyutu negatif olamaz, öğelerin sayısını negatif bir değer değerlendiren bir *constant_expression* olarak belirtmek için derleme zamanı hatası olur.

Form `stackalloc T[E]` bir yığın ayırma başlatıcısı, yönetilmeyen bir tür ([işaretçi türleri](unsafe-code.md#pointer-types)) ve `E` `int`türünde bir ifade olacak şekilde `T` gerektirir. Yapı, çağrı yığınından `E * sizeof(T)` bayt ayırır ve yeni ayrılan bloğa `T*`türünde bir işaretçi döndürür. `E` negatif bir değer ise, davranış tanımsızdır. `E` sıfırsa, hiçbir ayırma yapılmaz ve döndürülen işaretçi uygulama tanımlı olur. Verilen boyutta bir blok ayırmak için yeterli kullanılabilir bellek yoksa bir `System.StackOverflowException` oluşturulur.

Yeni ayrılan belleğin içeriği tanımlı değil.

`catch` veya `finally` blokları 'nda ([TRY ifadesinde](statements.md#the-try-statement)) yığın ayırma başlatıcılarına izin verilmez.

`stackalloc`kullanılarak ayrılan belleği açık bir şekilde serbest bırakma yöntemi yoktur. İşlev üyesinin yürütülmesi sırasında oluşturulan tüm yığın ayrılmış bellek blokları, bu işlev üyesi döndürüldüğünde otomatik olarak atılır. Bu, C ve C++ uygulamalarında yaygın olarak bulunan `alloca` işlevine karşılık gelir.

örnekte

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

yığın üzerinde 16 karakterlik bir arabellek ayırmak için `IntToString` yönteminde `stackalloc` başlatıcısı kullanılır. Yöntem döndürüldüğünde arabellek otomatik olarak atılır.

## <a name="dynamic-memory-allocation"></a>Dinamik bellek ayırma

`stackalloc` işleci dışında, C# atık olmayan toplanan belleği yönetmek için önceden tanımlanmış bir yapı sağlar. Bu tür hizmetler genellikle sınıf kitaplıklarını destekleyerek veya doğrudan temel işletim sisteminden içeri aktarılarak sağlanır. Örneğin, aşağıdaki `Memory` sınıfı, temel alınan bir işletim sisteminin yığın işlevlerine nasıl erişilebileceğini göstermektedir C#:

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

`Memory` sınıfını kullanan bir örnek aşağıda verilmiştir:

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

Örnek, `Memory.Alloc` aracılığıyla 256 baytlık belleği ayırır ve bellek bloğunu 0 ' dan 255 ' e kadar artan değerlerle başlatır. Daha sonra bir 256 öğe bayt dizisi ayırır ve bellek bloğunun içeriğini bayt dizisine kopyalamak için `Memory.Copy` kullanır. Son olarak, bellek bloğu `Memory.Free` kullanılarak serbest bırakılır ve byte dizisinin içeriği konsolun çıktılardır.
