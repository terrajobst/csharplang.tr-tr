---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122942"
---
# <a name="ranges"></a>Aralıklar

## <a name="summary"></a>Özet

Bu özellik, `System.Index` ve `System.Range` nesneleri oluşturmak ve çalışma zamanında koleksiyonlara/dilimlemek için onları kullanmak üzere iki yeni işleç sunmaya yönelik bir özelliktir.

## <a name="overview"></a>Genel Bakış

### <a name="well-known-types-and-members"></a>İyi bilinen türler ve Üyeler

Yeni sözdizimsel formları `System.Index` ve `System.Range`için kullanmak üzere, hangi sözdizimsel biçimlerinin kullanıldığına bağlı olarak, yeni iyi bilinen türler ve Üyeler gerekli olabilir.

"Hat" işlecini (`^`) kullanmak için aşağıdakiler gereklidir

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

`System.Index` türünü bir dizi öğesi erişiminde bağımsız değişken olarak kullanmak için aşağıdaki üye gereklidir:

```csharp
int System.Index.GetOffset(int length);
```

`System.Range` için `..` söz dizimi, aşağıdaki üyelerin bir veya daha fazla `System.Range` türünü de gerektirir:

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

`..` sözdizimi, her ikisinin de ya da bağımsız değişkenlerinin hiçbirinin olmaması için izin verir. Bağımsız değişken sayısından bağımsız olarak `Range` Oluşturucu `Range` söz dizimini kullanmak için her zaman yeterlidir. Ancak, diğer üyelerden biri varsa ve bir veya daha fazla `..` bağımsız değişkeni eksikse, uygun üye değiştirilebilir.

Son olarak, bir dizi öğesi erişim ifadesinde kullanılacak `System.Range` türünde bir değer için aşağıdaki üyenin mevcut olması gerekir:

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System. Index

C#uçtan bir koleksiyonu dizinlemenin bir yolu yoktur, ancak çoğu Dizin oluşturucular "başlatma öncesi" kavram kullanır veya "length-i" ifadesi kullanır. "Uçtan uca" anlamına gelen yeni bir dizin ifadesi tanıtıldık. Özelliği, yeni bir birli ön ek "hat" işleci getirir. Tek işleneni `System.Int32`dönüştürülebilir olmalıdır. Bu, uygun `System.Index` Factory yöntemi çağrısına indirgenmelidir.

Aşağıdaki ek sözdizimi formuyla *unary_expression* için dilbilgisi geliştirdik:

```antlr
unary_expression
    : '^' unary_expression
    ;
```

Bu *dizini End* işlecinden çağırıyoruz. *Son işleçlerden* önceden tanımlanmış dizin aşağıdaki gibidir:

```csharp
    System.Index operator ^(int fromEnd);
```

Bu işlecin davranışı yalnızca sıfıra eşit veya daha büyük giriş değerleri için tanımlanmıştır.

Örnekler:

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>System. Range

C#, koleksiyonların "aralıklarına" veya "dilimlerine" erişmenin sözdizimsel bir yolu yoktur. Genellikle kullanıcılar, bellek dilimlerinde filtrelemek/çalıştırmak için karmaşık yapıları uygulamaya veya `list.Skip(5).Take(2)`gibi LINQ yöntemlerine karşı çalışır. `System.Span<T>` ve diğer benzer türlerin eklenmesiyle, bu tür bir işlemin dil/çalışma zamanında daha derin bir düzeyde desteklenmesi ve arabirimin birleştirilmiş olması daha önemli hale gelir.

Dil `x..y`yeni bir Aralık işleci getirir. İki ifadeyi kabul eden bir ikili dosya operatörü işleçtir. Her iki işlenen de atlanabilir (Aşağıdaki örnekler) ve `System.Index`dönüştürülebilir olması gerekir. Bu, uygun `System.Range` Factory yöntemi çağrısına indirgenmelidir.

C# *Multiplicative_expression* için dilbilgisi kurallarını aşağıdakiler ile değiştirin (yeni bir öncelik düzeyi tanıtmak için):

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

*Aralık işlecinin* tüm formları aynı önceliğe sahiptir. Bu yeni öncelik grubu *birli işleçlerden* daha düşüktür ve *mulitipereptive aritmetik işleçlerinden*daha yüksektir.

*Aralık işleci*`..` işleci çağrılıyoruz. Yerleşik Aralık operatörü, bu formun yerleşik bir işlecinin çağrılması için kabaca bir biçimde anlaşılabilirler:

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

Örnekler:

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

Ayrıca, `System.Index`, çok boyutlu imzalar üzerinde tamsayılar ve dizinleri karıştırmak için aşırı yüklenmesine gerek olmaması halinde `System.Int32`örtülü dönüştürmeye sahip olmalıdır.

## <a name="adding-index-and-range-support-to-existing-library-types"></a>Var olan kitaplık türlerine dizin ve Aralık desteği ekleniyor

### <a name="implicit-index-support"></a>Örtük Dizin desteği

Dil, aşağıdaki ölçütlere uyan türler için `Index` türünde tek bir parametreye sahip bir örnek Dizin Oluşturucu üyesini sağlar:

- Tür Countable.
- Türün bağımsız değişken olarak tek bir `int` alan erişilebilir bir örnek Dizin Oluşturucusu vardır.
- Türün, ilk parametre olarak bir `Index` alan erişilebilir bir örnek dizin oluşturucusu yok. `Index` tek parametre olmalıdır veya kalan parametrelerin isteğe bağlı olması gerekir.

Bir tür, erişilebilir bir alıcı ve `int`dönüş türü `Length` veya `Count` adlı bir ***özelliğe sahipse bir*** tür oluşturulabilir. Dil, bir türü `Index` bir ifadeyi, `int` tür `Index` kullanılmasına gerek kalmadan ifadenin noktasında dönüştürmek için bu özellikten yararlanabilirsiniz. Hem `Length` hem de `Count` varsa `Length` tercih edilir. Kolaylık sağlamak için teklif, `Count` veya `Length`temsil etmek için `Length` adını kullanır.

Bu tür türler için, dil `ref` stil ek açıklamaları dahil olmak üzere `int` tabanlı dizin oluşturucunun dönüş türü olduğu `T` `T this[Index index]`, dilin bir Dizin Oluşturucu üyesi olduğu gibi davranır. Yeni üye, `int` Dizin Oluşturucu olarak eşleşen erişilebilirliği taşıyan `get` ve `set` üyelere sahip olur. 

Yeni Dizin Oluşturucu, `Index` türünün bağımsız değişkeni bir `int` dönüştürerek ve `int` tabanlı dizin oluşturucuya bir çağrı yayarak uygulanır. Tartışma amacıyla, `receiver[expr]`örneğini kullanalım. `expr` `int` dönüşümü aşağıdaki gibi olacaktır:

- Bağımsız değişken `^expr2` form ve `expr2` türü `int`olduğunda, `receiver.Length - expr2`çevrilir.
- Aksi takdirde, `expr.GetOffset(receiver.Length)`olarak çevrilir.

Bu, geliştiricilerin değişiklik gerektirmeden `Index` özelliğini mevcut türler üzerinde kullanmasına izin verir. Örneğin:

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

`receiver` ve `Length` ifadeleri, herhangi bir yan etkilerin yalnızca bir kez yürütüldüğünden emin olmak için uygun şekilde taşacaktır. Örneğin:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

Bu kod "Get length 3" olarak yazdırılır.

### <a name="implicit-range-support"></a>Örtük Aralık desteği

Dil, aşağıdaki ölçütlere uyan türler için `Range` türünde tek bir parametreye sahip bir örnek Dizin Oluşturucu üyesini sağlar:

- Tür Countable.
- Tür, `int`türünde iki parametreye sahip `Slice` adlı erişilebilir bir üyeye sahiptir.
- Türün ilk parametre olarak tek bir `Range` alan bir örnek dizin oluşturucusu yok. `Range` tek parametre olmalıdır veya kalan parametrelerin isteğe bağlı olması gerekir.

Bu tür türler için dil, `ref` stil ek açıklamaları dahil olmak üzere `Slice` yönteminin dönüş türü olduğu `T` `T this[Range range]` formun bir Dizin Oluşturucu üyesi olduğu gibi bağlanır. Yeni üyenin `Slice`ile eşleşen erişilebilirliği da olur. 

`Range` tabanlı dizin oluşturucu `receiver`adlı bir ifadeye bağlandığında, `Range` ifadesi daha sonra `Slice` yöntemine geçirilmiş iki değere dönüştürülerek indirgenirsiniz. Tartışma amacıyla, `receiver[expr]`örneğini kullanalım.

`Slice` ilk bağımsız değişkeni, Aralık türü belirtilen ifade aşağıdaki şekilde dönüştürülürken elde edilir:

- `expr` form `expr1..expr2` (`expr2` atlanabileceğini) ve `expr1` türü `int`olduğunda, `expr1`olarak yayılır.
- `expr` `^expr1..expr2` form olduğunda (`expr2` atlanabilir), `receiver.Length - expr1`olarak yayılır.
- `expr` `..expr2` form olduğunda (`expr2` atlanabilir), `0`olarak yayılır.
- Aksi takdirde, `expr.Start.GetOffset(receiver.Length)`olarak yayınlanacaktır.

Bu değer, ikinci `Slice` bağımsız değişkeninin hesaplamasında yeniden kullanılacaktır. Bunu yaparken, `start`olarak adlandırılır. `Slice` ikinci bağımsız değişkeni, Aralık türü belirtilen ifade aşağıdaki şekilde dönüştürülürken elde edilir:

- `expr` form `expr1..expr2` (`expr1` atlanabileceğini) ve `expr2` türü `int`olduğunda, `expr2 - start`olarak yayılır.
- `expr` `expr1..^expr2` form olduğunda (`expr1` atlanabilir), `(receiver.Length - expr2) - start`olarak yayılır.
- `expr` `expr1..` form olduğunda (`expr1` atlanabilir), `receiver.Length - start`olarak yayılır.
- Aksi takdirde, `expr.End.GetOffset(receiver.Length) - start`olarak yayınlanacaktır.

`receiver`, `Length`ve `expr` ifadeleri, herhangi bir yan etkilerin yalnızca bir kez yürütüldüğünden emin olmak için uygun şekilde taşacaktır. Örneğin:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

Bu kod, "Get length 2" öğesini yazdıracak.

Dil, aşağıdaki bilinen türler için özel durum olacaktır: 

- `string`: `Substring` yöntemi `Slice`yerine kullanılacak.
- `array`: `System.Reflection.CompilerServices.GetSubArray` yöntemi `Slice`yerine kullanılacak.

## <a name="alternatives"></a>Alternatifler

Yeni işleçler (`^` ve `..`) sözdizimsel cukr. İşlevsellik, `System.Index` ve `System.Range` fabrika yöntemlerine yönelik açık çağrılar tarafından uygulanabilir, ancak büyük miktarda daha fazla kod oluşmasına yol açabilir ve deneyim sezgisel hale gelir.

## <a name="il-representation"></a>Il temsili

Bu iki işleç, sonraki derleyici katmanlarında hiçbir değişiklik yapmadan normal Dizin Oluşturucu/yöntem çağrılarına indirgenmelidir.

## <a name="runtime-behavior"></a>Çalışma zamanı davranışı

- Derleyici, diziler ve dizeler gibi yerleşik türler için Dizin oluşturucuyu iyileştirebilirler ve dizin oluşturma işlemini uygun varolan yöntemlere düşürür.
- `System.Index`, negatif bir değerle oluşturulmuşsa oluşturulur.
- `^0` oluşturmaz, ancak sağlanan koleksiyonun uzunluğuna/Numaralandırılabilir.
- `Range.All`, `0..^0`anlam ile eşdeğerdir ve bu dizinler için geçersiz olabilir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

### <a name="detect-indexable-based-on-icollection"></a>ICollection 'e göre dizin algılamayı Algıla

Bu davranış için ilham, koleksiyon başlatıcıları idi. Bir özelliği kabul etmiş olduğunu iletmek için bir türün yapısını kullanma. Koleksiyon Başlatıcıları türleri söz konusu olduğunda, `IEnumerable` (genel olmayan) arabirimini uygulayarak özelliği kabul edebilir.

Bu teklif başlangıçta türlerin dizinlenebilir olarak nitelendirmek için `ICollection` uygulaması gerekir. Bu, birkaç özel durum gerektirse de:

- `ref struct`: Bu arabirimler, Dizin/Aralık desteği için ideal olan `Span<T>` gibi tür arabirimler uygulayamaz. 
- `string`: `ICollection` uygulamaz ve `interface` büyük bir maliyete sahip.

Bu, anahtar türlerini desteklemek için özel büyük küçük harfe zaten ihtiyaç duymasıdır. `string` özel büyük küçük harfleri, diğer alanlarda (`foreach` kısmayı, sabitleri, vb.) bunu yaparken bu dilin ne kadar ilginç olduğunu azaltır. `ref struct` özel büyük küçük harfleri, tüm tür sınıfları için özel büyük küçük harfe göre daha fazla ele aldır. `int`dönüş türü olan `Count` adlı bir özelliğe sahip olmaları halinde dizinlenebilir olarak etiketlenir. 

Göz önünde bulundurulduktan sonra tasarım, `int` dönüş türü `Length`  / `Count`bir özelliği olan herhangi bir tür için normalleştirilmelidir. Bu, `string` ve diziler için bile tüm özel büyük harfleri kaldırır.

### <a name="detect-just-count"></a>Yalnızca sayıyı Algıla

`Count` veya `Length` özellik adlarında algılama, tasarımın bir bit olduğunu karmaşıklaştırır. Çok sayıda tür hariç tutularak, standartlaştırmak için yalnızca bir tane seçilmesi yeterlidir:

- `Length`kullanın: System. Collections ve alt ad alanlarında oldukça çok her koleksiyonu dışlar. Bunlar `ICollection` türetmeye eğilimlidir ve bu nedenle `Count` uzunluğu tercih eder.
- `Count`kullanın: `string`, dizileri `Span<T>` ve en `ref struct` tabanlı türleri dışlar

Dizine eklenebilir türlerin ilk algılamasında daha karmaşıkma, diğer yönlerde basitleştirme tarafından belirlenir.

### <a name="choice-of-slice-as-a-name"></a>Ad olarak dilim seçimi

`Slice` adı, .NET 'teki dilim stili işlemlerine yönelik standart adı olarak seçilmiştir. Netcoreapp 2.1 'den başlayarak tüm yayılma stil türleri, Dilimleme işlemleri için `Slice` adını kullanır. Netcoreapp 2.1 'den önce bir örnek için aranacak örneklere örnek yok. `List<T>`, `ArraySegment<T>``SortedList<T>` gibi türler, Dilimleme için idealdir ancak türler eklendiğinde kavram yoktu. 

Bu nedenle, tek örnek olarak `Slice`, ad olarak seçilmiştir.

### <a name="index-target-type-conversion"></a>Dizin hedef türü dönüştürme

Bir Dizin Oluşturucu ifadesinde `Index` dönüşümünü görüntülemenin bir başka yolu da hedef tür dönüştürmesi olarak belirlenir. `return_type this[Index]`form üyesi gibi bağlamak yerine, bu dil, `int`için bir hedef türü dönüştürme atar. 

Bu kavram, countable türlerinde tüm üye erişimlerde genelleştirilemez. Türü `Index` olan bir ifade, örnek üye çağrısına bağımsız değişken olarak kullanıldığında ve alıcı Countable ise, ifadenin `int`hedef tür dönüştürmesi olur. Bu dönüştürmeye uygulanabilecek üye etkinleştirmeleri, Yöntemler, Dizin oluşturucular, özellikler, uzantı yöntemleri vb. içerir. Yalnızca oluşturucular, alıcısı olmadığı için hariç tutulur. 

Hedef türü dönüştürmesi, `Index`türü olan herhangi bir ifade için aşağıdaki şekilde uygulanır. Tartışma amaçları için `receiver[expr]`örneğini kullanın:

- `expr` form `^expr2` ve `expr2` türü `int`olduğunda, `receiver.Length - expr2`çevrilir.
- Aksi takdirde, `expr.GetOffset(receiver.Length)`olarak çevrilir.

`receiver` ve `Length` ifadeleri, herhangi bir yan etkilerin yalnızca bir kez yürütüldüğünden emin olmak için uygun şekilde taşacaktır. Örneğin:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

Bu kod "Get length 3" olarak yazdırılır. 

Bu özellik, bir dizini temsil eden bir parametreye sahip olan herhangi bir üyeye faydalı olabilir. Örneğin, `List<T>.InsertAt`. Ayrıca, bir ifadenin dizin oluşturmak için tasarlanmış olup olmadığı konusunda hiçbir rehberlik veremediğinden, bu da karışıklığın potansiyelini de sağlar. Tüm `Index` ifadelerini, bir Countable türünde üye çağrılırken `int` dönüştürür. 

Larından

- Bu dönüştürme yalnızca, türü `Index` olan ifade doğrudan üyenin bir bağımsız değişkeni olduğunda geçerlidir. Herhangi bir iç içe geçmiş ifadeye uygulanmaz.

## <a name="decisions-made-during-implementation"></a>Uygulama sırasında yapılan kararlar

- Düzendeki tüm Üyeler örnek üye olmalıdır
- Bir uzunluk yöntemi bulunursa ancak dönüş türü yanlış ise, sayı aramaya devam edin
- Dizin deseninin kullandığı dizin oluşturucunun tam olarak bir int parametresi olmalıdır
- Aralık deseninin kullandığı dilim yöntemi tam olarak iki int parametreye sahip olmalıdır
- Model üyelerini ararken, oluşturulan üyeleri değil, özgün tanımları aradık

## <a name="design-meetings"></a>Tasarım toplantıları

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>UL
