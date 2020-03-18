---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485128"
---

# <a name="discriminated-unions--enum-class"></a>Ayırt edici birleşimler/`enum class`

`enum class`es, bazen ayrılmış birleşimler olarak da adlandırılan, her olası örneğin türün listelendiği ve her örnek çakışmayan bir tür bildirimidir.

Aşağıdaki sözdizimi kullanılarak bir `enum class` tanımlanmıştır:

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

Örnek sözdizimi:

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>İçeriyor

`enum class` tanımı, `enum class` bildirimiyle aynı ada sahip bir soyut sınıf olan kök türünü ve her birinin kök türünün bir alt türü olan bir türü olan bir üye kümesini tanımlar. Birden çok `partial enum class` tanımı varsa, tüm Üyeler enum sınıfı tanımının üyeleri olarak kabul edilir. Kullanıcı tanımlı soyut sınıf tanımından farklı olarak, `enum class` kök türü varsayılan olarak kısmen olur ve varsayılan *özel* parametre-daha az oluşturucuya sahip olacak şekilde tanımlanır.

Kök türü kısmi bir soyut sınıf olmak üzere tanımlandığından, *kök türünün* kısmi tanımlarının de eklenebilir olması, bu da bir sınıf gövdesinin standart sözdizimi formlarına izin verileceğini unutmayın.
Ancak, hiçbir tür, hiçbir bildirimde doğrudan kök türünden devralınabilir ve bu, `enum class` üyeleri olarak belirtilenlerden farklı olabilir. Ayrıca, kök türü için Kullanıcı tanımlı oluşturuculara izin verilmez.

Dört tür `enum class` üye bildirimi vardır:

* Basit sınıf üyeleri

* karmaşık sınıf üyeleri

* `enum class` Üyeler

* değer üyeleri.

### <a name="simple-class-members"></a>Basit sınıf üyeleri

Basit bir sınıf üyesi bildirimi, aynı ada sahip yeni bir iç içe "kayıt" sınıfını (Bu belgede kasıtlı olarak ayrıldı) tanımlar. İç içe yerleştirilmiş sınıf kök türünden devralır.

Yukarıdaki örnek kod verildiğinde,

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

`enum class` bildiriminde semantikler aşağıdaki bildirime eşdeğerdir

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>Karmaşık sınıf üyeleri

Tüm sınıf bildirimini bir `enum class` bildiriminin altına de iç içe yerleştirebilirsiniz. Kök türünde iç içe geçmiş bir sınıf olarak kabul edilir. Söz dizimi tüm sınıf bildirimine izin verir, ancak karmaşık sınıf üyesinin doğrudan içeren `enum class` bildiriminden devralması için gereklidir. 

### <a name="enum-class-members"></a>`enum class` Üyeler

`enum classes` birbirlerine iç içe eklenebilir, ör.

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

Bu, üst düzey bir `enum class`ile neredeyse aynıdır, iç içe enum sınıfı iç içe geçmiş bir kök türü tanımlar ve iç içe enum sınıfının altındaki her şey, üst düzey kök türü yerine iç içe geçmiş kök türünün bir alt türüdür.

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>Değer üyeleri

`enum classes`, değer üyelerini de içerebilir. Değer üyeleri kök türünde kök türü de döndüren genel salt Al statik özelliklerini tanımlar, örn.

```C#
enum class Color
{
    Red,
    Green
}
```

ile eşdeğer özellikler vardır

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

Tüm semantikler bir uygulama ayrıntısı olarak değerlendirilir, ancak her bir özellik için bir benzersiz örnek döndürüldüğünden ve yinelenen etkinleştirmeleri üzerinde aynı örnek döndürülmeyecektir.


### <a name="switch-expression-and-patterns"></a>Anahtar ifadesi ve desenleri

`enum classes`işlemek için, model eşleştirme ve switch ifadesinin bazı önerilmiş ayarlamaları vardır. Anahtar ifadeleri türleri değişken düzeniyle zaten eşleştirebilir, ancak şu anda başvuru türleri için, anahtar ifadesinde anahtar kolları kümesi, bağımsız değişkenin statik türü veya bir alt tür için eşleşme dışında tamamlanmış olarak kabul edilir.

Anahtar ifadeleri, bir `enum class` kök türü, anahtar ifadesi için bağımsız değişkenin statik türü ise ve sabit listesinin tüm üyeleriyle eşleşen bir desen kümesi varsa, anahtar geniş olarak kabul edilir.

Değer üyeleri sabit olmadığından ve yeni statik türler tanımlamadıklarından, şu anda bu öğeler düzeniyle eşleştirilemez. Bunu mümkün kılmak için, `enum class` değer üyelerine eşleştirmeye izin vermek üzere sabit model sözdizimi kullanan yeni bir model eklenecektir. Eşleşme, yalnızca model eşleşmesi için bağımsız değişken ve `enum class` Value üyesi tarafından döndürülen değer eşitse, bu denetimi gerçekleştirmek için gerekli olmasa da, başarılı olarak tanımlanmıştır.


## <a name="open-questions"></a>Açık sorular

- [] Ortak tür algoritması `enum class` üye hakkında ne söylüyor? Bu geçerli bir kod mi?
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] Yalnızca değer üyeleri için yeni bir model eklemek ağır bir şekilde görünür. Daha anlamlı bir sürüm oluşturma var mı?
    - [] Değer üyeleri, bu nedenle bir paralel iç içe sınıf oluşturmaya de uygun şekilde eşlenmiyor

- [] Sabit bir `enum class` statik bir türe sahip bir bağımsız değişkene geçiş yapıyor mu?

- [] Anahtar ifadesinde `enum class`es 'nin tamamlanmış olarak kabul edilmediğinden emin olmak için bir yol olması gerekir mi? `virtual`önek mi?

- [] `enum class`değiştiricilere ne izin verilmelidir?