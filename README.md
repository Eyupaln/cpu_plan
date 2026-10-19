# Prosessorun planlaşdırılması — interaktiv simulyator

Əməliyyat Sistemləri kursunun **«Prosessorun planlaşdırılması»** (Mühazirə 8) mövzusu üçün interaktiv simulyator.
Tək HTML faylıdır: heç bir quraşdırma, asılılıq və ya server tələb etmir — faylı brauzerdə açmaq kifayətdir.

Simulyator CPU planlama alqoritmlərini **addım-addım və qrafik** şəkildə göstərir: hansı prosesin nə vaxt
icra olunduğunu, gözləmə/dövriyyə/cavab vaxtlarını və alqoritmlərin bir-biri ilə müqayisəsini hesablayır.

---

## İşə salma

**Variant 1 — birbaşa fayl:** `index.html` faylını endirib brauzerdə açın (iki dəfə klik).

**Variant 2 — GitHub Pages:** repo *Settings → Pages → Source: main / root* seçilərsə, simulyator
`https://eyupaln.github.io/cpu_plan/` ünvanında işləyəcək.

**Variant 3 — lokal server:**

```bash
python -m http.server 8000
# brauzerdə: http://127.0.0.1:8000/index.html
```

---

## Xüsusiyyətlər

- **6 planlama alqoritmi:** FCFS, SJF, SRTF (preemptive SJF), Prioritet (qeyri-preemptive),
  Prioritet (preemptive) və Round Robin (vaxt kvantı daxil edilir).
- **Proses cədvəli:** ad, gəlmə vaxtı, burst vaxtı və prioritet — sətir əlavə etmə/silmə,
  dəyər dəyişdikdə nəticə avtomatik yenilənir.
- **İcra qrafiki (Gantt diaqramı):** rəngli bloklar, zaman oxu və prosessorun boş qaldığı anlar.
- **Addım-addım rejim:** icranı tək-tək izləmə və sürət seçimi (yavaş / orta / sürətli).
- **Göstəricilər cədvəli:** hər proses üçün başlama (CT‑yə qədər), tamamlanma (CT), dövriyyə (TAT),
  gözləmə (WT) və cavab (RT) vaxtları + orta sətir.
- **Xülasə kartları:** orta WT, orta TAT, orta RT və CPU məşğulluğu faizi.
- **Müqayisə cədvəli:** eyni proseslər üzərində bütün alqoritmlərin ortalaması, ən yaxşısının
  işarələnməsi; sətrə klikləyərək həmin alqoritmlə hesablama.
- **Bələdçi (tur):** səhifə ilk dəfə açıldıqda 10 addımlıq interaktiv izah başlayır;
  «Turu izlə» düyməsi ilə istənilən vaxt təkrar açılır.
- **Çap:** nəticələri səliqəli çap etmək üçün «Çap» düyməsi (dizayn çap üçün uyğunlaşdırılıb).
- **Mühazirə nümunələri:** Dərs 8‑dəki hazır nümunələr bir kliklə yüklənir.
- **İki dil (AZ / TR):** yuxarı sağdakı bayraq düymələri ilə interfeys Azərbaycan və Türk dilləri
  arasında dəyişir; seçilmiş dil brauzerdə yadda saxlanılır.
- Klaviatura: `→` ileri, `←` geri, `Esc` turu bağla.

---

## Alqoritmlər

| Alqoritm | Növ | Seçim qaydası |
|---|---|---|
| **FCFS** | qeyri-preemptive | Gəlmə vaxtına görə növbə (First-Come, First-Served) |
| **SJF** | qeyri-preemptive | Ən kiçik burst vaxtı olan proses əvvəl |
| **SRTF** | preemptive | Ən kiçik **qalıq** vaxtı olan proses davam edir |
| **Prioritet** | qeyri-preemptive | Ən kiçik prioritet ədədi = ən yüksək prioritet |
| **Prioritet (P)** | preemptive | Prioriteti daha yüksək proses gələn kimi CPU‑nu alır |
| **Round Robin** | preemptive | Növbə ilə hər prosessə bir **vaxt kvantı** verilir |

**Bərabərlik hallarında** sıra: gəlmə vaxtı, sonra cədvəldəki sıra (FCFS prinsipi).

**Round Robin konvensiyası:** kvant bitdiyi anda (həmin ana) gələn proses növbəyə cari prosesdən
**əvvəl** qoyulur. Simulyasiyada bu qayda tətbiq olunur.

**Prioritet şkalası:** kiçik ədəd yüksək prioritet sayılır (məsələn `1` > `5`).

---

## Göstəricilərin hesablanması

```
Tamamlanma vaxtı      CT  = prosesin son icra anı
Dövriyyə vaxtı        TAT = CT − gəlmə vaxtı
Gözləmə vaxtı         WT  = TAT − burst vaxtı
Cavab vaxtı           RT  = ilk icra başlanğıcı − gəlmə vaxtı

Orta WT/TAT/RT        = bütün proseslər üzrə cəmin proses sayına bölünməsi
CPU məşğulluğu        = (məşğul vaxt / ümumi vaxt) × 100%
```

---

## Mühazirə nümunələri və nəticələri

Aşağıdakı dəyərlər simulyatorun verdiyi nəticələrdir və Mühazirə 8‑dəki nümunələrə uyğundur.

| Alqoritm | Giriş (gəlmə · burst · prioritet) | Orta WT | Orta TAT |
|---|---|---|---|
| FCFS | P1 0·24·3, P2 0·3·1, P3 0·3·2 | **17** | 27 |
| SJF | P1 0·6·3, P2 0·8·1, P3 0·7·2, P4 0·3·4 | **7** | 13 |
| SRTF | P1 0·8·3, P2 1·4·1, P3 2·9·4, P4 3·5·2 | **6,5** | 13 |
| Prioritet | P1 0·10·3, P2 0·1·1, P3 0·2·4, P4 0·1·5, P5 0·5·2 | **8,2** | 12 |
| Round Robin (q = 2) | P1 0·4·3, P2 0·5·2, P3 0·8·2, P4 0·7·1, P5 0·3·3 | **15,4** | 20,8 |

> Ondalıq ayırıcı kimi vergül (`,`), minliklər üçün isə nöqtə istifadə olunur (Azərbaycan formatı).

---

## Layihə strukturu

```
cpu_plan/
├── index.html   # bütün simulyator: HTML + CSS + JavaScript (asaanlıqla dəyişdirilə bilər)
└── README.md
```

`index.html` daxilindəki əsas hissələr:

- **CSS** — açıq tema, SVG ikonlar (emoji yoxdur), responsiv dizayn və çap görünüşü.
- **HTML** — giriş paneli (alqoritm, proses cədvəli, nümunələr) və nəticə kartları.
- **JavaScript** — planlama alqoritmləri, metrikaların hesablanması, Gantt çəkilişi,
  addım-addım oynadılması və bələdçi turu. Xarici kitabxana istifadə olunmur.

---

## Lisenziya və istifadə

Təhsil məqsədilə hazırlanıb — dərslərdə, seminar və colloquium hazırlığında sərbəst istifadə oluna bilər.
Mənbə: *Mühazirə 8 — Prosessorun planlaşdırılması* (Əməliyyat Sistemləri kursu).

---

## English overview

A single-file, dependency-free interactive simulator for CPU scheduling algorithms
(FCFS, SJF, SRTF, Priority — preemptive and non-preemptive — and Round Robin).
It shows a Gantt chart, per-process completion / turnaround / waiting / response times,
averages, CPU utilisation, a step-by-step playback mode and a side-by-side comparison of
all algorithms on the same process set. The interface is available in **Azerbaijani and
Turkish** (flag buttons in the top bar). Open `index.html` in any browser to run it.
