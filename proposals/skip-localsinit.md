---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484491"
---
# <a name="suppress-emitting-of-localsinit-flag"></a><span data-ttu-id="03531-101">`localsinit` bayrağını yaymayı gizleyin.</span><span class="sxs-lookup"><span data-stu-id="03531-101">Suppress emitting of `localsinit` flag.</span></span>

* <span data-ttu-id="03531-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="03531-102">[x] Proposed</span></span>
* <span data-ttu-id="03531-103">[] Prototipi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="03531-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="03531-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="03531-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="03531-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="03531-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="03531-106">Özet</span><span class="sxs-lookup"><span data-stu-id="03531-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="03531-107">`localsinit` bayrağının `SkipLocalsInitAttribute` özniteliği aracılığıyla bastırçıkmasına izin ver.</span><span class="sxs-lookup"><span data-stu-id="03531-107">Allow suppressing emit of `localsinit` flag via `SkipLocalsInitAttribute` attribute.</span></span> 

## <a name="motivation"></a><span data-ttu-id="03531-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="03531-108">Motivation</span></span>
[motivation]: #motivation


### <a name="background"></a><span data-ttu-id="03531-109">Arka Plan</span><span class="sxs-lookup"><span data-stu-id="03531-109">Background</span></span>
<span data-ttu-id="03531-110">CLR spec başına, başvuruları içermeyen yerel değişkenler VM/JıT tarafından belirli bir değere başlatılmaz.</span><span class="sxs-lookup"><span data-stu-id="03531-110">Per CLR spec local variables that do not contain references are not initialized to a particular value by the VM/JIT.</span></span> <span data-ttu-id="03531-111">Başlatma olmadan bu tür değişkenlerden okuma tür açısından güvenlidir, aksi halde davranış tanımsız ve uygulamaya özgüdür.</span><span class="sxs-lookup"><span data-stu-id="03531-111">Reading from such variables without initialization is type-safe, but otherwise the behavior is undefined and implementation specific.</span></span> <span data-ttu-id="03531-112">Genellikle başlatılmamış Yereller, artık yığın çerçevesinin kapladığı bellekte kalan değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="03531-112">Typically uninitialized locals contain whatever values were left in the memory that is now occupied by the stack frame.</span></span> <span data-ttu-id="03531-113">Bu, belirleyici olmayan davranışa ve hataları yeniden oluşturmaya neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="03531-113">That could lead to nondeterministic behavior and hard to reproduce bugs.</span></span> 

<span data-ttu-id="03531-114">Yerel bir değişken "atama" için iki yol vardır:</span><span class="sxs-lookup"><span data-stu-id="03531-114">There are two ways to "assign" a local variable:</span></span> 
- <span data-ttu-id="03531-115">bir değeri depolayarak veya</span><span class="sxs-lookup"><span data-stu-id="03531-115">by storing a value or</span></span> 
- <span data-ttu-id="03531-116">Yerel bellek havuzu için ayrılan her şeyin sıfır olarak başlatılmış olmasını zorlayan `localsinit` bayrağını belirterek: Bu, hem yerel değişkenleri hem de `stackalloc` verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="03531-116">by specifying `localsinit` flag which forces everything that is allocated form the local memory pool to be zero-initialized NOTE: this includes both local variables and `stackalloc` data.</span></span>    

<span data-ttu-id="03531-117">Başlatılmamış verilerin kullanılması önerilmez ve doğrulanabilir kodda buna izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="03531-117">Use of uninitialized data is discouraged and is not allowed in verifiable code.</span></span> <span data-ttu-id="03531-118">Akış Analizi ile ilgili olduğunu kanıtlamak mümkün olsa da, doğrulama algoritmasının koruyucu için izin verilir ve yalnızca `localsinit` ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="03531-118">While it might be possible to prove that by the means of flow analysis, it is permitted for the verification algorithm to be conservative and simply require that `localsinit` is set.</span></span>

<span data-ttu-id="03531-119">Tarihsel C# derleyici, yerelleri bildiren tüm yöntemlerde `localsinit` bayrağını yayar.</span><span class="sxs-lookup"><span data-stu-id="03531-119">Historically C# compiler emits `localsinit` flag on all methods that declare locals.</span></span>

<span data-ttu-id="03531-120">, C# CLR spec 'in gerektirenden daha katı olan kesin atama analizini kullanır (C# yerellerin kapsamını da dikkate almanız gerekir), sonuçta ortaya çıkan kodun tek bir biçimde doğrulanabilir olması kesin bir şekilde garanti edilmez:</span><span class="sxs-lookup"><span data-stu-id="03531-120">While C# employs definite-assignment analysis which is more strict than what CLR spec would require (C# also needs to consider scoping of locals), it is not strictly guaranteed that the resulting code would be formally verifiable:</span></span>
- <span data-ttu-id="03531-121">CLR ve C# kurallar, yerel bir as `out` bağımsız değişkeninin bir `use`geçirilmesi durumunda olup olmadığını kabul etmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="03531-121">CLR and C# rules may not agree on whether passing a local as `out` argument is a `use`.</span></span>
- <span data-ttu-id="03531-122">Koşullar bilindiğinde CLR ve C# kuralları koşullu dallar sırasında kabul etmeyebilir (Sabit yayma).</span><span class="sxs-lookup"><span data-stu-id="03531-122">CLR and C# rules may not agree on treatment of conditional branches when conditions are known (constant propagation).</span></span>
- <span data-ttu-id="03531-123">Buna izin verildiği için CLR `localinits`de yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="03531-123">CLR could as well simply require `localinits`, since that is permitted.</span></span>  

### <a name="problem"></a><span data-ttu-id="03531-124">Sorun</span><span class="sxs-lookup"><span data-stu-id="03531-124">Problem</span></span>
<span data-ttu-id="03531-125">Yüksek performanslı uygulamada zorlanan sıfır başlatma maliyeti fark edilebilir.</span><span class="sxs-lookup"><span data-stu-id="03531-125">In high-performance application the cost of forced zero-initialization could be noticeable.</span></span> <span data-ttu-id="03531-126">`stackalloc` kullanıldığında özellikle dikkat çekici hale gelir.</span><span class="sxs-lookup"><span data-stu-id="03531-126">It is particularly noticeable when `stackalloc` is used.</span></span>

<span data-ttu-id="03531-127">Bazı durumlarda JıT, sonraki atamalar tarafından bu başlatma "sonlandırıldı" olduğunda tek tek Yereller için ilk sıfır başlatma işlemi yapabilir.</span><span class="sxs-lookup"><span data-stu-id="03531-127">In some cases JIT can elide initial zero-initialization of individual locals when such initialization is "killed" by subsequent assignments.</span></span> <span data-ttu-id="03531-128">Tüm JITs bunu yapmayın ve bu iyileştirmenin sınırları vardır.</span><span class="sxs-lookup"><span data-stu-id="03531-128">Not all JITs do this and such optimization has limits.</span></span> <span data-ttu-id="03531-129">`stackalloc`yardımcı değildir.</span><span class="sxs-lookup"><span data-stu-id="03531-129">It does not help with `stackalloc`.</span></span>

<span data-ttu-id="03531-130">Sorunun gerçek olduğunu anlamak için, `IL` yerel öğeler içermeyen bir yöntemin `localsinit` bayrağına sahip olmaması durumunda bilinen bir hata vardır.</span><span class="sxs-lookup"><span data-stu-id="03531-130">To illustrate that the problem is real - there is a known bug where a method not containing any `IL` locals would not have `localsinit` flag.</span></span> <span data-ttu-id="03531-131">Hata, başlatma maliyetlerinin önüne geçmek için, bu tür yöntemlere `stackalloc` yerleştirilerek, kullanıcılar tarafından zaten kullanım dışı bırakılmış.</span><span class="sxs-lookup"><span data-stu-id="03531-131">The bug is already being exploited by users by putting `stackalloc` into such methods - intentionally to avoid initialization costs.</span></span> <span data-ttu-id="03531-132">Bunun nedeni, yerelin `IL` olmaması kararlı bir ölçüdür ve CodeGen stratejisindeki değişikliklere göre farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="03531-132">That is despite the fact that absence of `IL` locals is an unstable metric and may vary depending on changes in codegen strategy.</span></span> <span data-ttu-id="03531-133">Hatanın düzeltilmesi ve kullanıcıların, bayrağı gizlemeyi daha fazla belgelenmiş ve güvenilir bir şekilde alması gerekir.</span><span class="sxs-lookup"><span data-stu-id="03531-133">The bug should be fixed and users should get a more documented and reliable way of suppressing the flag.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="03531-134">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="03531-134">Detailed design</span></span>

<span data-ttu-id="03531-135">Derleyicinin `localsinit` bayrağını yaymadığından `System.Runtime.CompilerServices.SkipLocalsInitAttribute` belirtilmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="03531-135">Allow specifying `System.Runtime.CompilerServices.SkipLocalsInitAttribute` as a way to tell the compiler to not emit `localsinit` flag.</span></span>
 
<span data-ttu-id="03531-136">Bunun nihai sonucu, Yereller, büyük olasılıkla bir test eden JıT tarafından sıfır olarak başlatılmamış olabilir C#.</span><span class="sxs-lookup"><span data-stu-id="03531-136">The end result of this will be that the locals may not be zero-initialized by the JIT, which is in most cases unobservable in C#.</span></span>  
<span data-ttu-id="03531-137">Bu `stackalloc` verilerin yanı sıra sıfır olarak başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="03531-137">In addition to that `stackalloc` data will not be zero-initialized.</span></span> <span data-ttu-id="03531-138">Bu kesinlikle Observable ' dır, ancak aynı zamanda en çok mücadele senaryosu da vardır.</span><span class="sxs-lookup"><span data-stu-id="03531-138">That is definitely observable, but also is the most motivating scenario.</span></span>

<span data-ttu-id="03531-139">İzin verilen ve tanınan öznitelik hedefleri şunlardır: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span><span class="sxs-lookup"><span data-stu-id="03531-139">Permitted and recognized attribute targets are: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span></span> <span data-ttu-id="03531-140">Ancak derleyici, öznitelik listelenen hedeflerle tanımlanmalıdır veya özniteliğin hangi derlemede tanımlandığından emin olur.</span><span class="sxs-lookup"><span data-stu-id="03531-140">However compiler will not require that attribute is defined with the listed targets nor it will care in which assembly the attribute is defined.</span></span> 

<span data-ttu-id="03531-141">Öznitelik bir kapsayıcıda belirtildiğinde (`class`, `module`, iç içe geçmiş bir yöntem için bir yöntem,...), bayrak kapsayıcı içinde bulunan tüm yöntemleri etkiler.</span><span class="sxs-lookup"><span data-stu-id="03531-141">When attribute is specified on a container (`class`, `module`, containing method for a nested method, ...), the flag affects all methods contained within the container.</span></span>

<span data-ttu-id="03531-142">Birleştirme yöntemleri, mantıksal kapsayıcıdan/sahibe ait bayrağı "devralma".</span><span class="sxs-lookup"><span data-stu-id="03531-142">Synthesized methods "inherit" the flag from the logical container/owner.</span></span> 

<span data-ttu-id="03531-143">Bayrak, gerçek yöntem gövdeleri için yalnızca CodeGen stratejisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="03531-143">The flag affects only codegen strategy for actual method bodies.</span></span> <span data-ttu-id="03531-144">Yani.</span><span class="sxs-lookup"><span data-stu-id="03531-144">I.E.</span></span> <span data-ttu-id="03531-145">bayrak soyut yöntemleri etkilemez ve geçersiz kılma/uygulama yöntemlerine yayılmaz.</span><span class="sxs-lookup"><span data-stu-id="03531-145">the flag has no effect on abstract methods and is not propagated to overriding/implementing methods.</span></span>

<span data-ttu-id="03531-146">Bu, **_dil özelliği değil_** , açıkça bir **_derleyici özelliğidir_** .</span><span class="sxs-lookup"><span data-stu-id="03531-146">This is explicitly a **_compiler feature_** and **_not a language feature_**.</span></span>  
<span data-ttu-id="03531-147">Derleyici komut satırı anahtarlarına benzer şekilde, özellik belirli bir codegen stratejisinin uygulama ayrıntılarını denetler ve C# belirtim için gerekli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="03531-147">Similarly to compiler command line switches the feature controls implementation details of a particular codegen strategy and does not need to be required by the C# spec.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="03531-148">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="03531-148">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="03531-149">Eski/diğer derleyiciler özniteliği karşılamayabilir.</span><span class="sxs-lookup"><span data-stu-id="03531-149">Old/other compilers may not honor the attribute.</span></span>
<span data-ttu-id="03531-150">Özniteliğin uyumlu davranış yok sayılıyor.</span><span class="sxs-lookup"><span data-stu-id="03531-150">Ignoring the attribute is compatible behavior.</span></span> <span data-ttu-id="03531-151">Yalnızca hafif bir performans isabetini elde edebilir.</span><span class="sxs-lookup"><span data-stu-id="03531-151">Only may result in a slight perf hit.</span></span>

- <span data-ttu-id="03531-152">`localinits` bayrağı olmayan kod doğrulama başarısızlıklarını tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="03531-152">The code without `localinits` flag may trigger verification failures.</span></span>
<span data-ttu-id="03531-153">Bu özelliği istemek için gereken kullanıcılar genellikle doğruıyabilme ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="03531-153">Users that ask for this feature are generally unconcerned with verifiability.</span></span> 
 
- <span data-ttu-id="03531-154">Özniteliği bir bağımsız yöntemden daha yüksek düzeylerde uygulamak, `stackalloc` kullanıldığında observable olan yerel olmayan etkiye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="03531-154">Applying the attribute at higher levels than an individual method has nonlocal effect, which is observable when `stackalloc` is used.</span></span> <span data-ttu-id="03531-155">Henüz bu, en çok istenen senaryodur.</span><span class="sxs-lookup"><span data-stu-id="03531-155">Yet, this is the most requested scenario.</span></span>

## <a name="alternatives"></a><span data-ttu-id="03531-156">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="03531-156">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="03531-157">Yöntem `unsafe` bağlamda bildirildiğinde `localinits` bayrağını atlayın.</span><span class="sxs-lookup"><span data-stu-id="03531-157">omit `localinits` flag when method is declared in `unsafe` context.</span></span> <span data-ttu-id="03531-158">Bu durum, `stackalloc`olması durumunda sessiz ve tehlikeli davranışın belirleyici olan belirleyici olmayan şekilde değişmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="03531-158">That could cause silent and dangerous behavior change from deterministic to nondeterministic in a case of `stackalloc`.</span></span>

- <span data-ttu-id="03531-159">`localinits` bayrağını her zaman atlayın.</span><span class="sxs-lookup"><span data-stu-id="03531-159">omit `localinits` flag always.</span></span>
<span data-ttu-id="03531-160">Yukarıdakilerden daha da kötüsü.</span><span class="sxs-lookup"><span data-stu-id="03531-160">Even worse than above.</span></span>

- <span data-ttu-id="03531-161">Yöntem gövdesinde `stackalloc` kullanılmadığı müddetçe `localinits` bayrağını atlayın.</span><span class="sxs-lookup"><span data-stu-id="03531-161">omit `localinits` flag unless `stackalloc` is used in the method body.</span></span>
<span data-ttu-id="03531-162">En çok istenen senaryoyu ele almaz ve bu geri dönüş seçeneği olmadan kodu doğrulanamaz olarak döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="03531-162">Does not address the most requested scenario and may turn code unverifiable with no option to revert that back.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="03531-163">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="03531-163">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="03531-164">Öznitelik gerçekten meta verilere yayınlansın mı?</span><span class="sxs-lookup"><span data-stu-id="03531-164">Should the attribute be actually emitted to metadata?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="03531-165">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="03531-165">Design meetings</span></span>

<span data-ttu-id="03531-166">Henüz yok.</span><span class="sxs-lookup"><span data-stu-id="03531-166">None yet.</span></span> 