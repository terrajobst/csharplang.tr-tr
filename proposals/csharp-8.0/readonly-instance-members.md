---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485520"
---
# <a name="readonly-instance-members"></a>Salt okunur örnek üyeleri

Şıed sorun: <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>Özet
[summary]: #summary

Bir yapıda tek bir örnek üye belirtmenin bir yolunu, `readonly struct` hiçbir örnek üye değiştirme durumunu belirtmediğinden aynı şekilde değiştirmeyin.

Bu, `readonly instance member`! = `pure instance member`olduğunu belirtmekte bir değer. `pure` örnek üyesi, hiçbir durum değiştirilmeyecektir. `readonly` örnek üyesi yalnızca örnek durumunun değiştirilmeyecektir.

`readonly struct` tüm örnek üyeleri örtük olarak `readonly instance members`kabul edilebilir. ReadOnly olmayan yapılar üzerinde belirtilen açık `readonly instance members` aynı şekilde davranır. Örneğin, kendisi salt okunur olmayan bir örnek üye (geçerli örnekte veya örneğin bir alanında) çağrılırsa, hala gizli kopyalar oluşturamazlar.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Günümüzde, kullanıcılar derleyicinin tüm alanların salt okunur olmasını zorunlu kıldığı `readonly struct` türleri oluşturabilir ve bir örnek üye durumu değiştirmesiz uzantıya göre olur. Ancak, erişilebilir alanları açığa çıkaran veya bir dizi değiştirici ve değişikliğe ilmemiş üye karışımı olan bir API 'ye sahip olduğunuz bazı senaryolar vardır. Bu koşullarda, türü `readonly` olarak işaretleyemezsiniz (bir değişiklik).

Bu, `in` parametrelerinin olması dışında normalde çok daha etkili değildir. ReadOnly olmayan yapılar için `in` parametreleri ile, derleyicinin her örnek üye çağrısı için parametresinin bir kopyasını oluşturacak ve bu, çağrının iç durumu değiştirmediğinden emin olamaz. Bu, yapıyı doğrudan değere göre geçirmekten daha fazla sayıda kopyaya ve daha kötülere neden olabilir. Bir örnek için, bu koda bkz. [parça](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)

Gizli kopyaların gerçekleşebileceği bazı senaryolar `static readonly fields` ve `literals`içerir. Gelecekte destekleniyorsa, `blittable constants` aynı bot 'ta sona erdirmek gerekir; Bu, yapının `readonly`işaretlenmediği bir tam kopyaya (örnek üye çağrısında) sahip olurlar.

## <a name="design"></a>Tasarlama
[design]: #design

Bir kullanıcının bir örnek üyesinin, `readonly` ve örneğin durumunu (derleyici tarafından gerçekleştirilen tüm uygun doğrulamayla birlikte) değiştirmesine izin verin. Örneğin:

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

Salt okunur, `this` erişimciye atlanmayacak olduğunu göstermek için özellik erişimcilerine uygulanabilir. Aşağıdaki örneklerde salt okunur ayarlayıcılar vardır çünkü bu erişimciler üye alanının durumunu değiştirir, ancak bu üye alanının değerini değiştirmez.

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

Özellik söz dizimine `readonly` uygulandığında, tüm erişimcilerinin `readonly`olduğu anlamına gelir.

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

Salt okunur, yalnızca kapsayan türü olmayan erişimcilere uygulanabilir.

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

Salt okunur bazı otomatik uygulanmış özelliklere uygulanabilir, ancak anlamlı bir etkiye sahip olmayacaktır. Derleyici, `readonly` anahtar sözcüğünün var olup olmadığına bakılmaksızın, tüm otomatik uygulanan geticileri ReadOnly olarak değerlendirir.

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

ReadOnly, el ile uygulanan olaylara uygulanabilir, ancak alan benzeri olaylara uygulanmaz. Salt okunur, bireysel olay erişimcilerine (Ekle/Kaldır) uygulanamaz.

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

Diğer bazı sözdizimi örnekleri:

* İfade gövdeli Üyeler: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* Genel kısıtlamalar: `public static readonly void GenericMethod<T>(T value) where T : struct { }`

Derleyici örnek üyesini her zamanki gibi yayıp, örnek üyenin durumu değiştirmediğini belirten bir derleyici tanınan özniteliği de yaymalıdır. Bu, gizli `this` parametresinin `ref T`yerine `in T` hale gelmesine neden olur.

Bu, derleyicinin bir kopya oluşturmak zorunda kalmadan, kullanıcının söyleme yöntemini güvenle çağırmasına izin verir.

Kısıtlamalar şunları içerir:

* `readonly` değiştiricisi statik yöntemlere, oluşturuculara veya yıkıcılara uygulanamaz.
* `readonly` değiştiricisi temsilcilere uygulanamaz.
* `readonly` değiştiricisi, sınıf veya arabirim üyelerine uygulanamaz.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Günümüzde `readonly struct` yöntemlerle aynı sakıncalar var. Bazı kodlar hala gizli kopyalara neden olabilir.

## <a name="notes"></a>Notlar
[notes]: #notes

Bir öznitelik ya da başka bir anahtar sözcük kullanılması da mümkün olabilir.

Bu teklif, `functional purity` ve/veya `constant expressions`bir kısmı ile ilgilidir ve her ikisi de bazı mevcut tekliflere sahiptir.
