# <a name="unsafe-code"></a>Güvenli olmayan kod

C# dili, çekirdek önceki bölümde tanımlanan özellikle C ve C++ içindeki veri türü olarak işaretçiler, atlama farklıdır. Bunun yerine, C# başvurular ve atık toplayıcısı tarafından yönetilen nesneleri oluşturma yeteneği sağlar. Diğer özelliklerle birlikte bu tasarım, C# C veya C++ daha çok daha güvenli dil sağlar. Çekirdek C# dilinde, yalnızca başlatılmayan bir değişken, "Sallantıdaki" bir işaretçi veya dizi sınırlarının ötesine dizinler bir ifade olması mümkün değildir. Tüm kategoriler hataların C düzenli olarak bu tehdit ve bu nedenle C++ programları ortadan kalkar.

Neredeyse her işaretçi türü yapısı C veya C++ içinde bir başvuru türü karşılığı C# ' de olsa da yine de, burada seçeneği işaretçi türlerine erişimi olur durumlar vardır. Örneğin, temel işletim sistemi ile arabirim, bellekle eşlenen bir cihaza erişmek ve zaman açısından kritik bir algoritma uygulayan mümkün ve pratik işaretçileri erişimi olmadan olmayabilir. Bu ihtiyacı karşılamak için C# yazma olanağı sağlar ***güvenli olmayan kod***.

Güvenli olmayan kod bildirmek ve işaretçileri ve değişkenleri adresini almak için tam sayı türleri arasında dönüştürmeler gerçekleştirmek için işaretçiler üzerinde çalışacağı vb. mümkündür. Bir anlamda, güvenli olmayan kod yazma C kodu bir C# programı içinde yazma gibi olduğu kadar.

Güvenli olmayan kod aslında bir "güvenli" hem geliştiriciler hem de kullanıcılar açısından özelliğidir. Güvenli olmayan kod değiştiriciyle açıkça da işaretlenmelidir `unsafe`, böylece geliştiriciler büyük olasılıkla kullanamaz güvenli olmayan özellikler yanlışlıkla ve yürütme altyapısı, güvenli olmayan kod güvenilmeyen bir ortamda çalıştırılamayacağından emin olmak için çalışır.

## <a name="unsafe-contexts"></a>Güvenli olmayan bağlamları

C# güvenli olmayan özellikler yalnızca güvenli olmayan bağlamda kullanılabilir. Güvenli olmayan bir bağlam dahil ederek sunulan bir `unsafe` değiştiricisi bildiriminde tür veya üye veya kullanan bir *unsafe_statement*:

*  Bir sınıf, yapı, arabirim veya temsilci bildirimi içerebilir bir `unsafe` değiştiricisi, güvenli olmayan bir bağlam çalışması (sınıf, yapı veya arabirim gövdesinin dahil) bu tür bildirimi tüm metinsel kapsamını değerlendirilir.
*  Alan, yöntemi, özelliği, olay, dizin oluşturucu, işleci, örnek oluşturucusu, yok Edicisi veya statik Oluşturucu bildirimi içerebilir bir `unsafe` içinde bu üye bildirimi tüm metinsel kapsamını çalışması olarak kabul edilir güvenli olmayan bir değiştiricisi bağlamı.
*  Bir *unsafe_statement* güvenli olmayan bir bağlam içinde kullanımını sağlayan bir *blok*. Metinsel ilişkili tüm kapsamı *blok* güvenli olmayan bir bağlam kabul edilir.

İlişkili dilbilgisi üretim aşağıda gösterilmektedir.

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

Örnekte

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

`unsafe` struct bildiriminde belirtilen değiştirici güvenli olmayan bir bağlam olmasını yapı bildirimi tüm metinsel kapsamını neden olur. Bu nedenle, bildirmek olası `Left` ve `Right` işaretçi türünde alanlar. Yukarıdaki örnekte ayrıca yazılabilir

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

Burada, `unsafe` alanın bildiriminde değiştiriciler neden güvensiz bir bağlamı olarak kabul edilmesi bu bildirimleri.

Bu nedenle işaretçi türleri kullanımına izin veren bir güvenli olmayan bir bağlam oluşturma dışında `unsafe` değiştiricisi bir tür veya üye üzerinde hiçbir etkisi. Örnekte

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

`unsafe` değiştiricisi `F` yönteminde `A` metinsel kapsamını yalnızca neden `F` güvenli olmayan dil özelliklerinin kullanılabilecek güvenli olmayan bir bağlam olacak. Geçersiz kılmasını içinde `F` içinde `B`, yeniden belirtmek için gerek yoktur `unsafe` değiştiricisi--sürece, Elbette `F` yönteminde `B` kendisini güvensiz özellikleri erişmesi gerekiyor.

Bir işaretçi türü yöntemin imzasını bir parçası olduğunda durumu biraz farklıdır

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

Burada, çünkü `F`'s imzası içerir bir işaretçi türü, onu yalnızca güvenli olmayan bir bağlamda yazılabilir. Ancak, güvenli olmayan bir bağlam içinde olduğu gibi ya da sınıfın tamamı güvenli değildir, yaparak tanıtılabilir `A`, ya da dahil olmak üzere bir `unsafe` değiştirici yöntem bildiriminde içinde olduğu gibi `B`.

## <a name="pointer-types"></a>İşaretçi türleri

Güvenli olmayan bir bağlamda bir *türü* ([türleri](types.md)) olabilir bir *pointer_type* yanı *value_type* veya *reference_type* . Ancak, bir *pointer_type* içinde de kullanılabilir bir `typeof` ifade ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) dışında güvenli olmayan bir bağlam nedenle kullanım güvenli değildir.

```antlr
type_unsafe
    : pointer_type
    ;
```

A *pointer_type* olarak yazılmış bir *unmanaged_type* veya anahtar sözcüğü `void`ve ardından bir `*` belirteci:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

Önce belirtilen tür `*` bir işaretçi türü olarak adlandırılır ***başvurulan türü*** işaretçi türü. Bu değişkenin işaret ettiği işaretçi türünde bir değer türünü temsil eder.

Başvuruları (başvuru türleri değerlerinin), farklı işaretçiler çöp toplayıcı tarafından izlenmez--çöp toplayıcı işaretçiler ve bunların işaret veri olanağıyla sahiptir. Bu nedenle bir işaretçi, başvuru noktası izin verilmiyor veya bir yapı için başvuruları içeren ve grup türü bir işaretçi olmalıdır bir *unmanaged_type*.

Bir *unmanaged_type* olmayan herhangi bir tür bir *reference_type* veya türü oluşturulur ve içermiyor *reference_type* veya oluşturulan tüm düzeylerindeki tür alanları iç içe geçirme. Diğer bir deyişle, bir *unmanaged_type* aşağıdakilerden biridir:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, veya `bool`.
*  Tüm *enum_type*.
*  Tüm *pointer_type*.
*  Tüm kullanıcı tanımlı *struct_type* oluşturulan bir tür değil ve alanlarını içerir *unmanaged_type*yalnızca s.

İşaretçiler ve başvurular karıştırmak için sezgisel referents başvuruları (nesneler) işaretçiler içeren izin verilir, ancak referents işaretçiler, başvurular izin verilmeyen kuralıdır.

İşaretçi türlerinin bazı örnekler, aşağıdaki tabloda verilmiştir:

| __Örnek__ | __Açıklama__                               |
|-------------|-----------------------------------------------|
| `byte*`     | İşaretçi `byte`                             |
| `char*`     | İşaretçi `char`                             |
| `int**`     | İşaretçi işaretçisi `int`                   |
| `int*[]`    | Tek boyutlu dizi işaretçileri `int` |
| `void*`     | Bilinmeyen tür işaretçisi                       |

Belirli bir uygulama için tüm işaretçi türleri aynı büyüklük ve gösterimi olmalıdır.

C ve C++, C# ' de aynı bildirimde birden çok işaretçi bildirildiğinde aksine `*` yalnızca, temel alınan türü ile birlikte, her bir işaretçi adı bir önek noktalama işaretçisi olarak yazılır. Örneğin:

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

Türü olan bir işaretçi değeri `T*` türünde bir değişkenin adresini temsil eden `T`. İşaretçi yöneltme işleci `*` ([işaretçi yöneltmesi](unsafe-code.md#pointer-indirection)) Bu değişken erişmek için kullanılabilir. Örneğin, bir değişken verilen `P` türü `int*`, ifade `*P` gösterir `int` yer alan adreste bulunan değişkeni `P`.

Bir nesne başvurusu gibi bir işaretçi olabilir `null`. İçin yöneltme işlecini bir `null` işaretçi uygulama tanımlı davranış sonuçlanır. Bir işaretçi değeri `null` BITS tüm sıfır tarafından temsil edilir.

`void*` Türünü temsil eden bir işaretçi türü bilinmiyor. Grup türü bilinmediğinden, türünde bir işaretçi yöneltme işleci uygulanamaz `void*`, ya da herhangi bir aritmetik tür bir işaretçi üzerinde gerçekleştirilebilir. Ancak, bir işaretçi türü `void*` herhangi bir işaretçi türü (ve tersi) dönüştürme olabilir.

İşaretçi türleri için ayrı bir kategori türleridir. İşaretçi türleri seçeneğinden devralmamanızı başvuru türleri ve değer türlerinin aksine, `object` ve işaretçi türleri arasında dönüştürme işlemi yok var ve `object`. Özellikle, kutulama ve kutudan çıkarma ([kutulama ve kutudan çıkarma](types.md#boxing-and-unboxing)) işaretçiler için desteklenmez. Ancak, farklı işaretçi türleri ve işaretçi türleri ve tamsayı türleri arasında dönüştürmeler izin verilir. Bu açıklanan [işaretçi dönüşümleri](unsafe-code.md#pointer-conversions).

A *pointer_type* tür bağımsız değişkeni kullanılamaz ([oluşturulan türler](types.md#constructed-types)) ve anlam çıkarma ([anlam çıkarma](expressions.md#type-inference)) algılanan genel yöntem çağrıları başarısız bir tür bağımsız değişkeni, bir işaretçi türü.

A *pointer_type* geçici bir alan türü olarak kullanılabilir ([geçici alanları](classes.md#volatile-fields)).

İşaretçileri olarak geçirilebilir ancak `ref` veya `out` parametreleri, işaretçi yanı sıra beri tanımsız davranışa neden olabilir, böylece çağrılan yöntem döndürüldüğünde, artık var olmayan bir yerel değişken veya sabit hangi BT nesneye işaret edecek şekilde ayarlanması işaret etmek için kullanılan, artık düzeltildi. Örneğin:

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

Bir yöntem bazı türünde bir değer döndürebilir ve bu tür bir işaretçi olabilir. Örneğin, bir işaretçi bitişik dizisi ile verildiğinde `int`s, bu sırasının öğe sayısı ve bazı diğer `int` değeri, aşağıdaki yöntemi bir eşleşme olursa o sırada değeri adresini döndürür; Aksi halde döndürür`null`:

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

Güvenli olmayan bir bağlamda işaretçiler üzerinde çalışan için çeşitli yapıları kullanılabilir:

*  `*` İşleç, işaretçi yöneltmesi gerçekleştirmek için kullanılabilir ([işaretçi yöneltmesi](unsafe-code.md#pointer-indirection)).
*  `->` İşleci bir işaretçiyi bir yapı üyesi erişmek için kullanılabilir ([işaretçi üye erişimi](unsafe-code.md#pointer-member-access)).
*  `[]` İşleci, bir işaretçi dizini oluşturmak için kullanılabilir ([işaretçi öğe erişimi](unsafe-code.md#pointer-element-access)).
*  `&` İşleci, bir değişkenin adresini almak için kullanılabilir ([address-of işlecini](unsafe-code.md#the-address-of-operator)).
*  `++` Ve `--` işaretçileri artırma ve azaltma işleçleri kullanılabilir ([işaretçi artırma ve azaltma](unsafe-code.md#pointer-increment-and-decrement)).
*  `+` Ve `-` işleçleri, işaretçi aritmetik işlemleri için kullanılabilir ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)).
*  `==`, `!=`, `<`, `>`, `<=`, Ve `=>` işaretçileri Karşılaştırma işleçleri kullanılabilir ([işaretçi karşılaştırması](unsafe-code.md#pointer-comparison)).
*  `stackalloc` İşleci, çağrı yığınından bellek ayırmak için kullanılabilir ([sabit boyutlu arabellekler](unsafe-code.md#fixed-size-buffers)).
*  `fixed` Deyimi adresini alınabilmesi için bir değişkeni geçici olarak gidermek için kullanılabilir ([fixed deyimi](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Sabit ve taşınabilir değişkenleri

Address-of işlecini ([address-of işlecini](unsafe-code.md#the-address-of-operator)) ve `fixed` deyimi ([fixed deyimi](unsafe-code.md#the-fixed-statement)) iki kategoride değişkenleri bölmek: ***Değişkenler sabit*** ve ***taşınabilir değişkenleri***.

Sabit değişkenler, atık toplayıcı bir işlem tarafından etkilenmez, depolama konumları olarak yer alır. (Sabit değişkenler yerel değişkenler, değer parametreleri ve değişkenleri başvuruluyor işaretçiler tarafından oluşturulan örneklerindendir.) Öte yandan, yeniden konumlandırma veya çöp toplayıcı tarafından elden tabidir depolama konumları olarak taşınabilir değişkenleri bulunur. (Taşınabilir değişkenleri örnekleri alanları nesneler ve diziler öğelerini içerir.)

`&` İşleci ([address-of işlecini](unsafe-code.md#the-address-of-operator)) kısıtlama olmadan alınacağı sabit değişkenin adresini verir. Yeniden konumlandırma veya çöp toplayıcı tarafından elden tabi taşınabilir bir değişken olduğundan, ancak taşınabilir değişkenin adresini yalnızca kullanılarak elde edilebilir bir `fixed` deyimi ([fixed deyimi](unsafe-code.md#the-fixed-statement)) ve bu adresi yalnızca bu süre için geçerli kalır `fixed` deyimi.

Kesin bağlamında, sabit bir değişken aşağıdakilerden birini verilmiştir:

*  Bir değişken kaynaklanan bir *simple_name* ([basit adları](expressions.md#simple-names)) başvuran bir yerel değişken ya da bir değer parametresini sürece değişkeni anonim bir işlev tarafından yakalanır.
*  Bir değişken kaynaklanan bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `V.I`burada `V` , sabit bir değişken bir *struct_type*.
*  Bir değişken kaynaklanan bir *pointer_indirection_expression* ([işaretçi yöneltmesi](unsafe-code.md#pointer-indirection)) biçiminde `*P`, *pointer_member_access* ([İşaretçi üye erişimi](unsafe-code.md#pointer-member-access)) biçiminde `P->I`, veya bir *pointer_element_access* ([işaretçi öğe erişimi](unsafe-code.md#pointer-element-access)) biçiminde `P[E]`.

Diğer tüm değişkenleri taşınabilir değişkenleri olarak sınıflandırılır.

Statik alan taşınabilir bir değişken olarak sınıflandırılır unutmayın. Ayrıca bir `ref` veya `out` parametresi parametresi için belirtilen bağımsız değişkeni sabit bir değişken olsa bile taşınabilir bir değişken olarak sınıflandırılmış. Son olarak, bir işaretçinin tarafından üretilen bir değişken her zaman sabit bir değişken olarak sınıflandırıldığını unutmayın.

## <a name="pointer-conversions"></a>İşaretçi dönüşümleri

Mevcut örtük dönüştürmeler kümesini güvenli olmayan bir bağlamda ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) aşağıdaki örtük işaretçi dönüşümleri kapsayacak şekilde genişletilir:

*  Herhangi *pointer_type* türüne `void*`.
*  Gelen `null` herhangi bir değişmez değer *pointer_type*.

Ayrıca, güvenli olmayan bir bağlamda, kullanılabilir açık dönüştürmeler kümesini ([açık dönüştürmeler](conversions.md#explicit-conversions)) aşağıdaki açık işaretçi dönüşümleri kapsayacak şekilde genişletilir:

*  Herhangi *pointer_type* herhangi diğer *pointer_type*.
*  Gelen `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, veya `ulong` herhangi *pointer_type*.
*  Herhangi *pointer_type* için `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, veya `ulong`.

Son olarak, güvenli olmayan bir bağlamda, bir dizi standart örtük dönüştürmeler ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) aşağıdaki işaretçi dönüştürme içerir:

*  Herhangi *pointer_type* türüne `void*`.

İki işaretçi türleri arasında dönüştürmeler, gerçek bir işaretçi değeri hiçbir zaman değiştirin. Diğer bir deyişle, bir işaretçi türüne dönüştürmeye başka bir işaretçi tarafından verilen temel adresi üzerinde etkisi yoktur.

Elde edilen işaretçi işaret edilen türü için doğru bir şekilde hizalanmamış bir işaretçi türü diğerine dönüştürülür, sonuç referans edildi, davranış tanımlanmamıştır. Genel olarak, "düzgün hizalanmış" kavramı geçişli: tür işaretçisi, `A` tür işaretçisi için düzgün hizalanmış `B`, sırasıyla bir işaretçi türüne doğru hizalanır `C`, ardından bir işaretçi türüne `A`bir işaretçi türüne düzgün hizalanmış `C`.

Bir türü sahip bir değişken, farklı türden bir işaretçi aracılığıyla erişilir aşağıdaki örneği inceleyin:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Ne zaman bir işaretçi türü bayt, bir işaretçi değişken sonuç noktalarını düşük adresli bayta dönüştürülür. Bu değişkenin kalan baytlar işaretçilere değişkeni en fazla sonuç art arda gelen artışlarla yield. Örneğin, aşağıdaki yöntemi her sekiz bayt bir çift olarak bir onaltılık değer görüntüler:

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

Elbette, üretilen çıktı endianness üzerinde bağlıdır.

Uygulama tanımlı işaretçiler ve tamsayılar arasındaki eşlemeleri. Ancak, 32 üzerinde * ve 64-bit CPU mimarileri doğrusal bir adres alanı ile ya da tam sayı türlerinden dönüştürmeler işaretçiler genelde tam olarak dönüştürülme gibi davranır `uint` veya `ulong` değerler, sırasıyla ya da bu tam sayı türlerinden.

### <a name="pointer-arrays"></a>İşaretçi dizileri

Güvenli olmayan bir bağlamda işaretçiler dizileri oluşturulabilir. Diğer bir dizi türü için geçerli dönüşümler yalnızca bazıları, işaretçi dizilerde izin verilir:

*  Örtük bir başvuru dönüştürmesi ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) herhangi *array_type* için `System.Array` ve ayrıca bunu uyguladığı arayüzlerin işaretçi dizileri için geçerlidir. Ancak, tüm dizi öğeleri aracılığıyla erişme girişimi `System.Array` veya işaretçi türleri dönüştürülebilir değildir. Bunu uygulayan arabirimler çalışma zamanı bir özel duruma neden olur `object`.
*  Örtük ve açık dönüştürmeler başvuru ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions), [açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) bir tek boyutlu dizi türünden `S[]` için `System.Collections.Generic.IList<T>` ve Genel temel arabirimlerinden işaretçi türleri, tür bağımsız değişkeni olarak kullanılamaz ve işaretçi olmayan türlerin işaretçi türünden hiçbir dönüştürme hiçbir zaman işaretçi dizileri için geçerlidir.
*  Açık bir başvuru dönüştürmesi ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) öğesinden `System.Array` ve arabirimler için uygular *array_type* işaretçi dizileri için geçerlidir.
*  Açık bir başvuru Dönüşümleri ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) öğesinden `System.Collections.Generic.IList<S>` ve bunun temel arabirimleri için bir tek boyutlu dizi türü `T[]` işaretçi türleri olamaz bu yana hiçbir zaman işaretçi dizileri için geçerlidir kullanılan farklı tür bağımsız değişkenleri ve işaretçi olmayan türlerin işaretçi türünden dönüştürme yok.

Bu kısıtlamalar için genişletme anlamına `foreach` diziler üzerinden deyimi açıklanan [foreach deyimi](statements.md#the-foreach-statement) işaretçi dizileri için uygulanamaz. Bunun yerine, formun foreach deyimi

```csharp
foreach (V v in x) embedded_statement
```

Burada türünü `x` biçiminde bir dizi türü `T[,,...,]`, `N` eksi 1 dimensions sayısı ve `T` veya `V` işaretçi türünde, iç içe geçmiş for döngüleri kullanarak aşağıdaki gibi genişletilir:

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

Değişkenleri `a`, `i0`, `i1`,..., `iN` görünür veya erişilebilir olmayan `x` veya *embedded_statement* veya programın başka bir kaynak kodu. Değişken `v` katıştırılmış deyiminde salt okunur. Açık bir dönüştürme ise ([işaretçi dönüşümleri](unsafe-code.md#pointer-conversions)) öğesinden `T` (öğe türü) için `V`, bir hata oluşturulur ve başka bir adım alınır. Varsa `x` değerine sahip `null`, `System.NullReferenceException` çalışma zamanında oluşturulur.

## <a name="pointers-in-expressions"></a>İşaretçi ifadeleri

Güvensiz bir bağlamı bir ifade bir işaretçi türünün bir sonucunun hızlanmasını sağlayabilir, ancak isteğe bağlı olarak güvensiz bir bağlamı dışında bir derleme zamanı hatası için bir işaretçi türü olarak ifade bulunmamaktadır. Varsa güvensiz bir bağlamı dışında kesin bağlamında, bir derleme zamanı hatası oluşur *simple_name* ([basit adları](expressions.md#simple-names)), *member_access* ([üye erişimi ](expressions.md#member-access)), *invocation_expression* ([çağrı ifadeleri](expressions.md#invocation-expressions)), veya *element_access* ([öğe erişimi](expressions.md#element-access)) bir işaretçi türü.

Güvenli olmayan bir bağlamda *primary_no_array_creation_expression* ([birincil ifadeler](expressions.md#primary-expressions)) ve *unary_expression* ([birliişleçler](expressions.md#unary-operators)) Üretim aşağıdaki ek yapıları izin ver:

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

Bu yapılar, aşağıdaki bölümlerde açıklanmıştır. Öncelik ve ilişkisellik güvenli olmayan işleçlerinin dilbilgisi tarafından kapsanan.

### <a name="pointer-indirection"></a>İşaretçi yöneltmesi

A *pointer_indirection_expression* oluşur yıldız işareti (`*`) tarafından izlenen bir *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

Birli `*` işleci işaretçi yöneltmesi gösterir ve bir işaretçinin işaret ettiği değişken almak için kullanılır. Değerlendirme sonucu `*P`burada `P` bir işaretçi türü ifade `T*`, türünde bir değişken `T`. Birli uygulamak için bir derleme zamanı hata `*` türündeki bir ifade işlecine `void*` veya işaretçi türünde olmayan bir ifade.

Birli etkisi `*` işleci bir `null` işaretçisidir uygulama tanımlı. Özellikle, bu işlemi oluşturan bir garanti yoktur bir `System.NullReferenceException`.

Geçersiz bir değer işaretçisi, birli davranışını atandıysa `*` işleci tanımsızdır. Birli tarafından bir işaretçiye başvuruluyor için geçersiz değerler arasında `*` işleci işaret türü için uygunsuz bir şekilde hizalanmış bir adresi olan (örnekte bakın [işaretçi dönüşümleri](unsafe-code.md#pointer-conversions)) ve sonra bir değişkenin adresi geçerlilik süresi.

Belirli atama onayına çözümleme amacıyla bir değişken üretilen biçiminde bir ifade değerlendirmesi tarafından `*P` başlangıçta atanan olarak kabul edilir ([değişkenleri başlangıçta atanan](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>İşaretçi üye erişimi

A *pointer_member_access* oluşan bir *primary_expression*ve ardından bir "`->`" belirteci, izleyen bir *tanımlayıcı* ve isteğe bağlı bir *type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

Bir işaretçi üye erişimi formun içinde `P->I`, `P` bir işaretçi türü ifade dışında olmalıdır `void*`, ve `I` erişilebilir bir türün üyesi belirtmek gerekir `P` noktaları.

Bir işaretçi üye erişimi formun `P->I` tam olarak değerlendirilir `(*P).I`. İşaretçi yöneltme işleci bir açıklaması (`*`), bkz: [işaretçi yöneltmesi](unsafe-code.md#pointer-indirection). Üye erişimi işleci bir açıklaması (`.`), bkz: [üye erişimi](expressions.md#member-access).

Örnekte

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

`->` işleci alanlarına erişmek ve işaretçi üzerinden bir yapının bir yöntem çağırmak için kullanılır. Çünkü işlemi `P->I` tam olarak değerine eşdeğer olan `(*P).I`, `Main` yöntemi eşit derecede iyi yazılı:

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

### <a name="pointer-element-access"></a>İşaretçi öğe erişimi

A *pointer_element_access* oluşan bir *primary_no_array_creation_expression* içine bir ifadenin ardından "`[`"ve"`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

Bir işaretçi öğe erişimi formun içinde `P[E]`, `P` bir işaretçi türü ifade dışında olmalıdır `void*`, ve `E` örtük olarak dönüştürülebilir bir ifade olmalıdır `int`, `uint`, `long`, veya `ulong`.

Bir işaretçi öğe erişimi formun `P[E]` tam olarak değerlendirilir `*(P + E)`. İşaretçi yöneltme işleci bir açıklaması (`*`), bkz: [işaretçi yöneltmesi](unsafe-code.md#pointer-indirection). İşaretçi Toplama işleci bir açıklaması (`+`), bkz: [işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic).

Örnekte

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

bir işaretçi öğe erişimi karakter arabelleğine başlatmak için kullanılan bir `for` döngü. Çünkü işlemi `P[E]` tam olarak değerine eşdeğer olan `*(P + E)`, örnek eşit derecede iyi yazılmış:

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

İşaretçi öğesi erişim işleci işlemleri denetlemez hataları ve erişirken davranışı bir öğe işlemleri tanımsız olur. C ve C++ ile aynı olmasıdır.

### <a name="the-address-of-operator"></a>Address-of işleci

Bir *addressof_expression* oluşur ve işareti (`&`) tarafından izlenen bir *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Bir ifade verilen `E` türü olduğu `T` ve sabit bir değişken olarak sınıflandırıldığını ([sabit ve taşınabilir değişkenleri](unsafe-code.md#fixed-and-moveable-variables)), yapı `&E` tarafındanverilendeğişkeninadresihesaplar`E`. Sonuç türü olan `T*` ve bir değer olarak sınıflandırılır. Bir derleme zamanı hatası oluşur `E` durumunda bir değişken olarak sınıflandırılır değil `E` salt okunur yerel bir değişken, Sınıflandırılmamış veya `E` taşınabilir bir değişkeni gösterir. Son durumda, bir fixed deyimi ([fixed deyimi](unsafe-code.md#the-fixed-statement)) geçici olarak "değişkenin adresini edinme önce düzeltmek için" kullanılabilir. Bölümünde belirtildiği [üye erişimi](expressions.md#member-access), bir örnek oluşturucusu veya bir yapı ya da tanımlayan sınıf için statik Oluşturucu dışında bir `readonly` alan, bu alan bir değer, bir değişken değerlendirilir. Bu nedenle, adresi alınamaz. Benzer şekilde, bir sabit adresi alınamaz.

`&` İşleci kesinlikle atanmış, ancak aşağıdaki bağımsız değişkeni gerekli olmadığı bir `&` işlemi, değişkenin işlecin uygulandığı değerlendirilir işleminin gerçekleştiği yürütme yolu kesinlikle atanmış. Bu sorumluluk değişkeninin doğru başlatmanın emin olmak için programcının kararına aslında bu durumda gerçekleşmesi olur.

Örnekte

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

`i` Aşağıdaki kesinlikle atanmış olarak kabul edilir `&i` başlatmak için kullanılan işlem `p`. Atamayı `*p` yürürlükte başlatır `i`, ancak eklenmesi bu başlatma, programcının sorumluluğundadır ve atama kaldırıldıysa herhangi bir derleme zamanı hata oluşacak.

İçin belirli atama onayına kurallarına `&` işleci mevcut sağlayacak şekilde yerel değişkenlerin yedekli başlatma önlenebilir. Örneğin, çok sayıda dış API API tarafından doldurulan bir yapıya bir işaretçi alır. Bu API'lere giden çağrıların yerel yapı değişkenin adresini pass ve kural yapı değişkenin yedekli başlatma gerekli olacaktır.

### <a name="pointer-increment-and-decrement"></a>İşaretçi artırma ve azaltma

Güvenli olmayan bir bağlamda `++` ve `--` işleçleri ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) işaretçiye uygulanabilir değişkenleri tüm türlerin `void*`. Bu nedenle, her bir işaretçi türü için `T*`, aşağıdaki işleçleri örtük olarak tanımlanır:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

İşleçler olarak aynı sonuçlar `x + 1` ve `x - 1`sırasıyla ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)). Diğer bir deyişle, bir işaretçi değişkeninin türü için `T*`, `++` işleci ekler `sizeof(T)` değişkeninde bulunan adresine ve `--` işleci çıkarır `sizeof(T)` değişkeninde bulunan adresinden.

Bir işaretçi artırma veya azaltma işlemi taşmaları işaretçi türünün etki alanını, sonucu uygulama tanımlanır, ancak hiçbir özel durum oluşturulur.

### <a name="pointer-arithmetic"></a>İşaretçi aritmetiği

Güvenli olmayan bir bağlamda `+` ve `-` işleçleri ([Toplama işleci](expressions.md#addition-operator) ve [çıkarma işleci](expressions.md#subtraction-operator)) tüm İşaretçi türlerinin değerlerini uygulanabilir `void*`. Bu nedenle, her bir işaretçi türü için `T*`, aşağıdaki işleçleri örtük olarak tanımlanır:

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

Bir ifade verilen `P` bir işaretçi türünün `T*` ve ifade `N` türü `int`, `uint`, `long`, veya `ulong`, ifadeleri `P + N` ve `N + P` işlem İşaretçi türünün değerini `T*` sonuçlarının eklemesini `N * sizeof(T)` tarafından verilen adresine `P`. Benzer şekilde, ifade `P - N` türü işaretçi değeri hesaplar `T*` sonuçlarının arasındaki çıkarma işleminin `N * sizeof(T)` tarafından verilen adresinden `P`.

Verilen iki deyim `P` ve `Q`, bir işaretçi türünün `T*`, ifade `P - Q` tarafından verilen adresleri arasındaki farkı hesaplar `P` ve `Q` ve ardından bu fark tarafından böler`sizeof(T)`. Her zaman bir sonuç türü olan `long`. Aslında, `P - Q` olarak hesaplanan `((long)(P) - (long)(Q)) / sizeof(T)`.

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

Bu çıktıyı üretir:

```
p - q = -14
q - p = 14
```

Etki alanı işaretçi türü bir işaretçi aritmetik işlemi taşıyor, sonucu bir uygulama tarafından tanımlanan biçimde kesilmiş, ancak hiçbir özel durum oluşturulur.

### <a name="pointer-comparison"></a>İşaretçi karşılaştırması

Güvenli olmayan bir bağlamda `==`, `!=`, `<`, `>`, `<=`, ve `=>` işleçleri ([ilişkisel ve tür testi işleçleri](expressions.md#relational-and-type-testing-operators)) tüm değerlere uygulanan İşaretçi türleri. İşaretçi Karşılaştırma işleçleri şunlardır:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Örtük bir dönüştürme için herhangi bir işaretçi türü bulunduğundan `void*` Bu işleçleri kullanarak herhangi bir işaretçi türü işlenen türü karşılaştırılabilir. Karşılaştırma işleçleri, işaretsiz tamsayılar değilmiş gibi iki işlenen tarafından belirtilen adresi karşılaştırın.

### <a name="the-sizeof-operator"></a>Sizeof işleci

`sizeof` İşleci tarafından verilen bir türde bir değişken kapladığı bayt sayısını döndürür. İşleneni olarak belirtilen tür `sizeof` olmalıdır bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

Sonucu `sizeof` işleci türünde bir değer, `int`. Belirli türlerini önceden tanımlanmış `sizeof` aşağıdaki tabloda gösterildiği gibi bir sabit değer işlecini verir.


| __İfade__   | __Sonuç__ |
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

Diğer tüm türleri için sonucunu `sizeof` işleci uygulama tarafından tanımlanır ve sabit bir değer sınıflandırılır.

Üyeleri bir yapı içinde paketlenir sırasını belirtilmemiş.

Hizalama amacıyla olabilir adlandırılmamış bir yapı içinde bir yapının başında ve sonunda, struct doldurma. Doldurma olarak kullanılan bit içeriğini belirsiz.

Yapı türüne sahip bir işleç uygulandığında, sonucu herhangi doldurma dahil olmak üzere, bu türde bir değişken bayt toplam sayısıdır.

## <a name="the-fixed-statement"></a>Fixed deyimi

Güvenli olmayan bir bağlamda *embedded_statement* ([deyimleri](statements.md)) üretim izin veren bir ek yapı `fixed` "taşınabilir bir değişken düzeltmek için" kullanılan ifadesi gibi Adres deyim süresi boyunca sabit kalır.

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

Her *fixed_pointer_declarator* yerel bir değişken bildirir verilen *pointer_type* ve karşılık gelen tarafından hesaplanan adresiyle bu yerel değişken başlatır *fixed_ pointer_initializer*. Bildirilen yerel değişken bir `fixed` deyimi, tüm erişilebilir *fixed_pointer_initializer*sağındaki değişkenin bildirimi ve içinde gerçekleşen s *embedded_statement* , `fixed` deyimi. Tarafından bildirilen yerel değişken bir `fixed` deyimi salt okunur kabul edilir. Gömülü deyim bu yerel değişken değiştirme girişiminde bulunursa, bir derleme zamanı hatası oluşur (atama aracılığıyla veya `++` ve `--` işleçleri) veya olarak başarılı bir `ref` veya `out` parametresi.

A *fixed_pointer_initializer* aşağıdakilerden biri olabilir:

*  Belirteç "`&`" arkasından bir *variable_reference* ([belirli atama onayına belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) taşınabilir bir değişkene ([sabit ve taşınabilir değişkenleri](unsafe-code.md#fixed-and-moveable-variables)) yönetilmeyen bir türü `T`, türü sağlanan `T*` verilen işaretçi türüne açıkça dönüştürülemez `fixed` deyimi. Bu durumda, belirtilen değişkenin adresi Başlatıcı hesaplar ve değişkeni sabit bir adreste süresi boyunca kalan garanti `fixed` deyimi.
*  Bir ifade bir *array_type* nespravovaným typem öğelerini `T`, türü sağlanan `T*` verilen işaretçi türüne açıkça dönüştürülemez `fixed` deyimi. Bu durumda, dizideki ilk öğe adresini Başlatıcı hesaplar ve tüm dizi sabit bir adreste süresi boyunca kalan garanti `fixed` deyimi. Dizi ifadesi null ise veya dizi hiç öğe varsa, başlatıcı adresi eşit sıfıra hesaplar.
*  Türündeki bir ifade `string`, türü sağlanan `char*` verilen işaretçi türüne açıkça dönüştürülemez `fixed` deyimi. Bu durumda, ilk karakterin dizede adresi Başlatıcı hesaplar ve tüm dize sabit bir adreste süresi boyunca kalan garanti `fixed` deyimi. Davranışını `fixed` deyimi uygulama tanımlı dize ifadesi null ise.
*  A *simple_name* veya *member_access* sabit boyutlu arabellek üyenin türü verilen işaretçi türüne örtük olarak dönüştürülebilir sağlanan taşınabilir bir değişken bir sabit boyutlu arabellek üyesi başvuran içinde `fixed` deyimi. Bu durumda, başlatıcı bir sabit boyutlu arabellek ilk öğesinin işaretçisi hesaplar ([boyutlu arabellekler ifadelerde sabit](unsafe-code.md#fixed-size-buffers-in-expressions)), ve sabit boyutlu arabellek süresiboyuncasabitbiradrestekalmasınagaranti`fixed`deyimi.

Tarafından hesaplanan her adresi için bir *fixed_pointer_initializer* `fixed` deyimi adres tarafından başvurulan değişkeni yeniden konumlandırma veya çöp toplayıcı tarafından elden çıkarma süresince tabi değildir olmasını sağlar `fixed` deyimi. Örneğin, adresi tarafından hesaplanan bir *fixed_pointer_initializer* bir nesnenin bir alan veya bir dizi örnek, bir öğe başvuru `fixed` deyimi içeren bir nesne örneği değil konumlandırıldı güvence altına alır veya deyim ömrü boyunca elden.

Bu işaretçi tarafından oluşturulan emin olmak için programcının sorumluluğundadır `fixed` deyimleri Bu deyimler yürütme hayatta değil. Örneğin, ne zaman işaretçiler tarafından oluşturulan `fixed` deyimleri, dış API'lerine geçirilir, bu API'leri bu işaretçiler bellek korunmasını sağlamak için programcının sorumluluğundadır.

(Bunlar taşınamıyor çünkü) sabit nesneler yığın parçalanmasına neden olabilir. Bu nedenle, nesneler yalnızca gerçekten gerekli olduğunda düzeltilmelidir ve ardından yalnızca zaman mümkün olan en kısa miktarı için.

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

bazı kullanımlarını gösterir `fixed` deyimi. İlk ifade düzeltir ve adresini statik alanı alır, ikinci ifade düzeltir ve örnek alan adresini alır ve üçüncü deyim düzeltir ve bir dizi öğesinin adresini alır. Her durumda, normal kullanmak için bir hata bulunması gereken `&` değişkenleri tüm taşınabilir değişkenleri olarak sınıflandırılan beri işleci.

Dördüncü `fixed` deyimi yukarıdaki örnekte, üçüncü benzer bir sonuç üretir.

Bu örneği `fixed` deyimi kullandığı `string`:

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

Güvenli olmayan bir bağlamda tek boyutlu diziler, dizi öğeleri ile dizininden başlayarak, artan dizin sırasıyla depolanır `0` ve dizin ile biten `Length - 1`. Çok boyutlu diziler, dizi öğeleri dizinleri en sağdaki boyutun ilk olarak, artan şekilde depolanan sonra sonraki boyut vb. sola kaldı. İçinde bir `fixed` bir işaretçi ifadesi `p` array örneğine `a`, işaretçi değerleri arasında değişen `p` için `p + a.Length - 1` dizideki öğelerin adresleri temsil eder. Benzer şekilde, arasında değişen değişkenleri `p[0]` için `p[a.Length - 1]` gerçek dizi öğeleri temsil eder. İşlevmiş gibi doğrusal diziler depolandığı yolu verildiğinde, biz herhangi bir boyut dizisi davranabilirsiniz.

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

Bu çıktıyı üretir:

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

Örnekte

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

bir `fixed` deyimi adresini bir işaretçi alır bir yönteme geçirilmesi için bir dizi düzeltmek için kullanılır.

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

fixed deyimi, bir yapının bir sabit boyutlu arabellek adresini bir işaretçi olarak kullanılacak şekilde düzeltmek için kullanılır.

A `char*` değer üretilmediğini bir dize örneği her zaman bir boş sonlandırılmış dizeye işaret düzelterek. Bir işaretçi bir fixed deyimi içinde `p` dize örneğine `s`, işaretçi değerleri arasında değişen `p` için `p + s.Length - 1` adresleri karakter dizesi ve işaretçi değerini temsil eden `p + s.Length` her zaman null bir karaktere işaret eder (değer karakteriyle `'\0'`).

Sabit işaretçileri aracılığıyla yönetilen türü değiştirme nesneler, sonuçlar tanımsız davranışa olabilir. Dizeleri sabit olduğundan, örneğin, bu karakterlerin sabit bir dizeye bir işaretçi tarafından başvurulan değiştirilmedi emin olmak için programcının sorumluluğundadır.

Otomatik null sonlandırma dizeleri "C-style" dizeleri bekleyen dış API'lar çağırırken özellikle kullanışlıdır. Ancak, bir dize örneği null karakterleri içeren izni olduğunu unutmayın. Bu tür null karakterler varsa, dize bir null ile sonlandırılmış kabul edilir, kesilmiş görünür `char*`.

## <a name="fixed-size-buffers"></a>Sabit boyutlu arabellekler

Sabit boyutlu arabellekler "C style" satır içi dizi yapılar üye olarak bildirmek için kullanılır ve yönetilmeyen API'ler ile arabirim için öncelikle yararlıdır.

### <a name="fixed-size-buffer-declarations"></a>Sabit boyutlu arabellek bildirimleri

A ***sabit boyutlu arabellek*** depolama için belirli bir türde değişken bir sabit uzunluk arabelleği temsil eden bir üyesidir. Bir sabit boyutlu arabellek bildiriminde belirtilen öğe türünün bir veya daha fazla sabit boyutlu arabellekler tanıtır. Sabit boyutlu arabellekler yalnızca yapı bildirimlerinde izin verilir ve yalnızca güvenli bağlamlarda oluşabilir ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)).

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

Sabit boyutlu arabellek bildirimi öznitelikleri kümesi içerebilir ([öznitelikleri](attributes.md)), `new` değiştiricisi ([değiştiriciler](classes.md#modifiers)), geçerli bir bileşimin dört erişim değiştiricileri ([türü parametreleri ve kısıtlamalar](classes.md#type-parameters-and-constraints)) ve bir `unsafe` değiştirici ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)). Öznitelikler ve değiştiriciler tüm sabit boyutlu arabellek bildirim tarafından bildirilen üyeler için geçerlidir. Aynı değiştiricisi bir sabit boyutlu arabellek bildiriminde birden çok kez görünmesi için bir hatadır.

Sabit boyutlu arabellek bildirimi dahil etmek için izin verilmiyor `static` değiştiricisi.

Arabellek öğe türü sabit boyutlu arabellek bildirimi arabellekleri öğe türü bildirim tarafından tanıtılan belirtir. Arabellek öğe türü, önceden tanımlanmış türlerden biri olmalıdır `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, veya `bool`.

Arabellek öğe türü, her biri yeni bir üye tanıtır listesini sabit boyutlu arabellek Bildirimciler tarafından izlenir. İçine bir sabit ifade ardından üye adları bir tanımlayıcının bir sabit boyutlu arabellek bildirimci oluşur `[` ve `]` belirteçleri. Sabit ifade, sabit boyutlu arabellek bildirimci tarafından sunulan üye içindeki öğelerin sayısını gösterir. Sabit ifade türü türüne örtük olarak dönüştürülebilir olmalıdır `int`, ve sıfır olmayan pozitif bir tamsayı değeri olmalıdır.

Sabit boyutlu arabellek öğelerini sırayla bellekte düzenlendiği garanti edilir.

Birden çok sabit boyutlu arabellekler bildiren bir sabit boyutlu arabellek bildirimi, aynı özniteliklerle ve öğe türleri tek bir sabit boyutlu arabellek bildirimin birden fazla bildirimi eşdeğerdir. Örneğin:

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

Üye araması ([işleçleri](expressions.md#operators)) sabit boyutlu arabellek üye tam olarak bir alanın üye araması gibi devam eder.

Kullanarak bir ifade bir sabit boyutlu arabellek başvurulabilen bir *simple_name* ([anlam çıkarma](expressions.md#type-inference)) veya bir *member_access* ([derleme zamanı denetimi dinamik aşırı yükleme çözünürlüğü](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Sabit boyutlu arabellek üyesi basit bir ad başvurulduğunda etkisi biçiminde bir üye erişimi aynıdır `this.I`burada `I` sabit boyutlu arabellek üyesidir.

Üye erişimi formun içinde `E.I`, `E` bir yapı türü ve üye araması için `I` yapı türünün sabit boyutlu üyesi ardından belirler, `E.I` olduğu bir sınıflandırılmış gibi değerlendirilir:

*  İfade `E.I` oluşmaz güvenli olmayan bir bağlamda, bir derleme zamanı hatası oluşur.
*  Varsa `E` bir derleme zamanı hatası oluşur, bir değer olarak sınıflandırılır.
*  Aksi takdirde `E` taşınabilir bir değişken ([sabit ve taşınabilir değişkenleri](unsafe-code.md#fixed-and-moveable-variables)) ve ifade `E.I` değil bir *fixed_pointer_initializer* ([düzeltildi deyimi](unsafe-code.md#the-fixed-statement)), bir derleme zamanı hatası oluşur.
*  Aksi takdirde, `E` sabit bir değişkene başvuruyor ve sonuç ifadesi bir sabit boyutlu arabellek üyenin ilk öğesinin işaretçisi `I` içinde `E`. Sonuç türünde `S*`burada `S` öğe türü `I`ve bir değer olarak sınıflandırılır.

Sabit boyutlu arabellek sonraki öğeler ilk öğeye işaretçi işlemlerini kullanarak erişilebilir. Dizilere erişim, farklı bir sabit boyutlu arabellek öğelere erişim güvenli olmayan bir işlemdir ve kontrol aralığı değil.

Aşağıdaki örnek, bildirir ve sabit boyutlu arabellek üyesi bir yapı kullanır.

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

### <a name="definite-assignment-checking"></a>Belirli atama onayına denetleniyor

Sabit boyutlu arabellekler olmayan belirli atama onayına denetimi tabi ([belirli atama onayına](variables.md#definite-assignment)), ve sabit boyutlu arabellek üyeleri, struct türüne değişkenlerinin denetimi özelliğine belirli atama amacıyla yoksayılır.

En dıştaki içeren yapı değişkeni bir sabit boyutlu arabellek üyesinin, bir statik değişken, bir sınıf örneği ya da bir dizi öğesine bir örnek değişkeni sabit boyutlu arabellek öğeleri varsayılan değerlerine otomatik olarak başlatılır ([Varsayılan değerler](variables.md#default-values)). Diğer durumlarda, ilk sabit boyutlu arabellek içeriği tanımsızdır.

## <a name="stack-allocation"></a>Yığın ayırma

Bir yerel değişken bildiriminde güvenli olmayan bir bağlamda ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) çağrı yığından belleği ayırır bir yığın ayırma Başlatıcı içerebilir.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type* yeni ayrılan bir konumda depolanan öğelerin türünü belirtir ve *ifade* bu öğe sayısını gösterir. Birlikte ele alındığında, bunlar için gerekli ayırma boyutu belirtin. Yığın ayırma boyutu negatif olamaz, bu öğeleri olarak sayısını belirtmek için bir derleme zamanı hatası olduğu bir *constant_expression* negatif bir değer olarak değerlendirir.

Bir yığın ayırma Başlatıcı formun `stackalloc T[E]` gerektirir `T` nespravovaným typem olmasını ([işaretçi türleri](unsafe-code.md#pointer-types)) ve `E` türündeki bir ifade olmasını `int`. Yapı ayırır `E * sizeof(T)` çağrısından bayt yığın ve türünde bir işaretçi döndürür `T*`, yeni ayrılan bloğu için. Varsa `E` tanımsız bir davranıştır sonra negatif bir değer olan. Varsa `E` sıfırsa, ardından hiçbir ayırma yapılan ve döndürülen işaretçi uygulama tanımlanır. Verilen boyuta bloğunu ayırmak yeterli bellek yoksa bir `System.StackOverflowException` oluşturulur.

Yeni ayrılan bellek içeriğini tanımsızdır.

Yığın ayırma başlatıcıları içinde verilmez `catch` veya `finally` blokları ([try deyimi](statements.md#the-try-statement)).

Açıkça kullanılarak ayrılmış belleği boşaltmak için bir yolu yoktur `stackalloc`. Bu işlev üyesi döndürdüğünde işlevi üyesi yürütülmesi sırasında oluşturulan tüm yığın tarafından ayrılan bellek blokları otomatik olarak atılır. Bu karşılık gelir `alloca` işlev, genellikle C ve C++ uygulamalarında bulunan bir uzantı.

Örnekte

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

bir `stackalloc` Başlatıcı kullanılan `IntToString` yöntemi bir yığında 16 karakter arabelleği ayrılamadı. Yöntem döndürüldüğünde arabellek otomatik olarak atılır.

## <a name="dynamic-memory-allocation"></a>Dinamik bellek ayırma

Dışında `stackalloc` işleci, C# önceden tanımlanmış hiçbir yapıları olmayan atık toplanan bellek yönetmek için sağlar. Bu tür Hizmetleri genellikle sınıf kitaplıkları destekleyerek sağlanan veya doğrudan alttaki işletim sisteminden alınan. Örneğin, `Memory` sınıfı aşağıdaki nasıl temel işletim sistemi yığın işlevlerini C# ' den erişilebilen gösterilmektedir:

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

Kullanan bir örnek `Memory` sınıfı aşağıda verilmiştir:

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

Örneğin 256 bayt ile bellek ayırır `Memory.Alloc` ve bellek bloğu 0 ile 255 artırma değerlerle başlatır. Ardından 256 öğe bayt dizisi ayırır ve kullandığı `Memory.Copy` bellek bloğu içeriğini bayt dizisine kopyalamak için. Son olarak, bellek bloğu kullanılarak serbest `Memory.Free` ve bayt dizisinin içeriğini konsola.
