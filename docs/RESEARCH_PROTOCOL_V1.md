# RESEARCH PROTOCOL V1 — AlphaFold Reliability Audit

## Protokol Riset Pra-Registrasi: Kalibrasi pLDDT/PAE terhadap Error Struktural Terukur (Prediksi AlphaFold DB vs Struktur Eksperimen PDB)

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Versi** | V1 (pra-registrasi) |
| **Tanggal** | 2026-10-11 |
| **Status** | DRAF FINAL untuk review user — belum ada eksekusi apa pun |
| **Dasar** | `docs/PHASE1_BASELINE_AUDIT.md`, `docs/LITERATURE_REVIEW.md` (40 sumber [L1]–[L40]), `docs/DATA_AND_BIAS_POLICY.md` |
| **Prinsip** | Semua keputusan metodologis yang memengaruhi hasil ditetapkan **sebelum** data hasil dilihat. Perubahan setelah data dilihat = amendment tercatat (§H.3). |

---

## 0. Pertanyaan Riset (RQ) & Cakupan

### 0.1 RQ utama (dari prompt user, tidak diubah)

> **Seberapa andal pLDDT dan PAE sebagai indikator akurasi struktural model AlphaFold, diukur terhadap struktur eksperimen, dan seberapa besar gap antara akurasi prediksi dan struktur eksperimen?**

### 0.2 Operasionalisasi menjadi 4 sub-pertanyaan primer

| # | Sub-RQ | Level analisis | Endpoint utama |
|---|---|---|---|
| **RQ1** | Apakah pLDDT memprediksi error lokal terukur? Berapa distribusi error lokal di setiap bin pLDDT? | Per-residu (dalam protein) | Error lokal per-residu (C.5) di bin pLDDT (D.3) |
| **RQ2** | Apakah pLDDT tinggi menjamin kesesuaian global (TM-score/RMSD) — dan di mana ia gagal walau tinggi? | Per-protein | TM-score & RMSD Cα (C.1–C.2) vs mean pLDDT |
| **RQ3** | Apakah PAE cross-domain memprediksi error orientasi domain relatif? | Per-protein multidomain | Cross-PAE mean vs error orientasi domain (C.7, C.8) |
| **RQ4** | Berapa frekuensi dan bentuk "kegagalan high-confidence" (residu pLDDT≥90 yang menyimpang > 2 Å lokal)? | Per-residu terkondisi | Proporsi residu pLDDT≥90 dengan error lokal > 2 Å (C.5, C.9) |

### 0.3 Hipotesis kerja (arah yang DIHARAPKAN dari literatur — bukan hasil)

- **H1** (dari [L1], [L11], [L12], [L13]): pLDDT ↔ error lokal berkorelasi kuat negatif (makin tinggi pLDDT, makin kecil error). Arah: **didukung kuat**.
- **H2** (dari [L7], [L11], [L8]): korelasi pLDDT ↔ metrik global lebih lemah/heterogen daripada lokal; error orientasi domain dapat muncul pada confidence sangat tinggi. Arah: **didukung, magnitude tidak diketahui**.
- **H3** (dari [L7], [L26]): "kegagalan high-confidence" ada (frekuensi > 0) — akan **diukur**, bukan diasumsikan.
- **H4** (dari [L10], [L32]): sebagian besar pLDDT rendah = disorder/fleksibilitas, bukan "error" — harus dipisahkan sebelum menyimpulkan "pLDDT tidak reliabel".

Jika hasil bertentangan dengan arah ini, hasil dilaporkan apa adanya (komitmen pelaporan null/negatif, §G.5).

### 0.4 Ruang lingkup

- **DALAM lingkup:** protein single-chain (monomer) entri AlphaFold DB versi default (F1); perbandingan terhadap struktur eksperimen PDB; metrik global + lokal; analisis domain (untuk protein multidomain); analisis kalibrasi confidence.
- **LUAR lingkup (V1):** kompleks/interface (DockQ [L21] — definisi Tier-2 di §C.12, tidak dieksekusi di V1); pocket/ligan sebagai endpoint (§C.13 — ditunda dengan alasan eksplisit); AF3 [L6]; AlphaFold Server (peran tetap spesimen pembelajaran, DATASET_PLAN §2); training ML apa pun.
- **Alasan pembatasan monomer:** konsisten dengan pipeline lokal yang sudah ada (7 protein monomer, AFDB F1); pose ligan tidak dapat dievaluasi dari AFDB tanpa ligan [L14]; [L16] menunjukkan pose ≠ pocket, dan pocket saja tidak memadai untuk klaim binding.

### 0.5 Definisi istilah kunci

| Istilah | Definisi operasional di protokol ini |
|---|---|
| **Prediksi** | Model struktur dari AlphaFold DB (v6 atau versi yang dicatat saat unduh), 1 model / entri. |
| **Referensi (ground truth)** | Satu struktur eksperimen PDB terpilih (§B), bukan ensemble (kecuali uji sensitivitas). |
| **Error lokal** | Deviasi Cα per-residu setelah superposisi optimal domain (C.5) + kontribusi lDDT per-residu (C.3). |
| **Error global** | TM-score & RMSD Cα full-length (C.1–C.2). |
| **Error orientasi domain** | RMSD domain B setelah superposisi domain A sendiri (C.8). |
| **High-confidence failure** | Residu pLDDT ≥ 90 dengan error lokal > 2,0 Å (C.9). |
| **Cutoff data latih** | Tanggal batas PDB yang dipakai melatih AF2 — **nilai definitif wajib diverifikasi dari full text [L1] sebelum implementasi** (default sementara 2018-04-30, ditandai TBD; policy §5). |
| **Klaster homolog** | Sekelompok protein dengan identitas sekuens berpasangan > 30% (unit statistik; [L24]). |

---

## A. Populasi, Kriteria Inklusi/Eksklusi, Unit Analisis

### A.1 Populasi target

Protein monomer yang: (a) punya entri AlphaFold DB (single-chain, F1), dan (b) punya ≥1 struktur eksperimen PDB yang dapat dipetakan tanpa ambiguitas ke sekuens model.

### A.2 Kriteria inklusi (semua wajib terpenuhi)

| # | Kriteria | Ambang | Justifikasi/Asal |
|---|---|---|---|
| A2.1 | Entri AFDB single-chain (`isComplex=false`), versi default | — | Konsisten dengan dataset lokal (DATASET_PLAN §3) |
| A2.2 | Panjang sekuens | 50–600 aa | Konsisten batas validasi pipeline lokal (DATASET_PLAN §5: 20–600) dengan margin bawah realistis |
| A2.3 | Pemetaan UniProt→PDB tersedia (SIFTS/PDBe + RCSB) | ≥1 hit | Prosedur §B |
| A2.4 | Coverage alignment model↔PDB chain | ≥80% residu model | Sisanya di-exclude & dilaporkan (missing report) |
| A2.5 | Identitas sekuens pada region aligned | 100% (wild-type) | Model AFDB = sekuens UniProt canonical; PDB harus wild-type pada region terkait. Mutan/engineered variant tidak masuk V1 |
| A2.6 | Metode & resolusi eksperimen | X-ray atau cryo-EM ≤3,0 Å; NMR masuk hanya sebagai strata terpisah | Kualitas ground truth bervariasi [L30]; resolusi dicatat per protein |
| A2.7 | Tidak ada chain break model-side | — | Model AFDB selalu kontinu; pengecualian hanya PDB-side (residu missing di-exclude, dicatat) |

### A.3 Kriteria eksklusi (dicatat satu per satu dengan alasan — tanpa silent filtering)

| # | Eksklusi | Alasan |
|---|---|---|
| A3.1 | Multi-chain/complex | Luar lingkup V1 (§0.4) |
| A3.2 | PDB region = mutan, fusion tag besar, atau konstruksi non-wild-type > 5 residu di luar termini | Perbandingan sekuens tercemar |
| A3.3 | Coverage <80% (fragment, domain tunggal dari protein multi-domain) | Metrik global tidak bermakna |
| A3.4 | >30 residu internal missing di PDB tanpa penjelasan | Alignment tidak terpercaya |
| A3.5 | Entri PDB yang ditarik/digantikan (obsolete/superseded) | Ground truth tidak stabil |
| A3.6 | Protein yang mayoritas IDR (definisi: >70% residu di region disorder eksperimen/missing) | Bukan objek validasi struktur [L31, L32]; dimasukkan hanya sebagai studi kasus terpisah bila menarik |

### A.4 Unit analisis & independensi

- **Unit sampling & statistik = protein** (bukan residu). Alasan epistemik: residu dalam satu protein tidak independen (klaim F `PROJECT_STATUS.md` §12; concern C3).
- **Klaster homolog (≤30% → berbagi klaster)**: protein-protein dalam satu klaster tidak dihitung sebagai bukti independen penuh; semua CI & uji lintas-protein memakai **cluster-robust bootstrap** (D.2). Rasional: [L24], [L26] — kemiripan → memorization → dependensi.
- **Stratifikasi sampling** (target desain, bukan hasil): pastikan bin pLDDT rendah (mean pLDDT < 70, < 50), panjang, metode eksperimen, dan tanggal rilis (pre/post-cutoff) **terisi** — menutup kritik bahwa set lokal lama homogen (mean pLDDT 85–98; baseline §7).

---

## B. Prosedur Pencocokan Model ↔ Struktur Eksperimen

### B.1 Rantai prosedur (dijalankan per kandidat protein; semua langkah tercatat di manifest)

1. **Ambil entri AFDB** (accession UniProt → entri F1): model CIF + PAE JSON. Catat `model_version` (v6 dst.) + SHA256 file. (Alur sama seperti Stage 5 lokal; folder baru di root audit — bukan di `ai-biotech`.)
2. **Ambil kandidat PDB**: mapping resmi UniProt→PDB (PDBe/SIFTS API), diperkaya metadata RCSB (metode, resolusi, R-free, tanggal rilis, ligan). [L33]
3. **Filter kandidat** dengan §A.2–A.3; urutkan: (1) coverage alignment tertinggi, (2) resolusi terbaik, (3) paling sedikit residu missing. **Pilih 1 struktur primer**; kandidat lain disimpan untuk uji sensitivitas S1–S4 (§D.6).
4. **Alignment sekuens** model↔PDB chain (aligner global, tool dikunci di implementasi): verifikasi A2.4–A2.5; hasilkan tabel mapping residu↔residu (disimpan sebagai artifact).
5. **Klasifikasi tanggal rilis**: `pre_cutoff` / `post_cutoff` (cutoff TBD §0.5 — **tidak boleh diisi dari ingatan; verifikasi ke [L1] full text, dicatat di amendment log**). Pola rujukan: [L15] memakai pemisahan post-training-extraction.
6. **Klasifikasi state konformasi**: catat ligan/HETATM PDB (ada/tidak) → label `holo`/`apo`; catat pula oligomeric state kristal vs model. Penanganan: struktur primer = yang state biologisnya paling netral (default: apo; jika hanya holo → catat sebagai keterbatasan per protein). Uji sensitivitas membandingkan model-vs-apo vs model-vs-holo untuk mengukur "biaya konformasi" [L27].
7. **Simpan manifest**: 1 baris per protein: semua field di atas + jumlah residu matched/excluded + alasan eksklusi (bila kandidat gugur) + hash semua file. Format CSV deterministik (LF, UTF-8), tanpa timestamp di output data.

### B.2 Aturan anti-ambigu

- Jika dua struktur PDB sama-sama memenuhi syarat dengan skor pemeringkatan identik → **pilih deterministik** (mis. PDB ID leksikografis terkecil) dan catat; sensitivitas dijalankan terhadap yang lain.
- Jika alignment mengandung indel → residu di sekitar indel di-flag; metrik dihitung pada region colinear; jumlah residue di-flag dilaporkan.
- Tidak ada "perbaikan" manual struktur PDB (tidak ada modeling ulang residu missing).
- Semua keputusan penyaringan kandidat dicatat (tanggal, alasan) — dapat diaudit ulang.

---

## C. Definisi Metrik (rumus/lambang, unit, interpretasi)

Semua metrik dihitung antara **model AFDB** dan **struktur PDB primer** pada residu matched (§B.1 langkah 4).

| Kode | Metrik | Definisi operasional | Unit | Interpretasi |
|---|---|---|---|---|
| C.1 | **RMSD Cα** | RMSD posisi Cα setelah superposisi optimal Kabsch [L23], hanya residu matched | Å | Sensitif domain motion [L19]; angka kecil ≠ benar global |
| C.2 | **TM-score** | Skor TM [L17] via TM-align [L18] (versi tool dikunci) | 0–1 | > 0,5 ≈ fold sama ([L18] konvensi); length-normalized |
| C.3 | **lDDT** | Skor bebas-superposisi atas perbedaan jarak lokal semua atom [L19] (tool dikunci) | 0–1 | > 0,9 ≈ detail lokal sangat baik; tidak terpengaruh domain motion |
| C.4 | **GDT_TS** (pelengkap) | Berdasar LGA [L20] | 0–1 | Pelengkap konteks literatur CASP; bukan endpoint primer |
| C.5 | **Error lokal per-residu** | (a) jarak Cα residu i (model vs referensi) **setelah superposisi domain** (C.8); (b) kontribusi lDDT per-residu | Å / 0–1 | Inti RQ1/RQ4 |
| C.6 | **PAE intra-domain** | Mean blok PAE antar-residu dalam satu domain | Å | Konsistensi internal domain |
| C.7 | **PAE cross-domain** | Mean blok PAE antar dua domain berbeda | Å | Proxy orientasi relatif antar domain |
| C.8 | **Error orientasi domain** | Untuk protein multidomain: superposisi optimal domain A saja → RMSD domain B (posisi Cα) | Å | Inti RQ3; "relative domain displacement" |
| C.9 | **High-confidence failure** | residu: pLDDT ≥ 90 ∧ error lokal (C.5a) > 2,0 Å | proporsi | Inti RQ4; ambang 2,0 Å = konvensi backbone akurat |
| C.10 | **pLDDT** | Nilai confidence per-residu dari CIF AFDB (B-factor) | 0–100 | Model-side; catat granularitas AFDB (plddt_* redundan — C2 lokal, tidak relevan di sini karena hanya butuh 1 kanal) |
| C.11 | **pTM / ipTM** | **Tidak dipakai di V1** (tersedia hanya sebagai prediksi internal model; mengurangi independensi audit) | — | Dicatat sebagai keputusan |
| C.12 | **DockQ / kriteria CAPRI** | Fnat, LRMS, iRMS → DockQ [L21, L22] | 0–1 | **DITUNDA ke Tier-2** (butuh target kompleks — di luar lingkup monomer V1, §0.4) |
| C.13 | **Pocket / geometri sisi ikat** | Mis. pocket RMSD atau lDDT khusus residu pocket | Å / 0–1 | **DITUNDA — tidak dieksekusi di V1** karena model AFDB tidak memuat ligan/ion (B5; [L14]) dan akurasi pocket ≠ akurasi pose ligan ([L16]); dieksekusi hanya bila lingkup diperluas via amendment |

**Aturan umum:** semua definisi tool (nama, versi, flags default) **dikunci di file `TOOLS_LOCK.md` pada fase implementasi sebelum skor pertama dihitung**. Setiap tool harus menghasilkan output deterministik pada input sama (diuji).

---

## D. Rencana Statistik

### D.1 Dua level, tanpa pseudoreplikasi

- **Level dalam-protein (deskriptif + per-protein inference):** untuk tiap protein, dihitung distribusi error lokal (C.5a/b) per bin pLDDT (D.3). Statistika per protein: median, IQR, Spearman ρ antar residu (hanya deskriptif — residu tidak independen).
- **Level lintas-protein (unit klaim):** untuk tiap protein diringkas menjadi nilai tunggal per sel (mis. median error lokal pada bin pLDDT≥90). Uji/CI lintas-protein memakai ringkasan ini dengan **N = jumlah protein** (atau jumlah klaster, lihat D.2).

### D.2 Cluster-robust & bootstrap

- Bootstrap CI dihitung dengan **resampling pada level klaster homolog** (≥30% identity) — bukan per protein individual — untuk menghormati dependensi [L24].
- Untuk perbandingan bin pLDDT (paired per protein): uji Wilcoxon signed-rank antar ringkasan per protein + bootstrap CI median selisih.

### D.3 Bin pLDDT (pra-registrasi)

Bin: `[0,50)`, `[50,70)`, `[70,90)`, `[90,100]` (konvensi AF2 [L1, L5]). Untuk setiap bin dilaporkan: jumlah residu, jumlah protein yang menyumbang, median & IQR error lokal (C.5a), dan kontribusi lDDT (C.5b). **Tidak ada bin yang digabung/diubah setelah melihat hasil** — jika ada bin yang kosong/terlalu kecil, itu dilaporkan.

### D.4 Analisis primer (sesuai §0.2)

| Endpoint | Uji | Level | Catatan |
|---|---|---|---|
| RQ1: kalibrasi lokal | Ringkasan per-protein: median error lokal di tiap bin; uji perbandingan bin ≥90 vs 70–90 (Wilcoxon signed-rank, α=0,05 dua-sisi) + bootstrap CI | Klaster | Hipotesis arah: error(≥90) < error(70–90) [H1] |
| RQ2: global vs lokal | Scatter mean pLDDT vs TM-score (C.2) lintas protein; Spearman + CI; tabulasi protein TM>0,9 dengan error domain besar | Protein/klaster | **Deskriptif-deskriptif**; tidak ada klaim kausal |
| RQ3: PAE cross-domain | Scatter cross-PAE (C.7) vs error orientasi domain (C.8); Spearman + CI; hanya protein multidomain (≥2 domain, definisi §D.5) | Protein | Hipotesis arah: korelasi positif [H2/H3] |
| RQ4: high-confidence failure | Proporsi residu pLDDT≥90 dengan C.5a > 2 Å; CI Wilson per protein → agregasi klaster (bootstrap) | Klaster | Frekuensi dilaporkan apa adanya [H3] |

**Multiplicity:** 4 endpoint primer → koreksi **Holm** (α family = 0,05). Semua analisis lain (termasuk uji sensitivitas) berstatus **eksploratori** dan dilabeli demikian di pelaporan.

### D.5 Definisi domain (keputusan implementasi yang dikunci di `TOOLS_LOCK.md`)

Domain didefinisikan dari **struktur referensi** (bukan prediksi — menghindari sirkularitas) memakai: (prioritas 1) boundary domain dari UniProt/Pfam untuk protein terkait; (prioritas 2) segmentasi geometrik deterministik dari struktur referensi (metode dikunci; bukan pilihan termudah per protein). Protein dengan <2 domain → tidak masuk RQ3/analisis domain.

### D.6 Uji sensitivitas (pre-registered list, eksploratori)

| # | Faktor | Perbandingan |
|---|---|---|
| S1 | Metode eksperimen | X-ray vs cryo-EM vs NMR |
| S2 | Resolusi | ≤2,0 Å vs >2,0 Å |
| S3 | Tanggal rilis PDB | pre-cutoff vs post-cutoff (§0.5) |
| S4 | State konformasi | model-vs-apo vs model-vs-holo (jika keduanya tersedia) |
| S5 | Disordered/missing | dengan vs tanpa residu region disorder eksperimen |
| S6 | Panjang protein | 50–150 / 150–300 / 300–600 aa |
| S7 | Robustness metrik | ulangi RQ1/RQ2 memakai C.3 lDDT vs C.2 TM sebagai endpoint error |
| S8 | Struktur alternatif | kandidat PDB kedua (jika ada) sebagai referensi |

Semua sensitivitas dilaporkan termasuk yang tidak mengubah kesimpulan (bukan hanya yang berubah).

### D.7 Reproducibilitas statistik

- Semua script analisis: deterministik (seed tetap bila ada sampling; tanpa timestamp di output data).
- Dua run penuh pada set yang sama → output **byte-identical** (aturan proyek yang sudah terbukti di pipeline lama, `PROJECT_STATUS.md` §14) — gate QC untuk T1+ (§G.4).
- Environment dipisah dari `ai-biotech` (venv sendiri di root audit, dibangun pada fase implementasi setelah persetujuan — bukan sekarang).

---

## E. Bias & Keterbatasan (ringkas — register lengkap di `DATA_AND_BIAS_POLICY.md` §6)

| # | Bias | Efek ke RQ | Mitigasi di protokol |
|---|---|---|---|
| B1 | **Seleksi eksperimen** (hanya protein crystallizable/imaged) | Sample tidak mewakili proteom; error "mudah" ter-underestimate | Dinyatakan terbatas pada "protein yang punya struktur eksperimen"; tidak digeneralisasi ke proteom penuh |
| B2 | **Leakage/memorization** [L24, L26] | Keberhasilan pada homolog data latih ≠ generalisasi | Klaster 30% (§A.4); strata pre/post-cutoff (S3); pelaporan terpisah |
| B3 | **Ground truth tidak sempurna** [L30] | Error terukur sebagian milik referensi | Metadata kualitas (resolusi, R-free) + sensitivitas S1/S2; dilarang klaim "ground truth = kebenaran absolut" |
| B4 | **Konformasi tunggal** [L25, L27, L28, L29, L31] | Satu prediksi vs satu snapshot: perbedaan konformasi bisa salah dibaca sebagai error | S4 (apo/holo); pelaporan per-protein menyebut state; klaim dibatasi "terhadap state X" |
| B5 | **Tanpa ligan** [L14, C12 lokal] | Pocket kimiawi tidak dapat dinilai | Pocket/ligan di luar endpoint V1 (§C.13) |
| B6 | **Bias taksonomi & fungsi** | Set mungkin masih manusia-globular-heavy | Stratifikasi target (§A.4), tapi diakui tidak sepenuhnya terkoreksi |
| B7 | **pLDDT rendah = disorder** [L10, L32] | "Error besar" di low-bin bisa bukan error | Analisis tetap; interpretasi low-bin wajib memakai S5 |
| B8 | **PAE asimetris** [C8 lokal] | Arah blok PAE tidak simetris | Definisi C.6/C.7 memakai mean kedua arah; diuji |
| B9 | **Pseudoreplikasi residu** | CI terlalu sempit | Unit = protein/klaster (§A.4, D.1–D.2) |
| B10 | **Multiplicity** | False positive dari banyak uji | Holm untuk 4 primer; eksploratori dilabeli (§D.4) |

**Keterbatasan yang tidak dimitigasi (diakui):** audit ini membandingkan dua snapshot statis; tidak mengukur dinamika [L31]; tidak menguji utilitas hilir (docking/VHTS — beda pertanyaan [L16, L35, L37]); model AFDB adalah satu deterministik sample, bukan ensemble prediksi.

---

## F. Ukuran Sampel & Justifikasi

### F.1 Prinsip

- **N = jumlah protein independen (klaster).** Bukan N = jumlah residu (klaim F `PROJECT_STATUS.md` §12). Bukan N = jumlah pasangan PDB (beberapa pasangan dari protein sama = dependen).
- Tidak ada training ML → tidak ada kebutuhan split train/test; sebagai gantinya, kontrol leakage via strata & klaster (§A.4).

### F.2 Tier (pra-registrasi)

| Tier | N target | Status penggunaan | Justifikasi |
|---|---|---|---|
| **T0 — Pilot 30** | 30 protein | **HANYA untuk menguji pipeline & estimasi biaya** — dilarang untuk klaim hasil | Menutup kekhawatiran prompt (pilot ≠ sampel ilmiah); 30 cukup untuk menemukan bug mapping/metrik dan mengukur durasi per protein |
| **T1 — Core 100** | 100 protein | **Ambang minimum untuk klaim primer** (§G.4) | ≈ daya statistik untuk mendeteksi korelasi rank ρ ≈ 0,3 (α=0,05, 1−β=0,80) antar ringkasan per-protein — perhitungan aproksimasi Fisher-z: n ≈ ((1,96+0,84)/(0,5·ln((1+0,3)/(1−0,3))))² + 3 ≈ **85**, dibulatkan ke atas untuk kehilangan klaster & multiplicity. (Perhitungan tangan, bukan software; dicatat agar bisa direproduksi.) |
| **T2 — Extended 300** | 300 protein | Untuk analisis stratifikasi (S1–S6) dengan sel yang layak | Membagi 300 ke 6 strata ≈ 50/strata → CI per-strata masih informatif; literatur tile ini kecil (mis. [L15] n=10; [L34] n=152) |

### F.3 Referensi daya untuk RQ spesifik

- **RQ1 (dalam-protein):** tidak butuh N besar untuk *deskripsi* per protein; tetapi klaim lintas-protein tetap tunduk pada F.2 (T1).
- **RQ3 (domain):** hanya protein multidomain yang eligible — N efektif lebih kecil (estimasi dari sub-sampling pilot; jika <20 protein multidomain di T1 → RQ3 diturunkan menjadi **eksploratori** dan dilabeli demikian). Keputusan akhir dihitung di T0 report.
- **RQ4:** estimasi proporsi dengan CI Wilson; presisi ±3% absolut membutuhkan ~1000 residu high-confidence dari ≥40 protein berbeda — T1 dirancang menyediakan keduanya (diverifikasi di T0).

### F.4 Biaya (estimasi kasar, dikalibrasi ulang setelah T0)

Mapping + komputasi metrik per protein: kecil (file <1 MB; tool lokal). Beban utama = kurasi pasangan (30–60 menit/protein). Estimasi: T0 ≈ 1–2 hari; T1 ≈ 1–2 minggu; T2 ≈ 2–4 minggu kerja CLI. **Estimasi ini bukan komitmen waktu.**

---

## G. Kriteria Evaluasi Pra-Registrasi (Pre-Registered Decision Rules)

### G.1 Definisi "berhasil-untuk-klaim" (bukan "AF benar/salah")

Riset ini **tidak** menilai AF lulus/gagal secara absolut; ia mengukur & mengkuantifikasi kalibrasi. "Kriteria evaluasi" di bawah adalah aturan interpretasi yang ditetapkan sebelum data:

| # | Aturan keputusan (pre-registered) |
|---|---|
| G1.1 | **RQ1 terjawab kalibratif** jika: median error lokal bin `[90,100]` **≤ 2,0 Å** pada majority (≥70%) protein T1, DAN ordering median menurun monoton di 4 bin pada ≥70% protein. Jika tidak → hasil tetap dilaporkan sebagai "kalibrasi lokal tidak seragam/lebih lemah dari ekspektasi". |
| G1.2 | **Kegagalan high-confidence terkuantifikasi** (RQ4) dilaporkan sebagai proporsi + CI + daftar contoh (protein, residu, sifat struktural). Tidak ada ambang "lulus/gagal" — angkanya sendiri yang menjadi hasil [L7]. |
| G1.3 | **RQ2/RQ3** dilaporkan sebagai korelasi rank + CI + scatter penuh. Arah/ada-tidaknya korelasi tidak ditentukan lulus/gagal. Jika CI meliputi 0 → dilaporkan sebagai "tidak terdeteksi pada N ini" (bukan "tidak ada"). |
| G1.4 | **Interpretasi "reliabel"** dalam laporan akhir dibatasi ke kalimat operasional: "pLDDT ≥ 90 sesuai dengan error lokal ≤ 2 Å pada X% residu (CI ...) dari Y protein" — dilarang menyimpulkan "pLDDT dapat dipercaya" tanpa angka ini. |

### G.2 Definisi hasil yang akan **membantah** ekspektasi [H1–H4] (falsifikasi)

- H1 batal jika: untuk majority protein, median error bin ≥90 **tidak** lebih kecil dari bin 70–90.
- H2 batal jika: korelasi pLDDT↔TM lintas protein **sangat kuat (ρ>0,8) tanpa heterogenitas** (menunjukkan pLDDT memprediksi global sebaik lokal — bertentangan dengan [L7]).
- H4 bila: residu pLDDT ≥ 90 dengan error > 2 Å sangat jarang (<1%) — tetap dilaporkan dengan CI.

### G.3 Ambang kelengkapan laporan (reporting completeness)

Laporan hasil wajib memuat: (1) jumlah kandidat per tahap filtrasi + alasan setiap eksklusi (tabel numerik), (2) semua bin pLDDT walau kosong, (3) semua sensitivitas S1–S8 walau null, (4) daftar protein gagal QC mapping, (5) CI untuk semua estimasi, (6) hash file hasil, (7) pernyataan keterbatasan per §E. Laporan tanpa 7 unsur ini **tidak dianggap hasil audit**.

### G.4 Gate: T1 (100 protein) untuk klaim primer

Klaim primer (dipakai di abstrak/publikasi eksternal) hanya boleh dibangun di atas **T1** dengan QC reproduksibilitas (dua run byte-identical, §D.7). T0 hanya boleh dikutip sebagai "hasil pilot" dengan label eksplisit. Jika T1 tidak tercapai (kendala data/waktu), laporan tetap dibuat sebagai **technical report berskala N yang tercapai**, dengan judul & klaim yang jujur menyebut N.

### G.5 Komitmen pelaporan negatif & amendment

Semua hasil null/negatif/membingungkan dilaporkan. Setiap perubahan protokol setelah tanggal ini dicatat di **Amendment Log** `docs/PROTOCOL_AMENDMENTS.md` (dibuat saat amendment pertama) dengan: tanggal, alasan, apakah perubahan dibuat sebelum/setelah melihat hasil. Perubahan setelah melihat hasil yang memengaruhi endpoint primer = pelanggaran pra-registrasi → wajib diungkap di laporan.

---

## H. Deliverables, Keputusan Tertunda & Gate

### H.1 Deliverable fase implementasi (setelah persetujuan — BUKAN sekarang)

1. `TOOLS_LOCK.md` — versi tool & flags (TM-align [L18], lDDT [L19], aligner, mapping API, domain segmentation, disorder annotation).
2. Script unduh terkontrol (pola sama dengan pipeline lokal Tahap 5: manifest dulu, unduh kecil per file, hash) — **volume PDB dibatasi §policy**.
3. Manifest pencocokan (§B.1.7) + tabel eksklusi.
4. T0 report (pilot) → kalibrasi biaya & RQ3 feasibility.
5. T1(+T2) hasil + analisis statistik + semua sensitivitas.
6. Laporan hasil dengan 7 unsur §G.3.

### H.2 Keputusan yang harus dibuat SEBELUM fase implementasi (blocker)

| # | Keputusan | Pilihan yang ada | Rekomendasi saat ini |
|---|---|---|---|
| K-1 | Git init di root audit (temuan F3 baseline) | init / tidak | **init** (riwayat dokumen & skrip tertelusur; tanpa remote sampai user memutuskan) — tetapi bukan keputusan audit; menunggu user |
| K-2 | Verifikasi cutoff data latih dari full text [L1] | pakai default sementara / verifikasi resmi | **verifikasi** sebelum filtering tanggal dipakai |
| K-3 | Toolchain metrik (TM-align, lDDT impl, aligner, clustering) + lisensi | implementasi sendiri vs tool publik | pilih & kunci di `TOOLS_LOCK.md`; instalasi = keputusan fase implementasi |
| K-4 | Definisi domain & disorder annotation | opsi §D.5 | dikunci sebelum metrik pertama |
| K-5 | Tier dataset & anggaran volume unduhan PDB | T0→T1→T2 | mulai T0; gate review user sebelum T1 |
| K-6 | Environment: venv terpisah di root audit | ya/tidak | **ya** (isolasi dari `ai-biotech`; larangan menyentuh repo induk) |
| K-7 | Sitasi & lisensi output | CC BY 4.0 (AFDB) + atribusi PDB | lihat `DATA_AND_BIAS_POLICY.md` §4–§5 |

### H.3 Gate & stop condition

- **Gate P2→P3:** dokumen Phase 2 (4 dokumen ini + baseline) selesai & direview → user memutuskan GO/HOLD (lihat `PHASE2_DECISION.md`).
- **Gate P3→P4:** setelah T0 report direview user → lanjut T1 atau koreksi.
- **Stop condition tetap berlaku:** tanpa persetujuan user, tidak ada unduhan dataset baru, tidak ada instalasi dependensi, tidak ada eksekusi eksperimen, tidak ada perubahan pada `D:\project\ai-biotech`.

---

## Appendix — Peta RQ → Analisis → Kriteria

| RQ | Metrik | Bin/Strata | Uji/Statistik | Aturan hasil (G) |
|---|---|---|---|---|
| RQ1 | C.5a/b vs C.10 | 4 bin pLDDT | Wilcoxon paired (≥90 vs 70–90) + bootstrap klaster | G1.1 |
| RQ2 | C.2, C.1 vs mean C.10 | per-protein | Spearman + CI | G1.3 |
| RQ3 | C.8 vs C.7 | multidomain | Spearman + CI | G1.3 + F.3 |
| RQ4 | C.9 | pLDDT≥90 | Proporsi + Wilson + bootstrap | G1.2 |

---

*Dokumen ini adalah pra-registrasi. Tidak ada data hasil yang dilihat saat penyusunan. Referensi [L##] mengacu ke `docs/LITERATURE_REVIEW.md`; C1–C12/D1–D4/A–G mengacu ke `PROJECT_STATUS.md`; F1–F4 ke `PHASE1_BASELINE_AUDIT.md`.*
