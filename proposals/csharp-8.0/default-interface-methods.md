---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485184"
---
# <a name="default-interface-methods"></a><span data-ttu-id="eabd0-101">varsayılan arabirim yöntemleri</span><span class="sxs-lookup"><span data-stu-id="eabd0-101">default interface methods</span></span>

* <span data-ttu-id="eabd0-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="eabd0-102">[x] Proposed</span></span>
* <span data-ttu-id="eabd0-103">[] Prototip: [devam ediyor](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span><span class="sxs-lookup"><span data-stu-id="eabd0-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="eabd0-104">[] Uygulama: yok</span><span class="sxs-lookup"><span data-stu-id="eabd0-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="eabd0-105">[] Belirtim: devam ediyor, aşağıda</span><span class="sxs-lookup"><span data-stu-id="eabd0-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="eabd0-106">Özet</span><span class="sxs-lookup"><span data-stu-id="eabd0-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="eabd0-107">_Sanal uzantı yöntemleri_ için destek ekleyin-somut uygulamalarla arabirimlerdeki Yöntemler.</span><span class="sxs-lookup"><span data-stu-id="eabd0-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="eabd0-108">Bu tür bir arabirimi uygulayan bir sınıf veya yapının, sınıf veya yapı tarafından uygulanan ya da temel sınıflarından veya arabirimlerinden Devralındığı arabirim yöntemi için tek bir _en özel_ uygulamaya sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="eabd0-109">Sanal uzantı yöntemleri bir API yazarının, bu arabirimin var olan uygulamalarıyla kaynak veya ikili uyumluluğu bozmadan gelecek sürümlerde bir arabirime Yöntemler eklemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="eabd0-110">Bunlar, Java 'nın ["varsayılan yöntemlere"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)benzerdir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="eabd0-111">(Olası uygulama tekniğinin temel alınarak), bu özellik CLı/CLR 'de ilgili desteği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="eabd0-112">Bu özellikten faydalanan programlar platformun önceki sürümlerinde çalıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="eabd0-113">Amacı</span><span class="sxs-lookup"><span data-stu-id="eabd0-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="eabd0-114">Bu özelliğe ilişkin asıl lamalar şunlardır</span><span class="sxs-lookup"><span data-stu-id="eabd0-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="eabd0-115">Varsayılan arabirim yöntemleri bir API yazarının, bu arabirimin var olan uygulamalarıyla kaynak veya ikili uyumluluğu bozmadan gelecek sürümlerde bir arabirime Yöntemler eklemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="eabd0-116">Özelliği, benzer C# özellikleri destekleyen [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) ve [IOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)hedefleyen API 'lerle birlikte çalışabilmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="eabd0-117">, Varsayılan arabirim uygulamalarını eklemek, "nitelikler" dil özelliğinin (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>) öğelerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="eabd0-118">Nitelikler güçlü bir programlama tekniği (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>) olarak kanıtlanmış bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="eabd0-119">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="eabd0-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="eabd0-120">Bir arabirimin sözdizimi izin verecek şekilde genişletilir</span><span class="sxs-lookup"><span data-stu-id="eabd0-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="eabd0-121">sabitleri, işleçleri, statik oluşturucuları ve iç içe geçmiş türleri bildiren üye bildirimleri;</span><span class="sxs-lookup"><span data-stu-id="eabd0-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="eabd0-122">bir yöntem veya Dizin Oluşturucu, özellik veya olay erişimcisi (yani "varsayılan" bir uygulama) için *gövde* .</span><span class="sxs-lookup"><span data-stu-id="eabd0-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="eabd0-123">statik alanlar, Yöntemler, özellikler, Dizin oluşturucular ve olaylar bildiren üye bildirimleri;</span><span class="sxs-lookup"><span data-stu-id="eabd0-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="eabd0-124">Açık arabirim uygulama söz dizimini kullanan üye bildirimleri; '</span><span class="sxs-lookup"><span data-stu-id="eabd0-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="eabd0-125">Açık erişim değiştiricileri (varsayılan erişim `public`).</span><span class="sxs-lookup"><span data-stu-id="eabd0-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="eabd0-126">Gövdeler içeren Üyeler, arabirimin, geçersiz kılan bir uygulama sağlamayan sınıflarda ve yapılarda yöntemi için "varsayılan" bir uygulama sağlamasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="eabd0-127">Arabirimler örnek durumu içermeyebilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="eabd0-128">Statik alanlara artık izin verildiğinde, arabirimlerde örnek alanlarına izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="eabd0-129">Örnek otomatik özellikler, açıkça gizli bir alan bildirdiklerinde arabirimlerde desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="eabd0-130">Statik ve özel yöntemler, arabirimin ortak API 'sini uygulamak için kullanılan kodun faydalı yeniden düzenlemesi ve organizasyona izin verir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="eabd0-131">Bir arabirimdeki yöntem geçersiz kılma açık arabirim uygulama sözdizimini kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="eabd0-132">Bir *variance_annotation*ile tanımlanmış bir tür parametresinin kapsamı içinde bir sınıf türü, yapı türü veya sabit listesi türü bildirmek hatadır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="eabd0-133">Örneğin, aşağıdaki `C` bildirimi bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="eabd0-134">Arabirimlerde somut Yöntemler</span><span class="sxs-lookup"><span data-stu-id="eabd0-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="eabd0-135">Bu özelliğin en basit biçimi, gövde içeren bir yöntem olan bir arabirimde *somut bir yöntem* bildirebilmesidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="eabd0-136">Bu arabirimi uygulayan bir sınıf somut metodunu gerçekleştirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="eabd0-137">`C` sınıfındaki `IA.M` için nihai geçersiz kılma, `IA`içinde bildirildiği somut yöntemdir `M`.</span><span class="sxs-lookup"><span data-stu-id="eabd0-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="eabd0-138">Bir sınıfın, arabirimlerinden üyeleri devralmasını unutmayın; Bu özellik tarafından değiştirilmez:</span><span class="sxs-lookup"><span data-stu-id="eabd0-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="eabd0-139">Bir arabirimin örnek üyesi içinde, `this` kapsayan arabirimin türü vardır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="eabd0-140">Arabirimlerde değiştiriciler</span><span class="sxs-lookup"><span data-stu-id="eabd0-140">Modifiers in interfaces</span></span>

<span data-ttu-id="eabd0-141">Bir arabirimin sözdizimi, üyelerinde değiştiricilere izin vermek için gevşek olur.</span><span class="sxs-lookup"><span data-stu-id="eabd0-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="eabd0-142">Aşağıdakiler izin verilir: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`ve `partial`.</span><span class="sxs-lookup"><span data-stu-id="eabd0-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="eabd0-143">***Todo***: başka değiştiricilerin var olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="eabd0-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="eabd0-144">Bildirimi içeren bir arabirim üyesi, `sealed` veya `private` değiştiricisi kullanılmadığı müddetçe bir `virtual` üyesidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="eabd0-145">`virtual` değiştiricisi, aksi takdirde örtük olarak `virtual`bir işlev üyesinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="eabd0-146">Benzer şekilde, `abstract` gövdeler olmadan arabirim üyelerinde varsayılan değer olsa da, bu değiştirici açıkça verilebilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="eabd0-147">Sanal olmayan bir üye `sealed` anahtar sözcüğü kullanılarak bildirilebilecek.</span><span class="sxs-lookup"><span data-stu-id="eabd0-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="eabd0-148">Bir arabirimin `private` veya `sealed` işlev üyesinin gövdeye sahip olmaması hatadır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="eabd0-149">`private` işlev üyesi değiştirici `sealed`sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="eabd0-150">Erişim değiştiricileri, izin verilen tüm üye türlerinin arabirim üyelerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="eabd0-151">Erişim düzeyi `public` varsayılandır ancak açık olarak verilebilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="eabd0-152">***Açık sorun:*** `protected` ve `internal`gibi erişim değiştiricilerin kesin anlamlarını belirtmemiz ve bu bildirimleri hangi bildirimlerin (türetilmiş bir arabirimde) ve bunların (örneğin, arabirimini uygulayan bir sınıfta) (türetilen bir arabirimde) ve bunların nasıl uygulanacağını belirtmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="eabd0-153">Arabirimler, iç içe türler, Yöntemler, Dizin oluşturucular, özellikler, olaylar ve statik oluşturucular dahil `static` üyelerini bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="eabd0-154">Tüm arabirim üyeleri için varsayılan erişim düzeyi `public`.</span><span class="sxs-lookup"><span data-stu-id="eabd0-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="eabd0-155">Arabirimler örnek oluşturucular, Yıkıcılar veya alanlar bildiremeyebilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="eabd0-156">***Kapatılan sorun:*** Bir arabirimde işleç bildirimlerine izin verilmelidir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="eabd0-157">Büyük olasılıkla dönüştürme işleçleri değil, diğerleri ne olacak?</span><span class="sxs-lookup"><span data-stu-id="eabd0-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="eabd0-158">***Karar***: işleçlere dönüştürme, eşitlik ve eşitsizlik işleçleri *haricinde* izin verilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="eabd0-159">***Kapatılan sorun:*** Temel arabirimlerin üyelerini gizleyen arabirim üye bildirimlerinde `new` izin verilmelidir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="eabd0-160">***Karar***: Evet.</span><span class="sxs-lookup"><span data-stu-id="eabd0-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="eabd0-161">***Kapatılan sorun:*** Şu anda bir arabirim veya üyeleri üzerinde `partial` izin vermedik.</span><span class="sxs-lookup"><span data-stu-id="eabd0-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="eabd0-162">Bu, ayrı bir teklif gerektirir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-162">That would require a separate proposal.</span></span> <span data-ttu-id="eabd0-163">***Karar***: Evet.</span><span class="sxs-lookup"><span data-stu-id="eabd0-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="eabd0-164">Arabirimlerde geçersiz kılmalar</span><span class="sxs-lookup"><span data-stu-id="eabd0-164">Overrides in interfaces</span></span>

<span data-ttu-id="eabd0-165">Geçersiz kılma bildirimleri (yani `override` değiştiricisini içeren olanlar), programcının derleyicinin veya çalışma zamanının başka bir yerde bulamadığı bir arabirimde bir sanal üyenin en belirli bir uygulamasını sağlamasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="eabd0-166">Ayrıca, bir üst arabirimden bir Özet üyenin türetilmiş arabirimdeki varsayılan üyeye açılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="eabd0-167">Bir geçersiz kılma bildiriminin, arabirimi adıyla bildirimi niteleyerek belirli bir temel arabirim yöntemini *açıkça* geçersiz kılmasına izin verilir (Bu durumda erişim değiştiricisine izin verilmez).</span><span class="sxs-lookup"><span data-stu-id="eabd0-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="eabd0-168">Örtük geçersiz kılmalara izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-168">Implicit overrides are not permitted.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

<span data-ttu-id="eabd0-169">Arabirimlerdeki geçersiz kılma bildirimleri `sealed`bildirilmemiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="eabd0-170">Bir arabirimdeki ortak `virtual` işlev üyeleri, açıkça bir türetilmiş arabirimde geçersiz kılınabilir (geçersiz kılma bildiriminde adı, ilk olarak yöntemi tanımlayan arabirim türü ile niteleyerek ve bir erişim değiştiricisi hariç).</span><span class="sxs-lookup"><span data-stu-id="eabd0-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="eabd0-171">bir arabirimdeki `virtual` işlev üyeleri yalnızca türetilmiş arabirimlerde açıkça geçersiz kılınabilir (örtük değil) ve `public` olmayan üyeler yalnızca bir sınıf veya yapıda uygulanabilir (örtük değil).</span><span class="sxs-lookup"><span data-stu-id="eabd0-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="eabd0-172">Her iki durumda da, geçersiz kılınan veya uygulanan üyenin geçersiz kılındığı yerde *erişilebilir* olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="eabd0-173">Reabstraction</span><span class="sxs-lookup"><span data-stu-id="eabd0-173">Reabstraction</span></span>

<span data-ttu-id="eabd0-174">Bir arabirimde belirtilen sanal (somut) bir yöntem, türetilmiş bir arabirimde Özet olması için geçersiz kılınabilir</span><span class="sxs-lookup"><span data-stu-id="eabd0-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

<span data-ttu-id="eabd0-175">`abstract` değiştiricisi, `IB.M` bildiriminde gerekli değildir (Bu, arabirimlerde varsayılandır), ancak büyük olasılıkla bir geçersiz kılma bildiriminde açık olması iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="eabd0-176">Bu, bir yöntemin varsayılan uygulamasının uygunsuz olduğu ve sınıflar uygulanarak daha uygun bir uygulamanın sağlanması gereken türetilmiş arabirimlerde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="eabd0-177">***Açık sorun:*** Yeniden izin verilmelidir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="eabd0-178">En özel geçersiz kılma kuralı</span><span class="sxs-lookup"><span data-stu-id="eabd0-178">The most specific override rule</span></span>

<span data-ttu-id="eabd0-179">Her arabirim ve sınıfın, türde veya doğrudan ve dolaylı arabirimlerde görüntülenen geçersiz kılmalar arasında her sanal üye için *en belirgin bir geçersiz kılmalara* sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="eabd0-180">*En özel geçersiz* kılma, tüm diğer geçersiz kılmada daha belirgin olan benzersiz bir geçersiz kılma.</span><span class="sxs-lookup"><span data-stu-id="eabd0-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="eabd0-181">Geçersiz kılma yoksa, üyenin en belirgin geçersiz kılma kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="eabd0-182">Bir geçersiz kılma `M1` başka bir geçersiz kıltan *daha belirgin* kabul edilir `M2` `M1` tür `T1`bildiriminde bildirilirse, `M2` tür `T2`olarak bildirilirse ve her iki</span><span class="sxs-lookup"><span data-stu-id="eabd0-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="eabd0-183">`T1` doğrudan veya dolaylı arabirimleri arasında `T2` içerir ya da</span><span class="sxs-lookup"><span data-stu-id="eabd0-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="eabd0-184">`T2` bir arabirim türüdür, ancak `T1` bir arabirim türü değil.</span><span class="sxs-lookup"><span data-stu-id="eabd0-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="eabd0-185">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="eabd0-185">For example:</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

<span data-ttu-id="eabd0-186">En özel geçersiz kılma kuralı bir çakışmanın (Yani elmas devralım işleminden kaynaklanan bir belirsizlik), çakışmanın gerçekleştiği noktada programcı tarafından açıkça çözümlenmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="eabd0-187">Arabirimlerde açık soyut geçersiz kılmaları destekliyoruz, bu işlemleri de sınıflarda yapabiliriz</span><span class="sxs-lookup"><span data-stu-id="eabd0-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="eabd0-188">***Açık sorun***: sınıflarda açık arabirim soyut geçersiz kılmalarını destekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="eabd0-189">Ayrıca, bir sınıf bildiriminde bir hata olduğundan, bazı arabirim yönteminin en belirgin geçersiz kılma işlemi bir arabirimde tanımlanmış soyut bir geçersiz kılma niteliğindedir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="eabd0-190">Bu, yeni terminoloji kullanılarak yeniden kullanılan mevcut bir kuraldır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="eabd0-191">Bir arabirimde belirtilen bir sanal özelliğin, bir arabirimdeki `get` erişimcisi için en belirli bir geçersiz kılma ve farklı bir arabirimdeki `set` erişimcisi için en özel geçersiz kılma olması mümkündür.</span><span class="sxs-lookup"><span data-stu-id="eabd0-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="eabd0-192">Bu, *en özel geçersiz kılma* kuralı ihlali olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="eabd0-193">`static` ve `private` yöntemleri</span><span class="sxs-lookup"><span data-stu-id="eabd0-193">`static` and `private` methods</span></span>

<span data-ttu-id="eabd0-194">Arabirimler artık yürütülebilir kod içerebileceğinden, özel ve statik yöntemlere soyut ortak kod de faydalı olabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="eabd0-195">Artık arabirimlerde bunlara izin veriyoruz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="eabd0-196">***Kapatılan sorun***: özel yöntemleri desteklemelidir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="eabd0-197">Statik yöntemler destekliyoruz mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-197">Should we support static methods?</span></span> <span data-ttu-id="eabd0-198">**Karar: Evet**</span><span class="sxs-lookup"><span data-stu-id="eabd0-198">**Decision: YES**</span></span>

> <span data-ttu-id="eabd0-199">***Açık sorun***: arabirim yöntemlerinin `protected` veya `internal` ya da başka bir erişime izin vermemiz gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="eabd0-200">Öyleyse, semantik anlamı nedir?</span><span class="sxs-lookup"><span data-stu-id="eabd0-200">If so, what are the semantics?</span></span> <span data-ttu-id="eabd0-201">Bunlar varsayılan olarak `virtual` mı?</span><span class="sxs-lookup"><span data-stu-id="eabd0-201">Are they `virtual` by default?</span></span> <span data-ttu-id="eabd0-202">Öyleyse, bunları sanal olmayan hale getirmenin bir yolu var mı?</span><span class="sxs-lookup"><span data-stu-id="eabd0-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="eabd0-203">***Açık sorun***: statik yöntemleri destekliyoruz, (statik) işleçlerini destekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="eabd0-204">Temel arabirim etkinleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="eabd0-204">Base interface invocations</span></span>

<span data-ttu-id="eabd0-205">Varsayılan bir yöntemi olan bir arabirimden türetilen bir türdeki kod, bu arabirimin "temel" uygulamasını açıkça çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

<span data-ttu-id="eabd0-206">Örnek (statik olmayan) yönteminin, `base(Type).M`söz dizimi kullanılarak adlandırılarak doğrudan temel arabirimdeki erişilebilir bir örnek yönteminin uygulanmasını çağırmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="eabd0-207">Bu, elmas devralma nedeniyle belirli bir temel uygulamaya temsilci ile çözülebildiğinden, bunun için gerekli olan bir geçersiz kılma sağlanması yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="eabd0-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

<span data-ttu-id="eabd0-208">`virtual` veya `abstract` üyesine sözdizimi `base(Type).M`kullanılarak erişildiğinde, `Type` `M`için benzersiz bir *en özel geçersiz kılma* içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="eabd0-209">Temel yan tümceleri bağlama</span><span class="sxs-lookup"><span data-stu-id="eabd0-209">Binding base clauses</span></span>

<span data-ttu-id="eabd0-210">Arabirimler artık türler içerir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-210">Interfaces now contain types.</span></span>  <span data-ttu-id="eabd0-211">Bu türler taban yan tümcesinde temel arabirimler olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="eabd0-212">Bir taban yan tümcesini bağlarken, bu türleri bağlamak için temel arabirimlerin kümesini bilmemiz gerekebilir (örneğin, bunlarda arama yapmak ve korumalı erişimi çözümlemek için).</span><span class="sxs-lookup"><span data-stu-id="eabd0-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="eabd0-213">Bir arabirimin temel yan tümcesinin anlamı, bu nedenle devre dışı olarak tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="eabd0-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="eabd0-214">Döngüyü bölmek için, sınıflar için zaten var olan benzer bir kurala karşılık gelen yeni bir dil kuralı ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="eabd0-215">Bir arabirimin *interface_base* anlamı belirlenirken, temel arabirimlerin geçici olarak boş olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="eabd0-216">Intuicanlı bu, bir taban yan tümcesinin anlamını özyinelemeli olarak bağımlı olmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="eabd0-217">**Aşağıdaki kurallara sahip olmak için kullandık:**</span><span class="sxs-lookup"><span data-stu-id="eabd0-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="eabd0-218">"Bir sınıf B bir sınıftan türetildiği zaman, A için bir derleme zamanı hatası, B 'ye bağımlıdır. Bir sınıf doğrudan temel sınıfına (varsa) **bağlıdır** ve doğrudan iç içe geçmiş (varsa) ~~**sınıfa**~~ **bağlıdır** .</span><span class="sxs-lookup"><span data-stu-id="eabd0-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="eabd0-219">Bu tanım verildiğinde, bir sınıfın bağımlı olduğu ~~**tüm sınıf kümesi**~~ , **doğrudan ilişkiye bağlı olarak değişir** . "</span><span class="sxs-lookup"><span data-stu-id="eabd0-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="eabd0-220">Bir arabirimin doğrudan veya dolaylı olarak kendisinden devraldığı derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="eabd0-221">Bir arabirimin **temel arabirimleri** , açık temel arabirimler ve bunların temel arabirimlerdir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="eabd0-222">Diğer bir deyişle, temel arabirimlerin kümesi açık temel arabirimlerin, kendi açık temel arabirimlerinin ve benzeri tüm geçişli kapamelerden oluşur.</span><span class="sxs-lookup"><span data-stu-id="eabd0-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="eabd0-223">**Bunları aşağıdaki şekilde ayarlamamız:**</span><span class="sxs-lookup"><span data-stu-id="eabd0-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="eabd0-224">B sınıfı A sınıfından türediği zaman, A için bir derleme zamanı hatası, B 'ye bağımlıdır. Bir sınıf doğrudan temel sınıfına (varsa) **bağlıdır** ve doğrudan iç içe geçmiş (varsa) _**türüne**_ **bağlıdır** .</span><span class="sxs-lookup"><span data-stu-id="eabd0-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="eabd0-225">Bir arabirim arabirim bir arabirimini genişlettiği zaman, IA 'nin IB 'e bağlı olması için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="eabd0-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="eabd0-226">Bir arabirim doğrudan temel arabirimlerine (varsa) **bağlıdır** ve doğrudan iç içe geçmiş (varsa) türüne **bağlıdır** .</span><span class="sxs-lookup"><span data-stu-id="eabd0-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="eabd0-227">Bu tanımlar verildiğinde, bir türün bağımlı olduğu tüm **türlerin** kümesi, **doğrudan ilişkiye bağlı olarak değişir** .</span><span class="sxs-lookup"><span data-stu-id="eabd0-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="eabd0-228">Mevcut programlarla ilgili etki</span><span class="sxs-lookup"><span data-stu-id="eabd0-228">Effect on existing programs</span></span>

<span data-ttu-id="eabd0-229">Burada sunulan kuralların, var olan programların anlamı üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="eabd0-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="eabd0-230">Örnek 1:</span><span class="sxs-lookup"><span data-stu-id="eabd0-230">Example 1:</span></span>

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

<span data-ttu-id="eabd0-231">Örnek 2:</span><span class="sxs-lookup"><span data-stu-id="eabd0-231">Example 2:</span></span>

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

<span data-ttu-id="eabd0-232">Aynı kurallar, varsayılan arabirim yöntemlerini içeren benzer duruma benzer sonuçlar verir:</span><span class="sxs-lookup"><span data-stu-id="eabd0-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> <span data-ttu-id="eabd0-233">***Kapatılan sorun***: Bu, belirtimin amaçlanan bir sonucu olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="eabd0-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="eabd0-234">**Karar: Evet**</span><span class="sxs-lookup"><span data-stu-id="eabd0-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="eabd0-235">Çalışma zamanı Yöntem çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="eabd0-235">Runtime method resolution</span></span>

> <span data-ttu-id="eabd0-236">***Kapatılan sorun:*** Spec, çalışma zamanı yöntemi çözüm algoritmasını arabirim varsayılan yöntemlerinin yüzündeki betimlemelidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="eabd0-237">Sözdizimi semantiğinin uyumlu olduğundan emin olunması gerekir, örneğin, hangi tanımlanmış yöntemler, bir `internal` metodunu geçersiz kılmaz ve uygulamaz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="eabd0-238">CLR desteği API 'SI</span><span class="sxs-lookup"><span data-stu-id="eabd0-238">CLR support API</span></span>

<span data-ttu-id="eabd0-239">Derleyicilerin, bu özelliği destekleyen bir çalışma zamanı için derlendikleri zamanı tespit etmek için, bu tür çalışma zamanları için kitaplıklar, <https://github.com/dotnet/corefx/issues/17116>açıklanan API aracılığıyla tanıtmak için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="eabd0-240">Ekliyoruz</span><span class="sxs-lookup"><span data-stu-id="eabd0-240">We add</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> <span data-ttu-id="eabd0-241">***Açık sorun***: *clr* özelliği için en iyi ad midir?</span><span class="sxs-lookup"><span data-stu-id="eabd0-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="eabd0-242">CLR özelliği, tıpkı çok daha fazlasını yapar (örneğin, koruma kısıtlamalarını yeniden hedefleyen, arabirimlerde geçersiz kılmaları destekler, vb.).</span><span class="sxs-lookup"><span data-stu-id="eabd0-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="eabd0-243">Belki de "arabirimlerde somut Yöntemler" veya "nitelikler" gibi bir şey çağrılmalıdır mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="eabd0-244">Daha fazla belirtilecek alan</span><span class="sxs-lookup"><span data-stu-id="eabd0-244">Further areas to be specified</span></span>

- <span data-ttu-id="eabd0-245">[] Var olan arabirimlere varsayılan arabirim yöntemleri ve geçersiz kılmalar eklenerek oluşan kaynak ve ikili uyumluluk etkileri türlerini kataloglamak yararlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="eabd0-246">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="eabd0-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="eabd0-247">Bu teklif, CLR belirtimine yönelik eşgüdümlü bir güncelleştirme gerektirir (arabirimlerde ve Yöntem çözümlemede somut yöntemleri desteklemek için).</span><span class="sxs-lookup"><span data-stu-id="eabd0-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="eabd0-248">Bu nedenle oldukça "pahalıdır" ve ayrıca, aynı zamanda da tahmin ettiğimiz diğer özelliklerle birlikte, CLR değişiklikleri gerektirdiğini de büyük bir şekilde yapmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="eabd0-249">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="eabd0-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="eabd0-250">Yok.</span><span class="sxs-lookup"><span data-stu-id="eabd0-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="eabd0-251">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="eabd0-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="eabd0-252">Açık sorulara, yukarıdaki teklifle ilgili olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="eabd0-253">Ayrıca bkz. açık soruların listesi için <https://github.com/dotnet/csharplang/issues/406>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="eabd0-254">Ayrıntılı belirtim, çağrılacak kesin yöntemi seçmek için çalışma zamanında kullanılan çözüm mekanizmasını açıklamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="eabd0-255">Yeni derleyiciler tarafından üretilen ve eski derleyiciler tarafından tüketilen meta verilerin etkileşimi ayrıntılı olarak kullanıma hazır olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="eabd0-256">Örneğin, kullandığımız meta veri gösteriminin, daha eski bir derleyici tarafından derlendiğinde, bu arabirimi uygulayan mevcut bir sınıfı bölmek için bir arabirimdeki varsayılan uygulamanın eklenmesine neden olmadığından emin olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="eabd0-257">Bu, kullanabilmemiz için meta veri gösterimini etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="eabd0-258">Tasarım, diğer diller için diğer diller ve mevcut derleyicilerle birlikte çalışabilirliği göz önünde bulundurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="eabd0-259">Çözümlenen sorular</span><span class="sxs-lookup"><span data-stu-id="eabd0-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="eabd0-260">Soyut geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="eabd0-260">Abstract Override</span></span>

<span data-ttu-id="eabd0-261">Önceki taslak belirtimi devralınmış bir yöntemi "reabstract" olarak içeriyordu:</span><span class="sxs-lookup"><span data-stu-id="eabd0-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

<span data-ttu-id="eabd0-262">2017-03-20 için notlarım buna izin vermemeye karar verdiğimiz gösterildi.</span><span class="sxs-lookup"><span data-stu-id="eabd0-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="eabd0-263">Ancak, bunun için en az iki kullanım durumu vardır:</span><span class="sxs-lookup"><span data-stu-id="eabd0-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="eabd0-264">Bu özelliğin bazı kullanıcılarının birlikte çalışabilme özelliği olan Java API 'Leri, bu tesise bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="eabd0-265">Bu, *nitelikleri* avantajlarıyla programlama.</span><span class="sxs-lookup"><span data-stu-id="eabd0-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="eabd0-266">Reabstraction, "nitelikler" dil özelliğinin (https://en.wikipedia.org/wiki/Trait_(computer_programming))öğelerinden biridir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="eabd0-267">Sınıflarla şunlar için izin verilir:</span><span class="sxs-lookup"><span data-stu-id="eabd0-267">The following is permitted with classes:</span></span>

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

<span data-ttu-id="eabd0-268">Ne yazık ki bu kod, izin verilmediği sürece bir arabirim kümesi olarak yeniden düzenlenmiş (nitelikler) olamaz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="eabd0-269">*Gri olan Juned ilkesi*tarafından izin verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="eabd0-270">***Kapatılan sorun:*** Yeniden izin verilmelidir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="eabd0-271">Yes Notlarım yanlış.</span><span class="sxs-lookup"><span data-stu-id="eabd0-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="eabd0-272">[LDM notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) , bir arabirimde reabstraction 'ın izin verileceğini varsayalım.</span><span class="sxs-lookup"><span data-stu-id="eabd0-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="eabd0-273">Bir sınıfta değil.</span><span class="sxs-lookup"><span data-stu-id="eabd0-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="eabd0-274">Sanal değiştirici ile korumalı değiştirici</span><span class="sxs-lookup"><span data-stu-id="eabd0-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="eabd0-275">[Aleksey Tsingauz](https://github.com/AlekseyTs)öğesinden:</span><span class="sxs-lookup"><span data-stu-id="eabd0-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="eabd0-276">Bazı bunların izin verilmediği bir neden olmadıkça, arabirim üyelerinde açıkça belirtilen değiştiricilere izin vermeyi karardık.</span><span class="sxs-lookup"><span data-stu-id="eabd0-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="eabd0-277">Bu, sanal değiştiricinin etrafında ilginç bir soru getirir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="eabd0-278">Varsayılan uygulama olan üyelerde gerekli olması gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="eabd0-279">Şunu söyliyoruz:</span><span class="sxs-lookup"><span data-stu-id="eabd0-279">We could say that:</span></span>
>
> - <span data-ttu-id="eabd0-280">uygulama yoksa ve ne sanal, ne de mühürlü belirtilmemişse, üyenin soyut olduğunu varsayıyoruz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="eabd0-281">bir uygulama varsa ve soyut ya da Sealed belirtilmemişse, üyenin sanal olduğunu varsaydık.</span><span class="sxs-lookup"><span data-stu-id="eabd0-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="eabd0-282">korumalı değiştirici, bir yöntemi sanal veya soyut olmayan bir şekilde oluşturmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="eabd0-283">Alternatif olarak, sanal bir üye için sanal değiştiricinin gerekli olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="eabd0-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="eabd0-284">Yani, açıkça sanal değiştirici ile işaretlenmemiş bir üye varsa, bu, sanal ya da soyut değildir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="eabd0-285">Bu yaklaşım, bir yöntem bir sınıftan arabirime taşındığında daha iyi deneyim verebilir:</span><span class="sxs-lookup"><span data-stu-id="eabd0-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="eabd0-286">soyut bir yöntem soyut kalır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="eabd0-287">sanal bir yöntem sanal kalır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="eabd0-288">değiştirici olmadan bir yöntem sanal veya soyut olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="eabd0-289">sealed değiştiricisi geçersiz kılma olmayan bir yönteme uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="eabd0-290">Ne düşünüyorsun?</span><span class="sxs-lookup"><span data-stu-id="eabd0-290">What do you think?</span></span>

> <span data-ttu-id="eabd0-291">***Kapatılan sorun:*** Somut bir Yöntem (uygulama ile) örtük olarak `virtual`mı?</span><span class="sxs-lookup"><span data-stu-id="eabd0-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="eabd0-292">Yes</span><span class="sxs-lookup"><span data-stu-id="eabd0-292">[YES]</span></span>

<span data-ttu-id="eabd0-293">***Kararlar:*** LDM 2017-04-05:</span><span class="sxs-lookup"><span data-stu-id="eabd0-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="eabd0-294">sanal olmayan `sealed` veya `private`aracılığıyla açıkça ifade edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="eabd0-295">`sealed`, sanal olmayan gövdelerle arabirim örneği üyelerini oluşturmak için anahtar sözcüktür</span><span class="sxs-lookup"><span data-stu-id="eabd0-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="eabd0-296">Arabirimlerde tüm değiştiricilere izin vermek istiyoruz</span><span class="sxs-lookup"><span data-stu-id="eabd0-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="eabd0-297">Arabirim üyeleri için varsayılan erişilebilirlik, iç içe türler dahil geneldir</span><span class="sxs-lookup"><span data-stu-id="eabd0-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="eabd0-298">Arabirimlerdeki özel işlev üyeleri örtülü olarak mühürlenmiş ve `sealed` bunlara izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="eabd0-299">Özel sınıflara (arabirimlerde) izin verilir ve mühürlenebilir ve bu, mühürlenmiş sınıf hissi içinde mühürlenmiş anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="eabd0-300">İyi bir öneri olmadığından arabirimlerde veya üyelerinde kısmen hala izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="eabd0-301">İkili uyumluluk 1</span><span class="sxs-lookup"><span data-stu-id="eabd0-301">Binary Compatibility 1</span></span>

<span data-ttu-id="eabd0-302">Bir kitaplık varsayılan bir uygulama sağlıyorsa</span><span class="sxs-lookup"><span data-stu-id="eabd0-302">When a library provides a default implementation</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

<span data-ttu-id="eabd0-303">`C` `I1.M` uygulamasının `I1.M`olduğunu anladık.</span><span class="sxs-lookup"><span data-stu-id="eabd0-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="eabd0-304">`I2` içeren derleme aşağıdaki gibi değiştirilirse ve yeniden derlendiğinden ne olur?</span><span class="sxs-lookup"><span data-stu-id="eabd0-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="eabd0-305">Ancak `C` yeniden derlenmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-305">but `C` is not recompiled.</span></span> <span data-ttu-id="eabd0-306">Program çalıştırıldığında ne olur?</span><span class="sxs-lookup"><span data-stu-id="eabd0-306">What happens when the program is run?</span></span> <span data-ttu-id="eabd0-307">`(C as I1).M()` çağrısı</span><span class="sxs-lookup"><span data-stu-id="eabd0-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="eabd0-308">`I1.M` çalışır</span><span class="sxs-lookup"><span data-stu-id="eabd0-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="eabd0-309">`I2.M` çalışır</span><span class="sxs-lookup"><span data-stu-id="eabd0-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="eabd0-310">Bir tür çalışma zamanı hatası oluşturur</span><span class="sxs-lookup"><span data-stu-id="eabd0-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="eabd0-311">***Karar:*** 2017-04-11 yapıldı: çalışma zamanında en belirgin, kesin olmayan bir geçersiz kılma olan `I2.M`çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="eabd0-312">Olay erişimcileri (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-312">Event accessors (closed)</span></span>

> <span data-ttu-id="eabd0-313">***Kapatılan sorun:*** "Piecewise" olayının geçersiz kılınabilmesi</span><span class="sxs-lookup"><span data-stu-id="eabd0-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="eabd0-314">Şu durumu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="eabd0-314">Consider this case:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

<span data-ttu-id="eabd0-315">Olayın bu "kısmi" uygulamasına izin verilmez çünkü bir sınıfta olduğu gibi, bir olay bildiriminin sözdizimi yalnızca bir erişimciye izin vermez; her ikisi de (veya hiçbiri) sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="eabd0-316">Söz dizimi içindeki soyut kaldırma erişimcisinin bir gövde yokluğu tarafından örtük olarak soyut olmasını sağlayarak aynı şeyi gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="eabd0-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

<span data-ttu-id="eabd0-317">*Bunun yeni bir (önerilen) sözdizimi*olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="eabd0-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="eabd0-318">Geçerli dilbilgisinde, olay erişimcilerinin zorunlu bir gövdesi vardır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="eabd0-319">***Kapatılan sorun:*** Bir olay erişimcisinin bir gövde atlanmasıyla soyut olması (örtük olarak), arabirimler ve özellik erişimcileri içindeki yöntemlerin bir gövdenin atlanmasıyla soyutlanma biçimini (örtük olarak) sağlar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="eabd0-320">***Karar:*** (2017-04-18) Hayır, olay bildirimleri hem somut erişimciler (ya da hiçbirine) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="eabd0-321">Bir sınıfta reabstraction (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="eabd0-322">***Kapatılan sorun:*** Buna izin verildiğini doğrulamamız gerekir (Aksi takdirde, varsayılan bir uygulama eklemek bir son değişiklik olacaktır):</span><span class="sxs-lookup"><span data-stu-id="eabd0-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

<span data-ttu-id="eabd0-323">***Karar:*** (2017-04-18) Evet, bir arabirim üye bildirimine gövde eklemek C 'yi kesmemelidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="eabd0-324">Sealed geçersiz kılma (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-324">Sealed Override (closed)</span></span>

<span data-ttu-id="eabd0-325">Önceki soru, `sealed` değiştiricinin bir arabirimdeki `override` uygulanabileceğini dolaylı olarak varsayar.</span><span class="sxs-lookup"><span data-stu-id="eabd0-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="eabd0-326">Bu, taslak belirtimiyle çelişmektedir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-326">This contradicts the draft specification.</span></span> <span data-ttu-id="eabd0-327">Bir geçersiz kılmayı mühürme izin vermek istiyor musunuz?</span><span class="sxs-lookup"><span data-stu-id="eabd0-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="eabd0-328">Mühürleme 'un kaynak ve ikili uyumluluk etkileri göz önünde bulundurulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="eabd0-329">***Kapatılan sorun:*** Bir geçersiz kılmanın mühürlerine izin veririz mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="eabd0-330">***Karar:*** (2017-04-18) arabirimlerde geçersiz kılmalarda `sealed` izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="eabd0-331">Arabirim üyelerinde `sealed` tek kullanımı, ilk bildiriminde sanal olmamaları.</span><span class="sxs-lookup"><span data-stu-id="eabd0-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="eabd0-332">Elmas devralma ve sınıflar (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="eabd0-333">Teklifin taslağı, elmas devralma senaryolarındaki arabirim geçersiz kılmaları için sınıf geçersiz kılmalarını tercih eder:</span><span class="sxs-lookup"><span data-stu-id="eabd0-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="eabd0-334">Her arabirim ve sınıfın, türde veya doğrudan ve dolaylı arabirimlerde görüntülenen geçersiz kılmalar arasındaki her arabirim yöntemi için *en belirli bir geçersiz kılmalara* sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="eabd0-335">*En özel geçersiz* kılma, tüm diğer geçersiz kılmada daha belirgin olan benzersiz bir geçersiz kılma.</span><span class="sxs-lookup"><span data-stu-id="eabd0-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="eabd0-336">Geçersiz kılma yoksa, yöntemin kendisi en özel geçersiz kılma olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="eabd0-337">Bir geçersiz kılma `M1` başka bir geçersiz kıltan *daha belirgin* kabul edilir `M2` `M1` tür `T1`bildiriminde bildirilirse, `M2` tür `T2`olarak bildirilirse ve her iki</span><span class="sxs-lookup"><span data-stu-id="eabd0-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="eabd0-338">`T1` doğrudan veya dolaylı arabirimleri arasında `T2` içerir ya da</span><span class="sxs-lookup"><span data-stu-id="eabd0-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="eabd0-339">`T2` bir arabirim türüdür, ancak `T1` bir arabirim türü değil.</span><span class="sxs-lookup"><span data-stu-id="eabd0-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="eabd0-340">Senaryo budur</span><span class="sxs-lookup"><span data-stu-id="eabd0-340">The scenario is this</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

<span data-ttu-id="eabd0-341">Bu davranışı onaylamanız gerekir (veya başka bir karar vermeniz gerekir)</span><span class="sxs-lookup"><span data-stu-id="eabd0-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="eabd0-342">***Kapatılan sorun:*** Karma sınıflar ve arabirimler için geçerli olduğu için, yukarıdaki taslak belirtimini, yukarıda *belirtilen geçersiz kılma* için (bir sınıf bir arabirim üzerinden önceliklidir) onaylayın.</span><span class="sxs-lookup"><span data-stu-id="eabd0-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="eabd0-343">Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="eabd0-344">Arabirim yöntemleri vs yapılar (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="eabd0-345">Varsayılan arabirim yöntemleri ve yapılar arasında bazı talihsiz etkileşimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="eabd0-346">Arabirim üyelerinin devralınmadığını unutmayın:</span><span class="sxs-lookup"><span data-stu-id="eabd0-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="eabd0-347">Sonuç olarak, istemci arabirim yöntemlerini çağırmak için yapıyı Box olmalıdır</span><span class="sxs-lookup"><span data-stu-id="eabd0-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="eabd0-348">Bu şekilde kutulama, `struct` türünün asıl avantajlarını erteler.</span><span class="sxs-lookup"><span data-stu-id="eabd0-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="eabd0-349">Ayrıca, herhangi bir mutasyon yöntemi, yapının *paketlenmiş bir kopyasında* çalıştıkları için hiçbir şekilde görünür etkiye sahip olmaz:</span><span class="sxs-lookup"><span data-stu-id="eabd0-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> <span data-ttu-id="eabd0-350">***Kapatılan sorun:*** Bunun için neler yapabiliriz:</span><span class="sxs-lookup"><span data-stu-id="eabd0-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="eabd0-351">Bir `struct` varsayılan bir uygulamayı devralmayı yasaklaın.</span><span class="sxs-lookup"><span data-stu-id="eabd0-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="eabd0-352">Tüm arabirim yöntemleri bir `struct`Özet olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="eabd0-353">Daha sonra, daha sonra nasıl çalıştığını nasıl daha iyi hale getirebileceğine karar vereceğiz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="eabd0-354">Kutulamayı önleyen bir tür kod oluşturma stratejisiyle birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="eabd0-355">`IB.Increment`gibi bir yöntemin içinde, `this` türü muhtemelen `IB`kısıtlı bir tür parametresine akmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="eabd0-356">Bununla birlikte, çağıranın kutulamayı önlemek için, Özet olmayan yöntemler arabirimlerden devralınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="eabd0-357">Bu, derleyici ve CLR uygulama çalışmasını önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="eabd0-358">Bu konuda endişelenmeyin ve yalnızca bir Wart olarak bırakmayın.</span><span class="sxs-lookup"><span data-stu-id="eabd0-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="eabd0-359">Diğer fikirler?</span><span class="sxs-lookup"><span data-stu-id="eabd0-359">Other ideas?</span></span>

<span data-ttu-id="eabd0-360">***Karar:*** Bu konuda endişelenmeyin ve yalnızca bir Wart olarak bırakmayın.</span><span class="sxs-lookup"><span data-stu-id="eabd0-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="eabd0-361">Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="eabd0-362">Temel arabirim etkinleştirmeleri (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="eabd0-363">Taslak belirtimi, Java tarafından önerilen temel arabirim etkinleştirmeleri için bir sözdizimi önerir: `Interface.base.M()`.</span><span class="sxs-lookup"><span data-stu-id="eabd0-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="eabd0-364">En azından ilk prototip için bir sözdizimi seçmemiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="eabd0-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="eabd0-365">Sık kullanılanlardan `base<Interface>.M()`.</span><span class="sxs-lookup"><span data-stu-id="eabd0-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="eabd0-366">***Kapatılan sorun:*** Temel üye çağırma söz dizimi nedir?</span><span class="sxs-lookup"><span data-stu-id="eabd0-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="eabd0-367">***Karar:*** Söz dizimi `base(Interface).M()`.</span><span class="sxs-lookup"><span data-stu-id="eabd0-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="eabd0-368">Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="eabd0-369">Bu nedenle adlı arabirim bir temel arabirim olmalıdır, ancak doğrudan temel arabirim olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="eabd0-370">***Açık sorun:*** Sınıf üyelerinde temel arabirim etkinleştirmeleri izin verilmelidir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="eabd0-371">***Karar***: Evet.</span><span class="sxs-lookup"><span data-stu-id="eabd0-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="eabd0-372">Genel olmayan arabirim üyelerini geçersiz kılma (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="eabd0-373">Bir arabirimde, temel arabirimlerden ortak olmayan Üyeler `override` değiştiricisi kullanılarak geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="eabd0-374">Üyeyi içeren arabirime ad veren "açık" bir geçersiz kılma ise, erişim değiştiricisi atlanır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="eabd0-375">***Kapatılan sorun:*** Arabirimi adı olmayan bir "örtük" geçersiz kılma ise, erişim değiştiricisi eşleşmelidir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="eabd0-376">***Karar:*** Yalnızca ortak Üyeler örtük olarak geçersiz kılınabilir ve erişimin eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="eabd0-377">Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="eabd0-378">***Açık sorun:*** Erişim değiştiricisi gerekli, isteğe bağlı veya `override void IB.M() {}`gibi açık bir geçersiz kılma üzerinde atlanmış mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="eabd0-379">***Açık sorun:*** `override` gerekli, isteğe bağlı veya `void IB.M() {}`gibi açık bir geçersiz kılma üzerinde atlansın mı?</span><span class="sxs-lookup"><span data-stu-id="eabd0-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="eabd0-380">Bir sınıfta ortak olmayan bir arabirim üyesi nasıl uygulanır?</span><span class="sxs-lookup"><span data-stu-id="eabd0-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="eabd0-381">Açıkça yapılması gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-381">Perhaps it must be done explicitly?</span></span>

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> <span data-ttu-id="eabd0-382">***Kapatılan sorun:*** Bir sınıfta ortak olmayan bir arabirim üyesi nasıl uygulanır?</span><span class="sxs-lookup"><span data-stu-id="eabd0-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="eabd0-383">***Karar:*** Yalnızca ortak olmayan arabirim üyelerini açık bir şekilde uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="eabd0-384">Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="eabd0-385">***Karar***: arabirim üyelerinde `override` anahtar sözcüğe izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="eabd0-386">İkili uyumluluk 2 (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="eabd0-387">Her türün ayrı bir derlemede olduğu aşağıdaki kodu göz önünde bulundurun</span><span class="sxs-lookup"><span data-stu-id="eabd0-387">Consider the following code in which each type is in a separate assembly</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

<span data-ttu-id="eabd0-388">`C` `I1.M` uygulamasının `I2.M`olduğunu anladık.</span><span class="sxs-lookup"><span data-stu-id="eabd0-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="eabd0-389">`I3` içeren derleme aşağıdaki gibi değiştirilirse ve yeniden derlendiğinden ne olur?</span><span class="sxs-lookup"><span data-stu-id="eabd0-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="eabd0-390">Ancak `C` yeniden derlenmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-390">but `C` is not recompiled.</span></span> <span data-ttu-id="eabd0-391">Program çalıştırıldığında ne olur?</span><span class="sxs-lookup"><span data-stu-id="eabd0-391">What happens when the program is run?</span></span> <span data-ttu-id="eabd0-392">`(C as I1).M()` çağrısı</span><span class="sxs-lookup"><span data-stu-id="eabd0-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="eabd0-393">`I1.M` çalışır</span><span class="sxs-lookup"><span data-stu-id="eabd0-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="eabd0-394">`I2.M` çalışır</span><span class="sxs-lookup"><span data-stu-id="eabd0-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="eabd0-395">`I3.M` çalışır</span><span class="sxs-lookup"><span data-stu-id="eabd0-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="eabd0-396">2 veya 3, belirleyici</span><span class="sxs-lookup"><span data-stu-id="eabd0-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="eabd0-397">Bir tür çalışma zamanı özel durumu oluşturur</span><span class="sxs-lookup"><span data-stu-id="eabd0-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="eabd0-398">***Karar***: bir özel durum (5) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="eabd0-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="eabd0-399">Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="eabd0-400">Arabirimde `partial` izin veriyor musunuz?</span><span class="sxs-lookup"><span data-stu-id="eabd0-400">Permit `partial` in interface?</span></span> <span data-ttu-id="eabd0-401">kapandı</span><span class="sxs-lookup"><span data-stu-id="eabd0-401">(closed)</span></span>

<span data-ttu-id="eabd0-402">Bu arabirimler, soyut sınıfların kullanılma yöntemine benzer şekilde kullanılabileceği için, `partial`bildirmek faydalı olabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="eabd0-403">Bu, özellikle de genel kullanım açısından yararlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="eabd0-404">***Teklif:*** Arabirimlerin ve arabirimlerin üyelerinin `partial`bildirilmemiş olabileceği dil kısıtlamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="eabd0-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="eabd0-405">***Karar***: Evet.</span><span class="sxs-lookup"><span data-stu-id="eabd0-405">***Decision***: Yes.</span></span> <span data-ttu-id="eabd0-406">Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="eabd0-407">bir arabirimde `Main`?</span><span class="sxs-lookup"><span data-stu-id="eabd0-407">`Main` in an interface?</span></span> <span data-ttu-id="eabd0-408">kapandı</span><span class="sxs-lookup"><span data-stu-id="eabd0-408">(closed)</span></span>

> <span data-ttu-id="eabd0-409">***Açık sorun:*** Bir arabirimde programın giriş noktası olması için bir `static Main` yöntemi mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="eabd0-410">***Karar***: Evet.</span><span class="sxs-lookup"><span data-stu-id="eabd0-410">***Decision***: Yes.</span></span> <span data-ttu-id="eabd0-411">Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="eabd0-412">Ortak sanal olmayan yöntemleri destekleme amacını onaylayın (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="eabd0-413">Bir arabirimdeki sanal olmayan ortak yöntemlere izin vermek için lütfen kararmuzu onaylayın (veya ters çevirin)?</span><span class="sxs-lookup"><span data-stu-id="eabd0-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="eabd0-414">***Yarı kapalı sorun:*** (2017-04-18) faydalı olacağını düşündük, ancak buna geri Dönecekiz.</span><span class="sxs-lookup"><span data-stu-id="eabd0-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="eabd0-415">Bu, bir akıl modeli gidiş dönüşü bloğudur.</span><span class="sxs-lookup"><span data-stu-id="eabd0-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="eabd0-416">***Karar***: Evet.</span><span class="sxs-lookup"><span data-stu-id="eabd0-416">***Decision***: Yes.</span></span> <span data-ttu-id="eabd0-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="eabd0-418">Arabirimdeki bir `override` yeni bir üye sunuyor mu?</span><span class="sxs-lookup"><span data-stu-id="eabd0-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="eabd0-419">kapandı</span><span class="sxs-lookup"><span data-stu-id="eabd0-419">(closed)</span></span>

<span data-ttu-id="eabd0-420">Geçersiz kılma bildiriminin yeni bir üye tanıtıp tanıtılmadığını gözlemlemek için birkaç yol vardır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> <span data-ttu-id="eabd0-421">***Açık sorun:*** Arabirimdeki geçersiz kılma bildirimi yeni bir üye sunuyor mu?</span><span class="sxs-lookup"><span data-stu-id="eabd0-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="eabd0-422">kapandı</span><span class="sxs-lookup"><span data-stu-id="eabd0-422">(closed)</span></span>

<span data-ttu-id="eabd0-423">Bir sınıfta, bir geçersiz kılma yöntemi bazı senslerde "görünür" olur.</span><span class="sxs-lookup"><span data-stu-id="eabd0-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="eabd0-424">Örneğin, parametrelerinin adları geçersiz kılınan yöntemdeki parametrelerin adlarından önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="eabd0-425">Her zaman en belirli bir geçersiz kılma olduğundan, bu davranışı arabirimlerde çoğaltmak mümkün olabilir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="eabd0-426">Ancak bu davranışı yinelemek istiyoruz musunuz?</span><span class="sxs-lookup"><span data-stu-id="eabd0-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="eabd0-427">Ayrıca, bir geçersiz kılma yöntemini "geçersiz kılmak" mümkündür.</span><span class="sxs-lookup"><span data-stu-id="eabd0-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="eabd0-428">[Moot]</span><span class="sxs-lookup"><span data-stu-id="eabd0-428">[Moot]</span></span>

<span data-ttu-id="eabd0-429">***Karar***: arabirim üyelerinde `override` anahtar sözcüğe izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="eabd0-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span><span class="sxs-lookup"><span data-stu-id="eabd0-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="eabd0-431">Özel erişimcisi olan Özellikler (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="eabd0-432">Özel üyelerin sanal olduğunu ve sanal ve özel ' in birleşimine izin verildiğimiz olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="eabd0-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="eabd0-433">Ancak özel bir erişimciye sahip bir özellik hakkında ne olacak?</span><span class="sxs-lookup"><span data-stu-id="eabd0-433">But what about a property with a private accessor?</span></span>

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

<span data-ttu-id="eabd0-434">Buna izin veriliyor mu?</span><span class="sxs-lookup"><span data-stu-id="eabd0-434">Is this allowed?</span></span> <span data-ttu-id="eabd0-435">`set` erişimcisi burada `virtual` değil mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="eabd0-436">Erişilebilir olduğunda geçersiz kılınabilir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="eabd0-437">Aşağıdakiler `get` erişimcisini örtük olarak uygular mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-437">Does the following implicitly implement only the `get` accessor?</span></span>

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="eabd0-438">, IA nedeniyle aşağıdaki bir hata oluştu. P. set sanal değil ve erişilebilir durumda değil mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="eabd0-439">***Karar***: ilk örnek geçerli görünüyor, ancak en son bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="eabd0-440">Bu, ' de C#zaten çalıştığı şekilde çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="eabd0-441">Taban arabirim etkinleştirmeleri, Round 2 (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="eabd0-442">Temel etkinleştirmeleri nasıl ele almak için önceki "çözümümüzü" Aslında yeterli ifade sağlanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="eabd0-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="eabd0-443">Java 'dan farklı olarak, C# ve clr 'de bunu, yöntem bildirimini içeren arabirimi ve çağırmak istediğiniz uygulamanın konumunu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="eabd0-444">Arabirimlerde temel çağrılar için aşağıdaki sözdizimini önerdim.</span><span class="sxs-lookup"><span data-stu-id="eabd0-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="eabd0-445">Bununla çok sevdim, ancak hangi sözdiziminin hızlı bir şekilde ifade edebileceklerini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="eabd0-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

<span data-ttu-id="eabd0-446">Belirsizlik yoksa, bunu daha basit bir şekilde yazabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="eabd0-446">If there is no ambiguity, you can write it more simply</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

<span data-ttu-id="eabd0-447">Veya</span><span class="sxs-lookup"><span data-stu-id="eabd0-447">Or</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

<span data-ttu-id="eabd0-448">Veya</span><span class="sxs-lookup"><span data-stu-id="eabd0-448">Or</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

<span data-ttu-id="eabd0-449">***Karar***: `base(N.I1<T>).M(s)`karar verdi, daha sonra burada sorun olabileceğini belirten bir çağrı bağlama varsa bunu gizleme.</span><span class="sxs-lookup"><span data-stu-id="eabd0-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="eabd0-450">Struct için bir uyarı varsayılan yöntemi uygulamamı?</span><span class="sxs-lookup"><span data-stu-id="eabd0-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="eabd0-451">kapandı</span><span class="sxs-lookup"><span data-stu-id="eabd0-451">(closed)</span></span>

<span data-ttu-id="eabd0-452">@vancem, bir değer türü bildirimi bir arabirimden bu yöntemin bir uygulamasını devralmasını bile, bazı arabirim yöntemini geçersiz kılamazsa, bir uyarı üretmemiz gerektiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="eabd0-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="eabd0-453">Çünkü kutulama ve az mayın kısıtlı çağrılara neden olur.</span><span class="sxs-lookup"><span data-stu-id="eabd0-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="eabd0-454">***Karar***: Bu, bir çözümleyici için daha uygun bir şey gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="eabd0-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="eabd0-455">Ayrıca, varsayılan arabirim yöntemi asla çağrılmasa ve hiçbir paketleme gerçekleşmeyeceğinden bile tetikleneceği için bu uyarının gürültülü olması gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="eabd0-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="eabd0-456">Arabirim statik oluşturucuları (kapalı)</span><span class="sxs-lookup"><span data-stu-id="eabd0-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="eabd0-457">Arabirim statik oluşturucular ne zaman çalıştırılır?</span><span class="sxs-lookup"><span data-stu-id="eabd0-457">When are interface static constructors run?</span></span>  <span data-ttu-id="eabd0-458">Geçerli CLı taslağı, ilk statik yönteme veya alana erişildiğinde oluştuğunu önerir.</span><span class="sxs-lookup"><span data-stu-id="eabd0-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="eabd0-459">Bunların hiçbiri yoksa, hiç çalıştırılmayabilir mi?</span><span class="sxs-lookup"><span data-stu-id="eabd0-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="eabd0-460">[2018-10-09 CLR ekibi, "değer türleri için yaptığımız şeyleri yansıtmaya (her örnek yöntemine erişim üzerinde cctor denetimi)"] öneriyor.</span><span class="sxs-lookup"><span data-stu-id="eabd0-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="eabd0-461">***Karar***: statik oluşturucular, örnek yöntemlerine giriş üzerinde de çalışır, statik Oluşturucu `beforefieldinit`, bu durumda statik oluşturucular ilk statik alana erişmeden önce çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="eabd0-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="eabd0-462">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="eabd0-462">Design meetings</span></span>

<span data-ttu-id="eabd0-463">[2017-03-08 LDM toplantısı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) Meeting notları
[2017-03-23 toplantı "varsayılan arabirim yöntemleri için clr davranışı"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM meeting notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM toplantısı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) toplantısı [notları
2017-04-19 LDM toplantısı](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) notları
2017-05-17 LDM [toplantısı](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) [notları
2017-05-31](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) LDM toplantısı notları
[2017-06-14 LDM Toplantı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM toplantı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM toplantısı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span><span class="sxs-lookup"><span data-stu-id="eabd0-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
