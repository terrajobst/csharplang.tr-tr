---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485212"
---
# <a name="nullable-reference-types-specification"></a><span data-ttu-id="cfccd-101">Null yapılabilir başvuru türleri belirtimi</span><span class="sxs-lookup"><span data-stu-id="cfccd-101">Nullable Reference Types Specification</span></span>

<span data-ttu-id="cfccd-102">Bu, devam eden bir çalışmadır. birkaç bölüm eksik veya eksik.</span><span class="sxs-lookup"><span data-stu-id="cfccd-102">\*\*\* This is a work in progress - several parts are missing or incomplete.</span></span> ***

## <a name="syntax"></a><span data-ttu-id="cfccd-103">Sözdizimi</span><span class="sxs-lookup"><span data-stu-id="cfccd-103">Syntax</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="cfccd-104">Boş değer atanabilir başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-104">Nullable reference types</span></span>

<span data-ttu-id="cfccd-105">Null yapılabilir başvuru türleri, null yapılabilir değer türlerinin kısa biçimiyle aynı `T?` sözdizimine sahiptir, ancak karşılık gelen uzun bir biçime sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-105">Nullable reference types have the same syntax `T?` as the short form of nullable value types, but do not have a corresponding long form.</span></span>

<span data-ttu-id="cfccd-106">Belirtim amaçları doğrultusunda, geçerli `nullable_type` üretimi `nullable_value_type`olarak yeniden adlandırılır ve bir `nullable_reference_type` üretimi eklenir:</span><span class="sxs-lookup"><span data-stu-id="cfccd-106">For the purposes of the specification, the current `nullable_type` production is renamed to `nullable_value_type`, and a `nullable_reference_type` production is added:</span></span>

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

<span data-ttu-id="cfccd-107">Bir `nullable_reference_type` `non_nullable_reference_type` null yapılamayan bir başvuru türü (sınıf, arabirim, temsilci veya dizi) veya null yapılamayan bir başvuru türü (`class` kısıtlaması aracılığıyla veya `object`dışında bir sınıf) olarak kısıtlanmış bir tür parametresi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-107">The `non_nullable_reference_type` in a `nullable_reference_type` must be a non-nullable reference type (class, interface, delegate or array), or a type parameter that is constrained to be a non-nullable reference type (through the `class` constraint, or a class other than `object`).</span></span>

<span data-ttu-id="cfccd-108">Null yapılabilir başvuru türleri aşağıdaki konumlarda gerçekleşemez:</span><span class="sxs-lookup"><span data-stu-id="cfccd-108">Nullable reference types cannot occur in the following positions:</span></span>

- <span data-ttu-id="cfccd-109">temel sınıf veya arabirim olarak</span><span class="sxs-lookup"><span data-stu-id="cfccd-109">as a base class or interface</span></span>
- <span data-ttu-id="cfccd-110">`member_access` alıcısı olarak</span><span class="sxs-lookup"><span data-stu-id="cfccd-110">as the receiver of a `member_access`</span></span>
- <span data-ttu-id="cfccd-111">bir `object_creation_expression` `type` olarak</span><span class="sxs-lookup"><span data-stu-id="cfccd-111">as the `type` in an `object_creation_expression`</span></span>
- <span data-ttu-id="cfccd-112">`delegate_creation_expression` `delegate_type` olarak</span><span class="sxs-lookup"><span data-stu-id="cfccd-112">as the `delegate_type` in a `delegate_creation_expression`</span></span>
- <span data-ttu-id="cfccd-113">bir `is_expression`, `catch_clause` veya `type_pattern` `type` olarak</span><span class="sxs-lookup"><span data-stu-id="cfccd-113">as the `type` in an `is_expression`, a `catch_clause` or a `type_pattern`</span></span>
- <span data-ttu-id="cfccd-114">tam nitelikli arabirim üye adında `interface` olarak</span><span class="sxs-lookup"><span data-stu-id="cfccd-114">as the `interface` in a fully qualified interface member name</span></span>

<span data-ttu-id="cfccd-115">Null yapılabilir ek açıklama bağlamının devre dışı olduğu `nullable_reference_type` bir uyarı verilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-115">A warning is given on a `nullable_reference_type` where the nullable annotation context is disabled.</span></span>

### <a name="nullable-class-constraint"></a><span data-ttu-id="cfccd-116">Nullable sınıf kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="cfccd-116">Nullable class constraint</span></span>

<span data-ttu-id="cfccd-117">`class` kısıtlamasının null olabilen bir karşılığı `class?`vardır:</span><span class="sxs-lookup"><span data-stu-id="cfccd-117">The `class` constraint has a nullable counterpart `class?`:</span></span>

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a><span data-ttu-id="cfccd-118">Null-forverme işleci</span><span class="sxs-lookup"><span data-stu-id="cfccd-118">The null-forgiving operator</span></span>

<span data-ttu-id="cfccd-119">Onarma sonrası `!` işlecine null-forverme işleci denir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-119">The post-fix `!` operator is called the null-forgiving operator.</span></span>

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

<span data-ttu-id="cfccd-120">`primary_expression` bir başvuru türünde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-120">The `primary_expression` must be of a reference type.</span></span>  

<span data-ttu-id="cfccd-121">Sonek `!` işlecinin çalışma zamanı etkisi yoktur; temel alınan ifadenin sonucunu değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-121">The postfix `!` operator has no runtime effect - it evaluates to the result of the underlying expression.</span></span> <span data-ttu-id="cfccd-122">Tek rolü, ifadenin null durumunu değiştirmek ve kullanım sırasında verilen uyarıları sınırlamak içindir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-122">Its only role is to change the null state of the expression, and to limit warnings given on its use.</span></span>

### <a name="nullable-implicitly-typed-local-variables"></a><span data-ttu-id="cfccd-123">null atanabilir örtük olarak yazılan yerel değişkenler</span><span class="sxs-lookup"><span data-stu-id="cfccd-123">nullable implicitly typed local variables</span></span>

<span data-ttu-id="cfccd-124">`var`, başvuru türleri için açıklamalı bir tür.</span><span class="sxs-lookup"><span data-stu-id="cfccd-124">`var` infers an annotated type for reference types.</span></span>
<span data-ttu-id="cfccd-125">Örneğin, `var s = "";` `var` `string?`olarak algılanır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-125">For instance, in `var s = "";` the `var` is inferred as `string?`.</span></span>

### <a name="nullable-compiler-directives"></a><span data-ttu-id="cfccd-126">Null yapılabilir derleyici yönergeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-126">Nullable compiler directives</span></span>

<span data-ttu-id="cfccd-127">`#nullable` yönergeler null yapılabilir ek açıklama ve uyarı bağlamlarını denetler.</span><span class="sxs-lookup"><span data-stu-id="cfccd-127">`#nullable` directives control the nullable annotation and warning contexts.</span></span>

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

<span data-ttu-id="cfccd-128">`#pragma warning` yönergeler, null olabilen uyarı bağlamını değiştirmeye izin vermek ve varsayılan olarak devre dışı bırakılmış olsalar bile tek tek uyarıların etkinleştirilmesini sağlamak üzere genişletilir:</span><span class="sxs-lookup"><span data-stu-id="cfccd-128">`#pragma warning` directives are expanded to allow changing the nullable warning context, and to allow individual warnings to be enabled on even when they're disabled by default:</span></span>

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

<span data-ttu-id="cfccd-129">`pragma_warning_body` yeni biçiminin `warning_action`değil `nullable_action`kullandığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="cfccd-129">Note that the new form of `pragma_warning_body` uses `nullable_action`, not `warning_action`.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="cfccd-130">Null yapılabilir bağlamlar</span><span class="sxs-lookup"><span data-stu-id="cfccd-130">Nullable contexts</span></span>

<span data-ttu-id="cfccd-131">Her kaynak kodu satırında *null yapılabilir bir ek açıklama bağlamı* ve *null yapılabilir uyarı bağlamı*vardır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-131">Every line of source code has a *nullable annotation context* and a *nullable warning context*.</span></span> <span data-ttu-id="cfccd-132">Bu denetim, null yapılabilir ek açıklamaların etkin olup olmadığını ve null olabilme uyarılarının verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="cfccd-132">These control whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="cfccd-133">Verilen bir çizginin ek açıklama bağlamı *devre dışı* veya *etkin*.</span><span class="sxs-lookup"><span data-stu-id="cfccd-133">The annotation context of a given line is either *disabled* or *enabled*.</span></span> <span data-ttu-id="cfccd-134">Verilen bir satırın uyarı bağlamı *devre dışı* veya *etkin*değil.</span><span class="sxs-lookup"><span data-stu-id="cfccd-134">The warning context of a given line is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="cfccd-135">Her iki içerik de proje düzeyinde ( C# kaynak kodu dışında) veya kaynak dosya içinde herhangi bir yerde `#nullable` ön işlemci yönergeleri aracılığıyla belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-135">Both contexts can be specified at the project level (outside of C# source code), or anywhere within a source file via `#nullable` pre-processor directives.</span></span> <span data-ttu-id="cfccd-136">Hiçbir proje düzeyi ayarı sağlanmazsa, varsayılan olarak her iki bağlam da *devre dışı*bırakılır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-136">If no project level settings are provided the default is for both contexts to be *disabled*.</span></span>

<span data-ttu-id="cfccd-137">`#nullable` yönergesi, kaynak metin içindeki ek açıklamaları ve uyarı bağlamlarını denetler ve proje düzeyi ayarlarından önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-137">The `#nullable` directive controls the annotation and warning contexts within the source text, and take precedence over the project-level settings.</span></span>

<span data-ttu-id="cfccd-138">Bir yönerge, diğer bir yönerge tarafından geçersiz kılınana kadar veya kaynak dosyanın sonuna kadar, sonraki kod satırları için denetim bağlamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="cfccd-138">A directive sets the context(s) it controls for subsequent lines of code, until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="cfccd-139">Yönergelerin etkisi aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="cfccd-139">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="cfccd-140">`#nullable disable`: Nullable ek açıklama ve uyarı bağlamlarını *devre dışı* olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="cfccd-140">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*</span></span>
- <span data-ttu-id="cfccd-141">`#nullable enable`: null yapılabilir ek açıklama ve uyarı bağlamlarını *etkin* olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="cfccd-141">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*</span></span>
- <span data-ttu-id="cfccd-142">`#nullable restore`: proje ayarlarına Nullable ek açıklama ve uyarı bağlamlarını geri yükler</span><span class="sxs-lookup"><span data-stu-id="cfccd-142">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings</span></span>
- <span data-ttu-id="cfccd-143">`#nullable disable annotations`: Nullable ek açıklama bağlamını *devre dışı* olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="cfccd-143">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*</span></span>
- <span data-ttu-id="cfccd-144">`#nullable enable annotations`: Nullable ek açıklama bağlamını *etkin* olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="cfccd-144">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*</span></span>
- <span data-ttu-id="cfccd-145">`#nullable restore annotations`: proje ayarlarına Nullable ek açıklama bağlamını geri yükler</span><span class="sxs-lookup"><span data-stu-id="cfccd-145">`#nullable restore annotations`: Restores the nullable annotation context to project settings</span></span>
- <span data-ttu-id="cfccd-146">`#nullable disable warnings`: Nullable uyarı bağlamını *devre dışı* olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="cfccd-146">`#nullable disable warnings`: Sets the nullable warning context to *disabled*</span></span>
- <span data-ttu-id="cfccd-147">`#nullable enable warnings`: null yapılabilir uyarı bağlamını *etkin* olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="cfccd-147">`#nullable enable warnings`: Sets the nullable warning context to *enabled*</span></span>
- <span data-ttu-id="cfccd-148">`#nullable restore warnings`: proje ayarlarına Nullable uyarı bağlamını geri yükler</span><span class="sxs-lookup"><span data-stu-id="cfccd-148">`#nullable restore warnings`: Restores the nullable warning context to project settings</span></span>

## <a name="nullability-of-types"></a><span data-ttu-id="cfccd-149">Türlerin null olabilme sayısı</span><span class="sxs-lookup"><span data-stu-id="cfccd-149">Nullability of types</span></span>

<span data-ttu-id="cfccd-150">Verili bir tür dört adet *null değer içerebilir*: *zorunlulukou*, Nullable, *null yapılabilir* ve *bilinmiyor*.</span><span class="sxs-lookup"><span data-stu-id="cfccd-150">A given type can have one of four nullabilities: *Oblivious*, *nonnullable*, *nullable* and *unknown*.</span></span> 

<span data-ttu-id="cfccd-151">*Null yapılamayan* ve *Bilinmeyen* türler, bunlara olası bir `null` değeri atandığında uyarılara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-151">*Nonnullable* and *unknown* types may cause warnings if a potential `null` value is assigned to them.</span></span> <span data-ttu-id="cfccd-152">Ancak, *Zorunluluvou* ve *null yapılabilir* türler, "*null atanabilir*" ve uyarılar olmadan bunlara atanan `null` değerlere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-152">*Oblivious* and *nullable* types, however, are "*null-assignable*" and can have `null` values assigned to them without warnings.</span></span> 

<span data-ttu-id="cfccd-153">*Zorunluluvou* ve *null yapılamayan* türler uyarı vermeden bağlanabilir veya atanabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-153">*Oblivious* and *nonnullable* types can be dereferenced or assigned without warnings.</span></span> <span data-ttu-id="cfccd-154">Null *yapılabilir* ve *Bilinmeyen* türlerin değerleri, "*null*olarak çıkarılıyor" ve doğru null denetimi yapılmadan başvurulmadan veya atandıktan sonra uyarılara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-154">Values of *nullable* and *unknown* types, however, are "*null-yielding*" and may cause warnings when dereferenced or assigned without proper null checking.</span></span> 

<span data-ttu-id="cfccd-155">Null değerli bir türün *varsayılan null durumu* "Belki null" dır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-155">The *default null state* of a null-yielding type is "maybe null".</span></span> <span data-ttu-id="cfccd-156">Null olmayan bir türün varsayılan null durumu "Not null" olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-156">The default null state of a non-null-yielding type is "not null".</span></span>

<span data-ttu-id="cfccd-157">Tür türü ve null yapılabilir ek açıklama bağlamı, null olabilme değerini belirlemede oluşur:</span><span class="sxs-lookup"><span data-stu-id="cfccd-157">The kind of type and the nullable annotation context it occurs in determine its nullability:</span></span>

- <span data-ttu-id="cfccd-158">Null yapılamayan bir değer türü `S` her zaman *null atanamaz*</span><span class="sxs-lookup"><span data-stu-id="cfccd-158">A nonnullable value type `S` is always *nonnullable*</span></span>
- <span data-ttu-id="cfccd-159">Null yapılabilir bir değer türü `S?` her zaman *null yapılabilir*</span><span class="sxs-lookup"><span data-stu-id="cfccd-159">A nullable value type `S?` is always *nullable*</span></span>
- <span data-ttu-id="cfccd-160">*Devre dışı* bir ek açıklama bağlamında `C` açıklamalı olmayan bir başvuru türü, *zorunluluvou*</span><span class="sxs-lookup"><span data-stu-id="cfccd-160">An unannotated reference type `C` in a *disabled* annotation context is *oblivious*</span></span>
- <span data-ttu-id="cfccd-161">*Etkin* olmayan bir ek açıklama bağlamında `C` açıklamalı olmayan bir başvuru türü *null atanamaz*</span><span class="sxs-lookup"><span data-stu-id="cfccd-161">An unannotated reference type `C` in an *enabled* annotation context is *nonnullable*</span></span>
- <span data-ttu-id="cfccd-162">Null yapılabilir bir başvuru türü `C?` *null* olabilir (ancak bir uyarı, *devre dışı* bir ek açıklama bağlamında istenebilir)</span><span class="sxs-lookup"><span data-stu-id="cfccd-162">A nullable reference type `C?` is *nullable* (but a warning may be yielded in a *disabled* annotation context)</span></span>

<span data-ttu-id="cfccd-163">Ayrıca tür parametrelerinin kısıtlamaları hesaba alır:</span><span class="sxs-lookup"><span data-stu-id="cfccd-163">Type parameters additionally take their constraints into account:</span></span>

- <span data-ttu-id="cfccd-164">Bir tür parametresi, tüm kısıtlamaların (varsa) null değerli türler (null*yapılabilir* ve *bilinmiyor*) ya da `class?` kısıtlaması *bilinmiyorsa* `T`.</span><span class="sxs-lookup"><span data-stu-id="cfccd-164">A type parameter `T` where all constraints (if any) are either null-yielding types (*nullable* and *unknown*) or the `class?` constraint is *unknown*</span></span>
- <span data-ttu-id="cfccd-165">Bir tür parametresi, en az bir kısıtlamanın *zorunluluvou* veya *null yapılamayan* ya da `struct` veya `class` kısıtlamalarından biri olduğu `T`.</span><span class="sxs-lookup"><span data-stu-id="cfccd-165">A type parameter `T` where at least one constraint is either *oblivious* or *nonnullable* or one of the `struct` or `class` constraints is</span></span>
    - <span data-ttu-id="cfccd-166">*devre dışı* bir ek açıklama bağlamındaki *yükümlülüğü*</span><span class="sxs-lookup"><span data-stu-id="cfccd-166">*oblivious* in a *disabled* annotation context</span></span>
    - <span data-ttu-id="cfccd-167">*etkin* olmayan bir ek açıklama bağlamında *null değer atanamaz*</span><span class="sxs-lookup"><span data-stu-id="cfccd-167">*nonnullable* in an *enabled* annotation context</span></span>
- <span data-ttu-id="cfccd-168">Null olabilen bir tür parametresi, `T`kısıtlamalarından en az birinin *yükümlülüğü* *veya `struct`* ya da `class` kısıtlamalarından biri olduğu `T?`,</span><span class="sxs-lookup"><span data-stu-id="cfccd-168">A nullable type parameter `T?` where at least one of `T`'s constraints is *oblivious* or *nonnullable* or one of the `struct` or `class` constraints, is</span></span>
    - <span data-ttu-id="cfccd-169">*devre dışı* bir ek açıklama bağlamında *boş değer atanabilir* (ancak bir uyarı uyarılandırılan)</span><span class="sxs-lookup"><span data-stu-id="cfccd-169">*nullable* in a *disabled* annotation context (but a warning is yielded)</span></span>
    - <span data-ttu-id="cfccd-170">*etkin* bir ek açıklama bağlamında *null yapılabilir*</span><span class="sxs-lookup"><span data-stu-id="cfccd-170">*nullable* in an *enabled* annotation context</span></span>

<span data-ttu-id="cfccd-171">Bir tür parametresi `T`için, `T?` yalnızca `T` bir değer türü olarak bilindiğinde veya bir başvuru türü olarak bilindiğinde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-171">For a type parameter `T`, `T?` is only allowed if `T` is known to be a value type or known to be a reference type.</span></span>

### <a name="oblivious-vs-nonnullable"></a><span data-ttu-id="cfccd-172">Zorunluluvou ile null atanamaz</span><span class="sxs-lookup"><span data-stu-id="cfccd-172">Oblivious vs nonnullable</span></span>

<span data-ttu-id="cfccd-173">Bir `type`, türün son belirteci bu bağlam içinde olduğunda, belirli bir ek açıklama bağlamında gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-173">A `type` is deemed to occur in a given annotation context when the last token of the type is within that context.</span></span>

<span data-ttu-id="cfccd-174">Kaynak kodda verilen bir başvuru türü `C`, yükümlülüğü veya null değer atanabilir olarak yorumlanıp bu kaynak kodun ek açıklama bağlamına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-174">Whether a given reference type `C` in source code is interpreted as oblivious or nonnullable depends on the annotation context of that source code.</span></span> <span data-ttu-id="cfccd-175">Ancak, bir kez kurulduktan sonra bu türün bir parçası olarak değerlendirilir ve "genel tür bağımsız değişkenlerinin yerine geçen" gibi.</span><span class="sxs-lookup"><span data-stu-id="cfccd-175">But once established, it is considered part of that type, and "travels with it" e.g. during substitution of generic type arguments.</span></span> <span data-ttu-id="cfccd-176">Tür üzerinde `?` gibi bir ek açıklama var, ancak görünmez.</span><span class="sxs-lookup"><span data-stu-id="cfccd-176">It is as if there is an annotation like `?` on the type, but invisible.</span></span>

## <a name="constraints"></a><span data-ttu-id="cfccd-177">Kısıtlamalar</span><span class="sxs-lookup"><span data-stu-id="cfccd-177">Constraints</span></span>

<span data-ttu-id="cfccd-178">Null yapılabilir başvuru türleri, genel kısıtlamalar olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-178">Nullable reference types can be used as generic constraints.</span></span> <span data-ttu-id="cfccd-179">Ayrıca `object` artık açık bir kısıtlama olarak geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-179">Furthermore `object` is now valid as an explicit constraint.</span></span> <span data-ttu-id="cfccd-180">Bir kısıtlamanın yokluğu artık `object?` kısıtlamasına eşdeğerdir (`object`değil), ancak (daha önce `object` aksine) `object?` açık bir kısıtlama olarak yasaklanmış değildir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-180">Absence of a constraint is now equivalent to an `object?` constraint (instead of `object`), but (unlike `object` before) `object?` is not prohibited as an explicit constraint.</span></span>

<span data-ttu-id="cfccd-181">`class?`, "muhtemelen Nullable başvuru türü" belirten yeni bir kısıtlamadır, ancak `class` "Nullable başvuru türü" olarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-181">`class?` is a new constraint denoting "possibly nullable reference type", whereas `class` denotes "nonnullable reference type".</span></span>

<span data-ttu-id="cfccd-182">Bir tür bağımsız değişkeninin veya kısıtlama değerinin null olabilme durumu, türün Şu anda zaten olduğu durumlar dışında (null yapılabilir değer türleri `struct` kısıtlamasını karşılamıyor), türü kısıtlamayı karşılayıp karşılamadığını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="cfccd-182">The nullability of a type argument or of a constraint does not impact whether the type satisfies the constraint, except where that is already the case today (nullable value types do not satisfy the `struct` constraint).</span></span> <span data-ttu-id="cfccd-183">Ancak, tür bağımsız değişkeni kısıtlamanın null olabilme gereksinimlerini karşılamadığı takdirde bir uyarı verilebilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-183">However, if the type argument does not satisfy the nullability requirements of the constraint, a warning may be given.</span></span>

## <a name="null-state-and-null-tracking"></a><span data-ttu-id="cfccd-184">Null durum ve null izleme</span><span class="sxs-lookup"><span data-stu-id="cfccd-184">Null state and null tracking</span></span>

<span data-ttu-id="cfccd-185">Belirli bir kaynak konumundaki her ifadenin null olarak değerlendirilip değerlendirilmediğini belirten *null durumu*vardır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-185">Every expression in a given source location has a *null state*, which indicated whether it is believed to potentially evaluate to null.</span></span> <span data-ttu-id="cfccd-186">Null durumu "Not null" veya "Belki null" olabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-186">The null state is either "not null" or "maybe null".</span></span> <span data-ttu-id="cfccd-187">Null durumu, null güvenli olmayan dönüştürmeler ve başvuru işlemleri hakkında bir uyarı verilip verilmeyeceğini belirlemekte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-187">The null state is used to determine whether a warning should be given about null-unsafe conversions and dereferences.</span></span>

### <a name="null-tracking-for-variables"></a><span data-ttu-id="cfccd-188">Değişkenler için null izleme</span><span class="sxs-lookup"><span data-stu-id="cfccd-188">Null tracking for variables</span></span>

<span data-ttu-id="cfccd-189">Değişkenleri veya özellikleri belirten belirli ifadeler için, null durumu yineleme arasında, bunlara atamalara göre izlenir, bunlar üzerinde gerçekleştirilen testler ve bunlar arasındaki denetim akışı.</span><span class="sxs-lookup"><span data-stu-id="cfccd-189">For certain expressions denoting variables or properties, the null state is tracked between occurrences, based on assignments to them, tests performed on them and the control flow between them.</span></span> <span data-ttu-id="cfccd-190">Bu, değişkenler için kesin atamanın izlenme biçimine benzer.</span><span class="sxs-lookup"><span data-stu-id="cfccd-190">This is similar to how definite assignment is tracked for variables.</span></span> <span data-ttu-id="cfccd-191">İzlenen ifadeler aşağıdaki biçimdeki olanlardır:</span><span class="sxs-lookup"><span data-stu-id="cfccd-191">The tracked expressions are the ones of the following form:</span></span>

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

<span data-ttu-id="cfccd-192">Tanımlayıcılar alanları veya özellikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-192">Where the identifiers denote fields or properties.</span></span>

<span data-ttu-id="cfccd-193">***Kesin atamaya benzer null durum geçişlerini açıkla***</span><span class="sxs-lookup"><span data-stu-id="cfccd-193">***Describe null state transitions similar to definite assignment***</span></span>

### <a name="null-state-for-expressions"></a><span data-ttu-id="cfccd-194">İfadeler için null durumu</span><span class="sxs-lookup"><span data-stu-id="cfccd-194">Null state for expressions</span></span>

<span data-ttu-id="cfccd-195">Bir ifadenin null durumu, form ve türünden türetilir ve buna dahil edilen değişkenlerin null durumudur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-195">The null state of an expression is derived from its form and type, and from the null state of variables involved in it.</span></span>

### <a name="literals"></a><span data-ttu-id="cfccd-196">Sabit değerler</span><span class="sxs-lookup"><span data-stu-id="cfccd-196">Literals</span></span>

<span data-ttu-id="cfccd-197">`null` sabit değerinin null durumu "Belki null".</span><span class="sxs-lookup"><span data-stu-id="cfccd-197">The null state of a `null` literal is "maybe null".</span></span> <span data-ttu-id="cfccd-198">Null yapılamayan bir değer türü olarak bilinen bir türe dönüştürülmekte olan `default` sabit değerinin null durumu "Belki null" değeridir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-198">The null state of a `default` literal that is being converted to a type that is known not to be a nonnullable value type is "maybe null".</span></span> <span data-ttu-id="cfccd-199">Diğer herhangi bir sabit değerin null durumu "Not null" olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-199">The null state of any other literal is "not null".</span></span>

### <a name="simple-names"></a><span data-ttu-id="cfccd-200">Basit adlar</span><span class="sxs-lookup"><span data-stu-id="cfccd-200">Simple names</span></span>

<span data-ttu-id="cfccd-201">Bir `simple_name` değer olarak sınıflandırılmadığından, null durumu "null değil" olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-201">If a `simple_name` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="cfccd-202">Aksi takdirde, izlenen bir ifadedir ve null durumu bu kaynak konumda izlenen null durumundadır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-202">Otherwise it is a tracked expression, and its null state is its tracked null state at this source location.</span></span>

### <a name="member-access"></a><span data-ttu-id="cfccd-203">Üye erişimi</span><span class="sxs-lookup"><span data-stu-id="cfccd-203">Member access</span></span>

<span data-ttu-id="cfccd-204">Bir `member_access` değer olarak sınıflandırılmadığından, null durumu "null değil" olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-204">If a `member_access` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="cfccd-205">Aksi halde, izlenen bir ifadesiyse, null durumu bu kaynak konumda izlenen null durumundadır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-205">Otherwise, if it is a tracked expression, its null state is its tracked null state at this source location.</span></span> <span data-ttu-id="cfccd-206">Aksi halde, null durumu türünün varsayılan null durumudur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-206">Otherwise its null state is the default null state of its type.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="cfccd-207">Çağırma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-207">Invocation expressions</span></span>

<span data-ttu-id="cfccd-208">Bir `invocation_expression` özel null davranışı için bir veya daha fazla öznitelik ile tanımlanmış bir üyeyi çağıralıyorsa, null durumu bu öznitelikler tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-208">If an `invocation_expression` invokes a member that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="cfccd-209">Aksi takdirde, ifadenin null durumu, türünün varsayılan null durumudur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-209">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="element-access"></a><span data-ttu-id="cfccd-210">Öğe erişimi</span><span class="sxs-lookup"><span data-stu-id="cfccd-210">Element access</span></span>

<span data-ttu-id="cfccd-211">Bir `element_access` özel null davranışı için bir veya daha fazla öznitelik ile tanımlanmış bir dizin oluşturucuyu çağıralıyorsa, null durumu bu öznitelikler tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-211">If an `element_access` invokes an indexer that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="cfccd-212">Aksi takdirde, ifadenin null durumu, türünün varsayılan null durumudur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-212">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="base-access"></a><span data-ttu-id="cfccd-213">Temel erişim</span><span class="sxs-lookup"><span data-stu-id="cfccd-213">Base access</span></span>

<span data-ttu-id="cfccd-214">`B`, kapsayan türün temel türünü alıyorsa, `base.I` aynı null duruma sahip `((B)this).I` ve `base[E]` aynı null durumuna sahip `((B)this)[E]`.</span><span class="sxs-lookup"><span data-stu-id="cfccd-214">If `B` denotes the base type of the enclosing type, `base.I` has the same null state as `((B)this).I` and `base[E]` has the same null state as `((B)this)[E]`.</span></span>

### <a name="default-expressions"></a><span data-ttu-id="cfccd-215">Varsayılan ifadeler</span><span class="sxs-lookup"><span data-stu-id="cfccd-215">Default expressions</span></span>

<span data-ttu-id="cfccd-216">`T` null yapılamayan bir değer türü olarak tanınmışsa, `default(T)` null olmayan "null" durumunda olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-216">`default(T)` has the null state "non-null" if `T` is known to be a nonnullable value type.</span></span> <span data-ttu-id="cfccd-217">Aksi takdirde "Belki de null" durumunda null durumu vardır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-217">Otherwise it has the null state "maybe null".</span></span>

### <a name="null-conditional-expressions"></a><span data-ttu-id="cfccd-218">Null Koşullu ifadeler</span><span class="sxs-lookup"><span data-stu-id="cfccd-218">Null-conditional expressions</span></span>

<span data-ttu-id="cfccd-219">`null_conditional_expression` null durumunda "Belki null" değeri vardır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-219">A `null_conditional_expression` has the null state "maybe null".</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="cfccd-220">Atama ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-220">Cast expressions</span></span>

<span data-ttu-id="cfccd-221">Bir atama `(T)E` ifadesi Kullanıcı tanımlı bir dönüştürmeyi çağırmazsa, ifadenin null durumu, türü için varsayılan null durumdur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-221">If a cast expression `(T)E` invokes a user-defined conversion, then the null state of the expression is the default null state for its type.</span></span> <span data-ttu-id="cfccd-222">Aksi takdirde, `T` null halinde olursa (null*yapılabilir* veya *bilinmiyor*), null durumu "Belki null" olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-222">Otherwise, if `T` is null-yielding (*nullable* or *unknown*) then the null state is "maybe null".</span></span> <span data-ttu-id="cfccd-223">Aksi takdirde null durumu `E`null durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-223">Otherwise the null state is the same as the null state of `E`.</span></span>

### <a name="await-expressions"></a><span data-ttu-id="cfccd-224">Await ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-224">Await expressions</span></span>

<span data-ttu-id="cfccd-225">`await E` null durumu, türünün varsayılan null durumudur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-225">The null state of `await E` is the default null state of its type.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="cfccd-226">`as` işleci</span><span class="sxs-lookup"><span data-stu-id="cfccd-226">The `as` operator</span></span>

<span data-ttu-id="cfccd-227">Bir `as` ifadesi null durumunda "Belki null" değeri içeriyor.</span><span class="sxs-lookup"><span data-stu-id="cfccd-227">An `as` expression has the null state "maybe null".</span></span>

### <a name="the-null-coalescing-operator"></a><span data-ttu-id="cfccd-228">Null birleşim işleci</span><span class="sxs-lookup"><span data-stu-id="cfccd-228">The null-coalescing operator</span></span>

<span data-ttu-id="cfccd-229">`E1 ?? E2` aynı null duruma sahip `E2`</span><span class="sxs-lookup"><span data-stu-id="cfccd-229">`E1 ?? E2` has the same null state as `E2`</span></span>

### <a name="the-conditional-operator"></a><span data-ttu-id="cfccd-230">Koşullu işleç</span><span class="sxs-lookup"><span data-stu-id="cfccd-230">The conditional operator</span></span>

<span data-ttu-id="cfccd-231">Hem `E2` hem de `E3` null durumu "Not null" ise `E1 ? E2 : E3` null durumu "Not null" olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-231">The null state of `E1 ? E2 : E3` is "not null" if the null state of both `E2` and `E3` are "not null".</span></span> <span data-ttu-id="cfccd-232">Aksi takdirde "Belki de null" olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-232">Otherwise it is "maybe null".</span></span>

### <a name="query-expressions"></a><span data-ttu-id="cfccd-233">Sorgu ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-233">Query expressions</span></span>

<span data-ttu-id="cfccd-234">Sorgu ifadesinin null durumu, türünün varsayılan null durumudur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-234">The null state of a query expression is the default null state of its type.</span></span>

### <a name="assignment-operators"></a><span data-ttu-id="cfccd-235">Atama işleçleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-235">Assignment operators</span></span>

<span data-ttu-id="cfccd-236">`E1 = E2` ve `E1 op= E2`, herhangi bir örtük dönüştürme uygulandıktan sonra `E2` aynı null durumuna sahip.</span><span class="sxs-lookup"><span data-stu-id="cfccd-236">`E1 = E2` and `E1 op= E2` have the same null state as `E2` after any implicit conversions have been applied.</span></span>

### <a name="unary-and-binary-operators"></a><span data-ttu-id="cfccd-237">Birli ve ikili işleçler</span><span class="sxs-lookup"><span data-stu-id="cfccd-237">Unary and binary operators</span></span>

<span data-ttu-id="cfccd-238">Birli veya ikili işleç, özel null davranışı için bir veya daha fazla öznitelik ile belirtilen bir Kullanıcı tanımlı işleci çağırmazsa, null durumu bu öznitelikler tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-238">If a unary or binary operator invokes an user-defined operator that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="cfccd-239">Aksi takdirde, ifadenin null durumu, türünün varsayılan null durumudur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-239">Otherwise the null state of the expression is the default null state of its type.</span></span>

<span data-ttu-id="cfccd-240">***Dizeler ve temsilciler üzerinde ikili `+` için özel bir şey var mı?***</span><span class="sxs-lookup"><span data-stu-id="cfccd-240">***Something special to do for binary `+` over strings and delegates?***</span></span>

### <a name="expressions-that-propagate-null-state"></a><span data-ttu-id="cfccd-241">Null durumu yayan ifadeler</span><span class="sxs-lookup"><span data-stu-id="cfccd-241">Expressions that propagate null state</span></span>

<span data-ttu-id="cfccd-242">`(E)`, `checked(E)` ve `unchecked(E)` tümünün aynı null durumu `E`.</span><span class="sxs-lookup"><span data-stu-id="cfccd-242">`(E)`, `checked(E)` and `unchecked(E)` all have the same null state as `E`.</span></span>

### <a name="expressions-that-are-never-null"></a><span data-ttu-id="cfccd-243">Hiçbir koşulda null olmayan ifadeler</span><span class="sxs-lookup"><span data-stu-id="cfccd-243">Expressions that are never null</span></span>

<span data-ttu-id="cfccd-244">Aşağıdaki ifade formlarının null durumu her zaman "Not null" dır:</span><span class="sxs-lookup"><span data-stu-id="cfccd-244">The null state of the following expression forms is always "not null":</span></span>

- <span data-ttu-id="cfccd-245">`this` erişim</span><span class="sxs-lookup"><span data-stu-id="cfccd-245">`this` access</span></span>
- <span data-ttu-id="cfccd-246">enterpolasyonlu dizeler</span><span class="sxs-lookup"><span data-stu-id="cfccd-246">interpolated strings</span></span>
- <span data-ttu-id="cfccd-247">`new` ifadeleri (nesne, temsilci, anonim nesne ve dizi oluşturma ifadeleri)</span><span class="sxs-lookup"><span data-stu-id="cfccd-247">`new` expressions (object, delegate, anonymous object and array creation expressions)</span></span>
- <span data-ttu-id="cfccd-248">`typeof` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-248">`typeof` expressions</span></span>
- <span data-ttu-id="cfccd-249">`nameof` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-249">`nameof` expressions</span></span>
- <span data-ttu-id="cfccd-250">Anonim işlevler (anonim yöntemler ve lambda ifadeleri)</span><span class="sxs-lookup"><span data-stu-id="cfccd-250">anonymous functions (anonymous methods and lambda expressions)</span></span>
- <span data-ttu-id="cfccd-251">null-forverme ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-251">null-forgiving expressions</span></span>
- <span data-ttu-id="cfccd-252">`is` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cfccd-252">`is` expressions</span></span>

## <a name="type-inference"></a><span data-ttu-id="cfccd-253">Tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="cfccd-253">Type inference</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="cfccd-254">`var` için tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="cfccd-254">Type inference for `var`</span></span>

<span data-ttu-id="cfccd-255">`var` ile belirtilen yerel değişkenler için gösterilen tür, başlatma ifadesinin null durumu ile bilgilendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-255">The type inferred for local variables declared with `var` is informed by the null state of the initializing expression.</span></span>

```csharp
var x = E;
```

<span data-ttu-id="cfccd-256">`E` türü null yapılabilir bir başvuru türü `C?` ve `E` null durumu "Not null" ise, `x` için gösterilen tür `C`olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-256">If the type of `E` is a nullable reference type `C?` and the null state of `E` is "not null" then the type inferred for `x` is `C`.</span></span> <span data-ttu-id="cfccd-257">Aksi takdirde, çıkartılan tür `E`türüdür.</span><span class="sxs-lookup"><span data-stu-id="cfccd-257">Otherwise, the inferred type is the type of `E`.</span></span>

<span data-ttu-id="cfccd-258">`x` için gösterilen türü null olabilir, `var`ek açıklama bağlamına göre, örneğin, bu konumda açıkça verilip verilmediği gibi yukarıda açıklanan şekilde belirlenir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-258">The nullability of the type inferred for `x` is determined as described above, based on the annotation context of the `var`, just as if the type had been given explicitly in that position.</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="cfccd-259">`var?` için tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="cfccd-259">Type inference for `var?`</span></span>

<span data-ttu-id="cfccd-260">`var?` ile belirtilen yerel değişkenler için gösterilen tür, başlatma ifadesinin null durumundan bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-260">The type inferred for local variables declared with `var?` is independent of the null state of the initializing expression.</span></span>

```csharp
var? x = E;
```

<span data-ttu-id="cfccd-261">`E` tür `T` null yapılabilir bir değer türü veya null yapılabilir bir başvuru türü ise, `x` için gösterilen tür `T`olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-261">If the type `T` of `E` is a nullable value type or a nullable reference type then the type inferred for `x` is `T`.</span></span> <span data-ttu-id="cfccd-262">Aksi takdirde, `T` null yapılamayan bir değer türü ise, gösterilen tür `S?``S`.</span><span class="sxs-lookup"><span data-stu-id="cfccd-262">Otherwise, if `T` is a nonnullable value type `S` the type inferred is `S?`.</span></span> <span data-ttu-id="cfccd-263">Aksi takdirde, `T` null yapılamayan bir başvuru türü ise, gösterilen tür `C?``C`.</span><span class="sxs-lookup"><span data-stu-id="cfccd-263">Otherwise, if `T` is a nonnullable reference type `C` the type inferred is `C?`.</span></span> <span data-ttu-id="cfccd-264">Aksi takdirde, bildirim geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-264">Otherwise, the declaration is illegal.</span></span>

<span data-ttu-id="cfccd-265">`x` için gösterilen türden null değer atanabilir değeri her zaman *null olabilir*.</span><span class="sxs-lookup"><span data-stu-id="cfccd-265">The nullability of the type inferred for `x` is always *nullable*.</span></span>

### <a name="generic-type-inference"></a><span data-ttu-id="cfccd-266">Genel tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="cfccd-266">Generic type inference</span></span>

<span data-ttu-id="cfccd-267">Genel tür çıkarımı, gösterilen başvuru türlerinin null yapılabilir olup olmadığına karar vermenize yardımcı olmak için geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-267">Generic type inference is enhanced to help decide whether inferred reference types should be nullable or not.</span></span> <span data-ttu-id="cfccd-268">Bu en iyi çabadır, ve kendi başına uyarı durumunda değildir, ancak seçili aşırı yüklemenin Çıkarsanan türleri bağımsız değişkenlere uygulandığında null yapılabilir uyarılara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-268">This is a best effort, and does not in and of itself yield warnings, but may lead to nullable warnings when the inferred types of the selected overload are applied to the arguments.</span></span>

<span data-ttu-id="cfccd-269">Tür çıkarımı, gelen türlerin ek açıklama bağlamına bağlı değildir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-269">The type inference does not rely on the annotation context of incoming types.</span></span> <span data-ttu-id="cfccd-270">Bunun yerine, açıkça ifade edilmiş olması durumunda kendi ek açıklama bağlamını elde eden bir `type` algılanır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-270">Instead a `type` is inferred which acquires its own annotation context from where it "would have been" if it had been expressed explicitly.</span></span> <span data-ttu-id="cfccd-271">Bu, kendiniz yazdıklarınız için bir kolaylık olarak tür çıkarımı rolü alt çizgi olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-271">This underscores the role of type inference as a convenience for what you could have written yourself.</span></span>

<span data-ttu-id="cfccd-272">Daha kesin olarak, bir çıkartılan tür bağımsız değişkeni için ek açıklama bağlamı, `<...>` türü parametre listesi tarafından izlenen belirtecin bağlamıdır; Yani, çağrılan genel yöntemin adı.</span><span class="sxs-lookup"><span data-stu-id="cfccd-272">More precisely, the annotation context for an inferred type argument is the context of the token that would have been followed by the `<...>` type parameter list, had there been one; i.e. the name of the generic method being called.</span></span> <span data-ttu-id="cfccd-273">Bu tür çağrılara çeviren sorgu ifadeleri için bağlam, çağrının oluşturulduğu sorgu yan tümcesinin ilk bağlamsal anahtar sözcüğünden alınır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-273">For query expressions that translate to such calls, the context is taken from the initial contextual keyword of the query clause from which the call is generated.</span></span>

### <a name="the-first-phase"></a><span data-ttu-id="cfccd-274">İlk aşama</span><span class="sxs-lookup"><span data-stu-id="cfccd-274">The first phase</span></span>

<span data-ttu-id="cfccd-275">Null yapılabilir başvuru türleri, aşağıda açıklandığı gibi ilk ifadelerden sınırlara akar.</span><span class="sxs-lookup"><span data-stu-id="cfccd-275">Nullable reference types flow into the bounds from the initial expressions, as described below.</span></span> <span data-ttu-id="cfccd-276">Ayrıca, `null` ve `default` iki yeni tür sınır ortaya çıkartılır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-276">In addition, two new kinds of bounds, namely `null` and `default` are introduced.</span></span> <span data-ttu-id="cfccd-277">Bunun amaçları, giriş ifadelerinde `null` veya `default` örnekleri aracılığıyla ilerleyesağlamaktır. Bu, aksi takdirde bile, ortaya çıkarılan bir türün null yapılabilir olmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-277">Their purpose is to carry through occurrences of `null` or `default` in the input expressions, which may cause an inferred type to be nullable, even when it otherwise wouldn't.</span></span> <span data-ttu-id="cfccd-278">Bu işlem, çıkarım işleminde "nullanlam" çekmek üzere geliştirilmiş null yapılabilir *değer* türleri için de çalışır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-278">This works even for nullable *value* types, which are enhanced to pick up "nullness" in the inference process.</span></span>

<span data-ttu-id="cfccd-279">İlk aşamada hangi sınırların ekleneceğini belirlemek şu şekilde geliştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="cfccd-279">The determination of what bounds to add in the first phase are enhanced as follows:</span></span>

<span data-ttu-id="cfccd-280">Bir bağımsız değişken `Ei` bir başvuru türü ise, çıkarım için kullanılan tür `U`, `Ei` null durumuna ve onun bildirildiği tür ' ne bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="cfccd-280">If an argument `Ei` has a reference type, the type `U` used for inference depends on the null state of `Ei` as well as its declared type:</span></span>
- <span data-ttu-id="cfccd-281">Belirtilen tür null atanamaz bir başvuru türü veya null atanabilir bir başvuru türü `U0` `U0?`</span><span class="sxs-lookup"><span data-stu-id="cfccd-281">If the declared type is a nonnullable reference type `U0` or a nullable reference type `U0?` then</span></span>
    - <span data-ttu-id="cfccd-282">`Ei` null durumu "null değil" ise `U` `U0`</span><span class="sxs-lookup"><span data-stu-id="cfccd-282">if the null state of `Ei` is "not null" then `U` is `U0`</span></span>
    - <span data-ttu-id="cfccd-283">`Ei` null durumu "Belki null" ise `U` `U0?`</span><span class="sxs-lookup"><span data-stu-id="cfccd-283">if the null state of `Ei` is "maybe null" then `U` is `U0?`</span></span>
- <span data-ttu-id="cfccd-284">Aksi takdirde `Ei` tanımlanmış bir tür varsa `U` bu tür</span><span class="sxs-lookup"><span data-stu-id="cfccd-284">Otherwise if `Ei` has a declared type, `U` is that type</span></span>
- <span data-ttu-id="cfccd-285">Aksi takdirde `Ei` `null`, `U` özel bir sınırdır `null`</span><span class="sxs-lookup"><span data-stu-id="cfccd-285">Otherwise if `Ei` is `null` then `U` is the special bound `null`</span></span>
- <span data-ttu-id="cfccd-286">Aksi takdirde `Ei` `default`, `U` özel bir sınırdır `default`</span><span class="sxs-lookup"><span data-stu-id="cfccd-286">Otherwise if `Ei` is `default` then `U` is the special bound `default`</span></span>
- <span data-ttu-id="cfccd-287">Aksi takdirde hiçbir çıkarım yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="cfccd-287">Otherwise no inference is made.</span></span>

### <a name="exact-upper-bound-and-lower-bound-inferences"></a><span data-ttu-id="cfccd-288">Tam, üst sınır ve alt sınır</span><span class="sxs-lookup"><span data-stu-id="cfccd-288">Exact, upper-bound and lower-bound inferences</span></span>

<span data-ttu-id="cfccd-289">Tür *`U` `V`tür olarak `V`* , null atanabilir bir başvuru türü `V0?`ise, aşağıdaki yan tümcelerde `V0` yerine `V` kullanılır. *from*</span><span class="sxs-lookup"><span data-stu-id="cfccd-289">In inferences *from* the type `U` *to* the type `V`, if `V` is a nullable reference type `V0?`, then `V0` is used instead of `V` in the following clauses.</span></span>
- <span data-ttu-id="cfccd-290">`V` sabit olmayan tür değişkenlerinden biri ise, `U` daha önce olduğu gibi tam, üst veya alt sınır olarak eklenir</span><span class="sxs-lookup"><span data-stu-id="cfccd-290">If `V` is one of the unfixed type variables, `U` is added as an exact, upper or lower bound as before</span></span>
- <span data-ttu-id="cfccd-291">Aksi takdirde, `U` `null` veya `default`ise çıkarımı yapılmaz</span><span class="sxs-lookup"><span data-stu-id="cfccd-291">Otherwise, if `U` is `null` or `default`, no inference is made</span></span>
- <span data-ttu-id="cfccd-292">Aksi takdirde, `U` null yapılabilir bir başvuru türü `U0?`, sonraki yan tümcelerde `U` yerine `U0` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-292">Otherwise, if `U` is a nullable reference type `U0?`, then `U0` is used instead of `U` in the subsequent clauses.</span></span>

<span data-ttu-id="cfccd-293">Temelde, sabit olmayan tür değişkenlerinden biri ile ilgili olan null değer alabilme, kendi sınırlarına göre korunur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-293">The essence is that nullability that pertains directly to one of the unfixed type variables is preserved into its bounds.</span></span> <span data-ttu-id="cfccd-294">Kaynak ve hedef türleri için ek olarak, diğer yandan null değer alabilme yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-294">For the inferences that recurse further into the source and target types, on the other hand, nullability is ignored.</span></span> <span data-ttu-id="cfccd-295">Bu, eşleşmeyebilir veya eşleşmeyebilir, ancak daha sonra aşırı yükleme seçilir ve uygulanırsa bir uyarı gönderilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-295">It may or may not match, but if it doesn't, a warning will be issued later if the overload is chosen and applied.</span></span>

### <a name="fixing"></a><span data-ttu-id="cfccd-296">Düzelttikten</span><span class="sxs-lookup"><span data-stu-id="cfccd-296">Fixing</span></span>

<span data-ttu-id="cfccd-297">Spec Şu anda birden çok sınır birbirlerine dönüştürülebilir ancak farklılık belirten ne olduğunu açıklayan iyi bir iş gerçekleştirmez.</span><span class="sxs-lookup"><span data-stu-id="cfccd-297">The spec currently does not do a good job of describing what happens when multiple bounds are identity convertible to each other, but are different.</span></span> <span data-ttu-id="cfccd-298">Bu, `object` ve `dynamic`arasında, yalnızca öğe adlarında farklı olan türler arasında, oluşturulan türler arasında ve artık başvuru türleri için `C` ve `C?` arasında olabilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-298">This may happen between `object` and `dynamic`, between tuple types that differ only in element names, between types constructed thereof and now also between `C` and `C?` for reference types.</span></span>

<span data-ttu-id="cfccd-299">Ayrıca, giriş ifadelerinden sonuç türüne "nulltik" öğesini yaymaya ihtiyacımız vardır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-299">In addition we need to propagate "nullness" from the input expressions to the result type.</span></span> 

<span data-ttu-id="cfccd-300">Bunları işlemek için şu anda, düzeltmek üzere daha fazla aşama ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="cfccd-300">To handle these we add more phases to fixing, which is now:</span></span>

1. <span data-ttu-id="cfccd-301">Tüm limitlerde tüm türlere aday olarak toplayın, her türlü `?` null yapılabilir başvuru türünden kaldırır</span><span class="sxs-lookup"><span data-stu-id="cfccd-301">Gather all the types in all the bounds as candidates, removing `?` from all that are nullable reference types</span></span>
2. <span data-ttu-id="cfccd-302">Tam, alt ve üst sınırların gereksinimlerine göre adayları ortadan kaldırın (`null` ve `default` sınırlarını koruyarak)</span><span class="sxs-lookup"><span data-stu-id="cfccd-302">Eliminate candidates based on requirements of exact, lower and upper bounds (keeping `null` and `default` bounds)</span></span>
3. <span data-ttu-id="cfccd-303">Tüm diğer adaylar için örtük dönüştürmesi olmayan adayları ortadan kaldırın</span><span class="sxs-lookup"><span data-stu-id="cfccd-303">Eliminate candidates that do not have an implicit conversion to all the other candidates</span></span>
4. <span data-ttu-id="cfccd-304">Kalan adayların hepsi bir diğeri için kimlik dönüştürmeleri yoksa, tür çıkarımı başarısız olur</span><span class="sxs-lookup"><span data-stu-id="cfccd-304">If the remaining candidates do not all have identity conversions to one another, then type inference fails</span></span>
5. <span data-ttu-id="cfccd-305">Kalan adayları aşağıda açıklandığı gibi *birleştirin*</span><span class="sxs-lookup"><span data-stu-id="cfccd-305">*Merge* the remaining candidates as described below</span></span>
6. <span data-ttu-id="cfccd-306">Elde edilen aday bir başvuru türü veya null yapılamayan bir değer türü ise ve tüm tam *sınırlar ya da* alt sınırların *Tümü* null yapılabilir değer türleri, null yapılabilir başvuru türleri, `null` veya `default`, bu durumda `?` elde edilen aday öğesine eklenir ve bu, null olabilir bir değer türü veya başvuru türü yapar.</span><span class="sxs-lookup"><span data-stu-id="cfccd-306">If the resulting candidate is a reference type or a nonnullable value type and *all* of the exact bounds or *any* of the lower bounds are nullable value types, nullable reference types, `null` or `default`, then `?` is added to the resulting candidate, making it a nullable value type or reference type.</span></span>

<span data-ttu-id="cfccd-307">*Birleştirme* iki aday türü arasında tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="cfccd-307">*Merging* is described between two candidate types.</span></span> <span data-ttu-id="cfccd-308">Bu, bir geçişli ve sorun olduğundan, adaylar aynı nihai sonuçla herhangi bir sırada birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="cfccd-308">It is transitive and commutative, so the candidates can be merged in any order with the same ultimate result.</span></span> <span data-ttu-id="cfccd-309">İki aday türü birbirlerine dönüştürülebilir değilse, bu tanımsız olur.</span><span class="sxs-lookup"><span data-stu-id="cfccd-309">It is undefined if the two candidate types are not identity convertible to each other.</span></span>

<span data-ttu-id="cfccd-310">*Merge* işlevi iki aday türü ve bir yön ( *+* veya *-* ) alır:</span><span class="sxs-lookup"><span data-stu-id="cfccd-310">The *Merge* function takes two candidate types and a direction (*+* or *-*):</span></span>

- <span data-ttu-id="cfccd-311">*Birleştir*(`T`, `T`, *d*) = t</span><span class="sxs-lookup"><span data-stu-id="cfccd-311">*Merge*(`T`, `T`, *d*) = T</span></span>
- <span data-ttu-id="cfccd-312">*Birleştirme*(`S`, `T?`, *+* ) = *birleştirme*(`S?`, `T`, *+* ) = *birleştirme*(`S`, `T`, *+* )`?`</span><span class="sxs-lookup"><span data-stu-id="cfccd-312">*Merge*(`S`, `T?`, *+*) = *Merge*(`S?`, `T`, *+*) = *Merge*(`S`, `T`, *+*)`?`</span></span>
- <span data-ttu-id="cfccd-313">*Birleştirme*(`S`, `T?`, *-* ) = *birleştirme*(`S?`, `T`, *-* ) = *birleştirme*(`S`, `T`, *-* )</span><span class="sxs-lookup"><span data-stu-id="cfccd-313">*Merge*(`S`, `T?`, *-*) = *Merge*(`S?`, `T`, *-*) = *Merge*(`S`, `T`, *-*)</span></span>
- <span data-ttu-id="cfccd-314">*Birleştirme*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*birleştirme*(`S1`, `T1`, *d1*)`,...,`*birleştirme*(`Sn`, `Tn`, *DN*)`>`*where*</span><span class="sxs-lookup"><span data-stu-id="cfccd-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="cfccd-315">`C<...>` `i`' th türü parametresi covaryant ise `di` =  *+*</span><span class="sxs-lookup"><span data-stu-id="cfccd-315">`di` = *+* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="cfccd-316">`C<...>` `i`' th türü parametresi Contra veya sabit ise `di` =  *-*</span><span class="sxs-lookup"><span data-stu-id="cfccd-316">`di` = *-* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="cfccd-317">*Birleştirme*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*birleştirme*(`S1`, `T1`, *d1*)`,...,`*birleştirme*(`Sn`, `Tn`, *DN*)`>`*where*</span><span class="sxs-lookup"><span data-stu-id="cfccd-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="cfccd-318">`C<...>` `i`' th türü parametresi covaryant ise `di` =  *-*</span><span class="sxs-lookup"><span data-stu-id="cfccd-318">`di` = *-* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="cfccd-319">`C<...>` `i`' th türü parametresi Contra veya sabit ise `di` =  *+*</span><span class="sxs-lookup"><span data-stu-id="cfccd-319">`di` = *+* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="cfccd-320">*Birleştirme*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*birleştirme*(`S1`, `T1`, *d*)`n1,...,`*birleştirme*(`Sn`, `Tn`, *d*) `nn)`*where*</span><span class="sxs-lookup"><span data-stu-id="cfccd-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *where*</span></span>
    - <span data-ttu-id="cfccd-321">`si` ve `ti` farklıysa veya her ikisi de yoksa `ni` yok olur</span><span class="sxs-lookup"><span data-stu-id="cfccd-321">`ni` is absent if `si` and `ti` differ, or if both are absent</span></span>
    - <span data-ttu-id="cfccd-322">`si` ve `ti` aynı ise `ni` `si`</span><span class="sxs-lookup"><span data-stu-id="cfccd-322">`ni` is `si` if `si` and `ti` are the same</span></span>
- <span data-ttu-id="cfccd-323">*Birleştirme*(`object`, `dynamic`) = *birleştirme*(`dynamic`, `object`) = `dynamic`</span><span class="sxs-lookup"><span data-stu-id="cfccd-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span></span>

## <a name="warnings"></a><span data-ttu-id="cfccd-324">Uyarılar</span><span class="sxs-lookup"><span data-stu-id="cfccd-324">Warnings</span></span>

### <a name="potential-null-assignment"></a><span data-ttu-id="cfccd-325">Olası null atama</span><span class="sxs-lookup"><span data-stu-id="cfccd-325">Potential null assignment</span></span>

### <a name="potential-null-dereference"></a><span data-ttu-id="cfccd-326">Olası null başvurusu</span><span class="sxs-lookup"><span data-stu-id="cfccd-326">Potential null dereference</span></span>

### <a name="constraint-nullability-mismatch"></a><span data-ttu-id="cfccd-327">Kısıtlama null olabilme uyumsuzluğu</span><span class="sxs-lookup"><span data-stu-id="cfccd-327">Constraint nullability mismatch</span></span>

### <a name="nullable-types-in-disabled-annotation-context"></a><span data-ttu-id="cfccd-328">Devre dışı ek açıklama bağlamındaki null yapılabilir türler</span><span class="sxs-lookup"><span data-stu-id="cfccd-328">Nullable types in disabled annotation context</span></span>

## <a name="attributes-for-special-null-behavior"></a><span data-ttu-id="cfccd-329">Özel null davranışı için öznitelikler</span><span class="sxs-lookup"><span data-stu-id="cfccd-329">Attributes for special null behavior</span></span>


