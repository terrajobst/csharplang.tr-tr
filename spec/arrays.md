---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488837"
---
# <a name="arrays"></a><span data-ttu-id="088bb-101">Diziler</span><span class="sxs-lookup"><span data-stu-id="088bb-101">Arrays</span></span>

<span data-ttu-id="088bb-102">Bir dizi hesaplanan dizinlerini erişilen değişken bir sayı içeren bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="088bb-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="088bb-103">Dizinin öğeleri olarak da bilinen bir dizi içindeki tüm aynı türdeki değişkenlerdir ve bu tür dizinin öğe türü olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="088bb-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="088bb-104">Bir dizinin her dizi öğesi ile ilişkili sayısından belirleyen bir dereceli.</span><span class="sxs-lookup"><span data-stu-id="088bb-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="088bb-105">Dizi boyut, dizi boyutları da bilinir.</span><span class="sxs-lookup"><span data-stu-id="088bb-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="088bb-106">Bir boyut ile bir dizi olarak adlandırılan bir ***tek boyutlu dizi***.</span><span class="sxs-lookup"><span data-stu-id="088bb-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="088bb-107">Daha fazla biri derece olan bir dizi bir ***çok boyutlu dizi***.</span><span class="sxs-lookup"><span data-stu-id="088bb-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="088bb-108">Belirli bir boyuttaki çok boyutlu diziler, iki boyutlu diziler, üç boyutlu dizileri vb. da anılır.</span><span class="sxs-lookup"><span data-stu-id="088bb-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="088bb-109">Bir dizinin her boyutunun sıfır olarak bir tamsayı değerinden büyük veya eşit olan ilişkili bir uzunluğa sahip.</span><span class="sxs-lookup"><span data-stu-id="088bb-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="088bb-110">Boyut uzunlukları dizinin türü bir parçası değildir, ancak çalışma zamanında dizi türünün bir örneği oluşturulduğunda yerine oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="088bb-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="088bb-111">Bir boyutun uzunluğu o boyut için dizin geçerli aralık belirler: Boyut uzunluğu `N`, dizinler arasında değişebilir `0` için `N - 1` dahil.</span><span class="sxs-lookup"><span data-stu-id="088bb-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="088bb-112">Bir dizideki öğelerin sayısı dizideki her boyutun uzunluğu ürünüdür.</span><span class="sxs-lookup"><span data-stu-id="088bb-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="088bb-113">Sıfır uzunlukta bir veya daha fazla dizi boyutları varsa, dizi, boş olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="088bb-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="088bb-114">Bir dizinin öğe türü bir dizi türü de dahil olmak üzere herhangi bir tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="088bb-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="088bb-115">Dizi türleri</span><span class="sxs-lookup"><span data-stu-id="088bb-115">Array types</span></span>

<span data-ttu-id="088bb-116">Bir dizi türü olarak yazılmış bir *non_array_type* ardından bir veya daha fazla *rank_specifier*: %s</span><span class="sxs-lookup"><span data-stu-id="088bb-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

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

<span data-ttu-id="088bb-117">A *non_array_type* herhangi *türü* diğer bir deyişle kendisi bir *array_type*.</span><span class="sxs-lookup"><span data-stu-id="088bb-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="088bb-118">Boyut sayısı bir dizi türü soldaki tarafından verilen *rank_specifier* içinde *array_type*: A *rank_specifier* bir artı sayısı derecesi olan bir dizi dizi belirtir "`,`" içinde belirteçler *rank_specifier*.</span><span class="sxs-lookup"><span data-stu-id="088bb-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="088bb-119">Bir dizi türünün öğe türü soldaki silmesi sonuçları türdür *rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="088bb-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="088bb-120">Bir dizi türü formun `T[R]` dereceye sahip bir dizi `R` ve dizi olmayan öğe türü `T`.</span><span class="sxs-lookup"><span data-stu-id="088bb-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="088bb-121">Bir dizi türü formun `T[R][R1]...[Rn]` dereceye sahip bir dizi `R` ve bir öğe türü `T[R1]...[Rn]`.</span><span class="sxs-lookup"><span data-stu-id="088bb-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="088bb-122">Aslında, *rank_specifier*s okunur soldan sağa önce son dizi olmayan öğe türü.</span><span class="sxs-lookup"><span data-stu-id="088bb-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="088bb-123">Türü `int[][,,][,]` üç boyutlu dizi iki boyutlu dizi tek boyutlu bir dizidir `int`.</span><span class="sxs-lookup"><span data-stu-id="088bb-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="088bb-124">Çalışma zamanında, bir dizi türünde bir değer olabilir `null` veya dizi türü örneğine başvuru.</span><span class="sxs-lookup"><span data-stu-id="088bb-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="088bb-125">System.Array türü</span><span class="sxs-lookup"><span data-stu-id="088bb-125">The System.Array type</span></span>

<span data-ttu-id="088bb-126">Türü `System.Array` tüm dizi türleri soyut taban türü.</span><span class="sxs-lookup"><span data-stu-id="088bb-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="088bb-127">Örtük bir başvuru dönüştürmesi ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) için herhangi bir dizi türünden var. `System.Array`ve bir açık bir başvuru dönüştürmesi ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) var. gelen `System.Array` için herhangi bir dizi türü.</span><span class="sxs-lookup"><span data-stu-id="088bb-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="088bb-128">Unutmayın `System.Array` kendisi değil bir *array_type*.</span><span class="sxs-lookup"><span data-stu-id="088bb-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="088bb-129">Bunun yerine, olan bir *class_type* tüm gelen *array_type*s türetilir.</span><span class="sxs-lookup"><span data-stu-id="088bb-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="088bb-130">Çalışma zamanında, türünde bir değer `System.Array` olabilir `null` veya herhangi bir dizi türü örneğine başvuru.</span><span class="sxs-lookup"><span data-stu-id="088bb-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="088bb-131">Diziler ve genel IList arabirimi</span><span class="sxs-lookup"><span data-stu-id="088bb-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="088bb-132">Tek boyutlu dizi `T[]` arabirimi uygulayan `System.Collections.Generic.IList<T>` (`IList<T>` kısaca) ve kendi temel arabirimleri.</span><span class="sxs-lookup"><span data-stu-id="088bb-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="088bb-133">Buna göre örtük bir dönüştürme yoktur `T[]` için `IList<T>` ve kendi temel arabirimleri.</span><span class="sxs-lookup"><span data-stu-id="088bb-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="088bb-134">Ayrıca, bir örtük bir başvuru dönüştürme ise `S` için `T` ardından `S[]` uygulayan `IList<T>` ve gelen bir örtük bir başvuru dönüştürmesi yoktur `S[]` için `IList<T>` ve bunun temel arabirimleri ( [Örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="088bb-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="088bb-135">Bir açık başvuru dönüştürme ise `S` için `T` sonra gelen bir açık bir başvuru dönüştürmesi `S[]` için `IList<T>` ve temel arabirimlerini ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="088bb-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="088bb-136">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="088bb-136">For example:</span></span>
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

<span data-ttu-id="088bb-137">Atama `lst2 = oa1` dönüştürme işlemi bu yana bir derleme zamanı hatası oluşturur `object[]` için `IList<string>` , değil örtük açık bir dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="088bb-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="088bb-138">Cast `(IList<string>)oa1` bir özel çalışma zamanında bu yana durum oluşturulmasına neden olacak `oa1` başvurular bir `object[]` değil bir `string[]`.</span><span class="sxs-lookup"><span data-stu-id="088bb-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="088bb-139">Ancak cast `(IList<string>)oa2` bir özel bu yana durum neden olmaz `oa2` başvurular bir `string[]`.</span><span class="sxs-lookup"><span data-stu-id="088bb-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="088bb-140">Her başvuru örtük veya açık bir dönüştürme yoktur `S[]` için `IList<T>`, aynı zamanda gelen bir açık bir başvuru dönüştürmesi yoktur `IList<T>` ve bunun temel arabirimleri için `S[]` ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="088bb-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="088bb-141">Bir dizi türü olduğunda `S[]` uygulayan `IList<T>`, uygulanan arabirimin üyelerini bazı özel durumlar oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="088bb-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="088bb-142">Kesin arabiriminin uygulamasını bu belirtim kapsamı dışındadır davranışıdır.</span><span class="sxs-lookup"><span data-stu-id="088bb-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="088bb-143">Dizi oluşturma</span><span class="sxs-lookup"><span data-stu-id="088bb-143">Array creation</span></span>

<span data-ttu-id="088bb-144">Dizi örneği tarafından oluşturulan *array_creation_expression*s ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)) veya alan ya da içeren yerel bir değişken bildirimi bir *array_initializer*([Dizi başlatıcıları](arrays.md#array-initializers)).</span><span class="sxs-lookup"><span data-stu-id="088bb-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="088bb-145">Bir dizi örneği oluşturulduğunda, boyut sayısı ve her boyutun uzunluğu oluşturulur ve ardından örneğinin tüm ömrü boyunca sabit kalır.</span><span class="sxs-lookup"><span data-stu-id="088bb-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="088bb-146">Diğer bir deyişle, var olan bir dizi örneği derecesini değiştirmek mümkün değildir ve boyutlar yeniden boyutlandırmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="088bb-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="088bb-147">Bir dizi örneği her zaman bir dizi türüdür.</span><span class="sxs-lookup"><span data-stu-id="088bb-147">An array instance is always of an array type.</span></span> <span data-ttu-id="088bb-148">`System.Array` Türü, soyut bir tür örneği oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="088bb-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="088bb-149">Tarafından oluşturulan dizilerin öğelerine *array_creation_expression*s her zaman varsayılan değerlerine başlatıldı ([varsayılan değerler](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="088bb-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="088bb-150">Dizi öğesi erişimi</span><span class="sxs-lookup"><span data-stu-id="088bb-150">Array element access</span></span>

<span data-ttu-id="088bb-151">Dizi öğeleri kullanılarak erişilir *element_access* ifadeleri ([dizi erişim](expressions.md#array-access)) biçiminde `A[I1, I2, ..., In]`burada `A` bir dizi türü ve her bir ifade `Ix` olduğu bir türündeki ifade `int`, `uint`, `long`, `ulong`, ya da bir veya daha fazla bu türleri için örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="088bb-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="088bb-152">Bir değişken, yani dizinleri tarafından seçilen dizi öğesi bir dizi öğe erişimi sonucudur.</span><span class="sxs-lookup"><span data-stu-id="088bb-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="088bb-153">Kullanarak bir dizinin öğeleri numaralandırılabilen bir `foreach` deyimi ([foreach deyimi](statements.md#the-foreach-statement)).</span><span class="sxs-lookup"><span data-stu-id="088bb-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="088bb-154">Dizi üyeleri</span><span class="sxs-lookup"><span data-stu-id="088bb-154">Array members</span></span>

<span data-ttu-id="088bb-155">Her dizi türü tarafından bildirilen üyeleri devralan `System.Array` türü.</span><span class="sxs-lookup"><span data-stu-id="088bb-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="088bb-156">Dizi kovaryansı</span><span class="sxs-lookup"><span data-stu-id="088bb-156">Array covariance</span></span>

<span data-ttu-id="088bb-157">İçin herhangi iki *reference_type*s `A` ve `B`örtük bir başvuru dönüştürmesi, ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) ya da açık bir başvuru dönüştürmesi ([ Açık bir başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) öğesinden var. `A` için `B`, aynı başvuru dönüştürmesi da dizi türünden varsa `A[R]` dizi türüne `B[R]`burada `R` herhangi verilen *rank_specifier* (ancak her ikisi için de aynı dizi türleri).</span><span class="sxs-lookup"><span data-stu-id="088bb-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="088bb-158">Bu ilişki olarak da bilinen ***dizi kovaryansı***.</span><span class="sxs-lookup"><span data-stu-id="088bb-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="088bb-159">Dizi kovaryansı özellikle anlamına gelir, bir dizi türünde bir değer `A[R]` aslında bir dizi türünde bir örneğe bir başvuru olabilir `B[R]`, gelen bir örtük bir başvuru dönüştürmesi var sağlanan `B` için `A`.</span><span class="sxs-lookup"><span data-stu-id="088bb-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="088bb-160">Dizi kovaryansı nedeniyle, çalışma zamanı denetimi, bir dizi öğesine atanan değerin aslında izin verilen bir türde olmasını sağlar, başvuru türü dizilerin öğelerine atamaları dahil ([basit atama](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="088bb-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="088bb-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="088bb-161">For example:</span></span>
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

<span data-ttu-id="088bb-162">Atamayı `array[i]` içinde `Fill` örtük olarak includes yöntemi tarafından başvurulan nesne sağlar bir çalışma zamanı denetimi `value` ya da `null` veya gerçeköğetürüileuyumlubirörnek`array`.</span><span class="sxs-lookup"><span data-stu-id="088bb-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="088bb-163">İçinde `Main`, ilk iki çağrılarını `Fill` başarılı, ancak üçüncü çağırma neden bir `System.ArrayTypeMismatchException` ilk atama için yürütme sırasında durum `array[i]`.</span><span class="sxs-lookup"><span data-stu-id="088bb-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="088bb-164">Özel durum oluşur paketlenmiş bir `int` içinde depolanamaz bir `string` dizi.</span><span class="sxs-lookup"><span data-stu-id="088bb-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="088bb-165">Dizi kovaryansı özellikle genişletmiyoruz dizileri için *value_type*s.</span><span class="sxs-lookup"><span data-stu-id="088bb-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="088bb-166">Örneğin, bu izinleri dönüştürme mevcut bir `int[]` olarak kabul edilmesini bir `object[]`.</span><span class="sxs-lookup"><span data-stu-id="088bb-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="088bb-167">Dizi başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="088bb-167">Array initializers</span></span>

<span data-ttu-id="088bb-168">Dizi başlatıcıları, alan bildirimlerinde belirtilebilir ([alanları](classes.md#fields)), yerel değişken bildirimleri ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) ve dizi oluşturma ifadeleri ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)):</span><span class="sxs-lookup"><span data-stu-id="088bb-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

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

<span data-ttu-id="088bb-169">Bir dizi değişken başlatıcılar, kapsadığı bir dizi Başlatıcısı oluşan "`{`"ve"`}`"belirteçler ve ayrılmış"`,`" belirteçleri.</span><span class="sxs-lookup"><span data-stu-id="088bb-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="088bb-170">Her değişken başlatıcı bir ifade olan veya çok boyutlu bir dizi iç içe geçmiş dizi Başlatıcı söz konusu olduğunda.</span><span class="sxs-lookup"><span data-stu-id="088bb-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="088bb-171">Bir dizi Başlatıcısı kullanıldığı bağlamı başlatılmakta dizi türünü belirler.</span><span class="sxs-lookup"><span data-stu-id="088bb-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="088bb-172">Bir dizi oluşturma ifadesi dizi türü hemen Başlatıcı önündeki veya dizi Başlatıcı ifadelerinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="088bb-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="088bb-173">Bir alan veya Değişken bildiriminde dizi türü alan veya bildirilen değişken türüdür.</span><span class="sxs-lookup"><span data-stu-id="088bb-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="088bb-174">Bir dizi başlatıcısı bir alan ya da değişken bildirimi gibi kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="088bb-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="088bb-175">Bu, yalnızca bir eşdeğer dizi oluşturma ifadesi için toplu özelliktir:</span><span class="sxs-lookup"><span data-stu-id="088bb-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="088bb-176">Tek boyutlu bir dizi için bir dizi başlatıcısı, dizinin öğe türü ile uyumlu atama ifadeler bir dizi oluşmalıdır.</span><span class="sxs-lookup"><span data-stu-id="088bb-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="088bb-177">İfadeler, artan düzende sıfır dizinindeki öğe ile başlayarak, dizi öğelerini başlatmak.</span><span class="sxs-lookup"><span data-stu-id="088bb-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="088bb-178">Dizi Başlatıcısı ifadelerin sayısı, dizi örneği oluşturulan uzunluğunu belirler.</span><span class="sxs-lookup"><span data-stu-id="088bb-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="088bb-179">Örneğin, yukarıdaki dizi Başlatıcısı oluşturur bir `int[]` uzunluğu 5 örneğini ve ardından örneği aşağıdaki değerlerle başlatır:</span><span class="sxs-lookup"><span data-stu-id="088bb-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="088bb-180">Çok boyutlu bir dizi için dizi başlatıcıda dizideki boyutların gibi iç içe kadar düzeyleri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="088bb-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="088bb-181">En dıştaki iç içe geçme düzeyi en soldaki boyuta karşılık gelir ve en içteki iç içe geçme düzeyi en sağdaki boyuta karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="088bb-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="088bb-182">Dizinin her boyutunun uzunluğu, dizi başlatıcısında karşılık gelen iç içe geçme düzeyindeki öğelerin sayısı tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="088bb-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="088bb-183">Her iç içe geçmiş dizi Başlatıcısı için öğelerin sayısını bir dizi başlatıcıları aynı düzeyde aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="088bb-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="088bb-184">Örnek:</span><span class="sxs-lookup"><span data-stu-id="088bb-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="088bb-185">beş en soldaki boyutun uzunluğu ve iki en sağdaki boyutun uzunluğu ile iki boyutlu bir dizi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="088bb-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="088bb-186">ve ardından başlatır dizi örneği aşağıdaki değerlerle:</span><span class="sxs-lookup"><span data-stu-id="088bb-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="088bb-187">En sağdaki dışındaki bir boyut sıfır uzunluğunda belirtilmezse, sonraki boyutları sıfır uzunluk de varsayılır.</span><span class="sxs-lookup"><span data-stu-id="088bb-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="088bb-188">Örnek:</span><span class="sxs-lookup"><span data-stu-id="088bb-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="088bb-189">en soldaki ve sağdaki boyutu için sıfır uzunluğuna sahip iki boyutlu bir dizi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="088bb-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="088bb-190">Bir dizi oluşturma ifadesi, hem açık boyut uzunlukları hem de bir dizi Başlatıcısı içerdiğinde, uzunlukları sabit ifadeler olması gerekir ve her iç içe geçme düzeyindeki öğelerin sayısını ilgili boyut uzunluğu eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="088bb-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="088bb-191">Bazı örnekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="088bb-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="088bb-192">Burada, Başlatıcısı `y` boyut uzunluğu ifadesi bir sabit ve Başlatıcısı olmadığı için bir derleme zamanı hatası oluşur `z` sonuçları bir derleme zamanı hatası nedeniyle uzunluğu ve öğe sayısı Başlatıcı kabul.</span><span class="sxs-lookup"><span data-stu-id="088bb-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
