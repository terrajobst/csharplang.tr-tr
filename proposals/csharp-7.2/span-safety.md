---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485044"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>Ref benzeri türler için zaman güvenlik zorlaması derleme

## <a name="introduction"></a>Giriş

`Span<T>` ve `ReadOnlySpan<T>` gibi türlerle ilgilenirken ek güvenlik kurallarının ana nedeni, bu tür türlerin yürütme yığınına göre sınırlandırılanmasıdır.
 
`Span<T>` ve benzer türlerin yalnızca yığın türünde olması gereken iki neden vardır.

1. `Span<T>` anlam ve Aralık `(ref T data, int length)`içeren bir struct ' tır. Gerçek uygulamadan bağımsız olarak, bu tür yapıya yazma atomik olmaz. Bu tür yapının eşzamanlı "tearing", `data`eşleşmeyen `length` olasılığına neden olur. bu durum, sonuçta "güvenli" kodda GC yığın bozulması oluşmasına neden olabilecek, Aralık dışı erişimleri ve tür güvenlik ihlallerine yol açabilir.
2. `Span<T>` bazı uygulamaları, alanlarından birinde yönetilen bir işaretçi içerir. Yığın nesnelerinin alanları ve GC yığınında yönetilen bir işaretçi koymak için yöneten kod, genellikle JıT zamanında çöktüğü için yönetilen işaretçiler desteklenmez.

`Span<T>` örnekleri yalnızca yürütme yığınında var olarak kısıtlanacaksa yukarıdaki tüm sorunlar alleviated olur. 

Birleşim nedeniyle ek bir sorun ortaya çıkar. `Span<T>` ve `ReadOnlySpan<T>` örnekleri katıştırabilecek daha karmaşık veri türleri oluşturmak genellikle istenebilir. Bu tür bileşik türlerin yapılar olması ve `Span<T>`tüm tehlikeleri ve gereksinimlerinin paylaşılması gerekir. Sonuç olarak, burada açıklanan güvenlik kuralları **_ref benzeri türlerin_** tamamına uygun şekilde görüntülenmelidir.

[Taslak dil belirtimi](#draft-language-specification) , ref benzeri bir türün değerlerinin yalnızca yığında yer aldığından emin olmak için tasarlanmıştır.

## <a name="generalized-ref-like-types-in-source-code"></a>Kaynak kodunda Genelleştirilmiş `ref-like` türleri

`ref-like` yapılar, `ref` değiştirici kullanılarak kaynak kodda açıkça işaretlenir:

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

Bir yapının ref gibi belirlenmesi, yapının ref benzeri örnek alanlarına sahip olmasına olanak sağlar ve aynı zamanda yapı benzeri türlerin tüm gereksinimlerini yapısına uygulanabilir hale getirir. 

## <a name="metadata-representation-or-ref-like-structs"></a>Meta veri temsili veya ref benzeri yapılar

Ref benzeri yapılar, **System. Runtime. CompilerServices. IsRefLikeAttribute** özniteliğiyle işaretlenir.

Öznitelik, `mscorlib`gibi ortak temel kitaplıklara eklenecektir. Özniteliği kullanılabilir değilse, derleyici `IsReadOnlyAttribute`gibi diğer yerleşik isteğe bağlı özniteliklere benzer bir dahili bir olay oluşturur.

Güvenlik kurallarına alışkın olmayan derleyicilerde ref benzeri yapıların kullanılmasını engellemek için ek bir ölçü alınır (Bu özelliğin uygulandığı bir derleyiciler içerir C# ). 

Eski derleyicilerde hizmet vermeden çalışan başka iyi alternatifler olmadan, bilinen bir dizeye sahip bir `Obsolete` özniteliği, tüm ref benzeri yapılara eklenecektir. Ref benzeri türlerin nasıl kullanılacağını bilen derleyiciler, bu `Obsolete`belirli biçimini yoksayar.

Tipik meta veri gösterimi:

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

Not: eski derleyicilerde ref benzeri türlerin herhangi bir kullanımı %100 hatası verdiğinde, bunu yapmak için hedef değildir. Bu, elde etmek zordur ve kesinlikle gerekli değildir. Örneğin, dinamik kod kullanarak `Obsolete`, örneğin, yansıma aracılığıyla bir ref benzeri tür dizisi oluşturmak için her zaman bir yol olabilir.

Özellikle, Kullanıcı gerçekten bir `Obsolete` veya `Deprecated` özniteliğini ref benzeri bir türe koymak istiyorsa, `Obsolete` özniteliği birden çok kez uygulanamadığından, önceden tanımlanmış olanı yaymayan bir seçim olmayacaktır.  

## <a name="examples"></a>Örnekler:

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>Taslak dil belirtimi

Aşağıda, bu türlerin değerlerinin yalnızca yığında gerçekleşmesini sağlamak için, ref benzeri türler (`ref struct`s) için bir dizi güvenlik kuralı tanımlanıyoruz. Yereller başvuruya göre geçirilmezse, farklı, daha basit bir güvenlik kuralları kümesi mümkün olacaktır. Bu belirtim Ayrıca, ref yerellerine yönelik güvenli yeniden atamaya de izin verir.

### <a name="overview"></a>Genel Bakış

Derleme zamanında, ifadenin "güvenli-kaçış" öğesine geçmesine izin verilen kapsam kavramıyla birlikte her ifadeyle ilişkilendiyoruz. Benzer şekilde, her lvalue için, başvuruya yönelik bir başvurunun "ref-Safe-Escape" öğesine geçmesine izin verilen bir kavram vardır. Verili bir lvalue ifadesi için bu farklı olabilir.

Bunlar REF Yereller özelliğinin "güvenli hale getirme" ile benzerdir, ancak daha ayrıntılı bir özelliktir. Bir ifadenin "iadeyle güvenli" ifadesi, kapsayan metodun bir bütün olarak ne olduğunu (veya değil) bir bütün olarak bir bütün olarak, hangi kapsamdaki kapsama sahip olabileceğini (Bu kapsam ötesinde değil) kaydeder. Temel güvenlik mekanizması aşağıdaki gibi zorlanır. Bir ifadeden atama, güvenli-çıkış kapsamı S1 ile bir (lvalue) Expression E2 ile, güvenli-kaçış kapsamı S2 ile bir (lvalue) ifadesinde bir atama verildiğinde, S2 S1 'ten daha geniş bir kapsamsa, bu bir hatadır. Oluşturma işlemi sırasında, S1 ve S2 iki kapsam bir iç içe ilişkide olduğundan, yasal bir ifade her zaman ifadeyi kapsayan bazı kapsamlardan her zaman güvenli bir şekilde döndürülür.

Analiz amacı için, çözümlemenin amacına yönelik olarak yalnızca iki kapsamı ve yöntemin en üst düzey kapsamını desteklemek için gereken süre. Bunun nedeni, iç kapsamlarla başvuru benzeri değerler oluşturulamadığından ve ref yerelleri yeniden atamayı desteklemez. Ancak kurallar, ikiden fazla kapsam düzeyini destekleyebilir.

Bir ifadenin *güvenli dönüş* durumunu ve ifadelerin yasallığını yöneten kuralları izlemek için kesin kurallar.

### <a name="ref-safe-to-escape"></a>ref-güvenli-çıkış

*Ref-Safe-kaçış* , lvalue ile kaçış için bir başvuru için güvenli olduğu bir lvalue ifadesi kapsayan bir kapsamdır. Bu kapsam tüm yöntem ise, lvalue için bir başvurunun, yönteminden *geri dönmesi için güvenli* olduğunu varsayalım.

### <a name="safe-to-escape"></a>güvenle kaçış

*Güvenli-kaçış* , bir ifadeyi kapsayan ve değerin kaçış değeri için güvenli olduğu bir kapsamdır. Bu kapsam tüm yöntem ise, bir değerin yönteminden *döndürülmesi için güvenli* olduğunu varsayalım.

Türü `ref struct` olmayan bir ifade, kapsayan metodun bütünü ile *güvenli bir şekilde* döndürülür. Aksi takdirde aşağıdaki kurallara değineceğiz.

#### <a name="parameters"></a>Parametreler

Biçimsel bir parametre atayarak bir lvalue, *ref-Safe-kaçış* (başvuruya göre) ile aşağıdaki gibidir:
- Parametre bir `ref`, `out`veya `in` parametre ise, tüm metodun (ör. bir `return ref` ifadesiyle) *ref ile güvenli çıkış* olur; güvenmiyorsanız
- Parametresi bir yapı türünün `this` parametresi ise, yöntemin en üst düzey kapsamına *ref-to-kaçış* olur (ancak yöntemin tamamen değil); [Örnek](#struct-this-escape)
- Aksi takdirde parametre bir değer parametresidir ve yöntemin en üst düzey kapsamına *ref-Safe-kaçış* (yöntemin kendisinden değil).

Bir rvalue parametresinin kullanımını tanımlayan bir ifade, tüm metodun (örn. bir `return` deyimine göre) *güvenli kaçış* (değere göre) ' dır. Bu `this` parametresi için de geçerlidir.

#### <a name="locals"></a>Yerel öğeler

Yerel değişken atama bir lvalue, *ref-Safe-kaçış* (başvuruya göre) ile aşağıdaki gibidir:
- Değişken bir `ref` değişkense, *ref-Safe-kaçış* , başlatma ifadesinin *ref-Safe-kaçış* değerinden alınır; güvenmiyorsanız
- Değişken *ref-Safe-Escape* ' in bildirildiği kapsamdır.

Bir yerel değişken kullanımını belirleme rvalue olan bir ifade, *güvenli çıkış* (değere göre) ile aşağıdaki gibidir:
- Ancak yukarıdaki genel kural, türü `ref struct` olmayan yerel bir tür, kapsayan metodun tamamından daha *güvenli bir şekilde* döndürülür.
- Değişken bir `foreach` döngüsünün yineleme değişkensiyse, değişkenin *güvenli-çıkış* kapsamı, `foreach` döngüsünün ifadesinin *güvenli kaçış* ile aynıdır.
- `ref struct` türü yerel ve bildirim noktasında başlatılmamış, kapsayan metodun tamamından daha *güvenli bir şekilde* döndürülür.
- Aksi takdirde, değişkenin türü bir `ref struct` türüdür ve değişkenin bildirimi bir başlatıcı gerektirir. Değişkenin güvenli- *Çıkış* kapsamı, başlatıcısının *güvenli kaçış* ile aynıdır.

#### <a name="field-reference"></a>Alan başvurusu

Bir lvalue, `e.F`bir alana başvuru atayarak aşağıdaki gibi *ref-Safe-kaçış* (başvuruya göre) olur:
- `e` bir başvuru türü ise, tüm yöntemin *ref ile güvenli çıkış* olur; güvenmiyorsanız
- `e` bir değer türünde ise, *ref-Safe-kaçış* , `e`*ref-Safe* 'ten kaçış üzerinden alınır.

`e.F`bir alana başvuru atayarak bir rvalue, `e`*güvenli kaçış* ile aynı olan *güvenli-çıkış* kapsamına sahiptir.

#### <a name="operators-including-"></a>`?:` dahil işleçler

Kullanıcı tanımlı bir işlecin uygulaması, yöntem çağırma işlevi görür.

`e1 + e2` veya `c ? e1 : e2`gibi bir rvalue veren bir operatör için, sonucun *güvenli-kaçış* , işlecin işlenenlerinin *güvenli-kaçış* arasındaki en dar kapsamdır.  Sonuç olarak, `+e`gibi bir rvalue döndüren birli operatör için, *sonucun güvenli-* çıkış işlemi, işlenenin *güvenle kaçış* olur.

`c ? ref e1 : ref e2` gibi bir lvalue veren bir operatör için
- sonucun *ref-Safe-kaçış* , işlecin işlenenlerinin *ref-Safe-kaçış* arasındaki en dar kapsamdır.
- işlenenlerin *güvenli-çıkış* değeri kabul etmelidir ve sonuçta ortaya çıkan lvalue 'nin *güvenle kaçış* durumunda olur.

#### <a name="method-invocation"></a>Yöntem çağırma

Ref döndüren yöntem çağırma `e1.M(e2, ...)` sonucu olan bir lvalue, aşağıdaki kapsamların en küçüğü olan *ref-Safe-Escape* ' dir:
- Kapsayan metodun tamamı
- Tüm `ref` ve `out` bağımsız değişken ifadelerinin *ref-Safe-kaçış* (alıcı hariç)
- Yöntemin her `in` parametresi için, lvalue olan karşılık gelen bir ifade varsa, *ref-Safe-kaçış*, aksi halde en yakın kapsayan kapsam
- Tüm bağımsız değişken ifadelerinin (alıcı dahil) *güvenli-çıkış*

> Note: son madde işareti, gibi bir kodu işlemek için gereklidir
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> or
> ```csharp
> return ref M(sp, 0);
> ```

Yöntem çağırma `e1.M(e2, ...)` sonucu olan bir rvalue, aşağıdaki kapsamların en küçüğü ile *güvenli çıkış* olur:
- Kapsayan metodun tamamı
- Tüm bağımsız değişken ifadelerinin (alıcı dahil) *güvenli-çıkış*

#### <a name="an-rvalue"></a>Bir rvalue
Rvalue, en yakın kapsayan kapsamdan *Çıkış açısından güvenlidir* . Bu, örneğin, `d` `dynamic`türünde olduğu `M(ref d.Length)` gibi bir çağırmada meydana gelir. Ayrıca, `in` parametrelere karşılık gelen bağımsız değişkenlerin işlenmesiyle (ve belki de bu şekilde) tutarlıdır. *

#### <a name="property-invocations"></a>Özellik etkinleştirmeleri

Bir özellik çağrısı (`get` ya da `set`) Yukarıdaki kurallar tarafından temel alınan metodun Yöntem çağrısı olarak kabul edilir.

#### `stackalloc`

Bir stackalloc ifadesi, metodun en üst düzey kapsamına *güvenli-kaçış* olan bir rvalue 'dir (ancak yöntemin tamamen değil).

#### <a name="constructor-invocations"></a>Oluşturucu etkinleştirmeleri

Bir oluşturucuyu çağıran `new` ifadesi, oluşturulmakta olan türü döndürmek için kabul edilen bir yöntem çağrısı ile aynı kurallara uyar.

Buna ek olarak, *güvenli-kaçış* , nesne Başlatıcı ifadelerinin tüm bağımsız değişkenlerin/işlenenlerinin, yani Başlatıcı varsa özyinelemeli olarak *güvenli-kaçış* özelliğinden daha büyük değildir. 

#### <a name="span-constructor"></a>Span Oluşturucusu
Dil, aşağıdaki biçimdeki bir oluşturucuya sahip değil `Span<T>` bağımlıdır:

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

Böyle bir Oluşturucu, bir `ref` alanından ayırt edilemeyen alan olarak kullanılan `Span<T>` yapar. Bu belgede açıklanan güvenlik kuralları, veya .NET sürümünde C#geçerli bir yapı olmadığı `ref` alanlara bağlıdır.

#### <a name="default-expressions"></a>`default` ifadeleri

Bir `default` ifadesi, kapsayan metodun tamamından *güvenle kaçış* olur.

## <a name="language-constraints"></a>Dil kısıtlamaları

`ref` yerel değişken olmamasını ve `ref struct` türde değişken olmamasını sağlamak istiyoruz, yığın belleğine veya artık etkin olmayan değişkenlere başvuruda bulunur. Bu nedenle, aşağıdaki dil kısıtlamalarına sahip olduğumuz:

- Bir başvuru parametresi veya bir başvuru yerel veya bir parametre ya da bir `ref struct` türü yerel bir lambda veya yerel işleve yükseltilmemiş olamaz.

- Hiçbir başvuru parametresi ne de `ref struct` bir parametre bir yineleyici yöntemi veya bir `async` metodu üzerinde bir bağımsız değişken olabilir.

- Bir ref yerel veya `ref struct` türü yerel bir `yield return` deyimi veya `await` ifadesi noktasında kapsam içinde olabilir.

- `ref struct` türü, tür bağımsız değişkeni olarak veya demet türünde bir öğe türü olarak kullanılamaz.

- Bir `ref struct` türü, başka bir `ref struct`örnek alanının belirtilen türü olabileceğinden, bir alanın bildirilmeyen türü olamaz.

- `ref struct` türü bir dizinin öğe türü olamaz.

- `ref struct` bir türün değeri kutulanmamış olabilir:
  - `ref struct` türünden `object` veya `System.ValueType`türüne dönüştürme yok.
  - Bir `ref struct` türü, herhangi bir arabirim uygulamak için bildirilemez
  - `object` veya `System.ValueType` içinde bildirilmemiş ancak `ref struct` türünde geçersiz kılınamayan bir örnek yöntemi bu `ref struct` türünde bir alıcısıyla çağrılabilir.
  - Bir `ref struct` türünün örnek yöntemi, bir temsilci türüne Yöntem dönüştürmeye göre yakalanamaz.

- Ref yeniden atama `ref e1 = ref e2`için, *ref-safe* `e2`, `e1`*ref ile güvenli çıkış* olarak en az geniş bir kapsam olmalıdır.

- Ref Return ifadesinde `return ref e1`için *ref-Safe-kaçış* `e1` tüm metodun *ref-Safe-kaçış* olmalıdır. (TODO: `e1`, yöntemin tamamında *güvenli çıkış* olması veya gereksiz bir kural olması gerekir mi?)

- Bir return ifadesinin `return e1`için, `e1` *güvenli kaçış* , tüm metodun *güvenle kaçış* olmalıdır.

- Bir atama `e1 = e2`için, `e1` türü bir `ref struct` türü ise, `e2` *güvenli* *çıkış olarak* en az geniş bir kapsam olan `e1`.

- Bir `ref struct` türünün (alıcı dahil) bir `ref` veya `out` bağımsız değişkeni varsa, *güvenle kaçış* E1 ile, bir bağımsız değişken (alıcı dahil) varsa, daha dar bir *güvenli-kaçış* yöntemi olabilir. [Örnek](#method-arguments-must-match)

- Yerel bir işlev veya anonim işlev, kapsayan bir kapsamda belirtilen `ref struct` türünün yerel veya parametresine başvuramaz.

> ***Açık sorun:*** Örneğin, bir await ifadesinde bir `ref struct` türünün yığın değerini taşımamıza gerek olduğunda hata üretmemize olanak tanıyan bir kural olması gerekir, örneğin, koddaki
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>Açýkla
Bu açıklamalar ve örnekler, üzerinde kaç tane güvenlik kuralı olduğunu açıklamaya yardımcı olur

### <a name="method-arguments-must-match"></a>Yöntem bağımsız değişkenlerinin eşleşmesi gerekir
`out`, alıcı dahil bir `ref struct` `ref` parametresinin bulunduğu bir yöntemi çağırırken, tüm `ref struct` aynı yaşam süresine sahip olması gerekir. Bu gereklidir çünkü C# , yöntemin imzasında ve çağrı sitesindeki değerlerin yaşam süresi içinde bulunan bilgilere göre yaşam süresi güvenlerinin her türlü kararlarını vermelidir. 

`ref struct` `ref` parametreler varsa, bunların içerikleri etrafında takas olabilecek olasılık vardır. Bu nedenle, çağrı sitesinde bu **olası** tüm nesnelerin tümünün uyumlu olduğundan emin olunması gerekir. Dil bu şekilde zorlanmadıysa, aşağıdaki gibi hatalı kod sağlar.

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

İçeriğin hiçbiri ref ile güvenli kaçış olmadığından, alıcı üzerindeki kısıtlama gereklidir, ancak belirtilen değerleri depolayabilirler. Bu, eşleşmeyen yaşam süreleri ile aşağıdaki şekilde bir tür güvenliği deliği oluşturabileceğiniz anlamına gelir:

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>Bu kaçış yapısı
Bir örnek üyesinde `this` değeri, üyeye bir parametre olarak modellenir. Artık `struct` için `this` türü aslında yalnızca bir `class` `ref S` (örneğin, bir `S` olarak adlandırılan Üyeler için) `class / struct`. 

Henüz `this` diğer `ref` parametrelerinden farklı kaçış kurallarına sahip. Özel olarak, başka parametreler olduğu sürece ref ile güvenli kaçış değildir:

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

Bu kısıtlamanın nedeni aslında `struct` üye çağırma ile biraz daha fazla yapılır. Alıcının rvalue olduğu `struct` üyelerdeki üye çağrısına göre çalışması gereken bazı kurallar vardır. Ancak bu çok ulaşılabilir. 

Bu kısıtlamanın nedeni, arabirim çağırma ile ilgilidir. Özellikle, aşağıdaki örneğin derlenmeyeceği veya derlenmemelidir.

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

`T` `struct`olarak örneklendiği durumu göz önünde bulundurun. `this` parametresi ref-Safe-kaçış ise, `p.Get` dönüşü yığına işaret verebilir (özellikle de, Örneklenmiş türdeki `T`). Bu, dilin bir yığın konumuna `ref` döndürülürken bu örneğin derlemeye izin vermediği anlamına gelir. Diğer taraftan, `this` ref-Safe-kaçış değilse `p.Get` yığına başvuramaz ve bu nedenle döndürmek güvenli hale gelir. 

Bu, bir `struct` `this` escapability 'nin arabirimler hakkında olmasının nedendir. Bu, kesinlikle çalışmaya yapılabilir ancak bir ticareti vardır. Tasarım sonunda, arabirimleri daha esnek hale getirmek için bir süre aşağı doğru bir şekilde geldi. 

Bunu gelecekte rahat hale getirmemizi mümkün olacaktır. 

## <a name="future-considerations"></a>Gelecekteki konular

### <a name="length-one-spant-over-ref-values"></a>Ref değerleri üzerinde\<T > bir span uzunluğu
Artık geçerli olmasa da, bir değer üzerinde bir `Span<T>` örneği oluşturmak yararlı olabilir:

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

Bu özellik, daha fazla uzunluğu `Span<T>` örneklerine izin vermekle, [sabit boyutlu arabelleklere](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) yönelik kısıtlamaları kaldırırsam daha ilgi çekici bir hale gelir. 

Bu yol için bir sorun oluşursa, dil bu `Span<T>` tür örneklerin yalnızca bir yandan aşağı doğru olduğundan emin olarak buna uyum sağlayabilir. Bu, yalnızca oluşturuldukları kapsama *güvenli bir şekilde kaçış* amaçlıdır. Bu, dilin bir `ref` değerini bir `ref struct``ref struct` dönüşü veya alanı aracılığıyla kaçışmak zorunda olmamasını sağlar. Bunun yanı sıra, bu tür oluşturucuların bu şekilde `ref` bir parametre yakaladığı şekilde tanınması için de daha fazla değişiklik yapılmasını gerektirir.
