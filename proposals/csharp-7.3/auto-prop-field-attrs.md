---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484666"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="ebc62-101">Otomatik uygulanan özellik alanı-hedeflenen öznitelikler</span><span class="sxs-lookup"><span data-stu-id="ebc62-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="ebc62-102">Özet</span><span class="sxs-lookup"><span data-stu-id="ebc62-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ebc62-103">Bu özellik, geliştiricilerin otomatik uygulanan özelliklerin yedekleme alanlarına doğrudan öznitelik uygulamasına izin vermeyi amaçlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="ebc62-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="ebc62-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="ebc62-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ebc62-105">Şu anda özniteliklerin otomatik uygulanan özelliklerin yedekleme alanlarına uygulanması mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ebc62-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="ebc62-106">Geliştiricinin alan hedefleme özniteliği kullanması gereken durumlarda, alanı el ile bildirmeye zorlanır ve daha ayrıntılı özellik söz dizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="ebc62-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="ebc62-107">Oluşturulan yedekleme C# alanında her zaman alan tarafından hedeflenen öznitelikler de desteklendiğinden, aynı işlevselliği Özellik kaklarına genişletmek mantıklı olur.</span><span class="sxs-lookup"><span data-stu-id="ebc62-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="ebc62-108">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="ebc62-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ebc62-109">Kısaca, şunlar geçerlidir C# ve uyarı oluşturmaz:</span><span class="sxs-lookup"><span data-stu-id="ebc62-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="ebc62-110">Bu, alana hedeflenmiş özniteliklerin derleyici tarafından oluşturulan yedekleme alanına uygulanmasına neden olur:</span><span class="sxs-lookup"><span data-stu-id="ebc62-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

<span data-ttu-id="ebc62-111">Belirtildiği gibi, bu, aşağıdaki yasal olduğundan ve beklendiği C# gibi davrandığı için, eşlik 1,0 ' dan Event sözdizimi ile birlikte getirir:</span><span class="sxs-lookup"><span data-stu-id="ebc62-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="ebc62-112">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="ebc62-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ebc62-113">Bu değişikliğin uygulanması için iki olası sakıncalar vardır:</span><span class="sxs-lookup"><span data-stu-id="ebc62-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="ebc62-114">Otomatik olarak uygulanan bir özelliğin alanına bir özniteliği uygulamaya çalışılması, bu bloktaki özniteliklerin yoksayılacağını bildiren bir derleyici uyarısı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ebc62-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="ebc62-115">Derleyici bu öznitelikleri destekleyecek şekilde değiştirildiyse, bir sonraki yeniden derlemede bulunan yedekleme alanına uygulanırlar ve bu da programın çalışma zamanındaki davranışını değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="ebc62-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="ebc62-116">Derleyici, öznitelikleri otomatik uygulanan özelliğin alanına uygulamaya çalışırken özniteliklerin AttributeUsage hedeflerini şu anda doğrulamaz.</span><span class="sxs-lookup"><span data-stu-id="ebc62-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="ebc62-117">Derleyici alana hedeflenmiş öznitelikleri destekleyecek şekilde değiştirildiyse ve söz konusu öznitelik bir alana uygulanmazsa, derleyici bir uyarı yerine bir hata ve derlemeyi bozabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc62-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ebc62-118">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="ebc62-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="ebc62-119">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="ebc62-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="ebc62-120">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="ebc62-120">Design meetings</span></span>
