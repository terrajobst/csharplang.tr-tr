---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485520"
---
# <a name="readonly-instance-members"></a><span data-ttu-id="25ae9-101">Salt okunur örnek üyeleri</span><span class="sxs-lookup"><span data-stu-id="25ae9-101">Readonly Instance Members</span></span>

<span data-ttu-id="25ae9-102">Şıed sorun: <https://github.com/dotnet/csharplang/issues/1710></span><span class="sxs-lookup"><span data-stu-id="25ae9-102">Championed Issue: <https://github.com/dotnet/csharplang/issues/1710></span></span>

## <a name="summary"></a><span data-ttu-id="25ae9-103">Özet</span><span class="sxs-lookup"><span data-stu-id="25ae9-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="25ae9-104">Bir yapıda tek bir örnek üye belirtmenin bir yolunu, `readonly struct` hiçbir örnek üye değiştirme durumunu belirtmediğinden aynı şekilde değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="25ae9-104">Provide a way to specify individual instance members on a struct do not modify state, in the same way that `readonly struct` specifies no instance members modify state.</span></span>

<span data-ttu-id="25ae9-105">Bu, `readonly instance member`! = `pure instance member`olduğunu belirtmekte bir değer.</span><span class="sxs-lookup"><span data-stu-id="25ae9-105">It is worth noting that `readonly instance member` != `pure instance member`.</span></span> <span data-ttu-id="25ae9-106">`pure` örnek üyesi, hiçbir durum değiştirilmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-106">A `pure` instance member guarantees no state will be modified.</span></span> <span data-ttu-id="25ae9-107">`readonly` örnek üyesi yalnızca örnek durumunun değiştirilmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-107">A `readonly` instance member only guarantees that instance state will not be modified.</span></span>

<span data-ttu-id="25ae9-108">`readonly struct` tüm örnek üyeleri örtük olarak `readonly instance members`kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-108">All instance members on a `readonly struct` could be considered implicitly `readonly instance members`.</span></span> <span data-ttu-id="25ae9-109">ReadOnly olmayan yapılar üzerinde belirtilen açık `readonly instance members` aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="25ae9-109">Explicit `readonly instance members` declared on non-readonly structs would behave in the same manner.</span></span> <span data-ttu-id="25ae9-110">Örneğin, kendisi salt okunur olmayan bir örnek üye (geçerli örnekte veya örneğin bir alanında) çağrılırsa, hala gizli kopyalar oluşturamazlar.</span><span class="sxs-lookup"><span data-stu-id="25ae9-110">For example, they would still create hidden copies if you called an instance member (on the current instance or on a field of the instance) which was itself not-readonly.</span></span>

## <a name="motivation"></a><span data-ttu-id="25ae9-111">Amacı</span><span class="sxs-lookup"><span data-stu-id="25ae9-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="25ae9-112">Günümüzde, kullanıcılar derleyicinin tüm alanların salt okunur olmasını zorunlu kıldığı `readonly struct` türleri oluşturabilir ve bir örnek üye durumu değiştirmesiz uzantıya göre olur.</span><span class="sxs-lookup"><span data-stu-id="25ae9-112">Today, users have the ability to create `readonly struct` types which the compiler enforces that all fields are readonly (and by extension, that no instance members modify the state).</span></span> <span data-ttu-id="25ae9-113">Ancak, erişilebilir alanları açığa çıkaran veya bir dizi değiştirici ve değişikliğe ilmemiş üye karışımı olan bir API 'ye sahip olduğunuz bazı senaryolar vardır.</span><span class="sxs-lookup"><span data-stu-id="25ae9-113">However, there are some scenarios where you have an existing API that exposes accessible fields or that has a mix of mutating and non-mutating members.</span></span> <span data-ttu-id="25ae9-114">Bu koşullarda, türü `readonly` olarak işaretleyemezsiniz (bir değişiklik).</span><span class="sxs-lookup"><span data-stu-id="25ae9-114">Under these circumstances, you cannot mark the type as `readonly` (it would be a breaking change).</span></span>

<span data-ttu-id="25ae9-115">Bu, `in` parametrelerinin olması dışında normalde çok daha etkili değildir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-115">This normally doesn't have much impact, except in the case of `in` parameters.</span></span> <span data-ttu-id="25ae9-116">ReadOnly olmayan yapılar için `in` parametreleri ile, derleyicinin her örnek üye çağrısı için parametresinin bir kopyasını oluşturacak ve bu, çağrının iç durumu değiştirmediğinden emin olamaz.</span><span class="sxs-lookup"><span data-stu-id="25ae9-116">With `in` parameters for non-readonly structs, the compiler will make a copy of the parameter for each instance member invocation, since it cannot guarantee that the invocation does not modify internal state.</span></span> <span data-ttu-id="25ae9-117">Bu, yapıyı doğrudan değere göre geçirmekten daha fazla sayıda kopyaya ve daha kötülere neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-117">This can lead to a multitude of copies and worse overall performance than if you had just passed the struct directly by value.</span></span> <span data-ttu-id="25ae9-118">Bir örnek için, bu koda bkz. [parça](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span><span class="sxs-lookup"><span data-stu-id="25ae9-118">For an example, see this code on [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span></span>

<span data-ttu-id="25ae9-119">Gizli kopyaların gerçekleşebileceği bazı senaryolar `static readonly fields` ve `literals`içerir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-119">Some other scenarios where hidden copies can occur include `static readonly fields` and `literals`.</span></span> <span data-ttu-id="25ae9-120">Gelecekte destekleniyorsa, `blittable constants` aynı bot 'ta sona erdirmek gerekir; Bu, yapının `readonly`işaretlenmediği bir tam kopyaya (örnek üye çağrısında) sahip olurlar.</span><span class="sxs-lookup"><span data-stu-id="25ae9-120">If they are supported in the future, `blittable constants` would end up in the same boat; that is they all currently necessitate a full copy (on instance member invocation) if the struct is not marked `readonly`.</span></span>

## <a name="design"></a><span data-ttu-id="25ae9-121">Tasarlama</span><span class="sxs-lookup"><span data-stu-id="25ae9-121">Design</span></span>
[design]: #design

<span data-ttu-id="25ae9-122">Bir kullanıcının bir örnek üyesinin, `readonly` ve örneğin durumunu (derleyici tarafından gerçekleştirilen tüm uygun doğrulamayla birlikte) değiştirmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="25ae9-122">Allow a user to specify that an instance member is, itself, `readonly` and does not modify the state of the instance (with all the appropriate verification done by the compiler, of course).</span></span> <span data-ttu-id="25ae9-123">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="25ae9-123">For example:</span></span>

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

<span data-ttu-id="25ae9-124">Salt okunur, `this` erişimciye atlanmayacak olduğunu göstermek için özellik erişimcilerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-124">Readonly can be applied to property accessors to indicate that `this` will not be mutated in the accessor.</span></span> <span data-ttu-id="25ae9-125">Aşağıdaki örneklerde salt okunur ayarlayıcılar vardır çünkü bu erişimciler üye alanının durumunu değiştirir, ancak bu üye alanının değerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="25ae9-125">The following examples have readonly setters because those accessors modify the state of member field, but do not modify the value of that member field.</span></span>

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

<span data-ttu-id="25ae9-126">Özellik söz dizimine `readonly` uygulandığında, tüm erişimcilerinin `readonly`olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-126">When `readonly` is applied to the property syntax, it means that all accessors are `readonly`.</span></span>

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

<span data-ttu-id="25ae9-127">Salt okunur, yalnızca kapsayan türü olmayan erişimcilere uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-127">Readonly can only be applied to accessors which do not mutate the containing type.</span></span>

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

<span data-ttu-id="25ae9-128">Salt okunur bazı otomatik uygulanmış özelliklere uygulanabilir, ancak anlamlı bir etkiye sahip olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="25ae9-128">Readonly can be applied to some auto-implemented properties, but it won't have a meaningful effect.</span></span> <span data-ttu-id="25ae9-129">Derleyici, `readonly` anahtar sözcüğünün var olup olmadığına bakılmaksızın, tüm otomatik uygulanan geticileri ReadOnly olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-129">The compiler will treat all auto-implemented getters as readonly whether or not the `readonly` keyword is present.</span></span>

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

<span data-ttu-id="25ae9-130">ReadOnly, el ile uygulanan olaylara uygulanabilir, ancak alan benzeri olaylara uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="25ae9-130">Readonly can be applied to manually-implemented events, but not field-like events.</span></span> <span data-ttu-id="25ae9-131">Salt okunur, bireysel olay erişimcilerine (Ekle/Kaldır) uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="25ae9-131">Readonly cannot be applied to individual event accessors (add/remove).</span></span>

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

<span data-ttu-id="25ae9-132">Diğer bazı sözdizimi örnekleri:</span><span class="sxs-lookup"><span data-stu-id="25ae9-132">Some other syntax examples:</span></span>

* <span data-ttu-id="25ae9-133">İfade gövdeli Üyeler: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span><span class="sxs-lookup"><span data-stu-id="25ae9-133">Expression bodied members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span></span>
* <span data-ttu-id="25ae9-134">Genel kısıtlamalar: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span><span class="sxs-lookup"><span data-stu-id="25ae9-134">Generic constraints: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span></span>

<span data-ttu-id="25ae9-135">Derleyici örnek üyesini her zamanki gibi yayıp, örnek üyenin durumu değiştirmediğini belirten bir derleyici tanınan özniteliği de yaymalıdır.</span><span class="sxs-lookup"><span data-stu-id="25ae9-135">The compiler would emit the instance member, as usual, and would additionally emit a compiler recognized attribute indicating that the instance member does not modify state.</span></span> <span data-ttu-id="25ae9-136">Bu, gizli `this` parametresinin `ref T`yerine `in T` hale gelmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="25ae9-136">This effectively causes the hidden `this` parameter to become `in T` instead of `ref T`.</span></span>

<span data-ttu-id="25ae9-137">Bu, derleyicinin bir kopya oluşturmak zorunda kalmadan, kullanıcının söyleme yöntemini güvenle çağırmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-137">This would allow the user to safely call said instance method without the compiler needing to make a copy.</span></span>

<span data-ttu-id="25ae9-138">Kısıtlamalar şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="25ae9-138">The restrictions would include:</span></span>

* <span data-ttu-id="25ae9-139">`readonly` değiştiricisi statik yöntemlere, oluşturuculara veya yıkıcılara uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="25ae9-139">The `readonly` modifier cannot be applied to static methods, constructors or destructors.</span></span>
* <span data-ttu-id="25ae9-140">`readonly` değiştiricisi temsilcilere uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="25ae9-140">The `readonly` modifier cannot be applied to delegates.</span></span>
* <span data-ttu-id="25ae9-141">`readonly` değiştiricisi, sınıf veya arabirim üyelerine uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="25ae9-141">The `readonly` modifier cannot be applied to members of class or interface.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="25ae9-142">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="25ae9-142">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="25ae9-143">Günümüzde `readonly struct` yöntemlerle aynı sakıncalar var.</span><span class="sxs-lookup"><span data-stu-id="25ae9-143">Same drawbacks as exist with `readonly struct` methods today.</span></span> <span data-ttu-id="25ae9-144">Bazı kodlar hala gizli kopyalara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-144">Certain code may still cause hidden copies.</span></span>

## <a name="notes"></a><span data-ttu-id="25ae9-145">Notlar</span><span class="sxs-lookup"><span data-stu-id="25ae9-145">Notes</span></span>
[notes]: #notes

<span data-ttu-id="25ae9-146">Bir öznitelik ya da başka bir anahtar sözcük kullanılması da mümkün olabilir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-146">Using an attribute or another keyword may also be possible.</span></span>

<span data-ttu-id="25ae9-147">Bu teklif, `functional purity` ve/veya `constant expressions`bir kısmı ile ilgilidir ve her ikisi de bazı mevcut tekliflere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="25ae9-147">This proposal is somewhat related to (but is more a subset of) `functional purity` and/or `constant expressions`, both of which have had some existing proposals.</span></span>
