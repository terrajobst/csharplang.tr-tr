---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484456"
---
# <a name="covariant-return-types"></a>covaryant dönüş türleri

* [x] önerilir
* [] Prototipi: başlatılmadı
* [] Uygulama: başlatılmadı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

_Birlikte değişken dönüş türlerini_destekler. Özellikle, geçersiz kılan bir metodun, geçersiz kıldıkları yöntemden daha fazla türetilmiş başvuru türüne sahip olmasını sağlar.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Bu, kod içinde farklı yöntem adlarının, geçersiz kılınan yöntemle aynı türü döndürmesi gereken dil kısıtlaması etrafında çalışmak üzere oluşturulması gereken yaygın bir modeldir. Roslyn kod tabanından bir örnek için aşağıya bakın.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

_Birlikte değişken dönüş türlerini_destekler. Özellikle, geçersiz kılan bir metodun, geçersiz kıldıkları yöntemden daha fazla türetilmiş başvuru türüne sahip olmasını sağlar. Bu, Yöntemler ve özellikler için geçerlidir ve sınıflar ve arabirimlerde desteklenir.

Bu, fabrika düzeninde yararlı olacaktır. Örneğin, Roslyn kod tabanında

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

Bu, derleyicinin geçersiz kılma yöntemini temel sınıf yöntemini gizleyen bir "yeni" sanal yöntemi olarak, türetilmiş sınıf yöntemi çağrısıyla birlikte temel sınıf yöntemini uygulayan bir _köprü yöntemiyle_ yaymasıdır.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

- [] Her dil değişikliğinin kendisi için ödeme yapılmalıdır.
- [] Derin devralma hiyerarşileri durumunda bile performansın makul olduğundan emin olunması gerekir
- [] Eski derleyicilerden yeni Il kullanılırken bile, çeviri stratejisinin yapıtlarının dil semantiğini etkilemediğinden emin olunması gerekir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Kaynak olarak, dil kurallarını izin vermek için biraz daha rahat olabilir

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- [] Bu özelliği kullanmak için derlenen API 'Ler, dilin eski sürümlerinde çalışır mı?

## <a name="design-meetings"></a>Tasarım toplantıları

Henüz yok. <https://github.com/dotnet/roslyn/issues/357>konusunda bazı tartışmalar vardı.