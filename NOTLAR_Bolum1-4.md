# YMT2103 — Biçimsel Diller ve Otomata Teorisi
## Bölüm 1–4 Çalışma Notları

Kaynak: AÖF ders notları (Prof. Dr. Ünal Yarımağan), `LRN/` klasöründeki HTML sayfalar.
Parantez içindeki kodlar (örn. `BD01020`) orijinal sayfa dosyasıdır; konuyu animasyonlu/çizimli
haliyle görmek istersen o dosyayı tarayıcıda aç.

**Simge sözleşmeleri (bu kitapta):**

| Simge | Anlam |
|---|---|
| `Σ` | giriş alfabesi |
| `Δ` | çıkış alfabesi |
| `Q` | durumlar kümesi, `q0` başlangıç durumu, `F` uç (kabul) durumları |
| `δ` | durum geçiş işlevi |
| `λ` | **iki ayrı yerde kullanılır:** (1) boş simge / λ-geçişi, (2) Moore–Mealy çıkış işlevi |
| `V_N`, `V_T` | değişkenler (nonterminal), uç simgeler (terminal); `V = V_N ∪ V_T` |
| `P` | yeniden yazma kuralları kümesi, `S` başlangıç değişkeni |
| `⇒`, `⇒*` | tek adımda türetme / sıfır veya daha çok adımda türetme |
| `w`, `α`, `β` | dizgiler; `\|w\|` dizgi uzunluğu |

> Not: Orijinal sitede birçok tanım/teorem/algoritma ve **tüm çözümlü örnekler Flash animasyonu**
> olarak verilmiş (örn. `1.4`, `2.3`, `3.5`, `4.5` örnek bölümleri). Bu notlarda metin ve formül
> görüntülerinden okunabilen kısım birebir aktarılmıştır; animasyon içi adımlar için siteyi açman gerekir.

---

# BÖLÜM 1 — SONLU ÖZDEVİNİRLER (FA)

## 1.1 Sonlu Özdevinir (FA) Modeli (`BD01015`)

- Sonlu özdevinir (finite automata, **FA**): kesikli giriş ve çıkışları olan matematiksel bir model.
- Kullanım alanları: metin düzenleyiciler, derleyicilerin belirli kesimleri, zamanuyumlu dizsel
  devreler (synchronous sequential circuits); hatta sayısal bilgisayarlar bile bu modele uygun görülür.
- İki ana sınıf:
  1. **Sonlu durumlu tanıyıcı** (finite state recognizer) — girişi kabul/red eder,
  2. **Çıkış üreten özdevinir** (dönüştürücü) — giriş dizgisine çıkış dizgisi üretir.
- Çıplak "FA" ya da "sonlu özdevinir" denince temel model olan **deterministik sonlu durumlu
  tanıyıcı (DFA)** anlaşılır.

## 1.1.1 Deterministik Sonlu Özdevinir (DFA) (`BD01020`)

DFA bir **beşli** olarak tanımlanır: `M = <Q, Σ, δ, q0, F>`

Geçiş işlevi:

```
δ(qi, a) = qj        ∀ qi ∈ Q, a ∈ Σ  ⇒  qj ∈ Q
```

yani `δ : (Q × Σ) → Q`. Her (şimdiki durum, giriş simgesi) çiftine **bir ve yalnız bir** sonraki
durum eşlenir → model deterministiktir. Makine `q0`'dan başlar; her an bulunduğu durum kesindir.

### 1.1.1.1 Geçiş çizeneği (transition diagram) (`BD01025`)

- Yönlü çizge: her durum bir **düğüm**, her geçiş **yönlü yay**, yay üzerinde geçişi tetikleyen
  giriş simgesi.
- **Başlangıç durumu** gelen ok işaretiyle, **uç durumlar çift çember** ile gösterilir.
- δ'nın matematiksel tanımı uzun ve okunaksız olduğu için pratikte çizeneği kullanılır.

### 1.1.1.2 DFA'nın tanıdığı dizgiler kümesi (`BD01030`)

- `δ(q0, w) = qi` : `q0`'dan başlayıp `w` dizgisi işlendiğinde ulaşılan durum `qi`.
- `qi ∈ F` ise `w` **tanınır**.
- Tanınan dizgiler kümesi:

```
T(M) = { w | δ(q0, w) = qi ∈ F }
```

- `Σ` üzerindeki tüm dizgiler kümesi sonsuzdur; DFA bunu "tanınan / tanınmayan" diye ikiye böler.
- **Örnek (M1.1):** `0101` tanınmaz (q1'de biter, uç değil); `0110` tanınır (q2 uç durum).
  `T(M1.1)` = {0,1} alfabesinde **içinde `11` altdizgisi bulunan** dizgiler kümesi
  = {11, 011, 110, 0110, 01011, …}.

### 1.1.1.3 Şeritli makine modeli (`BD01035`)

Soyut DFA'yı somutlaştıran model, 3 eleman:

1. **Mıknatıslı şerit:** hücrelerden oluşur, her hücrede bir giriş simgesi; yalnız okunur, sağ ucu sonsuz.
2. **Sonlu denetim birimi (SDB):** sonlu sayıda durumu vardır, başlangıçta başlangıç durumundadır.
3. **Okuma kafası:** soldan sağa **tek yönlü** okur; her an bir hücre üzerindedir.

Çalışma döngüsü (her harekette): simge oku → kafa bir sağ hücreye geç → SDB sonraki duruma geç.
Hareketler bitince SDB `qi` durumunda ve `qi ∈ F` ise, kafanın solunda kalan `w` dizgisi tanınır.
("Makine" adı tamamen simgeseldir; modelin gerçek makineyle benzerliği olmak zorunda değildir.)

## 1.1.2 Deterministik Olmayan Sonlu Özdevinir (NFA) (`BD01040`)

- Sözlü tanımlanan bir kümeyi tanıyan **DFA çizeneğini kurmak zor** olabilir ve sözlü tanımdan
  DFA'ya giden sistematik bir yol yoktur.
  **Örnek 1.2:** {a,b,c} alfabesinde içinde `abc` ya da `bac` geçen dizgiler → DFA çizeneği karmaşık.
- NFA da bir beşlidir: `NFA = <Q, Σ, δ, q0, F>`; **tek fark geçiş işlevindedir**:
  - DFA: `δ : (Q × Σ) → Q`
  - NFA: `δ : (Q × Σ) → Q'nun altkümeleri`
- Yani bir durumdan bir simgeyle **sıfır, bir ya da birçok** duruma geçilebilir.

**Örnek 1.3 (`BD01045`)** — `M1.3 = <Q, Σ, δ, q0, F>`, `Q={q0,q1,q2,q3}`, `Σ={0,1}`, `F={q3}`:

| δ | 0 | 1 |
|---|---|---|
| q0 | {q0, q1} | {q0, q2} |
| q1 | {q3} | {} = Φ |
| q2 | Φ | {q3} |
| q3 | {q3} | {q3} |

- `w = 000` verilirse son durum belirsizdir: q0, q1 ya da q3 olabilir.
- **Tanıma kuralı:** giriş dizgisi için başlangıç durumundan bir **uç duruma ulaşan bir yol** varsa
  dizgi tanınır. `w = 10001` tanınır (q0→…→q3 yolu var); `w = 010` tanınmaz.
- `T(M1.3)` = {0,1} alfabesinde içinde `00` ya da `11` geçen dizgiler.
- NFA çizenekleri kurması ve okuması DFA'ya göre çok daha kolaydır.

## 1.1.3 λ (Lambda) Geçişi (`BD01050`)

- **λ = boş simge.** λ-geçişi: **hiçbir giriş simgesi işlenmeden** gerçekleşen durum geçişi.
- `q1 →(λ)→ q2` varsa: q1'deki makine kendiliğinden q2'ye geçebilir; işlevsel olarak
  - q1 başlangıç durumu ise **q2 de** başlangıç durumudur,
  - q2 uç durum ise **q1 de** uç durumdur.
- λ-geçişi de **tek yönlüdür** (tersi geçerli değildir). Çizenek kurmayı/okumayı kolaylaştırır.
- **Örnek 1.4 (`BD01052`):** {0,1,2} alfabesinde λ'lı ve λ'sız çizenek karşılaştırması — λ esneklik sağlar.

### 1.1.3.2 λ-geçişsiz eşdeğer çizeneğin bulunması (`BD01055`, `BD01057`)

- λ-geçişleri modeli **güçlendirmez**: her λ'lı çizeneğe denk λ'sız çizeneğe vardır
  (denklik = tanıdıkları kümenin eşitliği).
- Yok etme kuralı: `q1 →(λ)→ q2` varsa, q2'nin çıkış geçişleri ve uç/başlangıç nitelikleri q1'e
  kopyalanır, sonra λ yayı silinir.
- **Zincir halinde λ'ler varsa sondan başa (geriye doğru) yok et.**
- **Örnek 1.5:** `T(M1.5)` = {cc, aacc, bbcc, bbbbcc, …} = "aa ile ya da çift sayıda b ile başlayıp
  cc ile biten dizgiler"; λ'ler adım adım yok edilerek λ'sız çizeneğe geçilir.

## 1.1.4 DFA – NFA Denkliği (`BD01060`, `BD01062`)

- Her DFA zaten bir NFA'dır (δ'nın özel hali). Soru: her NFA için denk DFA var mı? **Evet** → modeller denktir.
- Yöntem (**altküme yapımı / ardıl çizelgesi**):
  - **Geçiş çizelgesi:** her durumun her simge için ardıllarını gösteren tablo.
  - Bir `w` dizgisinin tanınması için: başlangıç durumunun `w`-ardılı kümesinin içinde bir uç durum olmalı.
  - Başlangıç durumunun **tüm ardıl kümeleri** yeni DFA'nın durumları olur.
- **Örnek 1.6:** `w = 011` → A'nın ardılı `AC`, uç durum içerir → tanınır; `w = 110` → ardıl `AB`,
  uç yok → tanınmaz. Elde edilen denk DFA:

```
M_DL1.6 = <Q, Σ, δ, q0, F>
Q  = {A, BC, AB, AC, ABC}      δ(A,0)=A     δ(A,1)=BC
Σ  = {0,1}                     δ(BC,0)=AB   δ(BC,1)=AC
q0 = A                         δ(AB,0)=AB   δ(AB,1)=ABC
F  = {BC, AC, ABC}             δ(AC,0)=AB   δ(AC,1)=BC
                               δ(ABC,0)=AB  δ(ABC,1)=ABC
```

- Bulunduktan sonra durumlar yeniden adlandırılabilir (q0, q1, …).

## 1.1.5 İki Yönlü Sonlu Özdevinir (2DFA) (`BD01065`)

- DFA'da kafa yalnız sağa gider; **2DFA'da kafa sağa ve sola** gidebilir.
- Beşli tanım aynı; tek fark geçiş işlevi:

```
δ : (Q × Σ) → Q × {R, L}
```

(R = right, L = left: kafa bir sağa mı bir sola mı geçecek.)

- **Örnek 1.7 (`BD01067`):** `M1.7` = {0,1} alfabesinde içinde `11` **bulunmayan** dizgileri tanıyan
  2DFA; tüm durumlar uç durum. Tanıma koşulu: sonlu hareket sonunda **tüm dizgi işlenmiş ve kafa
  dizginin sağına geçmiş** olmalı. Bu koşul iki yüzden sağlanamayabilir:
  1. kafa hareketleri **döngüye** girer,
  2. kafa ilk hücredeyken **sola** gitmeyi gerektiren bir geçişle karşılaşır.

### 1.1.5.2 Anlık Tanım (ID) (`BD01070`, `BD01072`)

- `w = αβ` olsun; α kafanın solunda, β sağında (kafa β'nın ilk simgesinde).
- **Anlık tanım (üçlü):** `ID = (α, qi, β)`
- Tek hareket: `ID1 ⊢ ID2`;  bir dizi hareket: `ID1 ⊢* ID2`
- 2DFA'nın tanıdığı dil:

```
T(M) = { w | (q0, w) ⊢* (w, qi),  qi ∈ F }
```

  - Başlangıç ID'si `(q0, w)`: kafa ilk simgede, solda hiçbir şey yok (α yok).
  - Son ID `(w, qi)`: tüm dizgi solda (β yok), kafa dizginin sağındaki boş hücrede, `qi ∈ F`.
- Örnek izler: `w1 = 1000101`, `w2 = 100110` için `M1.7` üzerinde ID zincirleri (sitede animasyonlu).

### 1.1.5.4 Tek ve iki yönlü denklik (`BD01075`)

- Her 2DFA'ya denk tek yönlü DFA bulunabilir → **2DFA ≡ DFA** (ifade güçleri eşit; ispat derste yok).

## 1.2 Çıkış Üreten Özdevinirler (`BD01080`)

- Tanıyıcı: dizgileri kabul/red eder. Çıkış üreten model (**dönüştürücü**): giriş dizgisini çıkış
  dizgisine dönüştürür. (Tanıyıcı da aslında {1:tanıdı, 0:tanımadı} ikili çıkışlı görülebilir.)
- İki tür: **Moore makinesi** (çıkış **duruma** bağlı) ve **Mealy makinesi** (çıkış **geçişe** bağlı).

### 1.2.1 Moore makinesi (`BD01085`)

Bir **altılı**: `M = <Q, Σ, Δ, δ, λ, q0>` ; çıkış işlevi `λ : Q → Δ` (her duruma bir çıkış simgesi).

- DFA, Moore'un özel halidir: `Δ = {0,1}` ve `λ(qi) = 1 (qi ∈ F)`, `λ(qi) = 0 (qi ∉ F)` alınırsa DFA elde edilir.
- Gösterim: **durum çizelgesi (state table)** / **durum çizeneği (state diagram)**.
- **Örnek 1.8 (`BD01087`):** `M1.8`: giriş ikili sayı X ise çıkış `z = Mod(X, 5)`.

### 1.2.2 Mealy makinesi (`BD01090`)

Aynı altılı; tek fark çıkış işlevi: `λ : (Q × Σ) → Δ` (çıkış **geçiş sırasında** üretilir).

- **Örnek 1.9 (`BD01092`):** giriş {0,1}, çıkış {0,1,2}; çıkış, son iki giriş simgesinden kaçının
  bir öncekinden farklı olduğunu gösterir (başlangıçtan önceki iki giriş 00 varsayılır).
  Durumlar = önceki iki girişin değeri: A=00, B=01, C=10, D=11.

### 1.2.3 Moore – Mealy eşdeğerliği (`BD01095`)

- Moore: n giriş simgesine **n+1** çıkış üretir (başlangıçta, giriş olmadan da `z0` üretir): çıkış = `z0 · Z_Moore`
- Mealy: n girişe **n** çıkış üretir (giriş olmadan çıkış yok): çıkış = `Z_Mealy`
- `Z_Moore` ile `Z_Mealy` tüm girişler için aynıysa makineler denktir (Moore'un başlangıç `z0`'ı hariç).

### 1.2.4 Moore → eşdeğer Mealy (Teorem 1.1) (`BD01100`)

```
M1 = <Q, Σ, Δ, δ, λ, q0>   →   M2 = <Q, Σ, Δ, δ, λ', q0>,   λ'(q, a) = λ(δ(q, a))
```

- Yani her geçişin çıkışı, **varılan durumun** Moore çıkışıdır. Durum ve geçiş sayıları değişmez.
- Örnek: `M1.8` Moore'unun eşdeğer Mealy'si (Çizim 1.11).

### 1.2.5 Mealy → eşdeğer Moore (Teorem 1.2) (`BD01105`, `BD01107`)

- Moore'un durumları **[durum, çıkış simgesi]** ikilileridir: Mealy'nin `n` durumu, `m` çıkış simgesi
  varsa Moore'un **n × m** durumu olur (örn. 4 durum × 4 çıkış = 16 durum: [A,0] … [D,3]).
- Başlangıç durumu: birinci elemanı `q0` olan ikililerden **rasgele biri**.
- Geçiş işlevi:

```
δ'([q1, zk], xi) = [ δ(q1, xi), λ(q1, xi) ]
```

- Mealy'de `q1 →(xi / zj)→ q2` ise, Moore'da birinci elemanı q1 olan her durumdan xi ile
  `[q2, zj]` durumuna geçilir → her Mealy geçişine **m** Moore geçişi düşer.
- **Örnek 1.10 (`BD01107`):** `M1.10` Mealy → eşdeğer Moore (Çizim 1.12); bulunduktan sonra
  durumlar yeniden adlandırılabilir.

## 1.3 Sonlu Özdevinirlerin İndirgenmesi (`BD01110`)

- Makinenin karmaşıklığı **durum sayısı** ile orantılıdır.
- **İndirgeme (yalınlaştırma):** verilen makineye **denk, durum sayısı en küçük** makineyi bulmak.
- DFA, Moore, Mealy için yapılır. Model hiyerarşisi: FA ⊂ Moore ⊂ Mealy
  (FA = çıkış alfabesi {kabul, red} olan Moore; Moore = çıkışı girişten bağımsız Mealy).
  Bu yüzden tanımlar **Mealy üzerinden** verilir.

### 1.3.1 Temel tanımlar (`BD01115`–`BD01140`) — örnek makine `M1.11` (Mealy, Σ=Δ={0,1}, q0=A)

- **Ardıl:** `S1`'den `w` ile `S2`'ye geçiliyorsa `S2`, `S1`'in **w-ardılı**dır.
  Deterministikte tek durum; NFA'da altküme. Örn. `M1.11`: A'nın 1-ardılı D; B'nin 011-ardılı C.
  `M1.3`: q0'ın 0-ardılı {q0,q1}; 100-ardılı {q0,q1,q3}; q1'in 1-ardılı **yok (Φ)**.
- **Öncel:** deterministik modelde bile **altkümedir**. Örn. `M1.11`: B'nin 0-önceli {F,G};
  001-önceli {A,E,F}; D'nin 0 ve 110 öncelleri yok. (Öncel çizelgesi kullanılabilir.)
- **n-denk:** `S1` veya `S2`'den başlanıp **uzunluğu ≤ n** hangi giriş dizgisi uygulanırsa uygulansın
  aynı çıkış üretiliyorsa. Örn. `M1.11`: A,B 1-denk; C,F 1-denk; A,D 2-denk ama 3-denk **değil**;
  B,C 3-denk (dolayısıyla 4-,5-… denk). Denklik her zaman **en büyük n** ile söylenir.
- **Denk:** uzunluk ne olursa olsun aynı çıkış → tüm n'ler için n-denk. Örn. B≡C, D≡F.
- **n-ayırdedilebilir:** ayırmak için **en az n uzunluğunda** giriş dizgisi gerekiyorsa.
  O zaman durumlar (n−1)-denktir. Örn. A,E 1-ayırdedilebilir; B,D 2-; A,F 3-; D,F **ayırdedilemez** (denk).
- **Makine denkliği:** M1'in her durumunun M2'de, M2'nin her durumunun M1'de denk karşılığı varsa
  M1 ≡ M2.

### 1.3.2 İndirgeme yöntemi: denklik bölümlemeleri (`BD01150`)

- `Pk` = k-denklik bölümlemesi (k-denk durumlar aynı bölümde).
- `P0, P1, P2, …` türetilir; **`Pk+1 = Pk` olunca dur**, `P = Pk`.
- `Pm+1`, `Pm`'den **Teorem 1.3** ile türetilir: iki durumun (m+1)-denk olabilmesi için hem
  kendilerinin hem 0- ve 1-ardıllarının `Pm`'de aynı bölümde olması gerekir.
- Bölümleme bittiğinde **bölüm sayısı = en küçük makinenin durum sayısı**; her bölüm bir durum olur.

**Mealy indirgemesi (`BD01155`)** — `M1.11`: giriş uygulanmadan durumlar ayırt edilemez →
`P0 = (ABCDEFG)` (tek bölüm). Durum çizelgesinden `P1 = (ABCDFG)(E)`. Ardıl kontrolüyle
`P2 = (ADF)(BCG)(E)`, `P3 = (A)(DF)(BCG)(E)`, `P4 = P3` → **P = (A)(DF)(BCG)(E)** → 4 durum:
S0=A, S1=DF, S2=BCG, S3=E.

**Moore indirgemesi (`BD01160`)** — fark yalnız `P0`'da: çıkış **duruma** bağlı olduğu için giriş
uygulanmadan da ayırt edilebilir → **P0'da çıkış simgesi sayısı kadar bölüm** vardır. Sonrası aynı.
Örnek `M1.12` → 5 bölüm → S0=A, S1=BG, S2=DH, S3=C, S4=EF.

**DFA indirgemesi (`BD01165`)** — DFA, Δ={kabul,red} olan Moore'dur → `P0 = (uç durumlar)(uç olmayanlar)`.
Örnek `M1.13` → 5 bölüm: S0=q0, S1=q1, S2={q2,q3}, S3={q4,q6}, S4={q5,q7}.

---

# BÖLÜM 2 — DÜZGÜN KÜMELER VE DÜZGÜN DEYİMLER

## 2.1 Düzgün Kümeler (`BD02015`)

- Bir FA'nın tanıdığı dizgiler kümesine **düzgün küme (regular set)** denir.
- **Küme düzgündür ⇔ onu tanıyan en az bir sonlu özdevinir vardır.**
- Düzgün olup olmadığını anlamak için üç yol:
  1. kümeyi tanıyan FA kurmayı dene,
  2. kümeyi düzgün deyimle yazmayı dene (2.2),
  3. (biçimsel olmayan) tanımak için gereken **durum/bellek miktarını** irdele.
- **Düzgün olmayan örnekler:** `P5 = {0ⁿ1ⁿ | n ≥ 1}` ve `P6` (0'larla 1'lerin sayılarının farkının
  sıfır olduğu dizgiler). İkisinde de **sınırsız sayaç** gerekir; sonlu durum = sonlu bellek yetmez →
  tanıyan makine FA olamaz → kümeler düzgün değildir.

## 2.2 Düzgün Deyimler (`BD02020`)

Düzgün deyimler, düzgün kümeleri biçimsel tanımlayan dildir. **Tanım 2.1 (özyineli):**

1. `λ` (boş simge) ve `∅` (boş küme) birer düzgün deyimdir,
2. her `a ∈ Σ` bir düzgün deyimdir,
3. `P`, `Q` düzgün deyimse `PQ` (bitiştirme), `P + Q` (birleşim), `P*` (Yıldız/Kleene) düzgün deyimdir.

Örnekler sitede {a,b,c,d} ve {0,1} alfabeleriyle verilmiştir.

## 2.2.1 Düzgün deyim → sonlu özdevinir (`BD02025`, `BD02028`)

- **Önerme/Teorem 2.1:** Düzgün deyimlerle tanımlanan kümeler düzgün kümelerdir.
  İspat: tanımın her maddesi için çizeneği ver:
  - `a` için: iki durum, arada `a` yayı;
  - `λ` için: iki durum, arada λ-geçişi;
  - `∅` için: geçişi olmayan çizeneğe;
  - birden çok başlangıç/uç durum varsa: yeni `q0` / yeni uç durum ekleyip **λ-geçişleriyle** tekle.
  - Sonra `PQ` (ardışık ekle), `P + Q` (paralel/λ ile dallan), `P*` (λ ile dön) çizenekleri kur.
- **Sezgisel yaklaşım** biçimsel olandan çok daha yalın çizeneğe verir. İpuçları:
  1. `… + a*b …` ya da `(a*b…)*` örüntüsü varsa: **a döngülü düğüme λ-geçişi ile gel**,
  2. `… a b* + …` ya da `… a b*)*` örüntüsü varsa: **b döngülü düğümden λ-geçişi ile çık**,
  3. `… a* b* …` örüntüsü varsa: **a döngülü düğümden b döngülü düğüme λ-geçişi ile geç**.
- Örnek: `P = (a + bc*)*` (Çizim 2.2: biçimsel vs sezgisel karşılaştırma).

## 2.2.2 Sonlu özdevinir → düzgün deyim (`BD02030`)

- **Yöntem: denklem sistemi çözümü.** Her durum için aynı adlı bir küme değişkeni tanımlanır
  (A değişkeni = başlangıçtan başlayıp A'da biten tüm dizgiler).
- Her durum için denklem: sağ tarafta, o duruma giren her `δ(B, ai) = A` geçişi için `B·ai` çarpımı;
  başlangıç durumunun denklemine `+ λ` eklenir:

```
A = Σ (δ(B,ai)=A olan) B·ai        (başlangıç durumu için: + λ)
```

- **Teorem 2.3 (Arden benzeri):** `X = Xα + β` ve `λ ∉ α` ise `X = βα*`.
- Çözüm biter bitmez: `T(M)` = **uç durumlara karşı gelen deyimlerin birleşimi (+)**.

**Örnek 2.1 (`BD02031`):**
```
A = λ + A0 + B1      (1)
B = A0               (2)
(2)'yi (1)'de koy: A = λ + A0 + A01 = λ + A(0 + 01)      (3)
Teorem 2.3: A = λ(0 + 01)* = (0 + 01)*                  (4)
B = A0 = (0 + 01)*0                                     (5)
Tek uç durum B →  T(M2.1) = (0 + 01)*0
```

**Örnek 2.2 (`BD02032`):**
```
A = λ + A0 + B1            (1)
B = A0 + B1 + C0           (2)
C = B0 + C1                (3)
(3) + Teorem 2.3:  C = B01*                              (4)
(2)'de koy:  B = A0 + B(1 + 01*0)  →  B = A0(1 + 01*0)*  (5,6)
(1)'de koy:  A = λ + A[0 + 0(1 + 01*0)*1]  →  A = [0 + 0(1 + 01*0)*1]*   (7,8)
B = [0 + 0(1 + 01*0)*1]* 0(1 + 01*0)*                   (9)
C = [0 + 0(1 + 01*0)*1]* 0(1 + 01*0)* 01*
Uç durumlar B ve C:
T(M2.2) = B + C = B(λ + 01*)
        = [0 + 0(1 + 01*0)*1]* 0(1 + 01*0)* (λ + 01*)
```

---

# BÖLÜM 3 — DİLBİLGİSİ VE DİLLER

## 3.1 Dilbilgisi ve Dil (`BD03015`)

- **Alfabe:** sonlu sayıda simgeden oluşan küme. **Dizgi (string/sözcük):** alfabe simgelerinin
  ardarda getirilmesi. **Biçimsel dil:** bir alfabeden üretilebilecek dizgilerin (`Σ*`'ın) bir altkümesi.
- **Tümce (sentence):** dilde yer alan (geçerli) dizgi. Dizgi/sözcük eşanlamlı; tümce = dildeki dizgi.
- **Dilbilgisi (grammar):** dilin hangi tümcelerden oluştuğunu gösteren kurallar bütünü.
- Harf grupları (Çizelge 3.1): `V_N` değişkenler, `V_T` uç simgeler, `P` kurallar, `S` başlangıç simgesi.

### 3.1.1 Biçimsel tanım (`BD03025`)

```
G = <V_N, V_T, P, S>
```

- `P`: yeniden yazma kuralları (`α ⇒ β`).
- Dilin tümceleri: `S`'den başlayıp kuralları yeterince uygulayarak elde edilen **uç simge dizgileri**:

```
L(G) = { w | S ⇒* w ,  w ∈ V_T* }
```

## 3.2 Dilbilgisi ve Dillerin Sınıflandırılması (Chomsky sıradüzeni)

### Tür-0 — kısıtlamasız (`BD03035`)

```
α ⇒ β   :   α ∈ V⁺ ,  β ∈ V*
```

- Türetilen dil: **tür-0 dil = özyineli sayılabilir (recursively enumerable, r.e.) dil**.
- r.e. dil için tümceleri ardarda türeten bir **yordam (procedure)** vardır; verilen tümcenin dilde
  olup olmadığını belirleyen bir yordam da vardır (her zaman sonlanması gerekmez).
- Örnek: `L(G3.2) = { a^i | i = 2ⁿ, n ≥ 1 }`.

### Tür-1 — bağlama-bağımlı (context sensitive) (`BD03040`)

```
α ⇒ β   :   α ∈ V⁺ ,  β ∈ V* ,  |α| ≤ |β|
```

- Eşdeğer normal biçim: `α1 A α2 ⇒ α1 β α2`  (A ∈ V_N, α1,α2,β ∈ V*) → "α1α2 bağlamında A yerine β"
  → adını buradan alır.
- Türetilen dil: **tür-1 / bağlama-bağımlı dil = özyineli (recursive, r.) dil**: üyeliği belirleyen
  **her zaman sonlanan bir algoritma** vardır. Algoritma ⊂ yordam olduğundan **r. ⊂ r.e.**
- Örnek: `L(G3.3) = { aⁿ bⁿ cⁿ | n ≥ 1 }`.

### Tür-2 — bağlamdan-bağımsız (context free) (`BD03045`)

```
A ⇒ β   :   A ∈ V_N ,  β ∈ V*
```

- Sol tarafta tek değişken → kural **bağlamdan bağımsız** uygulanır.
- Türetilen dil: **tür-2 / bağlamdan-bağımsız dil**. Programlama dilleri, derleyiciler vb. için
  en önemli model.
- Örnek: `G3.4` → {a,b} alfabesinde **eşit sayıda a ve b** içeren dizgiler. (`G3.1` de tür-2'dir;
  `G3.2`, `G3.3` değildir.)

### Tür-3 — düzgün (regular) (`BD03050`)

```
A ⇒ aB      A ⇒ a      A ⇒ λ        (A,B ∈ V_N , a ∈ V_T)
```

- Türetilen dil: **tür-3 / düzgün dil**. Yalnız `G3.1` tür-3'tür.
- Örnek: `G3.5` → {0,1} alfabesinde "içindeki her 1'den önce en az iki 0 bulunan" dizgiler.

### Sağ-doğrusal ve sol-doğrusal (`BD03055`)

```
sağ-doğrusal:  A ⇒ wB ,  A ⇒ w        (w ∈ V_T*)
sol-doğrusal:  A ⇒ Bw ,  A ⇒ w        (w ∈ V_T*)
```

(w sıfır uzunluklu da olabilir.)
- Üçünün (tür-3, sağ-doğrusal, sol-doğrusal) türettiği diller **düzgün dillerdir** → üçü de **denk**
  dilbilgisidir; topluca **düzgün dilbilgileri** denir.
- Tür-3, sağ-doğrusalın özel halidir; sağ-doğrusal → tür-3 dönüşümü ek değişkenlerle kolaydır.
- Sol-doğrusal ≡ sağ-doğrusal denkliği, üçünün de FA'nın tanıdığı düzgün dilleri ürettiği
  gösterilerek dolaylı ispatlanır (bkz. 2.2.2 / 3.3).
- **Örnek 3.6:** `L(G3.6) = 0(10)* = (01)*0`; aynı dil sağ-doğrusal `G3.6`, sol-doğrusal `G'3.6`,
  tür-3 `G''3.6` ile türetilebilir → denk dilbilgileri.

**Kapsama zinciri:** düzgün (tür-3) ⊂ bağlamdan-bağımsız (tür-2) ⊂ bağlama-bağımlı (tür-1) ⊂ tür-0.

## 3.3 Düzgün Dil – Sonlu Özdevinir İlişkisi

### 3.3.1 Düzgün dilbilgisi → FA (Teorem 3.1) (`BD03065`)

- **Teorem 3.1:** düzgün dilbilgisinin türettiği dili tanıyan bir FA vardır → düzgün diller = düzgün kümeler.

**Tür-3 için (`BD03070`):** `G = <V_N, V_T, P, S>` verilsin; `M = <Q, Σ, q0, δ, F>`:
- her değişkene bir durum, `S → q0`;
- `A ⇒ aB` kuralı → `δ(A, a) = B`;
- `A ⇒ a` kuralı → `δ(A, a) = F` (yeni eklenen tek uç durum);
- `A ⇒ λ` kuralı varsa → `A` uç durum.

**Sağ-doğrusal için (`BD03075`):** kuralların sağ tarafındaki uç simge **dizgileri** için ara durumlar
(`[αi]` = sağ tarafın sonekleri) eklenir: `Q = { [S], [λ], [α1], [α2], … }`; geçişler kurallardan ve
soneklerden türetilir.

**Sol-doğrusal için (`BD03080`):**
1. tüm kuralların sağ taraflarını ters çevir → `G^R` (sağ-doğrusal olur),
2. `G^R`'yi tanıyan `M^R`'i bul,
3. `M^R`'i ters çevir: başlangıç durumu ↔ uç durumlar, tüm geçişleri yönünü değiştir → `M`.

### 3.3.2 FA → düzgün dilbilgisi (`BD03085`)

`M` verilsin; `L(G) = T(M)` olan tür-3 dilbilgisi:
- her duruma bir değişken, `q0 → S`;
- `δ(A, a) = B` → `A ⇒ aB`;
- `δ(A, a) = F` (uç) → `A ⇒ a`;
- başlangıç durumu aynı zamanda uç ise `S ⇒ λ`.

## 3.4 Düzgün Dillerin Kimi Özellikleri — Pumping Lemma (`BD03090`)

- L düzgün olsun; L'yi tanıyan **en küçük** DFA'nın durum sayısı `n` olsun.
- `|w| = m > n` olan bir `w ∈ L` için, `w`'yi tanıyan yol **en az bir durumdan iki kez geçer**
  (yol döngülüdür).
- O halde `w = u v t` biçiminde yazılabilir ve makine

```
w = u v^i t      (i ≥ 0)
```

  biçimindeki **tüm** dizgileri tanır → L sonsuzdur.
- **Pumping lemma (özet):** "Her düzgün dil için öyle bir `n` tamsayısı vardır ki, dilde uzunluğu
  `n`'den büyük bir `w` tümcesi varsa, `w = uvt` biçiminde yazılabilir ve dil `u v^i t` (i ≥ 0)
  biçimindeki tüm tümceleri içerir."
- **Kullanımı:** bir dilin **düzgün olmadığını** ispatlamak (çelişki kur).

### 3.4.1 Kapalılık özellikleri (`BD03095`)

- Düzgün diller **birleşim, kesişim ve tümleme** işlemlerinde kapalıdır:
  L1, L2 düzgün ise `L1 ∪ L2`, `L1 ∩ L2`, `L1'` düzgündür.
- Tümleme: `L1' = Σ* − L1` (L1, Σ üzerinde tanımlıysa).

---

# BÖLÜM 4 — BAĞLAMDAN-BAĞIMSIZ DİLBİLGİSİ (CFG) VE DİLLER

## 4.1 Bağlamdan-Bağımsız Dilbilgisi (`BD04015`)

- Programlama dilleri, derleyiciler, yorumlayıcılar, sözdizim çözümleyiciler, aritmetik deyimler…
  CFG'yi bilgisayar bilimleri için önemli kılar.
- CFG: `G = <V_N, V_T, P, S>`, kurallar `A ⇒ β` (A ∈ V_N).
- Bu dili tanıyan makine modeli: **yığıtlı özdevinir (PDA)** — Bölüm 5.
- `G3.1` ve `G3.4` birer CFG örneğidir.

## 4.2 Türetme Ağacı (`BD04020`)

- Türetme zinciri:

```
S ⇒ β1 ⇒ β2 ⇒ … ⇒ βn-1 ⇒ βn ⇒ w        βi ∈ V*  (tümcesel yapılar),  w ∈ V_T* (tümce)
```

- Her tümcesel yapıya ve her tümceye bir **türetme / ayrıştırma ağacı (derivation–parsing tree)** karşı gelir.
- Örnek (`G4.4`): `β = 010A010` tümcesel yapısı ve `w = 0010100100` tümcesi için ağaçlar (Çizim 4.1).

### 4.2.1 Ağacın tanımı (`BD04025`)
- Kök `S`; iç düğümler değişkenler; yapraklar uç simgeler (ya da λ); her iç düğümün çocukları,
  uygulanan kuralın sağ tarafıdır. (Sitede animasyonlu.)

### 4.2.2 Soldan ve sağdan türetme (`BD04030`)

- **Belirginlik (unambiguous):** dilin her tümcesine **tek** türetme ağacı karşı geliyorsa dil belirgindir.
  Bir tümceye birden çok ağaç geliyorsa dil **belirgin değildir (ambiguous)**; böyle dillerin
  uygulamada değeri yoktur → çalışmada tüm diller belirgin varsayılır.
- Her tümceye bir ağaç, bir **soldan türetme**, bir de **sağdan türetme** karşı gelir:
  - **Soldan türetme (leftmost):** her adımda **en soldaki** değişkene kural uygulanır.
  - **Sağdan türetme (rightmost):** her adımda **en sağdaki** değişkene kural uygulanır.

## 4.3 Dilbilgisinin Yalınlaştırılması (`BD04035`)

Amaç: aynı dili türeten, "gereksiz yükleri" atılmış eşdeğer dilbilgisi.

### 4.3.1 Özyineli kural, özyineli değişken (`BD04040`)

- `A ⇒* … A …` biçiminde türetme varsa A **özyineli değişken**dir.
- Bazı işlemler başlangıç değişkeninin özyineli olmamasını ister: yeni değişken `S'` ve yeni kural
  `S' ⇒ S` eklenip **S' başlangıç değişkeni** yapılır.

### 4.3.2 Yok edilebilir (nullable) değişken (`BD04045`)

- `A ⇒ λ` ya da `A ⇒* λ` ise A **yok edilebilir** değişkendir.
- İstenen biçim: `S ⇒ λ` dışında λ-kuralı olmasın →
  - `λ ∉ L` ise dilbilgisinde **hiç** λ-kuralı kalmaz,
  - `λ ∈ L` ise tek λ-kuralı `S ⇒ λ` olur.
- Yok edilebilir değişkenler bir algoritmayla bulunur (Algoritma 4.1); ikinci algoritma (4.2)
  dilbilgisini istenen eşdeğer biçime dönüştürür.
- **Örnek 4.5 (`BD04047`):** `TL = {C, A, S}`; S yok edilebilir olduğu için `λ ∈ L` → eşdeğer
  dilbilgisinde `S ⇒ λ` kalır.

### 4.3.3 Birim türetme kuralları (unit/chain rules) (`BD04050`)

- `(A ⇒ B)` biçimindeki kurallara **birim türetme kuralı / zincir kural** denir; türetmeyi geciktirir.
- Her değişken için ondan türetilebilen değişkenler kümesi:

```
∀A ∈ V_N  ⇒  T_A = { B | B ∈ V_N,  A ⇒* B }
```

  (Algoritma 4.3). Sonra Algoritma 4.4 ile birim kuralsız eşdeğer dilbilgisi kurulur.
- **Örnek 4.6 (`BD04052`):** `G4.6` için S, A, B, C'nin `T` kümeleri bulunur, birim kurallar elenir.

### 4.3.4 Yararsız değişken, simge ve kurallar (`BD04055`, `BD04057`)

- **Yararsız simge:** hiçbir tümcenin içinde yer almayan uç simge.
- **Yararlı değişken:** `S ⇒* α1 A α2 ⇒* w` sağlanıyorsa (bir türetmede görünür ve oradan tümceye
  inilebiliyorsa). Sağlanmıyorsa **yararsız değişken**.
- **Yararlı kural:** en az bir tümcenin türetilmesinde kullanılan kural; kullanılmayan → yararsız kural.
- **Atma iki adımda (sıra önemli):**
  1. **Algoritma 4.5** — uç simge dizgisi türeten (üretken) değişkenler `T_D`; bu kümede olmayanlar
     atılır → `G'`.
  2. **Algoritma 4.6** — `S`'ten ulaşılabilen değişkenler `T_U`; ulaşılamayanlar atılır → `G''`.
- **Örnek 4.7 (`BD04058`):** `T_D = {S, A, B, E, F}` → C, D atılır; sonra `T_U = {S, B}` →
  A, E, F atılır; bu arada `a` simgesi de yararsız hale gelir ve atılır.

## 4.4 Normal Biçimler (`BD04060`)

### 4.4.1 Chomsky Normal Biçimi (CNF) (`BD04065`, `BD04070`)

- **Tanım 4.1:** tüm kurallar şu üç biçimdeyse dilbilgisi CNF'dir:

```
S ⇒ λ        A ⇒ BC        A ⇒ a        (A,B,C ∈ V_N , a ∈ V_T)
```

- **CFG → CNF dönüşümü:** `A ⇒ BC` ve `A ⇒ a` kuralları korunur; diğerleri (uzun sağ taraflar,
  uç+değişken karışık sağ taraflar, λ- ve birim kurallar) önce yalınlaştırma adımlarıyla temizlenir,
  sonra yeni değişkenlerle parçalanarak bu biçime sokulur. (Örnek 4.8 sitede adım adım.)

### 4.4.2 Greibach Normal Biçimi (GNF) (`BD04075`, `BD04080`)

- **Tanım 4.2:** tüm kurallar şu biçimdeyse GNF'dir:

```
S ⇒ λ        A ⇒ aα        (A ∈ V_N , a ∈ V_T , α ∈ V_N*)
```

  (sağ taraf **tek uç simge ile başlar**, ardından yalnız değişkenler gelir).
- **Teorem 4.2:** bağlamdan-bağımsız her dili türeten bir GNF dilbilgisi bulunabilir
  (dönüşümde iki yardımcı lemma kullanılır).
- **CFG → GNF algoritması:** dilbilgisi **önce CNF'e** dönüştürülür; sonra 3 adımlı algoritma
  (değişkenleri sıraya sokma / sol özyinelemeyi kırma / ilk simgeyi uç simgeye indirgeme) uygulanır.
  3. adımın sonunda GNF elde edilir; değişkenler istenirse yeniden adlandırılır.
- **Örnek 4.9, 4.10 (`BD04082`, `BD04084`):** adım adım dönüşüm; 4.10'da sonuçta `A2` yararsız
  değişken, `A2 ⇒ a` yararsız kural çıkarılıp kalanlar S, B, C diye adlandırılır.

---

# Hızlı Özet Tablosu

| Konu | Biçimsel tanım |
|---|---|
| DFA | `<Q, Σ, δ, q0, F>`, `δ:(Q×Σ)→Q` |
| NFA | aynı beşli, `δ:(Q×Σ)→2^Q` |
| 2DFA | aynı beşli, `δ:(Q×Σ)→Q×{R,L}` |
| Moore | `<Q, Σ, Δ, δ, λ, q0>`, `λ:Q→Δ` |
| Mealy | `<Q, Σ, Δ, δ, λ, q0>`, `λ:(Q×Σ)→Δ` |
| Moore→Mealy | `λ'(q,a) = λ(δ(q,a))` |
| Mealy→Moore | durumlar `[q,z]`, `δ'([q1,zk],xi) = [δ(q1,xi), λ(q1,xi)]` |
| Tanınan dil (FA) | `T(M) = { w \| δ(q0,w) ∈ F }` |
| Tanınan dil (2DFA) | `T(M) = { w \| (q0,w) ⊢* (w,qi), qi ∈ F }` |
| Dilbilgisi | `G = <V_N, V_T, P, S>`, `L(G) = { w \| S ⇒* w, w ∈ V_T* }` |
| Tür-0 | `α ⇒ β`, `α ∈ V⁺, β ∈ V*` → r.e. dil |
| Tür-1 | `+ \|α\| ≤ \|β\|` → recursive dil |
| Tür-2 | `A ⇒ β` → CFG |
| Tür-3 | `A ⇒ aB \| a \| λ` → düzgün dil |
| Sağ/sol-doğrusal | `A ⇒ wB \| w`  /  `A ⇒ Bw \| w` → düzgün dil |
| CNF | `S ⇒ λ`, `A ⇒ BC`, `A ⇒ a` |
| GNF | `S ⇒ λ`, `A ⇒ aα` |
| Pumping | `\|w\| > n ⇒ w = uvt`, `uv^i t ∈ L (i ≥ 0)` |
| Arden (Teorem 2.3) | `X = Xα + β ⇒ X = βα*` |
