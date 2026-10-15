# LITERATURE REVIEW — AlphaFold Reliability Audit

## Tinjauan Literatur Terverifikasi untuk RQ: Akurasi Struktural Prediksi AlphaFold vs Struktur Eksperimen, dan Reliabilitas pLDDT/PAE sebagai Indikator Akurasi

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal** | 2026-10-11 |
| **Status** | SELESAI — 40 sumber terverifikasi metadata + abstrak |
| **Metode verifikasi** | Europe PMC REST API (`resultType=core`) untuk metadata + abstrak; Crossref REST API untuk sumber pra-PubMed (Kabsch 1976) |
| **Batas verifikasi** | **Metadata (judul, penulis, jurnal, tahun, DOI, PMID) dan abstrak setiap sumber dibaca dan dicocokkan. Teks penuh (full text) TIDAK dibaca.** Semua temuan yang dikutip di bawah dibatasi pada apa yang dinyatakan dalam abstrak sumber. |
| **Aturan** | Tidak ada sitasi, DOI, angka, atau temuan yang dikarang. 5 DOI keliru terdeteksi dan dikoreksi sebelum masuk dokumen ini (§0.3). |

---

## 0. Metode & Integritas Verifikasi

### 0.1 Prosedur

1. Setiap kandidat sumber diverifikasi lewat Europe PMC (`DOI:"..."` atau pencarian judul) — metadata dicetak dan dicocokkan dengan klaim.
2. Untuk sumber yang tidak terindeks PubMed (Kabsch 1976), verifikasi dilakukan lewat Crossref REST API.
3. Setiap DOI diverifikasi **berdasarkan isi yang dikembalikan server**, bukan berdasarkan ingatan — prosedur ini menangkap 5 DOI yang keliru (dugaan awal tidak cocok dengan paper yang dimaksud).
4. Temuan yang dikutip dibatasi pada teks abstrak yang benar-benar terbaca dari API.

### 0.2 Yang TIDAK dilakukan

- Tidak membaca full text, supplementary, atau PDF.
- Tidak mengutip angka/figure/table dari dalam paper yang tidak muncul di abstrak.
- Tidak menyimpulkan "konsensus" dari ingatan umum — setiap pernyataan di §1–§7 harus bisa dilacak ke entri bernomor `[L##]`.

### 0.3 Catatan koreksi (dugaan awal yang salah — tertangkap verifikasi)

| Dugaan awal (salah) | Hasil verifikasi yang benar |
|---|---|
| Pereira 2021 di PNAS, DOI `10.1073/pnas.2112616118` | DOI tersebut **404/tidak ada di Crossref**. Paper yang benar: **Proteins 2021, `10.1002/prot.26171`** (PMID 34218458) |
| Hiranuma 2021 (DeepAccNet) di Nature Methods, DOI `10.1038/s41592-021-01177-5` | DOI tersebut = paper Potel et al. (phosphorylation). Paper yang benar: **Nature Communications 2021, `10.1038/s41467-021-21511-x`** (PMID 33637700) |
| AlQuraishi 2019 (ProteinNet) DOI `10.1371/journal.pcbi.1007320` | DOI tersebut = paper Wang & Dai 2019 (fitness landscape). Paper yang benar: **BMC Bioinformatics 2019, `10.1186/s12859-019-2932-0`** (PMID 31185886) |
| Méndez 2003 (CAPRI) DOI `10.1002/prot.10523` | DOI tersebut = paper ProVal (Berglund 2004). Paper yang benar: **Proteins 2003, `10.1002/prot.10393`** (PMID 12784368) |
| Kwon 2021 (EMA CASP14) DOI versi awal tidak cocok | Paper yang benar: **`10.1002/prot.26192`** (PMID 34324227) |

Tabel ini sengaja dipertahankan sebagai jejak audit: 5 DOI keliru (dari puluhan percobaan verifikasi DOI selama penyusunan) — semua salah bila diambil dari ingatan, semua benar setelah diverifikasi ke server. **Tidak ada dokumen fase riset berikutnya yang boleh menyalin DOI dari luar dokumen ini.**

---

## 1. TOPIK 1 — Benchmark AF2 / CASP14 & CASP15

**Pertanyaan topik:** Seberapa akurat AlphaFold2 menurut penilaian blind CASP, dan apa sifat benchmark tersebut?

### [L1] Jumper et al. 2021 — AlphaFold2 (paper utama)
- **Sitasi:** Jumper J, Evans R, Pritzel A, et al. *Highly accurate protein structure prediction with AlphaFold.* Nature 596, 583–589 (2021). DOI `10.1038/s41586-021-03819-2`. PMID 34265844.
- **Metode relevan:** arsitektur end-to-end; evaluasi pada CASP14 (blind).
- **Temuan kunci (dari abstrak):** sekitar 100.000 protein unik telah ditentukan strukturnya secara eksperimen, sebagian kecil dari miliaran sekuens; AF2 mencetak kemajuan besar pada CASP14.
- **Relevansi ke RQ:** mendefinisikan objek audit (output AF2) dan konteks klaim akurasinya. Detail teknis di luar abstrak (mis. tanggal cutoff data latih) **belum diverifikasi** dan dicatat sebagai item verifikasi sebelum implementasi (lihat `DATA_AND_BIAS_POLICY.md` §5).
- **Batas:** abstrak; tidak ada angka GDT/median di abstrak.

### [L2] Pereira et al. 2021 — Asesmen kategori high-accuracy, CASP14
- **Sitasi:** Pereira J, Simpkin AJ, Hartmann MD, Rigden DJ, Keegan RM, Lupas AN. *High-accuracy protein structure prediction in CASP14.* Proteins 89(12), 1687–1699 (2021). DOI `10.1002/prot.26171`. PMID 34218458.
- **Metode:** metrik kategori "high-accuracy" CASP; menilai semua grup peserta ≥10 target; menilai kegunaan model AF2 sebagai model pencarian *molecular replacement* dengan AMPLE; memperkenalkan metrik baru DipDiff.
- **Temuan kunci (dari abstrak):** kategori "high-accuracy" di CASP14 **meluas mencakup semua target** — digerakkan oleh pendekatan deep learning. Model AF2 juga dievaluasi sebagai model MR.
- **Relevansi:** bukti bahwa klaim akurasi AF2 berasal dari penilaian independen (bukan self-assessment) — dasar kuat; namun dengan catatan struktural benchmark CASP itu sendiri (lihat [L24], [L26]).
- **Batas:** angka spesifik per kategori tidak ada di abstrak.

### [L3] Kryshtafovych et al. 2021 — Overview CASP14
- **Sitasi:** Kryshtafovych A, Schwede T, Topf M, Fidelis K, Moult J. *Critical assessment of methods of protein structure prediction (CASP)—Round XIV.* Proteins 89(12), 1607–1617 (2021). DOI `10.1002/prot.26237`. PMID 34533838.
- **Temuan kunci (kutipan langsung abstrak):** metode deep learning dari **satu kelompok riset** konsisten memberi struktur komputasi yang "*rivaling the corresponding experimental ones in accuracy*"; hasil ini disebut "*a solution to the classical protein-folding problem, at least for single proteins*".
- **Relevansi:** basis klaim "mendekati akurasi eksperimen untuk protein tunggal" — penting karena **RQ kita fokus protein monomer** (sesuai batas dataset lokal).
- **Batas:** "for single proteins" adalah kualifikasi penting yang sering dikutip tanpa konteks — dokumen ini mempertahankannya.

### [L4] Kryshtafovych et al. 2023 — Overview CASP15
- **Sitasi:** Kryshtafovych A, Schwede T, Topf M, Fidelis K, Moult J. *Critical assessment of methods of protein structure prediction (CASP)—Round XV.* Proteins 91(12), 1539–1549 (2023). DOI `10.1002/prot.26617`. PMID 37920879.
- **Temuan kunci:** merangkum CASP15 (2022) dengan penekanan pada metode baru yang digerakkan deep learning; merujuk CASP14 sebagai "*major progress*" dengan "*accuracy comparable with experiment for many single proteins*".
- **Relevansi:** menunjukkan benchmark berlanjut (CASP15, 2022) setelah AF2 rilis; area fokus bergeser ke kompleks/assemblies — singgungan untuk topik 3 (interaksi).
- **Batas:** detail per target tidak di abstrak.

### [L5] Tunyasuvunakool et al. 2021 — Proteom manusia
- **Sitasi:** Tunyasuvunakool K, Adler J, Wu Z, et al. *Highly accurate protein structure prediction for the human proteome.* Nature 596, 590–596 (2021). DOI `10.1038/s41586-021-03828-1`. PMID 34293799.
- **Temuan kunci (angka dari abstrak):** cakupan 98,5% protein manusia; **58% residu berprediksi confident**; subset **36% residu bersangat tinggi (very high confidence)**; sebelumnya hanya 17% residu tercakup struktur eksperimen.
- **Relevansi:** angka populasi penting untuk RQ — bahkan di proteom yang paling banyak dipelajari, mayoritas residu berprediksi confident, artinya sampel benchmark umum didominasi protein "mudah" (hubungan dengan bias seleksi, §5).
- **Batas:** definisi "confident" = pLDDT>70 dan "very high" = pLDDT>90 adalah konvensi paper; angka tepatnya di abstrak.

### [L6] Abramson et al. 2024 — AlphaFold3
- **Sitasi:** Abramson J, Adler J, Dunger J, et al. *Accurate structure prediction of biomolecular interactions with AlphaFold 3.* Nature 630, 493–500 (2024). DOI `10.1038/s41586-024-07487-w`. PMID 38718835.
- **Temuan kunci:** AF3 (arsitektur diffusion) memprediksi struktur gabungan protein–nukleat–ligan; akurasi protein–ligan jauh lebih baik daripada tool docking sebelumnya.
- **Relevansi:** batas lingkup — riset ini **tidak** memakai AF3; ia menjadi konteks arah perkembangan (dan penanda bahwa objek audit kita = era AF2/AFDB v6).

---

## 2. TOPIK 2 — Akurasi Global vs Lokal vs Domain/Interface

**Pertanyaan topik:** Apa yang diketahui tentang di mana AF2 akurat (lokal) dan di mana ia bisa keliru meskipun confidence tinggi (global/domain)?

### [L7] Terwilliger et al. 2024 — Prediksi vs peta eksperimen (sumber kunci untuk RQ)
- **Sitasi:** Terwilliger TC, Liebschner D, Croll TI, et al. *AlphaFold predictions are valuable hypotheses and accelerate but do not replace experimental structure determination.* Nature Methods 21, 110–116 (2024). DOI `10.1038/s41592-023-02087-4`. PMID 38036854.
- **Metode:** membandingkan prediksi AlphaFold **langsung terhadap peta kristalografi eksperimen** (bukan hanya RMSD model-vs-model).
- **Temuan kunci (kutipan abstrak):** dalam banyak kasus prediksi cocok luar biasa dekat dengan peta eksperimen; **"even very high-confidence predictions differed from experimental maps on a global scale through distortion and domain orientation, and on a local sc[ale]…"** — yaitu: error pada prediksi ber-confidence sangat tinggi muncul sebagai distorsi global & orientasi domain, dan pada skala lokal tertentu.
- **Relevansi:** **bukti paling langsung** bahwa pLDDT tinggi ≠ struktur global benar; mendasari kebutuhan RQ ini dan mendikte desain analisis (pisahkan error lokal vs global/domain; lihat protokol §D).
- **Batas:** kutipan terpotong di abstrak; kalimat lengkap (bagian "local scale…") harus diverifikasi dari full text sebelum direproduksi persis di publikasi.

### [L8] Akdel et al. 2022 — Asesmen komunitas lintas aplikasi
- **Sitasi:** Akdel M, Pires DEV, Pardo EP, et al. *A structural biology community assessment of AlphaFold2 applications.* Nature Structural & Molecular Biology 29, 1056–1067 (2022). DOI `10.1038/s41594-022-00849-w`. PMID 36344848.
- **Temuan kunci (dari abstrak):** untuk 11 proteom, rata-rata **25% residu tambahan** dapat dimodelkan dengan confident dibanding homology modeling; prediksi disorder & kompleks AF2 melampaui tool khusus; model AF2 dapat dipakai lintas aplikasi "*equally well compared with experimentally determined structures, **when the confidence metrics are critically considered***".
- **Relevansi:** frasa kunci "*when the confidence metrics are critically considered*" — reliabilitas pemakaian **bergantung pada pembacaan confidence yang benar**; persis objek audit RQ.
- **Batas:** rincian per aplikasi tidak di abstrak.

### [L9] Guo et al. 2022 — pLDDT/PAE dan dinamika
- **Sitasi:** Guo HB, Perminov A, Bekele S, et al. *AlphaFold2 models indicate that protein sequence determines both structure and dynamics.* Scientific Reports 12, 10696 (2022). DOI `10.1038/s41598-022-14382-9`. PMID 35739160.
- **Temuan kunci:** AF2 "men-decode" fleksibilitas residu lewat pLDDT dan PAE; diuji pada protein globular, multi-domain, IDP, protein besar, dimer/heterodimer.
- **Relevansi:** mendukung interpretasi pLDDT sebagai indikator **fleksibilitas/dinamika**, bukan hanya akurasi statis — nuansa penting untuk membaca hasil audit.
- **Batas:** tidak ada angka di abstrak.

---

## 3. TOPIK 3 — Makna & Batasan pLDDT/PAE; Akurasi Pocket/Binding

**Pertanyaan topik:** Apa arti pLDDT dan PAE secara tepat, apa batasnya, dan bagaimana akurasi binding pocket dilaporkan?

### [L10] Bruley et al. 2022 — pLDDT rendah & disorder
- **Sitasi:** Bruley A, Mornon JP, Duprat E, Callebaut I. *Digging into the 3D Structure Predictions of AlphaFold2 with Low Confidence: Disorder and Beyond.* Biomolecules 12(10), 1467 (2022). DOI `10.3390/biom12101467`. PMID 36291675.
- **Temuan kunci:** pLDDT "telah ditunjukkan memberikan ukuran yang baik untuk disorder per-residu"; analisis fokus pada segmen foldable panjang yang ber-confidence rendah di 21 proteom referensi (AFDB v1).
- **Relevansi:** pLDDT rendah **tidak selalu** berarti "prediksi salah" — sering berarti disorder; interpretasi error harus memisahkan kedua hal ini.
- **Batas:** berbasis AFDB v1.

### [L11] Kwon et al. 2021 — Estimasi akurasi model di CASP14
- **Sitasi:** Kwon S, Won J, Kryshtafovych A, Seok C. *Assessment of protein model structure accuracy estimation in CASP14: Old and new challenges.* Proteins 89(12), 1940–1948 (2021). DOI `10.1002/prot.26192`. PMID 34324227.
- **Temuan kunci:** estimasi akurasi dievaluasi untuk akurasi struktur **global dan lokal**; metode single-model terbaik (grup Baker) meningkat terutama untuk estimasi akurasi **global** dibanding CASP sebelumnya.
- **Relevansi:** estimasi akurasi global (yang relevan dengan PAE/domain) secara historis **lebih sulit** daripada lokal — konsisten dengan [L7].
- **Batas:** angka per-metode tidak di abstrak.

### [L12] Roney & Ovchinnikov 2022 — AF2 sebagai estimator akurasi
- **Sitasi:** Roney JP, Ovchinnikov S. *State-of-the-Art Estimation of Protein Model Accuracy Using AlphaFold.* Physical Review Letters 129, 238101 (2022). DOI `10.1103/PhysRevLett.129.238101`. PMID 36563190.
- **Temuan kunci:** bukti bahwa AlphaFold mempelajari fungsi energi biophysical perkiraan; fungsi energi ini dapat **meranking kualitas struktur kandidat dengan akurasi state-of-the-art, tanpa data coevolution**; pLDDT terkait dengan properti energi model.
- **Relevansi:** dasar teoritis mengapa pLDDT informatif tentang kesalahan fisik struktur — tetapi juga penanda bahwa pLDDT **bukan probabilitas** dan bukan estimasi error terkalibrasi; ia sinyal energi.
- **Batas:** tidak ada angka kalibrasi di abstrak.

### [L13] Hiranuma et al. 2021 — DeepAccNet
- **Sitasi:** Hiranuma N, Park H, Baek M, Anishchenko I, Dauparas J, Baker D. *Improved protein structure refinement guided by deep learning based accuracy estimation.* Nature Communications 12, 1340 (2021). DOI `10.1038/s41467-021-21511-x`. PMID 33637700.
- **Temuan kunci:** DeepAccNet memperkirakan akurasi per-residu **dan signed residue–residue distance error**; prediksi akurasinya berkorelasi dengan resolusi struktur X-ray/cryo-EM di PDB.
- **Relevansi:** preseden metode: error struktural dapat diestimasi per-residu; namun estimasi ini tetap prediksi model, bukan pengukuran — memperkuat kebutuhan ground truth eksperimen di RQ kita.
- **Batas:** bukan paper kalibrasi pLDDT.

### [L14] Hekkelman et al. 2023 — AlphaFill: model AFDB kehilangan ligan
- **Sitasi:** Hekkelman ML, de Vries I, Joosten RP, Perrakis A. *AlphaFill: enriching AlphaFold models with ligands and cofactors.* Nature Methods 20, 205–213 (2023). DOI `10.1038/s41592-022-01685-y`. PMID 36424442.
- **Temuan kunci (kutipan abstrak):** "*all lack coordinates for small molecules*": hemoglobin tanpa heme; zinc-finger tanpa Zn; metaloprotease tanpa ion logam; ATPase/kinase tanpa ADP/ATP.
- **Relevansi:** **rujukan langsung** untuk concern C12 dataset lokal (heme/Zn/kromofor tidak dimodelkan) — ini karakteristik sistem AFDB yang diketahui literature, bukan kecelakaan proyek. Implikasi ke protokol: analisis pocket/ligan harus memisahkan backbone pocket dari ligan.
- **Batas:** AlphaFill fokus transplantasi ligan; tidak mengaudit akurasi geometri pocket.

### [L15] Meller et al. 2023 — Pocket kriptik & post-cutoff
- **Sitasi:** Meller A, Bhakat S, Solieva S, Bowman GR. *Accelerating Cryptic Pocket Discovery Using AlphaFold.* Journal of Chemical Theory and Computation 19(14), 4355–4363 (2023). DOI `10.1021/acs.jctc.2c01189`. PMID 36948209.
- **Temuan kunci:** dari 10 contoh pocket kriptik — **termasuk 5 yang di-deposit setelah data latih AlphaFold diekstrak dari PDB** — AF2 menyampel state terbuka pada 6/10 kasus; banyak pocket kriptik tetap sulit.
- **Relevansi:** (a) contoh desain benchmark dengan pemisahan **post-training-cutoff** — pola yang akan dipakai protokol kita; (b) batas AF2 untuk kantong konformasional.
- **Batas:** sampel kecil (10), setting MD-seeded.

### [L16] Karelina et al. 2023 — Pocket structure vs pose docking (pembeda penting)
- **Sitasi:** Karelina M, Noh JJ, Dror RO. *How accurately can one predict drug binding modes using AlphaFold models?* eLife 12, RP89386 (2023). DOI `10.7554/eLife.89386`. PMID 38131311.
- **Temuan kunci (dari abstrak):** pada GPCR — struktur **pocket** di model AF2 jauh lebih akurat daripada homology model (error hampir sekecil perbedaan antar-struktur eksperimen protein sama dengan ligan berbeda); **tetapi** akurasi **pose ligan** dari docking ke model AF2 **tidak signifikan lebih baik** daripada docking ke homology model, dan jauh lebih rendah daripada docking ke struktur eksperimen (apo).
- **Relevansi:** pemisahan tajam "akurasi pocket (backbone)" vs "akurasi pose ligan" — protokol kita harus memisahkan dua level ini secara eksplisit, dan mencatat bahwa level pose **tidak** dapat dievaluasi dari model AFDB tanpa ligan.
- **Batas:** fokus GPCR.

---

## 4. TOPIK 4 — Metrik Perbandingan Struktur

**Pertanyaan topik:** Definisi, sifat, dan batasan metrik yang akan dipakai.

### [L17] Zhang & Skolnick 2004 — TM-score
- **Sitasi:** Zhang Y, Skolnick J. *Scoring function for automated assessment of protein structure template quality.* Proteins 57(4), 702–710 (2004). DOI `10.1002/prot.20264`. PMID 15476259.
- **Temuan kunci:** TM-score menghilangkan ketergantungan ukuran protein (size dependence) dengan skala bergantung-ukuran; mengevaluasi **semua** pasangan residu pada alignment (bukan hanya yang di bawah cutoff).
- **Relevansi:** metrik global utama protokol; nilai ≈0 ("random") dan >0.5 ("fold mirip") membantu interpretasi.
- **Batas:** cutoff interpretasi (0.5) adalah konvensi komunitas — dirujuk ke [L18] di mana ia dipakai.

### [L18] Zhang & Skolnick 2005 — TM-align
- **Sitasi:** Zhang Y, Skolnick J. *TM-align: a protein structure alignment algorithm based on the TM-score.* Nucleic Acids Research 33(7), 2302–2309 (2005). DOI `10.1093/nar/gki524`. PMID 15849316.
- **Temuan kunci:** algoritma alignment berbasis TM-score + dynamic programming; ~4× lebih cepat dari CE; dipakai all-against-all PDB (10.515 chain); threshold TM-score 0.5 → 1996 fold.
- **Relevansi:** tool referensi penghitung TM-score (sertakan versi yang dikunci di implementasi).
- **Batas:** tool belum terpasang di lingkungan lokal (bukti audit F/§3.4 baseline).

### [L19] Mariani et al. 2013 — lDDT
- **Sitasi:** Mariani V, Biasini M, Barbato A, Schwede T. *lDDT: a local superposition-free score for comparing protein structures and models using distance difference tests.* Bioinformatics 29(21), 2724–2730 (2013). DOI `10.1093/bioinformatics/btt473`. PMID 23986568.
- **Temuan kunci (kutipan abstrak):** lDDT adalah skor **tanpa superposisi** yang mengevaluasi perbedaan jarak lokal semua atom; **metrik berbasis superposisi Cα global "strongly influenced by domain motions and do not assess … local atomic details"**. Referensi bisa berupa struktur tunggal **atau ensemble**.
- **Relevansi:** metrik lokal utama; juga penjelasan mengapa RMSD/TM global dan lDDT dapat berbeda arah — dasar desain analisis dua level (protokol §C–D). dukungan ensemble = penting untuk NMR/multiple-model.
- **Batas:** definisi threshold inklusi jarak ada di full text (tidak dikutip).

### [L20] Zemla 2003 — LGA / fondasi GDT
- **Sitasi:** Zemla A. *LGA: A method for finding 3D similarities in protein structures.* Nucleic Acids Research 31(13), 3370–3374 (2003). DOI `10.1093/nar/gkg571`. PMID 12824330.
- **Temuan kunci:** metodologi Local–Global Alignment; dasar dari GDT_TS yang dipakai bertahun-tahun di CASP.
- **Relevansi:** latar historis metrik CASP (GDT_TS); protokol kita memilih TM-score+lDDT sebagai utama, GDT sebagai pelengkap konteks literatur.
- **Batas:** angka GDT tidak di abstrak.

### [L21] Basu & Wallner 2016 — DockQ
- **Sitasi:** Basu S, Wallner B. *DockQ: A Quality Measure for Protein-Protein Docking Models.* PLoS ONE 11(8), e0161879 (2016). DOI `10.1371/journal.pone.0161879`. PMID 27560519.
- **Temuan kunci:** menyatukan tiga measure CAPRI (Fnat, LRMS, iRMS) → satu skor kontinu; contoh abstrak: model dengan LRMS>10 Å masih bisa "acceptable" dengan Fnat>0.50 & iRMS<3.0 Å.
- **Relevansi:** metrik interaksi bila fase lanjutan memasukkan protein kompleks; juga contoh bahwa kesimpulan kualitas bergantung metrik.
- **Batas:** —.

### [L22] Méndez et al. 2003 — Kriteria CAPRI (asal Fnat/LRMS/iRMS)
- **Sitasi:** Méndez R, Leplae R, De Maria L, Wodak SJ. *Assessment of blind predictions of protein-protein interactions: current status of docking methods.* Proteins 52(1), 51–67 (2003). DOI `10.1002/prot.10393`. PMID 12784368.
- **Temuan kunci:** evaluasi blind pertama CAPRI; mendefinisikan tiga ukuran kualitas interaksi (cikal bakal kriteria CAPRI).
- **Relevansi:** definisi asal metrik interaksi yang dikutip di [L21].

### [L23] Kabsch 1976 — Rotasi optimal (dasar RMSD)
- **Sitasi:** Kabsch W. *A solution for the best rotation to relate two sets of vectors.* Acta Crystallographica Section A 32(5), 922–923 (1976). DOI `10.1107/S0567739476001873`. (Tidak terindeks PubMed/Europe PMC — diverifikasi via Crossref, 2026-10-11.)
- **Temuan kunci:** solusi rotasi optimal untuk dua set vektor (SVD).
- **Relevansi:** dasar algoritmik superposisi RMSD yang dipakai protokol; sitasi klasik tanpa PMID (diverifikasi Crossref).

---

## 5. TOPIK 5 — Bias Benchmark: Leakage, Kesamaan Sekuens, Tanggal Rilis, Konformasi, Struktur Eksperimen

**Pertanyaan topik:** Sumber-sumber bias yang harus dikendalikan dalam benchmark AF-vs-eksperimen.

### [L24] AlQuraishi 2019 — ProteinNet (split terinsulasi)
- **Sitasi:** AlQuraishi M. *ProteinNet: a standardized data set for machine learning of protein structure.* BMC Bioinformatics 20, 311 (2019). DOI `10.1186/s12859-019-2932-0`. PMID 31185886.
- **Temuan kunci (kutipan abstrak):** dataset protein untuk ML perlu "*insulated training/validation splits that account for deep but only weakly detectable homology across protein space*".
- **Relevansi:** dasar prinsip **pemisahan homolog** dalam benchmark — protokol kita mengadopsi prinsip ini (klaster homolog 30% sebagai unit statistik; lihat protokol §A, §D).
- **Batas:** ProteinNet menyelesaikan ini untuk data latih/validasi; untuk audit kita, prinsipnya diadaptasi (tidak ada training).

### [L25] Chakravarty & Porter 2022 — AF2 gagal pada fold-switching
- **Sitasi:** Chakravarty D, Porter LL. *AlphaFold2 fails to predict protein fold switching.* Protein Science 31(6), e4353 (2022). DOI `10.1002/pro.4353`. PMID 35634782.
- **Temuan kunci (dari abstrak):** diuji pada 98 protein fold-switching (≥2 struktur distinktif dan stabil): **94% prediksi AF2 hanya menangkap satu** dari struktur eksperimennya.
- **Relevansi:** bukti konformasional: terhadap protein yang secara genuine multi-state, satu prediksi = satu snapshot (sering salah state). Implikasi audit: definisi "ground truth" harus eksplisit soal state konformasi (apo/holo/state) — protokol §B.

### [L26] Chakravarty et al. 2024 — Memorization (sumber kunci untuk leakage)
- **Sitasi:** Chakravarty D, Schafer JW, Chen EA, Thole JF, Ronish LA, Lee M, Porter LL. *AlphaFold predictions of fold-switched conformations are driven by structure memorization.* Nature Communications 15, 6826 (2024). DOI `10.1038/s41467-024-51801-z`. PMID 39181864.
- **Metode:** >280.000 model dari beberapa implementasi AF2 **dan AF3** pada protein fold-switching.
- **Temuan kunci (dari abstrak):** AF "*is a weak predictor of fold switching*"; dan **sebagian keberhasilannya berasal dari memorization struktur data latih, bukan pembelajaran energetika protein**.
- **Relevansi:** bukti langsung fenomena memorization — mengapa audit kita **wajib memfilter/mencatat kesamaan dengan data latih**, karena keberhasilan pada target homolog data latih tidak membuktikan generalisasi.
- **Batas:** fokus fold-switching; implikasi ke protein biasa dipandang sebagai mekanisme umum (dicatat sebagai inferensi).

### [L27] Saldaño et al. 2022 — Diversitas konformasi (apo–holo)
- **Sitasi:** Saldaño T, Escobedo N, Marchetti J, et al. *Impact of protein conformational diversity on AlphaFold predictions.* Bioinformatics 38(22), 5025–5032 (2022). DOI `10.1093/bioinformatics/btac202`. PMID 35561203.
- **Metode:** koleksi terkurasi **pasangan konformer apo–holo**; memakai ColabFold.
- **Temuan kunci (dari abstrak):** paradigma ensemble menantang metode prediksi struktur karena model "should represent a set of conformers instead of single structures"; kinerja AF2 dievaluasi dalam paradigma ini.
- **Relevansi:** protokol harus menetapkan bagaimana memperlakukan pasangan apo/holo dari protein yang sama (protokol §B: pilih satu pasangan utama + analisis sensitivitas).

### [L28] Wayment-Steele et al. 2024 — AF-Cluster
- **Sitasi:** Wayment-Steele HK, Ojoawo A, Otten R, et al. *Predicting multiple conformations via sequence clustering and AlphaFold2.* Nature 625, 832–839 (2024). DOI `10.1038/s41586-023-06832-9`. PMID 37956700.
- **Temuan kunci:** clustering MSA membuat AF2 menyampel state alternatif protein metamorphic dengan confidence tinggi.
- **Relevansi:** mendukung interpretasi: confidence tinggi pada **satu** state bukan jaminan state itu yang relevan secara biologis — rekvanta untuk klaim "reliabel".

### [L29] del Alamo et al. 2022 — Transporter/reseptor multi-state
- **Sitasi:** Del Alamo D, Sala D, Mchaourab HS, Meiler J. *Sampling alternative conformational states of transporters and receptors with AlphaFold2.* eLife 11, e75751 (2022). DOI `10.7554/eLife.75751`. PMID 35238773.
- **Temuan kunci:** AF2 "didesain untuk memprediksi struktur statis" namun sampling/observasi varian MSA dapat menyingkap state alternatif (transporter inward/outward-facing).
- **Relevansi:** memperkuat §5 konformasional; juga metode observasi varian MSA yang dapat menginformasikan analisis PAE multi-state.

### [L30] Wlodawer et al. 2013 — Pitfalls kristalografi (bias "ground truth")
- **Sitasi:** Wlodawer A, Minor W, Dauter Z, Jaskolski M. *Protein crystallography for aspiring crystallographers or how to avoid pitfalls and traps in macromolecular structure determination.* The FEBS Journal 280(14), 3329–3347 (2013). DOI `10.1111/febs.12495`. PMID 24034303.
- **Temuan kunci (dari abstrak):** kristalografi "appears to be an almost routine (but still **far from being analytical**) technique"; banyak struktur ditentukan peneliti dengan pengalaman terbatas; menjabarkan jebakan umum penentuan struktur.
- **Relevansi:** **struktur eksperimen bukan kebenaran sempurna** — "ground truth" punya ketidakpastian (resolusi, B-factor, model bias). Protokol harus mencatat metadata kualitas struktur eksperimen dan menguji sensitivitas (protokol §B, §D; policy §6 B3/B6).

### [L31] Henzler-Wildman & Kern 2007 — Dinamika protein
- **Sitasi:** Henzler-Wildman K, Kern D. *Dynamic personalities of proteins.* Nature 450, 964–972 (2007). DOI `10.1038/nature06522`. PMID 18075575.
- **Temuan kunci:** fungsi protein ditentukan oleh karakter dinamiknya; struktur statis tidak cukup menggambarkan perilaku atom dalam ruang-waktu.
- **Relevansi:** dasar konseptual keterbatasan membandingkan dua snapshot statis (prediksi vs satu struktur eksperimen) — bias struktural mendasar yang diakui protokol.

### [L32] Ruff & Pappu 2021 — IDP & pLDDT rendah
- **Sitasi:** Ruff KM, Pappu RV. *AlphaFold and Implications for Intrinsically Disordered Proteins.* Journal of Molecular Biology 433(20), 167208 (2021). DOI `10.1016/j.jmb.2021.167208`. PMID 34418423.
- **Temuan kunci (dari abstrak):** region ber-confidence sangat rendah pada prediksi sebagian besar **tumpang-tindih dengan IDR**; >30% proteom diprediksi disordered.
- **Relevansi:** kategori error yang bukan "kesalahan" — dasar klasifikasi residu low-confidence dalam protokol (dipisahkan dari residu terstruktur yang salah).

### [L33] Burley et al. 2023 — RCSB PDB (sumber ground truth)
- **Sitasi:** Burley SK, Bhikadiya C, Bi C, et al. *RCSB Protein Data Bank (RCSB.org): delivery of experimentally-determined PDB structures alongside one million computed structure models of proteins from artificial intelligence/machine learning.* Nucleic Acids Research 51(D1), D488–D508 (2023). DOI `10.1093/nar/gkac1077`. PMID 36420884.
- **Temuan kunci:** RCSB PDB menyajikan struktur eksperimen + >1 juta computed models; portal & API untuk konsumen data.
- **Relevansi:** sumber ground truth resmi + jalur API (RCSB data API) untuk fase implementasi; konteks bahwa PDB kini bercampur struktur eksperimen & model.

**Catatan pemeriksa (jumlah struktur eksperimen vs sekuens):** [L1] menyatakan ~100.000 protein unik ber-struktur eksperimen vs miliaran sekuens; [L5] menyatakan hanya 17% residu proteom manusia tercakup struktur eksperimen sebelum AF. → Bias seleksi eksperimen (hanya protein yang bisa dikristalkan/di-imaging) adalah struktural, bukan kecelakaan. Tidak ada sumber terpisah yang dikutip untuk angka PDB terkini (di luar lingkup). 

---

## 6. TOPIK 6 — Studi yang Menghubungkan Confidence dengan Error Struktural

**Pertanyaan topik:** Apa bukti empiris bahwa pLDDT/PAE memprediksi error, dan di mana hubungan itu gagal?

| Sumber | Bukti hubungan confidence ↔ error | Arah |
|---|---|---|
| [L1] Jumper 2021 | pLDDT = prediksi lDDT-Cα lokal; dipakai sebagai confidence default CASP14 | Positif (lokal) |
| [L11] Kwon 2021 | Estimasi akurasi global historis lebih sulit daripada lokal; ada kemajuan CASP14 | Positif lemah (global) |
| [L12] Roney 2022 | Fungsi energi AF2 dapat meranking kualitas model tanpa MSA; pLDDT state-of-the-art sebagai estimator | Positif (ranking) |
| [L13] Hiranuma 2021 | Prediksi akurasi DeepAccNet berkorelasi dengan resolusi struktur eksperimen | Positif (korelasi resolusi) |
| [L7] Terwilliger 2024 | **Prediksi confidence sangat tinggi tetap dapat menyimpang dari peta eksperimen secara global (distorsi, orientasi domain) dan lokal** | **Batas atas — kegagalan terdokumentasi** |
| [L10] Bruley 2022 | pLDDT rendah ↔ disorder (rendah ≠ pasti salah) | Ambiguitas interpretasi |
| [L14] Hekkelman 2023 | Model AFDB tanpa ligan — kesetiaan kimia tidak ditangkap confidence | Batas cakupan |
| [L16] Karelina 2023 | Pocket backbone akurat; pose ligan tidak — confidence tidak menjamin level ligan | Batas cakupan |
| [L26] Chakravarty 2024 | Keberhasilan sebagian karena memorization — sukses pada homolog ≠ generalisasi | **Kontaminasi sinyal** |

**Sintesis untuk RQ (ditandai sebagai interpretasi audit, bukan kutipan):** literatur menunjukkan hubungan confidence↔error kuat di level lokal, lebih lemah di level global/domain, dan dapat dikacaukan oleh (a) disorder, (b) memorization, (c) cakupan model (tanpa ligan). Ini menjustifikasi desain audit yang **memisahkan level lokal/global** dan **mengontrol kesamaan data latih** — bukan sekadar menghitung korelasi agregat.

---

## 7. TOPIK 7 — Benchmark Sebelumnya yang Paling Dekat dengan RQ, dan Celah yang Tersisa

### [L34] Yin et al. 2022 — Benchmark kompleks (152 heterodimer)
- **Sitasi:** Yin R, Feng BY, Varshney A, Pierce BG. *Benchmarking AlphaFold for protein complex modeling reveals accuracy determinants.* Protein Science 31(8), e4379 (2022). DOI `10.1002/pro.4379`. PMID 35900023.
- **Temuan kunci:** dari 152 kompleks heterodimer: **43% punya model near-native** (kategori medium/high CAPRI) sebagai prediksi top-ranked AF(2) — jauh di atas docking unbound (9% success rate). AF2 gagal pada kompleks antibody–antigen; AF-Multimer juga rendah pada antibody–antigen (11%) dan TCR–antigen.
- **Relevansi:** benchmark kompleks terdekat; **celah**: fokus interaksi, bukan kalibrasi pLDDT; antibody/immune adalah kegagalan sistematis.

### [L35] Wong et al. 2022 — Benchmark docking berbasis AF2 (antibiotik)
- **Sitasi:** Wong F, Krishnan A, Zheng EJ, et al. *Benchmarking AlphaFold-enabled molecular docking predictions for antibiotic discovery.* Molecular Systems Biology 18(9), e11081 (2022). DOI `10.15252/msb.202211081`. PMID 36065847.
- **Temuan kunci:** 296 protein proteom esensial E. coli × 218 senyawa aktif + 100 inaktif; **auROC rata-rata 0,48** (≈ tebak acak) dengan docking directly to AF2; rescoring ML menaikkan menjadi ≤0,63.
- **Relevansi:** contoh bahwa struktur AF2 yang "akurat" belum otomatis berguna untuk aplikasi hilir — pemisahan akurasi struktural vs utilitas adalah tema yang harus dijaga protokol (RUANG LINGKUP: kita mengaudit akurasi struktural + kalibrasi confidence, bukan utility docking).

### [L36] Díaz-Rovira et al. 2023 — AF2 tanpa templat ≥30% ID
- **Sitasi:** Díaz-Rovira AM, Martín H, Beuming T, Díaz L, Guallar V, Ray SS. *Are Deep Learning Structural Models Sufficiently Accurate for Virtual Screening? Application of Docking Algorithms to AlphaFold2 Predicted Structures.* Journal of Chemical Information and Modeling 63(6), 1668–1674 (2023). DOI `10.1021/acs.jcim.2c01270`. PMID 36892986.
- **Temuan kunci:** versi AF2 dengan **eksklusi template >30% identitas** dibangun; kesimpulan: model AF2 out-of-the-box bukan skenario ideal untuk virtual screening; perlu post-processing ke arah holo.
- **Relevansi:** preseden teknis **protokol "tanpa template homolog"** — sejalan dengan strategi kontrol leakage kita; juga menegaskan beda akurasi jaringan vs kelayakan aplikasi.

### [L37] Buttenschoen et al. 2024 — PoseBusters
- **Sitasi:** Buttenschoen M, Morris GM, Deane CM. *PoseBusters: AI-based docking methods fail to generate physically valid poses or generalise to novel sequences.* Chemical Science 15, 3130–3139 (2024). DOI `10.1039/D3SC04185A`. PMID 38425520.
- **Temuan kunci:** RMSD terhadap native **tidak cukup**: metode deep learning sering menghasilkan struktur yang secara fisik tidak masuk akal; evaluasi harus memuat kriteria sterik/energetik; generalisasi ke sekuens baru bermasalah.
- **Relevansi:** pelajaran evaluasi: RMSD/metrik tunggal menyesatkan; protokol kita memakai pasangan metrik global+lokal+confidence, dengan multiple-testing awareness (protokol §D).

### [L38] Tsaban et al. 2022 — Peptide docking: memorized vs learned
- **Sitasi:** Tsaban T, Varga JK, Avraham O, Ben-Aharon Z, Khramushin A, Schueler-Furman O. *Harnessing protein folding neural networks for peptide-protein docking.* Nature Communications 13, 176 (2022). DOI `10.1038/s41467-021-27838-9`. PMID 35013344.
- **Temuan kunci:** AF2 dapat memodelkan interaksi peptida–protein; penulis "explore what AlphaFold2 has memorized and learned" dan menggambarkan kasus spesifik.
- **Relevansi:** memperkuat tema memorization dari sudut aplikasi.

### [L15], [L27], [L25], [L26] — konformasi & memorization (sudah dikutip): baris-baris ini juga merupakan "prior work" terdekat pada sumbu konformasional.

### Celah yang tersisa (penilaian audit, dari sintesis [L1]–[L38]):

1. **Benchmark blind CASP ([L2]–[L4])** mengevaluasi sekali per 2 tahun, target terkurasi, bukan distribusi AFDB sehari-hari; audit populasi AFDB-vs-PDB tidak blind, tetapi skalabel.
2. **Benchmark besar yang ada** berfokus pada *aplikasi hilir* ([L34] kompleks, [L35]/[L36]/[L16] docking) atau *satu sumbu akurasi* ([L7] peta eksperimen; [L11] estimasi akurasi) — belum ada (menurut literatur yang dibaca) audit sistematis **kalibrasi pLDDT/PAE terhadap error terukur pada set yang dikontrol leakage & konformasi**, dengan pemisahan lokal/global eksplisit — inilah niche RQ ini.
3. **Kejujuran kontribusi:** celah ini bersifat *replikasi-terkontrol + audit kalibrasi*, bukan metode baru. Nilai = protokol terbuka + audit bias + angka kalibrasi pada set baru. Klaim "pertama di dunia" TIDAK dibuat.

---

## 8. Log Verifikasi Sumber (40 sumber)

Legenda: **EPMC** = Europe PMC (metadata+abstrak); **CR** = Crossref (metadata). Semua diverifikasi 2026-10-11.

| ID | First author, tahun | Jurnal | DOI | ID alternatif | Verifikasi | Status |
|---|---|---|---|---|---|---|
| L1 | Jumper 2021 | Nature | 10.1038/s41586-021-03819-2 | PMID 34265844 | EPMC | OK |
| L2 | Pereira 2021 | Proteins | 10.1002/prot.26171 | PMID 34218458 | EPMC (koreksi DOI) | OK |
| L3 | Kryshtafovych 2021 | Proteins | 10.1002/prot.26237 | PMID 34533838 | EPMC | OK |
| L4 | Kryshtafovych 2023 | Proteins | 10.1002/prot.26617 | PMID 37920879 | EPMC | OK |
| L5 | Tunyasuvunakool 2021 | Nature | 10.1038/s41586-021-03828-1 | PMID 34293799 | EPMC | OK |
| L6 | Abramson 2024 | Nature | 10.1038/s41586-024-07487-w | PMID 38718835 | EPMC | OK |
| L7 | Terwilliger 2024 | Nat Methods | 10.1038/s41592-023-02087-4 | PMID 38036854 | EPMC | OK |
| L8 | Akdel 2022 | NSMB | 10.1038/s41594-022-00849-w | PMID 36344848 | EPMC | OK |
| L9 | Guo 2022 | Sci Rep | 10.1038/s41598-022-14382-9 | PMID 35739160 | EPMC | OK |
| L10 | Bruley 2022 | Biomolecules | 10.3390/biom12101467 | PMID 36291675 | EPMC | OK |
| L11 | Kwon 2021 | Proteins | 10.1002/prot.26192 | PMID 34324227 | EPMC (koreksi DOI) | OK |
| L12 | Roney 2022 | PRL | 10.1103/PhysRevLett.129.238101 | PMID 36563190 | EPMC | OK |
| L13 | Hiranuma 2021 | Nat Commun | 10.1038/s41467-021-21511-x | PMID 33637700 | EPMC (koreksi DOI) | OK |
| L14 | Hekkelman 2023 | Nat Methods | 10.1038/s41592-022-01685-y | PMID 36424442 | EPMC | OK |
| L15 | Meller 2023 | JCTC | 10.1021/acs.jctc.2c01189 | PMID 36948209 | EPMC | OK |
| L16 | Karelina 2023 | eLife | 10.7554/eLife.89386 | PMID 38131311 | EPMC | OK |
| L17 | Zhang 2004 | Proteins | 10.1002/prot.20264 | PMID 15476259 | EPMC | OK |
| L18 | Zhang 2005 | NAR | 10.1093/nar/gki524 | PMID 15849316 | EPMC | OK |
| L19 | Mariani 2013 | Bioinformatics | 10.1093/bioinformatics/btt473 | PMID 23986568 | EPMC | OK |
| L20 | Zemla 2003 | NAR | 10.1093/nar/gkg571 | PMID 12824330 | EPMC | OK |
| L21 | Basu 2016 | PLoS ONE | 10.1371/journal.pone.0161879 | PMID 27560519 | EPMC | OK |
| L22 | Méndez 2003 | Proteins | 10.1002/prot.10393 | PMID 12784368 | EPMC (koreksi DOI) | OK |
| L23 | Kabsch 1976 | Acta Cryst A | 10.1107/S0567739476001873 | — (non-PubMed) | CR | OK |
| L24 | AlQuraishi 2019 | BMC Bioinf | 10.1186/s12859-019-2932-0 | PMID 31185886 | EPMC (koreksi DOI) | OK |
| L25 | Chakravarty 2022 | Prot Sci | 10.1002/pro.4353 | PMID 35634782 | EPMC | OK |
| L26 | Chakravarty 2024 | Nat Commun | 10.1038/s41467-024-51801-z | PMID 39181864 | EPMC | OK |
| L27 | Saldaño 2022 | Bioinformatics | 10.1093/bioinformatics/btac202 | PMID 35561203 | EPMC | OK |
| L28 | Wayment-Steele 2024 | Nature | 10.1038/s41586-023-06832-9 | PMID 37956700 | EPMC | OK |
| L29 | del Alamo 2022 | eLife | 10.7554/eLife.75751 | PMID 35238773 | EPMC | OK |
| L30 | Wlodawer 2013 | FEBS J | 10.1111/febs.12495 | PMID 24034303 | EPMC | OK |
| L31 | Henzler-Wildman 2007 | Nature | 10.1038/nature06522 | PMID 18075575 | EPMC | OK |
| L32 | Ruff 2021 | JMB | 10.1016/j.jmb.2021.167208 | PMID 34418423 | EPMC | OK |
| L33 | Burley 2023 | NAR | 10.1093/nar/gkac1077 | PMID 36420884 | EPMC | OK |
| L34 | Yin 2022 | Prot Sci | 10.1002/pro.4379 | PMID 35900023 | EPMC | OK |
| L35 | Wong 2022 | MSB | 10.15252/msb.202211081 | PMID 36065847 | EPMC | OK |
| L36 | Díaz-Rovira 2023 | JCIM | 10.1021/acs.jcim.2c01270 | PMID 36892986 | EPMC | OK |
| L37 | Buttenschoen 2024 | Chem Sci | 10.1039/D3SC04185A | PMID 38425520 | EPMC | OK |
| L38 | Tsaban 2022 | Nat Commun | 10.1038/s41467-021-27838-9 | PMID 35013344 | EPMC | OK |
| L39 | Varadi 2022 | NAR | 10.1093/nar/gkab1061 | PMID 34791371 | EPMC | OK |
| L40 | Varadi 2024 | NAR | 10.1093/nar/gkad1011 | PMID 37933859 | EPMC | OK |

### [L39] Varadi et al. 2022 & [L40] Varadi et al. 2024 — Sumber data AFDB (rujukan dataset)
- **L39:** Varadi M, Anyango S, Deshpande M, et al. *AlphaFold Protein Structure Database: massively expanding the structural coverage of protein-sequence space with high-accuracy models.* NAR 50(D1), D439–D444 (2022). DOI `10.1093/nar/gkab1061`. PMID 34791371. Release awal >360.000 struktur, 21 proteom; menyediakan koordinat + confidence per-residu + PAE.
- **L40:** Varadi M, Bertoni D, Magana P, et al. *AlphaFold Protein Structure Database in 2024: providing structure coverage for over 214 million protein sequences.* NAR 52(D1), D368–D375 (2024). DOI `10.1093/nar/gkad1011`. PMID 37933859. Database bertumbuh ke >214 juta sekuens; integrasi ke PDB, UniProt, Ensembl, InterPro, MobiDB.
- **Relevansi:** mendefinisikan sumber prediksi audit (AFDB) + sitasi wajib saat data dipakai (lihat `DATA_AND_BIAS_POLICY.md` §4).

---

## 9. Batasan Literature Review Ini

1. **Level abstrak, bukan full text.** Angka/kutipan di luar abstrak tidak dipakai. Kalimat [L7] yang terpotong harus diverifikasi di full text sebelum dikutip persis dalam publikasi.
2. **Satu kanal pencarian** (Europe PMC; Crossref untuk L23). Kemungkinan ada paper relevan yang tidak terindeks/terlewat — daftar ini tidak diklaim sistematis (bukan PRISMA).
3. **Tanggal berhenti:** 2026-10-11. Paper setelah tanggal ini tidak masuk.
4. **Tidak ada akses berbayar** — hanya metadata/abstrak terbuka.
5. **"Celah riset" (§7) adalah penilaian editorial** dari sintesis sumber yang dibaca, bukan klaim dari sumber mana pun; dinyatakan sebagai penilaian, bukan kutipan.

---

*Semua 40 sumber di dokumen ini disimpan dengan DOI + PMID terverifikasi pada 2026-10-11. Dokumen fase berikutnya (protokol, kebijakan data) dilarang memakai referensi di luar daftar ini tanpa melalui verifikasi yang sama.*
