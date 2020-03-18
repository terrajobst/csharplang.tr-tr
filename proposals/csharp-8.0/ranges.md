---
ms.openlocfilehash: d6519ff57b4a98c4eec8ccbf310303432ac3255e
ms.sourcegitcommit: 65ea1e6dc02853e37e7f2088e2b6cc08d01d1044
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485030"
---
# <a name="ranges"></a><span data-ttu-id="2b8bf-101">Aralıklar</span><span class="sxs-lookup"><span data-stu-id="2b8bf-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="2b8bf-102">Özet</span><span class="sxs-lookup"><span data-stu-id="2b8bf-102">Summary</span></span>

<span data-ttu-id="2b8bf-103">Bu özellik, `System.Index` ve `System.Range` nesneleri oluşturmak ve çalışma zamanında koleksiyonlara/dilimlemek için onları kullanmak üzere iki yeni işleç sunmaya yönelik bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="2b8bf-104">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="2b8bf-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="2b8bf-105">İyi bilinen türler ve Üyeler</span><span class="sxs-lookup"><span data-stu-id="2b8bf-105">Well-known types and members</span></span>

<span data-ttu-id="2b8bf-106">Yeni sözdizimsel formları `System.Index` ve `System.Range`için kullanmak üzere, hangi sözdizimsel biçimlerinin kullanıldığına bağlı olarak, yeni iyi bilinen türler ve Üyeler gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="2b8bf-107">"Hat" işlecini (`^`) kullanmak için aşağıdakiler gereklidir</span><span class="sxs-lookup"><span data-stu-id="2b8bf-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="2b8bf-108">`System.Index` türünü bir dizi öğesi erişiminde bağımsız değişken olarak kullanmak için aşağıdaki üye gereklidir:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="2b8bf-109">`System.Range` için `..` söz dizimi, aşağıdaki üyelerin bir veya daha fazla `System.Range` türünü de gerektirir:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

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

<span data-ttu-id="2b8bf-110">`..` sözdizimi, her ikisinin de ya da bağımsız değişkenlerinin hiçbirinin olmaması için izin verir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="2b8bf-111">Bağımsız değişken sayısından bağımsız olarak `Range` Oluşturucu `Range` söz dizimini kullanmak için her zaman yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="2b8bf-112">Ancak, diğer üyelerden biri varsa ve bir veya daha fazla `..` bağımsız değişkeni eksikse, uygun üye değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="2b8bf-113">Son olarak, bir dizi öğesi erişim ifadesinde kullanılacak `System.Range` türünde bir değer için aşağıdaki üyenin mevcut olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="2b8bf-114">System. Index</span><span class="sxs-lookup"><span data-stu-id="2b8bf-114">System.Index</span></span>

<span data-ttu-id="2b8bf-115">C#uçtan bir koleksiyonu dizinlemenin bir yolu yoktur, ancak çoğu Dizin oluşturucular "başlatma öncesi" kavram kullanır veya "length-i" ifadesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="2b8bf-116">"Uçtan uca" anlamına gelen yeni bir dizin ifadesi tanıtıldık.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="2b8bf-117">Özelliği, yeni bir birli ön ek "hat" işleci getirir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="2b8bf-118">Tek işleneni `System.Int32`dönüştürülebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="2b8bf-119">Bu, uygun `System.Index` Factory yöntemi çağrısına indirgenmelidir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="2b8bf-120">Aşağıdaki ek sözdizimi formuyla *unary_expression* için dilbilgisi geliştirdik:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="2b8bf-121">Bu *dizini End* işlecinden çağırıyoruz.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="2b8bf-122">*Son işleçlerden* önceden tanımlanmış dizin aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="2b8bf-123">Bu işlecin davranışı yalnızca sıfıra eşit veya daha büyük giriş değerleri için tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="2b8bf-124">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="2b8bf-125">System. Range</span><span class="sxs-lookup"><span data-stu-id="2b8bf-125">System.Range</span></span>

<span data-ttu-id="2b8bf-126">C#, koleksiyonların "aralıklarına" veya "dilimlerine" erişmenin sözdizimsel bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="2b8bf-127">Genellikle kullanıcılar, bellek dilimlerinde filtrelemek/çalıştırmak için karmaşık yapıları uygulamaya veya `list.Skip(5).Take(2)`gibi LINQ yöntemlerine karşı çalışır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="2b8bf-128">`System.Span<T>` ve diğer benzer türlerin eklenmesiyle, bu tür bir işlemin dil/çalışma zamanında daha derin bir düzeyde desteklenmesi ve arabirimin birleştirilmiş olması daha önemli hale gelir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="2b8bf-129">Dil `x..y`yeni bir Aralık işleci getirir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="2b8bf-130">İki ifadeyi kabul eden bir ikili dosya operatörü işleçtir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="2b8bf-131">Her iki işlenen de atlanabilir (Aşağıdaki örnekler) ve `System.Index`dönüştürülebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="2b8bf-132">Bu, uygun `System.Range` Factory yöntemi çağrısına indirgenmelidir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="2b8bf-133">\- C# *Multiplicative_expression* için dilbilgisi kurallarını aşağıdakiler ile değiştirin (yeni bir öncelik düzeyi tanıtmak için):</span><span class="sxs-lookup"><span data-stu-id="2b8bf-133">-We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

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

<span data-ttu-id="2b8bf-134">*Aralık işlecinin* tüm formları aynı önceliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="2b8bf-135">Bu yeni öncelik grubu *birli işleçlerden* daha düşüktür ve *mulitipereptive aritmetik işleçlerinden*daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="2b8bf-136">*Aralık işleci*`..` işleci çağrılıyoruz.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="2b8bf-137">Yerleşik Aralık operatörü, bu formun yerleşik bir işlecinin çağrılması için kabaca bir biçimde anlaşılabilirler:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="2b8bf-138">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="2b8bf-139">Ayrıca, `System.Index`, çok boyutlu imzalar üzerinde tamsayılar ve dizinleri karıştırmak için aşırı yüklenmesine gerek olmaması halinde `System.Int32`örtülü dönüştürmeye sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="2b8bf-140">Var olan kitaplık türlerine dizin ve Aralık desteği ekleniyor</span><span class="sxs-lookup"><span data-stu-id="2b8bf-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="2b8bf-141">Örtük Dizin desteği</span><span class="sxs-lookup"><span data-stu-id="2b8bf-141">Implicit Index support</span></span>

<span data-ttu-id="2b8bf-142">Dil, aşağıdaki ölçütlere uyan türler için `Index` türünde tek bir parametreye sahip bir örnek Dizin Oluşturucu üyesini sağlar:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="2b8bf-143">Tür Countable.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-143">The type is Countable.</span></span>
- <span data-ttu-id="2b8bf-144">Türün bağımsız değişken olarak tek bir `int` alan erişilebilir bir örnek Dizin Oluşturucusu vardır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="2b8bf-145">Türün, ilk parametre olarak bir `Index` alan erişilebilir bir örnek dizin oluşturucusu yok.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="2b8bf-146">`Index` tek parametre olmalıdır veya kalan parametrelerin isteğe bağlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="2b8bf-147">Bir tür, erişilebilir bir alıcı ve `int`dönüş türü `Length` veya `Count` adlı bir ***özelliğe sahipse bir*** tür oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="2b8bf-148">Dil, bir türü `Index` bir ifadeyi, `int` tür `Index` kullanılmasına gerek kalmadan ifadenin noktasında dönüştürmek için bu özellikten yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="2b8bf-149">Hem `Length` hem de `Count` varsa `Length` tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="2b8bf-150">Kolaylık sağlamak için teklif, `Count` veya `Length`temsil etmek için `Length` adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="2b8bf-151">Bu tür türler için, dil `ref` stil ek açıklamaları dahil olmak üzere `int` tabanlı dizin oluşturucunun dönüş türü olduğu `T` `T this[Index index]`, dilin bir dizin üyesi olup olmadığı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-151">For such types, the language will act as if there is an index member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="2b8bf-152">Yeni üye, `int` Dizin Oluşturucu olarak eşleşen erişilebilirliği taşıyan `get` ve `set` üyelere sahip olur.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="2b8bf-153">Yeni Dizin Oluşturucu, `Index` türünün bağımsız değişkeni bir `int` dönüştürerek ve `int` tabanlı dizin oluşturucuya bir çağrı yayarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="2b8bf-154">Tartışma amacıyla, `receiver[expr]`örneğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-154">For discussion purposes, lets use the example of `receiver[expr]`.</span></span> <span data-ttu-id="2b8bf-155">`expr` `int` dönüşümü aşağıdaki gibi olacaktır:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="2b8bf-156">Bağımsız değişken `^expr2` form ve `expr2` türü `int`olduğunda, `receiver.Length - expr2`çevrilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="2b8bf-157">Aksi takdirde, `expr.GetOffset(receiver.Length)`olarak çevrilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="2b8bf-158">Bu, geliştiricilerin değişiklik gerektirmeden `Index` özelliğini mevcut türler üzerinde kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="2b8bf-159">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="2b8bf-160">`receiver` ve `Length` ifadeleri, herhangi bir yan etkilerin yalnızca bir kez yürütüldüğünden emin olmak için uygun şekilde taşacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="2b8bf-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-161">For example:</span></span>

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

<span data-ttu-id="2b8bf-162">Bu kod "Get length 3" olarak yazdırılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="2b8bf-163">Örtük Aralık desteği</span><span class="sxs-lookup"><span data-stu-id="2b8bf-163">Implicit Range support</span></span>

<span data-ttu-id="2b8bf-164">Dil, aşağıdaki ölçütlere uyan türler için `Range` türünde tek bir parametreye sahip bir örnek Dizin Oluşturucu üyesini sağlar:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="2b8bf-165">Tür Countable.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-165">The type is Countable.</span></span>
- <span data-ttu-id="2b8bf-166">Tür, `int`türünde iki parametreye sahip `Slice` adlı erişilebilir bir üyeye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="2b8bf-167">Türün ilk parametre olarak tek bir `Range` alan bir örnek dizin oluşturucusu yok.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="2b8bf-168">`Range` tek parametre olmalıdır veya kalan parametrelerin isteğe bağlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="2b8bf-169">Bu tür türler için dil, `ref` stil ek açıklamaları dahil `Slice` yönteminin dönüş türü olduğu `T` `T this[Range range]`, formun bir dizin üyesi olduğu gibi bağlanır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-169">For such types, the language will bind as if there is an index member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="2b8bf-170">Yeni üyenin `Slice`ile eşleşen erişilebilirliği da olur.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="2b8bf-171">`Range` tabanlı dizin oluşturucu `receiver`adlı bir ifadeye bağlandığında, `Range` ifadesi daha sonra `Slice` yöntemine geçirilmiş iki değere dönüştürülerek indirgenirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="2b8bf-172">Tartışma amacıyla, `receiver[expr]`örneğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-172">For discussion purposes, lets use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="2b8bf-173">`Slice` ilk bağımsız değişkeni, yazılan ifade aşağıdaki şekilde dönüştürülürken elde edilir:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-173">The first argument of `Slice` will be obtained by converting the typed expression in the following way:</span></span>

- <span data-ttu-id="2b8bf-174">`expr` form `expr1..expr2` (`expr2` atlanabileceğini) ve `expr1` türü `int`olduğunda, `expr1`olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="2b8bf-175">`expr` `^expr1..expr2` form olduğunda (`expr2` atlanabilir), `receiver.Length - expr1`olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="2b8bf-176">`expr` `..expr2` form olduğunda (`expr2` atlanabilir), `0`olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="2b8bf-177">Aksi takdirde, `expr.Start.GetOffset(receiver.Length)`olarak yayınlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="2b8bf-178">Bu değer, ikinci `Slice` bağımsız değişkeninin hesaplamasında yeniden kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="2b8bf-179">Bunu yaparken, `start`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="2b8bf-180">`Slice` ikinci bağımsız değişkeni, Aralık türü belirtilen ifade aşağıdaki şekilde dönüştürülürken elde edilir:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="2b8bf-181">`expr` form `expr1..expr2` (`expr1` atlanabileceğini) ve `expr2` türü `int`olduğunda, `expr2 - start`olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="2b8bf-182">`expr` `expr1..^expr2` form olduğunda (`expr1` atlanabilir), `(receiver.Length - expr2) - start`olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="2b8bf-183">`expr` `expr1..` form olduğunda (`expr1` atlanabilir), `receiver.Length - start`olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="2b8bf-184">Aksi takdirde, `expr.End.GetOffset(receiver.Length) - start`olarak yayınlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="2b8bf-185">`receiver`, `Length` ve `expr` ifadeleri, herhangi bir yan etkilerin yalnızca bir kez yürütüldüğünden emin olmak için uygun şekilde taşacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-185">The `receiver`, `Length` and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="2b8bf-186">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-186">For example:</span></span>

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

<span data-ttu-id="2b8bf-187">Bu kod, "Get length 2" öğesini yazdıracak.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="2b8bf-188">Dil, aşağıdaki bilinen türler için özel durum olacaktır:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="2b8bf-189">`string`: `Substring` yöntemi `Slice`yerine kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="2b8bf-190">`array`: `System.Reflection.CompilerServices.GetSubArray` yöntemi `Slice`yerine kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="2b8bf-191">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="2b8bf-191">Alternatives</span></span>

<span data-ttu-id="2b8bf-192">Yeni işleçler (`^` ve `..`) sözdizimsel cukr.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="2b8bf-193">İşlevsellik, `System.Index` ve `System.Range` fabrika yöntemlerine yönelik açık çağrılar tarafından uygulanabilir, ancak büyük miktarda daha fazla kod oluşmasına yol açabilir ve deneyim sezgisel hale gelir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="2b8bf-194">Il temsili</span><span class="sxs-lookup"><span data-stu-id="2b8bf-194">IL Representation</span></span>

<span data-ttu-id="2b8bf-195">Bu iki işleç, sonraki derleyici katmanlarında hiçbir değişiklik yapmadan normal Dizin Oluşturucu/yöntem çağrılarına indirgenmelidir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="2b8bf-196">Çalışma zamanı davranışı</span><span class="sxs-lookup"><span data-stu-id="2b8bf-196">Runtime behavior</span></span>

- <span data-ttu-id="2b8bf-197">Derleyici, diziler ve dizeler gibi yerleşik türler için Dizin oluşturucuyu iyileştirebilirler ve dizin oluşturma işlemini uygun varolan yöntemlere düşürür.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="2b8bf-198">`System.Index`, negatif bir değerle oluşturulmuşsa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="2b8bf-199">`^0` oluşturmaz, ancak sağlanan koleksiyonun uzunluğuna/Numaralandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="2b8bf-200">`Range.All`, `0..^0`anlam ile eşdeğerdir ve bu dizinler için geçersiz olabilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="2b8bf-201">Dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="2b8bf-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="2b8bf-202">ICollection 'e göre dizin algılamayı Algıla</span><span class="sxs-lookup"><span data-stu-id="2b8bf-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="2b8bf-203">Bu davranış için ilham, koleksiyon başlatıcıları idi.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="2b8bf-204">Bir özelliği kabul etmiş olduğunu iletmek için bir türün yapısını kullanma.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="2b8bf-205">Koleksiyon Başlatıcıları türleri söz konusu olduğunda, `IEnumerable` (genel olmayan) arabirimini uygulayarak özelliği kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="2b8bf-206">Bu teklif başlangıçta türlerin dizinlenebilir olarak nitelendirmek için `ICollection` uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="2b8bf-207">Bu, birkaç özel durum gerektirse de:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="2b8bf-208">`ref struct`: Bu arabirimler, Dizin/Aralık desteği için ideal olan `Span<T>` gibi tür arabirimler uygulayamaz.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="2b8bf-209">`string`: `ICollection` uygulamaz ve `interface` büyük bir maliyete sahip.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="2b8bf-210">Bu, anahtar türlerini desteklemek için özel büyük küçük harfe zaten ihtiyaç duymasıdır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="2b8bf-211">`string` özel büyük küçük harfleri, diğer alanlarda (`foreach` kısmayı, sabitleri, vb.) bunu yaparken bu dilin ne kadar ilginç olduğunu azaltır. `ref struct` özel büyük küçük harfleri, tüm tür sınıfları için özel büyük küçük harfe göre daha fazla ele aldır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="2b8bf-212">`int`dönüş türü olan `Count` adlı bir özelliğe sahip olmaları halinde dizinlenebilir olarak etiketlenir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="2b8bf-213">Göz önünde bulundurulduktan sonra tasarım, `int` dönüş türü `Length`  / `Count`bir özelliği olan herhangi bir tür için normalleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="2b8bf-214">Bu, `string` ve diziler için bile tüm özel büyük harfleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="2b8bf-215">Yalnızca sayıyı Algıla</span><span class="sxs-lookup"><span data-stu-id="2b8bf-215">Detect just Count</span></span>

<span data-ttu-id="2b8bf-216">`Count` veya `Length` özellik adlarında algılama, tasarımın bir bit olduğunu karmaşıklaştırır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="2b8bf-217">Çok sayıda tür hariç tutularak, standartlaştırmak için yalnızca bir tane seçilmesi yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="2b8bf-218">`Length`kullanın: System. Collections ve alt ad alanlarında oldukça çok her koleksiyonu dışlar.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="2b8bf-219">Bunlar `ICollection` türetmeye eğilimlidir ve bu nedenle `Count` uzunluğu tercih eder.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="2b8bf-220">`Count`kullanın: `string`, dizileri `Span<T>` ve en `ref struct` tabanlı türleri dışlar</span><span class="sxs-lookup"><span data-stu-id="2b8bf-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="2b8bf-221">Dizine eklenebilir türlerin ilk algılamasında daha karmaşıkma, diğer yönlerde basitleştirme tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="2b8bf-222">Ad olarak dilim seçimi</span><span class="sxs-lookup"><span data-stu-id="2b8bf-222">Choice of Slice as a name</span></span>

<span data-ttu-id="2b8bf-223">`Slice` adı, .NET 'teki dilim stili işlemlerine yönelik standart adı olarak seçilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="2b8bf-224">Netcoreapp 2.1 'den başlayarak tüm yayılma stil türleri, Dilimleme işlemleri için `Slice` adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="2b8bf-225">Netcoreapp 2.1 'den önce bir örnek için aranacak örneklere örnek yok.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="2b8bf-226">`List<T>`, `ArraySegment<T>``SortedList<T>` gibi türler, Dilimleme için idealdir ancak türler eklendiğinde kavram yoktu.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="2b8bf-227">Bu nedenle, tek örnek olarak `Slice`, ad olarak seçilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="2b8bf-228">Dizin hedef türü dönüştürme</span><span class="sxs-lookup"><span data-stu-id="2b8bf-228">Index target type conversion</span></span>

<span data-ttu-id="2b8bf-229">Bir Dizin Oluşturucu ifadesinde `Index` dönüşümünü görüntülemenin bir başka yolu da hedef tür dönüştürmesi olarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="2b8bf-230">`return_type this[Index]`form üyesi gibi bağlamak yerine, bu dil, `int`için bir hedef türü dönüştürme atar.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="2b8bf-231">Bu kavram, countable türlerinde tüm üye erişimlerde genelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="2b8bf-232">Türü `Index` olan bir ifade, örnek üye çağrısına bağımsız değişken olarak kullanıldığında ve alıcı Countable ise, ifadenin `int`hedef tür dönüştürmesi olur.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="2b8bf-233">Bu dönüştürmeye uygulanabilecek üye etkinleştirmeleri, Yöntemler, Dizin oluşturucular, özellikler, uzantı yöntemleri vb. içerir. Yalnızca oluşturucular, alıcısı olmadığı için hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="2b8bf-234">Hedef türü dönüştürmesi, `Index`türü olan herhangi bir ifade için aşağıdaki şekilde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="2b8bf-235">Tartışma amaçları için `receiver[expr]`örneğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="2b8bf-236">`expr` form `^expr2` ve `expr2` türü `int`olduğunda, `receiver.Length - expr2`çevrilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="2b8bf-237">Aksi takdirde, `expr.GetOffset(receiver.Length)`olarak çevrilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="2b8bf-238">`receiver` ve `Length` ifadeleri, herhangi bir yan etkilerin yalnızca bir kez yürütüldüğünden emin olmak için uygun şekilde taşacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="2b8bf-239">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2b8bf-239">For example:</span></span>

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

<span data-ttu-id="2b8bf-240">Bu kod "Get length 3" olarak yazdırılır.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="2b8bf-241">Bu özellik, bir dizini temsil eden bir parametreye sahip olan herhangi bir üyeye faydalı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="2b8bf-242">Örneğin, `List<T>.InsertAt`.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="2b8bf-243">Ayrıca, bir ifadenin dizin oluşturmak için tasarlanmış olup olmadığı konusunda hiçbir rehberlik veremediğinden, bu da karışıklığın potansiyelini de sağlar.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="2b8bf-244">Tüm `Index` ifadelerini, bir Countable türünde üye çağrılırken `int` dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="2b8bf-245">Larından</span><span class="sxs-lookup"><span data-stu-id="2b8bf-245">Restrictions:</span></span>

- <span data-ttu-id="2b8bf-246">Bu dönüştürme yalnızca, türü `Index` olan ifade doğrudan üyenin bir bağımsız değişkeni olduğunda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="2b8bf-247">Herhangi bir iç içe geçmiş ifadeye uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="2b8bf-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="2b8bf-248">Uygulama sırasında yapılan kararlar</span><span class="sxs-lookup"><span data-stu-id="2b8bf-248">Decisions made during implementation</span></span>

- <span data-ttu-id="2b8bf-249">Düzendeki tüm Üyeler örnek üye olmalıdır</span><span class="sxs-lookup"><span data-stu-id="2b8bf-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="2b8bf-250">Bir uzunluk yöntemi bulunursa ancak dönüş türü yanlış ise, sayı aramaya devam edin</span><span class="sxs-lookup"><span data-stu-id="2b8bf-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="2b8bf-251">Dizin deseninin kullandığı dizin oluşturucunun tam olarak bir int parametresi olmalıdır</span><span class="sxs-lookup"><span data-stu-id="2b8bf-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="2b8bf-252">Aralık deseninin kullandığı dilim yöntemi tam olarak iki int parametreye sahip olmalıdır</span><span class="sxs-lookup"><span data-stu-id="2b8bf-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="2b8bf-253">Model üyelerini ararken, oluşturulan üyeleri değil, özgün tanımları aradık</span><span class="sxs-lookup"><span data-stu-id="2b8bf-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="2b8bf-254">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="2b8bf-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="2b8bf-255">UL</span><span class="sxs-lookup"><span data-stu-id="2b8bf-255">Questions</span></span>
