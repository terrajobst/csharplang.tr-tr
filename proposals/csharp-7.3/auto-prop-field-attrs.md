---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484666"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>Otomatik uygulanan özellik alanı-hedeflenen öznitelikler

## <a name="summary"></a>Özet
[summary]: #summary

Bu özellik, geliştiricilerin otomatik uygulanan özelliklerin yedekleme alanlarına doğrudan öznitelik uygulamasına izin vermeyi amaçlamaktadır.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Şu anda özniteliklerin otomatik uygulanan özelliklerin yedekleme alanlarına uygulanması mümkün değildir.  Geliştiricinin alan hedefleme özniteliği kullanması gereken durumlarda, alanı el ile bildirmeye zorlanır ve daha ayrıntılı özellik söz dizimini kullanır.  Oluşturulan yedekleme C# alanında her zaman alan tarafından hedeflenen öznitelikler de desteklendiğinden, aynı işlevselliği Özellik kaklarına genişletmek mantıklı olur.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Kısaca, şunlar geçerlidir C# ve uyarı oluşturmaz:

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

Bu, alana hedeflenmiş özniteliklerin derleyici tarafından oluşturulan yedekleme alanına uygulanmasına neden olur:

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

Belirtildiği gibi, bu, aşağıdaki yasal olduğundan ve beklendiği C# gibi davrandığı için, eşlik 1,0 ' dan Event sözdizimi ile birlikte getirir:

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Bu değişikliğin uygulanması için iki olası sakıncalar vardır:

1. Otomatik olarak uygulanan bir özelliğin alanına bir özniteliği uygulamaya çalışılması, bu bloktaki özniteliklerin yoksayılacağını bildiren bir derleyici uyarısı oluşturur.  Derleyici bu öznitelikleri destekleyecek şekilde değiştirildiyse, bir sonraki yeniden derlemede bulunan yedekleme alanına uygulanırlar ve bu da programın çalışma zamanındaki davranışını değiştirebilir.
1. Derleyici, öznitelikleri otomatik uygulanan özelliğin alanına uygulamaya çalışırken özniteliklerin AttributeUsage hedeflerini şu anda doğrulamaz.  Derleyici alana hedeflenmiş öznitelikleri destekleyecek şekilde değiştirildiyse ve söz konusu öznitelik bir alana uygulanmazsa, derleyici bir uyarı yerine bir hata ve derlemeyi bozabilir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Tasarım toplantıları
