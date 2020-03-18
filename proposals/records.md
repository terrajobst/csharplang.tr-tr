---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484946"
---
# <a name="records"></a><span data-ttu-id="261c1-101">kayıtlar</span><span class="sxs-lookup"><span data-stu-id="261c1-101">records</span></span>

* <span data-ttu-id="261c1-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="261c1-102">[x] Proposed</span></span>
* <span data-ttu-id="261c1-103">[] Prototip: [Tamam](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="261c1-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="261c1-104">[] Uygulama: [sürüyor](https://github.com/dotnet/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="261c1-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="261c1-105">[] Belirtimi: eklenen taslak belirtimi</span><span class="sxs-lookup"><span data-stu-id="261c1-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="261c1-106">Özet</span><span class="sxs-lookup"><span data-stu-id="261c1-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="261c1-107">Kayıtlar, bir dizi daha basit özelliğin avantajlarını birleştiren C# sınıf ve yapı türleri için yeni ve basitleştirilmiş bir bildirim formudur.</span><span class="sxs-lookup"><span data-stu-id="261c1-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="261c1-108">Yeni özellikleri (çağıran alıcı parametreleri ve *ile ifadesi*) anladık, kayıt bildirimleri için söz dizimi ve semantiğe ve sonra bazı örnekler sunarız.</span><span class="sxs-lookup"><span data-stu-id="261c1-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="261c1-109">Amacı</span><span class="sxs-lookup"><span data-stu-id="261c1-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="261c1-110">İçinde C# önemli sayıda tür bildirimi, yazılı verilerin toplam koleksiyonlarından çok daha fazla.</span><span class="sxs-lookup"><span data-stu-id="261c1-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="261c1-111">Ne yazık ki, böyle bir tür bildirirken ortak kod için harika bir işlem yapılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="261c1-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="261c1-112">*Kayıtlar* , toplama üyelerini, varsa her zamanki ortak kod veya sapmaları açıklayarak tanımlamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="261c1-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="261c1-113">Aşağıdaki [örneklere](#examples)bakın.</span><span class="sxs-lookup"><span data-stu-id="261c1-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="261c1-114">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="261c1-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="261c1-115">arayan alıcı parametreleri</span><span class="sxs-lookup"><span data-stu-id="261c1-115">caller-receiver parameters</span></span>

<span data-ttu-id="261c1-116">Şu anda bir yöntem parametresinin *Varsayılan bağımsız değişkeni* olmalıdır</span><span class="sxs-lookup"><span data-stu-id="261c1-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="261c1-117">*sabit ifade*; veya</span><span class="sxs-lookup"><span data-stu-id="261c1-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="261c1-118">formun bir ifadesi, `S` bir değer türüdür `new S()`; veya</span><span class="sxs-lookup"><span data-stu-id="261c1-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="261c1-119">formun bir ifadesi `default(S)` `S` bir değer türüdür</span><span class="sxs-lookup"><span data-stu-id="261c1-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="261c1-120">Şunu eklemek için bunu genişlettik</span><span class="sxs-lookup"><span data-stu-id="261c1-120">We extend this to add the following</span></span>
- <span data-ttu-id="261c1-121">formun bir ifadesi `this.Identifier`</span><span class="sxs-lookup"><span data-stu-id="261c1-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="261c1-122">Bu yeni form, *çağıran alıcı varsayılan bağımsız değişkeni*olarak adlandırılır ve yalnızca aşağıdakilerin tümü karşılandığında izin verilir</span><span class="sxs-lookup"><span data-stu-id="261c1-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="261c1-123">İçinde göründüğü Yöntem bir örnek yöntemidir; '</span><span class="sxs-lookup"><span data-stu-id="261c1-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="261c1-124">İfade `this.Identifier`, bir alan veya özellik olması gereken kapsayan türün bir örnek üyesine bağlanır; '</span><span class="sxs-lookup"><span data-stu-id="261c1-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="261c1-125">Bağlandığı üye (ve bir özellik ise `get` erişimcisi) en az Yöntem olarak erişilebilir olur; '</span><span class="sxs-lookup"><span data-stu-id="261c1-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="261c1-126">`this.Identifier` türü, örtük olarak bir kimliğe dönüştürülebilir veya parametrenin türüne null değer atanabilir dönüştürmeye (Bu, *Varsayılan bağımsız değişkende*var olan bir kısıtlamadır).</span><span class="sxs-lookup"><span data-stu-id="261c1-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="261c1-127">Bir bağımsız değişken, *çağıran-alıcı varsayılan bağımsız değişkenli*karşılık gelen bir isteğe bağlı parametre için bir işlev üyesinin çağrısından atlandığında, alıcı üyesinin değeri örtük olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="261c1-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="261c1-128">**Tasarım Notları**: arayan alıcı parametresinin ana nedeni *WITH ifadesi*' i desteklemelidir.</span><span class="sxs-lookup"><span data-stu-id="261c1-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="261c1-129">Buradaki gibi bir yöntemi bildirebilmeniz önerilir</span><span class="sxs-lookup"><span data-stu-id="261c1-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="261c1-130">daha sonra bunu bu şekilde kullanın</span><span class="sxs-lookup"><span data-stu-id="261c1-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="261c1-131">Var olan bir `Point` gibi yeni bir `Point` oluşturmak için `X` değeri değişti.</span><span class="sxs-lookup"><span data-stu-id="261c1-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="261c1-132">Bu bir açık sorudır ve *WITH ifadesi* için, çağıran alıcı parametrelerine yönelik destek edindikten sonra *bunun yerine* bu şekilde ekleme yapıp yapmadığımızda *,* *WITH ifadesi*yerine bunu yapmanız mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="261c1-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="261c1-133">[] **Açık sorun**: bir *arayan alıcı varsayılan bağımsız değişkeninin* diğer bağımsız değişkenlere göre değerlendirildiği sıra nedir?</span><span class="sxs-lookup"><span data-stu-id="261c1-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="261c1-134">Belirtilmemiş olduğunu söyliyoruz mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="261c1-135">with ifadeleri</span><span class="sxs-lookup"><span data-stu-id="261c1-135">with-expressions</span></span>

<span data-ttu-id="261c1-136">Yeni bir ifade formu önerilir:</span><span class="sxs-lookup"><span data-stu-id="261c1-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="261c1-137">Belirteç `with`, bağlama duyarlı yeni bir anahtar sözcüktür.</span><span class="sxs-lookup"><span data-stu-id="261c1-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="261c1-138">*With_initializer* solundaki her *tanımlayıcı* , *with_expression* *primary_expression* türünün erişilebilir bir örnek alanına veya özelliğine bağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="261c1-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="261c1-139">Belirli bir *with_expression*bu tanımlayıcılar arasında yinelenen ad olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="261c1-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="261c1-140">Formun *with_expression*</span><span class="sxs-lookup"><span data-stu-id="261c1-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="261c1-141">*e1* `with` `{` *tanımlayıcı* = *e2*,... `}`</span><span class="sxs-lookup"><span data-stu-id="261c1-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="261c1-142">Form çağrısı olarak değerlendirilir</span><span class="sxs-lookup"><span data-stu-id="261c1-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="261c1-143">*e1*`.With(`*identifier2*`:` *e2*,...`)`</span><span class="sxs-lookup"><span data-stu-id="261c1-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="261c1-144">Burada, `With` adlı her bir yöntem için, bu yöntemin erişilebilir bir örnek üyesi olan her *yöntemin, bu*yöntemdeki örnek alan veya *özellik ile aynı*üye olan bir arayan alıcı parametresine sahip olan ilk parametrenin adı olarak *identifier2* ' i seçeceğiz.</span><span class="sxs-lookup"><span data-stu-id="261c1-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="261c1-145">Böyle bir parametre tanımlanamıyorsa yöntemin göz önünde bulundurulmaz.</span><span class="sxs-lookup"><span data-stu-id="261c1-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="261c1-146">Çağrılacak yöntem, aşırı yükleme çözünürlüğünden kalan adaylar arasından seçilir.</span><span class="sxs-lookup"><span data-stu-id="261c1-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="261c1-147">**Tasarım Notları**: çağıran alıcı parametreleri, bu özel söz dizimi formu olmadan *WITH ifadesinin* avantajlarından birçoğu kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="261c1-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="261c1-148">Bu nedenle, gerekli olup olmadığını düşünüyoruz.</span><span class="sxs-lookup"><span data-stu-id="261c1-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="261c1-149">Temel avantajı, parametrelerin adları bakımından değil, bir programa, alanların ve özelliklerin adları açısından izin verir.</span><span class="sxs-lookup"><span data-stu-id="261c1-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="261c1-150">Bu şekilde, her ikisi de okunabilirlik ve araç kalitesini geliştirdik (örn. bir *with_expression* tanımlayıcısına göre tanım, yöntem parametresi yerine özelliğe gidebilir).</span><span class="sxs-lookup"><span data-stu-id="261c1-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="261c1-151">[] **Açık sorun**: Bu açıklama uzantı yöntemlerini destekleyecek şekilde değiştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="261c1-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="261c1-152">desen eşleştirme</span><span class="sxs-lookup"><span data-stu-id="261c1-152">pattern-matching</span></span>

<span data-ttu-id="261c1-153">`Deconstruct` belirtimi ve onun düzeniyle eşleşen ilişkisi için bkz. [model eşleştirme belirtimi](csharp-8.0/patterns.md#positional-pattern) .</span><span class="sxs-lookup"><span data-stu-id="261c1-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="261c1-154">**Tasarım Notları**: derleyici tarafından oluşturulan `Deconstruct` burada belirtilen şekilde sanallaştırarak ve model eşleme, bir kayıt bildirimi belirtimi</span><span class="sxs-lookup"><span data-stu-id="261c1-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="261c1-155">Konumsal model eşleştirmeyi aşağıdaki gibi destekleyecektir</span><span class="sxs-lookup"><span data-stu-id="261c1-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="261c1-156">Kayıt türü bildirimleri</span><span class="sxs-lookup"><span data-stu-id="261c1-156">record type declarations</span></span>

<span data-ttu-id="261c1-157">`class` veya `struct` bildirimi için sözdizimi, değer parametrelerini destekleyecek şekilde genişletilir; Parametreler, türün özellikleri olur:</span><span class="sxs-lookup"><span data-stu-id="261c1-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="261c1-158">**Tasarım Notları**: kayıt türleri genellikle sınıf gövdesinde açıkça belirtilen herhangi bir üyeye gerek olmadan yararlı olduğundan, bir gövdenin sözdizimini yalnızca noktalı virgül olacak şekilde değiştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="261c1-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="261c1-159">Kayıt *parametreleriyle* belirtilen bir sınıf (struct), biri *kayıt türü*olan bir *kayıt sınıfı* (*kayıt yapısı*) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="261c1-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="261c1-160">[] **Açık sorun**: bir kayıt türü bildiriminin içinde görünebilmesi için *primary_constructor_body* dilbilgisinde yer almalıdır.</span><span class="sxs-lookup"><span data-stu-id="261c1-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="261c1-161">[] **Açma sorunu**: parametre adları için ad çakışması kuralları nelerdir?</span><span class="sxs-lookup"><span data-stu-id="261c1-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="261c1-162">Bir tür parametresiyle veya başka bir *kayıt parametresiyle*çakışmayla izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="261c1-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="261c1-163">[] **Açma sorunu**: kayıt parametrelerinin kapsamını belirtmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="261c1-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="261c1-164">Nerede kullanılabilir?</span><span class="sxs-lookup"><span data-stu-id="261c1-164">Where can they be used?</span></span> <span data-ttu-id="261c1-165">Örnek alanı başlatıcıları ve en azından *primary_constructor_body* içinde görünür.</span><span class="sxs-lookup"><span data-stu-id="261c1-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="261c1-166">[] **Açık sorun**: bir kayıt türü bildirimi kısmi olabilir mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="261c1-167">Varsa, parametrelerin her bölümde yinelenmeleri gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="261c1-168">Bir kayıt türünün üyeleri</span><span class="sxs-lookup"><span data-stu-id="261c1-168">Members of a record type</span></span>

<span data-ttu-id="261c1-169">*Sınıf gövdesinde*belirtilen üyelere ek olarak, bir kayıt türü aşağıdaki ek üyelere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="261c1-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="261c1-170">Birincil Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="261c1-170">Primary Constructor</span></span>

<span data-ttu-id="261c1-171">Bir kayıt türünün, imzası tür bildiriminin değer parametrelerine karşılık gelen `public` Oluşturucusu vardır.</span><span class="sxs-lookup"><span data-stu-id="261c1-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="261c1-172">Bu, türü için *birincil Oluşturucu* olarak adlandırılır ve örtük olarak belirtilen *varsayılan oluşturucunun* gizlenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="261c1-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="261c1-173">Çalışma zamanında birincil Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="261c1-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="261c1-174">değer parametrelerine karşılık gelen özellikler için derleyici tarafından oluşturulan yedekleme alanlarını başlatır (Bu özellikler derleyici tarafından sağlanmışsa). [bkz. 1.1.2](#1.1.2)); ni</span><span class="sxs-lookup"><span data-stu-id="261c1-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="261c1-175">*sınıf gövdesinde*görünen örnek alanı başlatıcıları 'nı yürütür; ve ardından</span><span class="sxs-lookup"><span data-stu-id="261c1-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="261c1-176">bir temel sınıf oluşturucusunu çağırır:</span><span class="sxs-lookup"><span data-stu-id="261c1-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="261c1-177">*Record_base_arguments*bağımsız değişkenler varsa, bu bağımsız değişkenlerle aşırı yükleme çözümlemesi tarafından seçilen bir temel oluşturucu çağrılır;</span><span class="sxs-lookup"><span data-stu-id="261c1-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="261c1-178">Aksi halde, bir temel Oluşturucu bağımsız değişken olmadan çağrılır.</span><span class="sxs-lookup"><span data-stu-id="261c1-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="261c1-179">, varsa, kaynak sırada her bir *primary_constructor_body*gövdesini yürütür.</span><span class="sxs-lookup"><span data-stu-id="261c1-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="261c1-180">[] **Açık sorun**: Bu sırayı, özellikle de parmalar için derleme birimleri arasında belirtmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="261c1-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="261c1-181">[] **Açma sorunu**: açıkça belirtilen her oluşturucunun birincil oluşturucuya zincirlemesi gerektiğini belirtmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="261c1-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="261c1-182">[] **Açık sorun**: birincil oluşturucuda erişim değiştiricisinin değiştirilmesine izin verilmesi gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="261c1-183">[] **Açma sorunu**: bir kayıt yapısı içinde, hiçbir kayıt parametresi olmaması hatadır mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="261c1-184">Birincil Oluşturucu gövdesi</span><span class="sxs-lookup"><span data-stu-id="261c1-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="261c1-185">*Primary_constructor_body* , yalnızca bir kayıt türü bildiriminde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="261c1-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="261c1-186">Bir *primary_constructor_body* *tanımlayıcısı* , bildirildiği kayıt türünü olarak adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="261c1-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="261c1-187">*Primary_constructor_body* kendi kendine üye bildirmiyor, ancak Programcı 'nin öznitelikleri sağlaması ve bir kayıt türünün birincil oluşturucusunun erişimini belirtmesi için bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="261c1-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="261c1-188">Ayrıca, programcının bir kayıt türü örneği oluşturulduğunda yürütülecek ek kod sağlamasına de olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="261c1-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="261c1-189">[] **Açık sorun**: bir struct varsayılan oluşturucusunun bunu atladığını unutmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="261c1-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="261c1-190">[] **Açma sorunu**: başlatmanın yürütme sırasını belirttik.</span><span class="sxs-lookup"><span data-stu-id="261c1-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="261c1-191">[] **Açma sorunu**: bir kayıt olmayan tür bildiriminde *primary_constructor_body* (öznitelikler ve değiştiriciler olmadan) gibi bir şeye izin vermemiz ve bir örnek alanı başlatıcısının kodu yaptığımız gibi davranmız gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="261c1-192">Özellikler</span><span class="sxs-lookup"><span data-stu-id="261c1-192">Properties</span></span>

<span data-ttu-id="261c1-193">Bir kayıt türü bildiriminin her bir kayıt parametresi için, Name ve Type değeri Parameter bildiriminden alınan karşılık gelen bir `public` Property üyesi vardır.</span><span class="sxs-lookup"><span data-stu-id="261c1-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="261c1-194">Bunun adı, varsa *identifier* *record_property_name*, varsa *record_parameter* *tanımlayıcısıdır* .</span><span class="sxs-lookup"><span data-stu-id="261c1-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="261c1-195">`get` erişimcisine sahip ve bu ad ve türe sahip bir somut (yani soyut olmayan) ortak özelliği yoksa, derleyici tarafından aşağıdaki gibi üretilir:</span><span class="sxs-lookup"><span data-stu-id="261c1-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="261c1-196">Bir *kayıt yapısı* veya `sealed` *kayıt sınıfı*için:</span><span class="sxs-lookup"><span data-stu-id="261c1-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="261c1-197">Bir `private` `readonly` alanı bir `readonly` özelliği için bir yedekleme alanı olarak üretilir.</span><span class="sxs-lookup"><span data-stu-id="261c1-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="261c1-198">Değeri, oluşturma sırasında karşılık gelen birincil Oluşturucu parametresinin değeri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="261c1-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="261c1-199">Özelliğin `get` erişimcisi, yedekleme alanının değerini döndürecek şekilde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="261c1-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="261c1-200">Devralınan her "eşleşen" sanal özelliğin `get` erişimcisi geçersiz kılındı.</span><span class="sxs-lookup"><span data-stu-id="261c1-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="261c1-201">**Tasarım Notları**: diğer bir deyişle, bir temel sınıfı genişletebilir veya aynı ada sahip ortak soyut özelliği bildiren bir arabirim ve bir kayıt parametresi olarak yazın, bu özellik geçersiz kılınır veya uygulanır.</span><span class="sxs-lookup"><span data-stu-id="261c1-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="261c1-202">[] **Açık sorun**: açıkça bildirildiği zaman bir özelliğin erişim değiştiricisini değiştirmek mümkün olmalıdır mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="261c1-203">[] **Açık sorun**: bir özellik için bir alanı değiştirmek mümkün olmalıdır mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="261c1-204">Nesne yöntemleri</span><span class="sxs-lookup"><span data-stu-id="261c1-204">Object Methods</span></span>

<span data-ttu-id="261c1-205">Bir *kayıt yapısı* veya `sealed` *kayıt sınıfı*için, `object.GetHashCode()` ve `object.Equals(object)` yöntemlerinin uygulamaları, Kullanıcı tarafından sağlanmadığı takdirde derleyici tarafından üretilir.</span><span class="sxs-lookup"><span data-stu-id="261c1-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="261c1-206">[] **Açık sorun**: uygulamasını tam olarak belirtmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="261c1-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="261c1-207">[] **Açık sorun**: Ayrıca, kayıt türü için arabirim `IEquatable<T>` eklemeli ve uygulamaların sağlandığını belirtmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="261c1-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="261c1-208">[] **Açık sorun**: her `IEquatable<T>.Equals`uyguladığımızda de belirteceğiz.</span><span class="sxs-lookup"><span data-stu-id="261c1-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="261c1-209">[] **Açık sorun**: tam olarak, kayıt devralma aşamasında eşittir sorununu çözeceğiz: özel olarak simetrik, geçişli, yansımalı, vb. gibi eşitlik yöntemleri üretme.</span><span class="sxs-lookup"><span data-stu-id="261c1-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="261c1-210">[] **Açık sorun**: kayıt türleri için `operator ==` ve `operator !=` uyguladığımızda teklif edildi.</span><span class="sxs-lookup"><span data-stu-id="261c1-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="261c1-211">[] **Açık sorun**: `object.ToString`bir uygulamasını otomatik olarak oluşturmamız gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="261c1-212">Kullanıcı tarafından herhangi bir imzaya sahip olmadığı müddetçe bir kayıt türü derleyicinin ürettiği `public` yöntemine sahiptir `void Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="261c1-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="261c1-213">Her parametre aynı ada sahip bir `out` parametresidir ve kayıt türünün karşılık gelen parametresiyle aynı türde.</span><span class="sxs-lookup"><span data-stu-id="261c1-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="261c1-214">Bu yöntemin derleyici tarafından sağlanmış uygulanması, her bir `out` parametresini karşılık gelen özelliğin değeriyle atayacaktır.</span><span class="sxs-lookup"><span data-stu-id="261c1-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="261c1-215">`Deconstruct`semantiği için bkz. [model eşleme belirtimi](csharp-8.0/patterns.md#positional-pattern) .</span><span class="sxs-lookup"><span data-stu-id="261c1-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="261c1-216">`With` yöntemi</span><span class="sxs-lookup"><span data-stu-id="261c1-216">`With` method</span></span>

<span data-ttu-id="261c1-217">`With` bildirildiği adlı Kullanıcı tarafından belirtilen bir üye olmadıkça, kayıt türü, dönüş türü kayıt türü olan `With` adlı derleyici tarafından sağlanmış bir yönteme sahiptir ve bu parametrelerin kayıt türü bildiriminde göründüğü sırada her bir *Record parametresi* ile eşleşen bir değer parametresi içerir.</span><span class="sxs-lookup"><span data-stu-id="261c1-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="261c1-218">Her parametrenin, karşılık gelen özelliğe ait bir *arayan alıcı varsayılan bağımsız değişkeni* olacaktır.</span><span class="sxs-lookup"><span data-stu-id="261c1-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="261c1-219">`abstract` bir kayıt sınıfında, derleyici tarafından sağlanmış `With` yöntemi soyuttur.</span><span class="sxs-lookup"><span data-stu-id="261c1-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="261c1-220">Bir kayıt yapısına veya korumalı bir kayıt sınıfında, derleyici tarafından sağlanmış `With` yöntemi `sealed`.</span><span class="sxs-lookup"><span data-stu-id="261c1-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="261c1-221">Aksi takdirde, derleyici tarafından sağlanmış `With` yöntemi ' sanal ' dir ve bunun uygulanması, parametrelerden yeni bir örnek oluşturmak için bağımsız değişken olarak parametrelere sahip birincil oluşturucuyu çağırarak oluşturulan yeni bir örneği döndürür ve bu yeni örneği döndürür.</span><span class="sxs-lookup"><span data-stu-id="261c1-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="261c1-222">[] **Açık sorun**: aynı zamanda devralınan sanal `With` yöntemleri veya uygulanan arabirimlerden `With` yöntemleri hangi koşullarda belirttiğimiz veya uygulamamız gerekir.</span><span class="sxs-lookup"><span data-stu-id="261c1-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="261c1-223">[] **Açık sorun**: sanal olmayan bir `With` yöntemi devraldığı zaman ne olacağını söylemelidir.</span><span class="sxs-lookup"><span data-stu-id="261c1-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="261c1-224">**Tasarım Notları**: kayıt türleri varsayılan olarak sabit olduğu için `With` yöntemi, var olan bir örnekle aynı ancak seçili özelliklerle yeni değerler verilen yeni bir örnek oluşturma yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="261c1-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="261c1-225">Örneğin, verilen</span><span class="sxs-lookup"><span data-stu-id="261c1-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="261c1-226">derleyici tarafından belirtilen üye var</span><span class="sxs-lookup"><span data-stu-id="261c1-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="261c1-227">Kayıt türü değişkenini sağlayan</span><span class="sxs-lookup"><span data-stu-id="261c1-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="261c1-228">bir veya daha fazla özelliği farklı olan bir örnekle değiştirilmesini sağlamak için</span><span class="sxs-lookup"><span data-stu-id="261c1-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="261c1-229">Bu da *with_expression*kullanılarak ifade edilebilir:</span><span class="sxs-lookup"><span data-stu-id="261c1-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="261c1-230">Örnekler</span><span class="sxs-lookup"><span data-stu-id="261c1-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="261c1-231">Kayıt türlerinin uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="261c1-231">Compatibility of record types</span></span>

<span data-ttu-id="261c1-232">Programcı bir kayıt türü bildirimine üye ekleyebildiğinden, varolan istemcileri etkilemeden kayıt öğelerinin kümesini değiştirmek genellikle mümkündür.</span><span class="sxs-lookup"><span data-stu-id="261c1-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="261c1-233">Örneğin, bir kayıt türünün ilk sürümü verildiğinde</span><span class="sxs-lookup"><span data-stu-id="261c1-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="261c1-234">Kayıt türünün yeni bir öğesi, ikili veya kaynak uyumluluğunu etkilemeden türün bir sonraki düzeltmesine göre uyumluluk sağlayabilir:</span><span class="sxs-lookup"><span data-stu-id="261c1-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="261c1-235">kayıt yapısı örneği</span><span class="sxs-lookup"><span data-stu-id="261c1-235">record struct example</span></span>

<span data-ttu-id="261c1-236">Bu kayıt yapısı</span><span class="sxs-lookup"><span data-stu-id="261c1-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="261c1-237">Bu koda çevrilir</span><span class="sxs-lookup"><span data-stu-id="261c1-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="261c1-238">[] **Açık sorun**: eşittir (diğer çifti) uygulanması, Çift çiftinin ortak üyesi olmalıdır mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="261c1-239">[] **Açma sorunu**: Bu `Equals` uygulanması devralma aşamasında simetrik değildir.</span><span class="sxs-lookup"><span data-stu-id="261c1-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="261c1-240">[] **Açık sorun**: bir kayıt `operator ==` ve `operator !=`bildirmelidir mi?</span><span class="sxs-lookup"><span data-stu-id="261c1-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="261c1-241">**Tasarım Notları**: bir kayıt türü diğerinden devraldığı ve bu `Equals` uygulanması bu durumda simetrik olmadığından, doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="261c1-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="261c1-242">Bu şekilde eşitlik uygulamayı öneriyoruz:</span><span class="sxs-lookup"><span data-stu-id="261c1-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="261c1-243">Türetilmiş kayıtlar `override EqualityContract`.</span><span class="sxs-lookup"><span data-stu-id="261c1-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="261c1-244">Daha az çekici bir alternatif, devralmayı kısıtlamadır.</span><span class="sxs-lookup"><span data-stu-id="261c1-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="261c1-245">korumalı kayıt örneği</span><span class="sxs-lookup"><span data-stu-id="261c1-245">sealed record example</span></span>

<span data-ttu-id="261c1-246">Bu korumalı kayıt sınıfı</span><span class="sxs-lookup"><span data-stu-id="261c1-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="261c1-247">Bu koda çevrilir</span><span class="sxs-lookup"><span data-stu-id="261c1-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="261c1-248">Soyut kayıt sınıfı örneği</span><span class="sxs-lookup"><span data-stu-id="261c1-248">abstract record class example</span></span>

<span data-ttu-id="261c1-249">Bu soyut kayıt sınıfı</span><span class="sxs-lookup"><span data-stu-id="261c1-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="261c1-250">Bu koda çevrilir</span><span class="sxs-lookup"><span data-stu-id="261c1-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="261c1-251">soyut ve korumalı kayıtları birleştirme</span><span class="sxs-lookup"><span data-stu-id="261c1-251">combining abstract and sealed records</span></span>

<span data-ttu-id="261c1-252">Yukarıdaki `Person` soyut kayıt sınıfı verildiğinde, bu korumalı kayıt sınıfı</span><span class="sxs-lookup"><span data-stu-id="261c1-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="261c1-253">Bu koda çevrilir</span><span class="sxs-lookup"><span data-stu-id="261c1-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="261c1-254">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="261c1-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="261c1-255">Herhangi bir dil özelliğinde olduğu gibi, bu dilin ek karmaşıklığının, özellikten faydalanabilecek C# programlar gövdesine sunulan ek açıklıkla bir repaıp olup olmadığını sormalıdır.</span><span class="sxs-lookup"><span data-stu-id="261c1-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="261c1-256">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="261c1-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="261c1-257">6 ' C# da *birincil oluşturucular* ekleme kabul ettik.</span><span class="sxs-lookup"><span data-stu-id="261c1-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="261c1-258">Bu teklifle aynı sözdizimsel yüzeyi içerse de, kayıtlar tarafından sunulan avantajların ne kadar kısa olduğunu bulduk.</span><span class="sxs-lookup"><span data-stu-id="261c1-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="261c1-259">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="261c1-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="261c1-260">Açık sorular teklifin gövdesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="261c1-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="261c1-261">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="261c1-261">Design meetings</span></span>

<span data-ttu-id="261c1-262">TBD</span><span class="sxs-lookup"><span data-stu-id="261c1-262">TBD</span></span>
