# PHASE 2 — REVIEW GATE

## Laporan Review Independen atas Kelima Dokumen Phase 2 (Gerbang Sebelum T0)

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal review** | 2026-10-11 |
| **Status** | SELESAI — menunggu keputusan pemilik proyek |
| **Objek review** | `PHASE1_BASELINE_AUDIT.md`, `LITERATURE_REVIEW.md`, `RESEARCH_PROTOCOL_V1.md`, `DATA_AND_BIAS_POLICY.md`, `PHASE2_DECISION.md` |
| **Sifat** | Read-only terhadap seluruh artefak & data; satu-satunya file yang dibuat = laporan ini. Tidak ada unduhan data, instalasi, eksekusi, `git init`, atau perubahan pada `ai-biotech`. |
| **Metode verifikasi** | SHA256 ulang; pembacaan penuh kelima dokumen; verifikasi ulang 40 sumber (Europe PMC REST + Crossref — pipa yang sama, read-only); pembacaan full text [L1] & [L5] via Europe PMC `fullTextXML`; verifikasi dokumen resmi (catatan teknis DeepMind v2.3.0, halaman download AFDB, kebijakan lisensi RCSB, halaman resmi tool). |
| **Batas review** | Klaim diverifikasi pada level metadata + abstrak (dan full text hanya untuk [L1]/[L5] serta dokumen resmi). Teks penuh 40 sumber TIDAK dibaca seluruhnya. Tidak ada tool yang dijalankan; tidak ada dataset yang diunduh. |

---

## 1. Ringkasan Keputusan

**Rekomendasi reviewer: CONDITIONAL GO** — T0 diizinkan **hanya setelah** persetujuan pemilik dan enam koreksi wajib (§8) diterapkan sebagai amendment. Tidak ada temuan yang membatalkan desain; semua blocker adalah perbaikan *dokumentasi/pra-registrasi*, bukan sains baru.

Poin utama:

1. **Integritas artefak: 5/5 COCOK.** Empat hash di lampiran `PHASE2_DECISION.md` cocok persis; hash kelima (dokumen keputusan sendiri) cocok dengan nilai yang dilaporkan di sesi sebelumnya. Tidak ada perubahan file setelah pembekuan.
2. **Literatur: 40/40 sumber terverifikasi ulang** (39 via Europe PMC, Kabsch 1976 via Crossref). Kutipan-kutipan kunci yang menopang desain dicek verbatim — **semuanya cocok** dengan isi yang dikembalikan server (§4.2). Lima koreksi DOI lama terkonfirmasi benar.
3. **Cutoff data latih TERJAWAB.** Tanggal kandidat `2018-04-30` kini didukung **bukti primer**: full text [L1] (verbatim) + metadata resmi AFDB (`toolUsed: "AlphaFold Monomer v2.0 pipeline"` pada 7/7 entri lokal). Nuansa penting ditemukan dan didokumentasikan (§4.3): cutoff `2021-09-30` yang lebih baru berlaku untuk **bobot Multimer v2.3.0**, bukan untuk entri monomer AFDB. **K-2 dapat diselesaikan** (ratifikasi pemilik + amendment). S3 (strata pre/post-cutoff) layak dijalankan.
4. **Toolchain (K-3) disiapkan konkret** dengan versi dari dokumentasi resmi + catatan lisensi yang sudah diverifikasi (§6, K-3). Satu risiko platform ditemukan: ketersediaan binary Windows native untuk sebagian tool perlu diverifikasi di `TOOLS_LOCK` (kemungkinan butuh WSL2/conda).
5. **Enam koreksi wajib sebelum T0** (§8) — semuanya bersifat pra-registrasi/dokumentasi: (a) prosedur sampling belum ditetapkan (gap terbesar), (b) definisi klaster & parameter bootstrap belum terkunci, (c) aturan bin kosong/uji berpasangan belum dispesifikasi, (d) aturan tepi pencocokan (modifikasi residu, isoform, pemilihan chain, model NMR), (e) `TOOLS_LOCK.md` belum ada (K-3), (f) ratifikasi keputusan K + pencatatan hasil verifikasi cutoff.
6. **Temuan informasi baru sejak dokumen dibekukan** (§4.4): halaman resmi AFDB kini mensyaratkan sitasi **Fleming et al. 2025 (JMB)** — DOI terverifikasi `10.1016/j.jmb.2025.168967`, PMID 40133787 — untuk penggunaan data; lisensi AFDB (CC BY 4.0) dan PDB (CC0 1.0) terverifikasi live.
7. **Satu DOI keliru dari sumber sekunder tertangkap lagi** saat review ini (mesin pencari mengatributkan DOI paper Fleming yang salah) — konfirmasi ulang nilai pipa verifikasi (§4.4).
8. **Konsistensi silang antar dokumen**: angka-angka bersama (tier 30/100/300; 1.098 baris; 1.469.156 byte; cutoff 2018-04-30 berlabel TBD; rujukan [L1]–[L40] seluruhnya dalam rentang) diperiksa ulang dan konsisten, sebagaimana diklaim lampiran `PHASE2_DECISION.md`.

**Stop condition berlaku:** tanpa persetujuan pemilik, tidak ada implementasi T0, unduhan, instalasi, atau perubahan dokumen sumber.

---

## 2. Verifikasi Integritas Artefak & Hash

Dihitung ulang dari disk pada 2026-10-11 (sesi review ini):

| Dokumen | Ukuran (byte) | SHA256 (recompute 2026-10-11) | vs hash tercatat | Status |
|---|---|---|---|---|
| `docs/PHASE1_BASELINE_AUDIT.md` | 19.145 | `da5adfb9087be790f97b39115c63b0fc198b164529dbb5bac61467b576fa0430` | lampiran `PHASE2_DECISION.md` | **COCOK** |
| `docs/LITERATURE_REVIEW.md` | 41.430 | `471b24cc3c2edf3482dc4a55a2484f12db42efb383aea55ffb7279a67e5fd069` | lampiran `PHASE2_DECISION.md` | **COCOK** |
| `docs/RESEARCH_PROTOCOL_V1.md` | 27.147 | `71e665e64697c269eef80437b243409ccff5f5d07b6db3df5ec8da0324ac1536` | lampiran `PHASE2_DECISION.md` | **COCOK** |
| `docs/DATA_AND_BIAS_POLICY.md` | 12.952 | `fa08e40c4817748770ad353b460e5356fe5ce90773dd16d6e74c864827320b6f` | lampiran `PHASE2_DECISION.md` | **COCOK** |
| `docs/PHASE2_DECISION.md` | 13.581 | `bd0a4deac66637625f77877610275f6eb68841f8766ad185ca3888ea3cc4d9b2` | dilaporkan di sesi penyusunan (tidak bisa memuat hash sendiri) | **COCOK** (dicek silang ke transkrip sesi sebelumnya) |

Catatan tambahan:

- `docs/` berisi hanya kelima dokumen; `docs/PROTOCOL_AMENDMENTS.md` belum ada — konsisten dengan aturan "dibuat saat amendment pertama". **Amendment pertama justru akan diperlukan** bila koreksi §8 disetujui.
- Repo induk `D:\project\ai-biotech` diperiksa ulang saat review: `git status --short` **kosong**; HEAD = `origin/main` = `b942843` — tetap bersih, tidak tersentuh.
- Root audit tetap bukan repository Git (temuan F3; keputusan K-1 menunggu pemilik).

**Kesimpulan integritas:** artefak tidak berubah sejak pembekuan; tidak ada indikasi modifikasi pasca-hash.

---

## 3. Metode & Batas Review

**Dilakukan:** pembacaan penuh kelima dokumen; recompute hash; verifikasi ulang seluruh 40 DOI (Europe PMC `resultType=core`; Crossref untuk L23); pencocokan klaim-kutipan kunci terhadap abstrak yang dikembalikan server (beberapa abstrak dibaca melebihi 700 karakter); pembacaan full text [L1] (PMC8371605) dan [L5] (PMC8387240) via `fullTextXML`; pembacaan dokumen resmi: catatan teknis AlphaFold v2.3.0 (repo DeepMind), halaman `alphafold.ebi.ac.uk/download` (+ License-Disclaimer), RCSB Usage Policies, halaman resmi TM-align/US-align/Foldseek/OpenStructure/MMseqs2; pembacaan metadata lokal AFDB (7 file `api_*.json` — read-only) untuk field `toolUsed`.

**Tidak dilakukan:** membaca full text seluruh 40 sumber; menjalankan tool/metrik; mengunduh dataset; menguji API SIFTS/RCSB/AFDB secara live; mengubah file apa pun selain laporan ini. Semua pernyataan di bawah dibatasi pada apa yang benar-benar diperiksa; yang tidak diperiksa ditandai eksplisit.

---

## 4. Temuan Review Literatur

### 4.1 Verifikasi 40 sumber (ulang, 2026-10-11)

Seluruh 40 sumber di `LITERATURE_REVIEW.md` §8 ditemukan kembali dengan metadata yang cocok (judul, penulis, jurnal, tahun, DOI, PMID). Ringkasan status per sumber:

| ID | Sumber (first author, tahun, jurnal) | Verifikasi ulang |
|---|---|---|
| L1 | Jumper 2021, Nature — `10.1038/s41586-021-03819-2` (PMID 34265844) | EPMC OK + full text dibaca (cutoff, §4.3) |
| L2 | Pereira 2021, Proteins — `10.1002/prot.26171` (PMID 34218458) | EPMC OK; klaim abstrak cocok (koreksi DOI benar) |
| L3 | Kryshtafovych 2021, Proteins — `10.1002/prot.26237` | EPMC OK; **2 kutipan verbatim cocok** |
| L4 | Kryshtafovych 2023, Proteins — `10.1002/prot.26617` | EPMC OK; frasa "accuracy comparable with experiment…" cocok |
| L5 | Tunyasuvunakool 2021, Nature — `10.1038/s41586-021-03828-1` | EPMC OK; angka 98,5%/58%/36%/17% cocok; **full text dibaca** (ambang bin, §4.2) |
| L6 | Abramson 2024, Nature — `10.1038/s41586-024-07487-w` | EPMC OK |
| L7 | Terwilliger 2024, Nat Methods — `10.1038/s41592-023-02087-4` | EPMC OK; **kutipan kunci cocok** (lihat catatan pemotongan §4.5) |
| L8 | Akdel 2022, NSMB — `10.1038/s41594-022-00849-w` | EPMC OK; "25% additional residues" + frasa confidence cocok |
| L9 | Guo 2022, Sci Rep — `10.1038/s41598-022-14382-9` | EPMC OK |
| L10 | Bruley 2022, Biomolecules — `10.3390/biom12101467` | EPMC OK; klaim "good measure of residue-wise disorder" cocok |
| L11 | Kwon 2021, Proteins — `10.1002/prot.26192` | EPMC OK (koreksi DOI benar) |
| L12 | Roney 2022, PRL — `10.1103/PhysRevLett.129.238101` | EPMC OK; klaim "without using any coevolution data" cocok |
| L13 | Hiranuma 2021, Nat Commun — `10.1038/s41467-021-21511-x` | EPMC OK (koreksi DOI benar); klaim korelasi resolusi cocok |
| L14 | Hekkelman 2023, Nat Methods — `10.1038/s41592-022-01685-y` | EPMC OK; kutipan "all lack coordinates for small molecules" cocok |
| L15 | Meller 2023, JCTC — `10.1021/acs.jctc.2c01189` | EPMC OK; "including five that were deposited after AlphaFold's training data were extrac[ted]" cocok |
| L16 | Karelina 2023, eLife — `10.7554/eLife.89386` | EPMC OK; klaim pocket-vs-pose cocok verbatim |
| L17 | Zhang 2004, Proteins — `10.1002/prot.20264` | EPMC OK |
| L18 | Zhang 2005, NAR — `10.1093/nar/gki524` | EPMC OK; angka (4×, 10.515, 1996, 0.5) cocok |
| L19 | Mariani 2013, Bioinformatics — `10.1093/bioinformatics/btt473` | EPMC OK; kutipan "strongly influenced by domain motions…" cocok |
| L20 | Zemla 2003, NAR — `10.1093/nar/gkg571` | EPMC OK |
| L21 | Basu 2016, PLoS ONE — `10.1371/journal.pone.0161879` | EPMC OK; contoh LRMS/Fnat/iRMS cocok |
| L22 | Méndez 2003, Proteins — `10.1002/prot.10393` | EPMC OK (koreksi DOI benar) |
| L23 | Kabsch 1976, Acta Cryst A — `10.1107/S0567739476001873` | **Crossref OK** (non-PubMed, sesuai catatan dokumen); judul/penulis/halaman cocok |
| L24 | AlQuraishi 2019, BMC Bioinf — `10.1186/s12859-019-2932-0` | EPMC OK (koreksi DOI benar); kutipan "insulated training/validation splits…" cocok |
| L25 | Chakravarty 2022, Prot Sci — `10.1002/pro.4353` | EPMC OK; "94% … captured one … but not the other" cocok |
| L26 | Chakravarty 2024, Nat Commun — `10.1038/s41467-024-51801-z` | EPMC OK; memorization + ">280,000 models" cocok |
| L27 | Saldaño 2022, Bioinformatics — `10.1093/bioinformatics/btac202` | EPMC OK; klaim ensemble/apo-holo cocok |
| L28 | Wayment-Steele 2024, Nature — `10.1038/s41586-023-06832-9` | EPMC OK; klaim MSA clustering cocok |
| L29 | del Alamo 2022, eLife — `10.7554/eLife.75751` | EPMC OK; klaim "designed to predict sta[tic]" + sampling state alternatif cocok |
| L30 | Wlodawer 2013, FEBS J — `10.1111/febs.12495` | EPMC OK; "far from being analytical" cocok |
| L31 | Henzler-Wildman 2007, Nature — `10.1038/nature06522` | EPMC OK |
| L32 | Ruff 2021, JMB — `10.1016/j.jmb.2021.167208` | EPMC OK; ">30% disordered" + overlap IDR cocok |
| L33 | Burley 2023, NAR — `10.1093/nar/gkac1077` | EPMC OK; "one million computed models" cocok |
| L34 | Yin 2022, Prot Sci — `10.1002/pro.4379` | EPMC OK; 152 kompleks, 43%, 9%, 11% cocok |
| L35 | Wong 2022, MSB — `10.15252/msb.202211081` | EPMC OK; auROC 0,48 → 0,63 cocok |
| L36 | Díaz-Rovira 2023, JCIM — `10.1021/acs.jcim.2c01270` | EPMC OK; eksklusi template >30% + kesimpulan virtual screening cocok |
| L37 | Buttenschoen 2024, Chem Sci — `10.1039/D3SC04185A` | EPMC OK; klaim RMSD-tidak-cukup + generalisasi cocok |
| L38 | Tsaban 2022, Nat Commun — `10.1038/s41467-021-27838-9` | EPMC OK; frasa "memorized and learned" cocok |
| L39 | Varadi 2022, NAR — `10.1093/nar/gkab1061` | EPMC OK; 360.000 struktur/21 proteom; "Powered by AlphaFold v2.0" cocok |
| L40 | Varadi 2024, NAR — `10.1093/nar/gkad1011` | EPMC OK; 214 juta sekuens cocok |

Tidak ada sumber yang gagal verifikasi. Tidak ditemukan klaim dokumen yang bertentangan dengan isi yang dikembalikan server.

### 4.2 Verifikasi kutipan/angka kunci (contoh yang menopang desain)

| Klaim di dokumen | Isi server (2026-10-11) | Status |
|---|---|---|
| [L3] "rivaling the corresponding experimental ones in accuracy" + "a solution to the classical protein-folding problem, at least for single proteins" | Kutipan ada verbatim di abstrak | COCOK |
| [L7] "even very high-confidence predictions differed from experimental maps on a global scale through distortion and domain orientation, and on a local sc…" | Cocok; abstrak terpotong pada "local sc" — dokumen sudah benar menandai perlunya verifikasi full text sebelum kutipan verbatim di publikasi | COCOK (dengan penanda dokumen) |
| [L5] 98,5% protein; 58% residu confident; 36% very high; 17% sebelumnya | Semua angka cocok | COCOK |
| [L5 full text] ambang konvensi pLDDT | "pLDDT > 90 is taken as the high accuracy cut-off…"; "A lower cut-off of pLDDT > 70 corresponds to a generally correct backbone prediction"; "regions with pLDDT < 50 … should not be interpreted as structures but rather as a prediction of disorder" | COCOK — menopang bin D.3 [0,50/50,70/70,90/90,100] |
| [L8] "25% additional residues" + "when the confidence metrics are critically considered" | Keduanya cocok verbatim | COCOK |
| [L12] "…without using any coevolution data" | Cocok | COCOK |
| [L14] "all lack coordinates for small molecules" + contoh heme/Zn/ATP | Cocok verbatim | COCOK |
| [L16] pocket akurat ≈ antar-struktur eksperimen; pose tidak lebih baik dari homology model | Cocok verbatim | COCOK |
| [L19] "strongly influenced by domain motions and do not assess…local atomic details"; reference boleh ensemble | Cocok | COCOK |
| [L24] "insulated training/validation splits…weakly detectable homology" | Cocok verbatim | COCOK |
| [L25] 94% prediksi menangkap satu konformasi saja | Cocok | COCOK |
| [L26] memorization + >280.000 model (AF2 & AF3) | Cocok | COCOK |
| [L30] "far from being analytical" | Cocok | COCOK |
| [L32] >30% proteom disordered; low-confidence ↔ IDR | Cocok | COCOK |
| [L34] 152 heterodimer; 43% near-native; docking unbound 9%; antibodi 11% | Cocok | COCOK |
| [L35] auROC 0,48; rescoring ≤0,63 | Cocok | COCOK |
| [L36] template >30% identitas dieksklusi; "not an ideal scenario for virtual screening" | Cocok | COCOK |
| [L15] 5 kasus cryptic pocket pasca-ekstraksi data latih | Cocok | COCOK |

### 4.3 Cutoff data latih AF2 — verifikasi definitif (menjawab U1/K-2)

**Bukti primer [L1] — full text Jumper et al. 2021 (PMC8371605), verbatim:**

> "Training regimen — To train, we use structures from the PDB with a maximum release date of 30 April 2018."

> "Structures were filtered to those with a release date after 30 April 2018 (the date limit for inclusion in the training set for AlphaFold)."

Pendukung lain dari full text yang sama: evaluasi internal paper memakai set PDB pasca-cutoff dan menyatakan *"This dataset excludes proteins with a template (identified by hmmsearch) from the training set with more than 40% sequence identity covering more than 1% of the chain (n = 3,144 protein chains)"* — preseden langsung untuk kontrol paparan data latih (relevan ke B13). Fine-tuning pasca-CASP14 (objektif pTM) memakai data latih yang sama; self-distillation memakai sekuens (UniClust 2018_08, MGnify 2018_12) — **bukan** struktur eksperimen baru.

**Bukti primer kedua — metadata resmi AFDB (7/7 file `api_*.json` lokal yang diaudit):**

> `"toolUsed": "AlphaFold Monomer v2.0 pipeline"` — `"modelCreatedDate":"2025-08-01T00:00:00Z"` — `"latestVersion":6`

Artinya: entri AFDB v6 (termasuk dataset lokal; dibuat ulang 2025-08-01) dihasilkan oleh **pipeline monomer v2.0** — jalur yang cutoff-nya 2018-04-30 menurut [L1].

**Nuansa kritis yang ditemukan (dan harus dicatat, bukan diabaikan) — catatan teknis resmi AlphaFold v2.3.0 (repo DeepMind), verbatim:**

> "We have fine-tuned new AlphaFold-Multimer weights using identical model architecture but a new training cutoff of 2021-09-30."

> "Previously released versions of AlphaFold and AlphaFold-Multimer were trained using PDB structures with a release date before 2018-04-30, a cutoff date chosen to coincide with the start of the 2018 CASP13 assessment."

Jadi ada **dua cutoff** di ekosistem: `2021-09-30` untuk bobot **Multimer** v2.3.0 (fine-tuned), dan `2018-04-30` untuk versi-versi sebelumnya termasuk model monomer. Catatan teknis yang sama **tidak menyatakan** bobot monomer di-retrain dengan 2021-09-30; dan metadata AFDB tetap melabeli entri monomer sebagai "v2.0 pipeline".

**Kesimpulan untuk audit:**

- **Untuk objek audit V1 (entri AFDB monomer F1), cutoff 2018-04-30 adalah nilai yang benar dan kini didukung bukti primer yang memadai.** Kandidat tanggal di protokol §0.5 bukan lagi TBD.
- **Ketidakpastian residual (dicatat, tidak ditutup dengan asumsi):**
  1. Label "v2.0 pipeline" adalah pernyataan penyedia pada metadata; tidak ada publikasi yang secara eksplisit menegaskan bobot monomer tidak pernah berubah secara diam-diam. Risiko rendah, tetapi klaim harus dibatasi pada "sesuai metadata resmi AFDB + [L1]".
  2. Cutoff berlaku untuk **struktur eksperimen**; data **sekuens** yang lebih baru (UniClust 2018_08, MGnify 2018_12) ikut dipakai dalam MSA/self-distillation — struktur pasca-cutoff yang sekuensnya ada di basis sekuens itu tetap bisa "terlihat" sebagai sekuens. Untuk stratifikasi B2/B13: cutoff = struktur.
  3. Jika AFDB di masa depan meregenerasi entri dengan model lebih baru (mis. jalur v2.3.x), cutoff bergeser → **wajib mencatat `toolUsed` + `modelCreatedDate` + `latestVersion` per entri pada manifest unduhan** (usulan koreksi §8).
  4. U5 (versi live AFDB) tetap berlaku untuk fase unduh.

### 4.4 Temuan informasi baru sejak pembekuan dokumen (2026-10-11)

1. **Sitasi resmi AFDB berubah.** Halaman `alphafold.ebi.ac.uk/download` (diakses 2026-10-11) kini menempatkan **Fleming J. et al., "AlphaFold Protein Structure Database and 3D-Beacons: New Data and Capabilities", J Mol Biol (2025)** sebagai paper database, di samping Jumper 2021. DOI terverifikasi via Europe PMC: **`10.1016/j.jmb.2025.168967`** (PMID 40133787). Paper Varadi 2022/2024 ([L39]/[L40]) tidak lagi tercantum di halaman itu. → **U7 terjawab sebagian**: saat publikasi, sitasi dataset yang dipakai = Fleming 2025 + [L1] + [L39]/[L40] sebagai rujukan historis (keputusan format akhir = pemilik, §6 K-7).
2. **Lisensi terverifikasi live (sebelumnya "tidak dicek ulang ke server" di policy §4):**
   - AFDB: *"Data is available for academic and commercial use, under a CC-BY-4.0 licence."* → klaim policy §4 **terkonfirmasi**.
   - PDB (RCSB Usage Policies): *"data files contained in the PDB archive are available under the CC0 1.0 Universal (CC0 1.0) Public Domain Dedication"*; atribusi kepada penulis asli *"encouraged"* (bukan wajib) → policy §2.2/§4 dapat diperbarui (CC0; atribusi tetap praktik baik + sitasi depositor).
3. **Jebakan DOI kembali terkonfirmasi.** Ringkasan mesin pencari (sumber sekunder) sempat mengatributkan DOI `10.1016/j.jmb.2025.168935` ke paper Fleming; verifikasi pipa menunjukkan DOI itu milik paper lain (Schwartze et al., BMP-9/10). DOI benar `…168967`. **Tidak satu pun DOI baru masuk laporan ini tanpa lolos pipa.**
4. **Catatan teknis v2.3.0** (DeepMind; dipublikasikan di repo resmi) — bukti primer untuk nuansa cutoff di §4.3.

### 4.5 Batas kejujuran review literatur

- Verifikasi pada level metadata+abstrak (kecuali [L1]/[L5] full text). Kutipan panjang untuk publikasi tetap wajib dicek ke full text masing-masing sumber saat itu (aturan [L7] sudah ada di dokumen).
- FAQ resmi AFDB (`/faq`) tidak dapat dirender (situs dinamis) saat review; pernyataan versi model disandarkan pada field API `toolUsed` dan catatan teknis DeepMind — keduanya sumber resmi mesin-pembaca.
- Tidak ada akses berbayar; tidak ada klaim "konsensus" baru yang dibuat reviewer ini.

---

## 5. Temuan Review Metodologi

Dinilai terhadap 10 pertanyaan review; temuan diberi tingkat [KRITIS]/[SEDANG]/[MINOR] dengan target koreksi (bagian dokumen).

### 5.1 Apakah RQ & RQ1–RQ4 dapat diuji dengan desain yang diusulkan?

**Ya, secara umum.** Operasionalisasi (endpoint C.x, bin D.3, unit protein/klaster, aturan G) konsisten dengan RQ. Catatan:

- RQ3 benar dikondisikan pada jumlah protein multidomain (F.3) dan punya pre-komitmen turun-ke-eksploratori. [MINOR] Kuantifikasi "≥2 domain" bergantung D.5 (lihat 5.4).
- RQ4 "high-confidence failure" didefinisikan melalui C.5a (jarak Cα setelah superposisi domain) — perlu satu klarifikasi: untuk protein satu-domain, superposisi domain = superposisi global (konsisten); untuk multi-domain, superposisi per-domain justru yang membuat error lokal terisolasi dari domain motion (sesuai [L19]). [MINOR] Tambahkan kalimat klarifikasi ini agar implementasi tidak "memilih sendiri".
- RV2/RV4 deskriptif-korelasional sudah benar dibingkai (bukan kausal). OK.

### 5.2 Apakah 40 sumber mendukung klaim? (lihat §4) — Ya. Tidak ada koreksi.

### 5.3 Cutoff (lihat §4.3) — Terjawab; tindak lanjut = ratifikasi + amendment.

### 5.4 Apakah pencocokan struktur menangani identity, coverage, isoform, mutasi, konformasi, missing residues, ligan?

Sebagian besar ya (A2.4, A2.5, A2.7, A3.2, A3.4, B.1, B.2, S4, S8), tetapi ada **lubang aturan tepi yang wajib ditutup sebelum implementasi** [KRITIS untuk T0 karena mapping dijalankan di T0]:

1. **Residu termodifikasi tidak disebut sama sekali.** MSE (selenomethionine) sangat umum di X-ray; CSE, SEP, dsb. harus dinormalisasi ke residu induk (M, C, S) pada pengecekan identitas, atau (lebih baik) pakai mapping residu-level SIFTS yang sudah menangani ini. Tanpa aturan ini, A2.5 (identitas 100%) akan mengeksklusi struktur yang sebenarnya valid, atau sebaliknya.
2. **Isoform tidak disebut eksplisit.** A2.5 menyiratkan (identitas 100% → isoform berbeda gugur), tetapi aturan harus eksplisit: mapping via SIFTS hanya untuk UniProt **canonical**; entri yang dipetakan ke isoform non-kanonik dieksklusi & dicatat (relevan ke B11/R11 yang sudah mengakui risiko ini).
3. **Pemilihan chain tidak dispesifikasi** untuk entri multi-chain yang lolos (B.1.6 mengakomodasi oligomeric state, jadi entri multi-chain memang bisa masuk — **kontradiksi halus dengan bunyi A3.1 "Multi-chain/complex"** yang perlu diklarifikasi: yang dilarang = objek/endpoint kompleks & interface, bukan keberadaan >1 chain di entri). Aturan deterministik pemilihan chain (semua chain identik sekuens → pilih: paling sedikit residu missing → resolusi sama → ID leksikografis terkecil) harus ditulis di B.2.
4. **Model NMR tidak dispesifikasi** (ensemble): pilih model pertama sebagai referensi utama + laporkan sensitivitas ensemble (lDDT mendukung ensemble [L19]); strata S1 sudah memisahkan NMR — cukup dipin di `TOOLS_LOCK`.
5. **Definisi operasional "apo"** ("state biologisnya paling netral") belum dapat dieksekusi apa adanya — perlu definisi konkret (mis. tidak ada HETATM non-air dalam jarak X Å dari chain model) atau disederhanakan menjadi aturan biner (ada ligand fungsional teranotasi → holo). [SEDANG]
6. Ligan/HETATM: pencatatan sudah benar; endpoint pocket ditunda secara jujur (C.13) — OK.
7. Konformasi: S4 + label state + bahasa "terhadap state X" — desain bagus; hanya butuh definisi apo di atas.
8. Missing residues: A2.7 + A3.4 + pelaporan — OK.

### 5.5 Definisi & penggunaan metrik (TM-score, lDDT, RMSD, pocket metrics, pLDDT, PAE)

**Definisi sesuai tujuan masing-masing** (global vs lokal vs confidence) dan didukung literatur yang benar. Temuan:

- [MINOR] **C.5b "kontribusi lDDT per-residu"** perlu dipin ke implementasi nyata: OpenStructure menyediakan `--local-lddt` (per-residu) — definisikan C.5b = skor `local_lddt` OpenStructure, bukan "kontribusi" (istilah non-standar).
- [MINOR] **TM-align tanpa nomor versi** (halaman resmi: update terakhir 2022-04-12; changelog 2022/04/15) → penguncian versi harus lewat hash source/commit, atau migrasi ke **US-align** (aktif diperbarui; update terakhir 2026-05-27) yang menghitung TM-score dengan konvensi sama.
- [MINOR] **PAE↔CIF index mapping** belum disebut (PAE JSON 0-based urutan sekuens vs penomoran CIF label/auth). Implementasi harus mendefinisikan & menguji pemetaan ini (uji pada 2 protein lokal). Ini sumber bug klasik.
- [MINOR] C.4 GDT_TS "pelengkap": putuskan pin tool (LGA legacy) atau drop dari output V1 (tidak dipakai di endpoint D.4).
- C.8/C.7 (error orientasi domain vs cross-PAE) — definisi baik; B8 (PAE asimetris, mean dua arah) benar dan sudah ada uji QC.
- pLDDT dari kolom B-factor CIF (C.10) benar; granularitas redundan sudah dikenali (B12).
- Pocket metrics: ditunda dengan alasan eksplisit — konsisten dengan B5/[L14]/[L16]. OK.

### 5.6 Unit analisis, dependensi, CI, uji statistik, sensitivitas

Desain dua level (residu deskriptif; klaim di protein/klaster) benar dan merupakan kekuatan dokumen. Temuan:

1. [KRITIS — pra-registrasi] **Definisi klaster belum dapat direproduksi**: algoritma (single-linkage? connected components? MMseqs `easy-cluster`/`linclust`), parameter coverage, dan tool belum ditetapkan. "Identitas berpasangan >30%" tanpa algoritma → hasil klaster bisa berbeda antar implementasi. Lock di `TOOLS_LOCK.md` + simpan daftar klaster sebagai artefak.
2. [KRITIS — pra-registrasi] **Jumlah klaster = N efektif untuk semua CI**. Power T1 dihitung dengan N=protein (≈85→100); dengan cluster-robust bootstrap, N efektif = jumlah klaster (bisa jauh <100). Dokumen sudah membulatkan 85→100 "untuk kehilangan klaster & multiplicity" — tidak memadai bila klaster ternyata mis. 40. Koreksi: laporkan jumlah klaster di T0; tambahkan aturan gate T1 (mis. bila klaster < 30 → CI dinyatakan aproksimatif/butuh T2, atau tambah protein).
3. [KRITIS — pra-registrasi] **Penanganan bin kosong belum ditetapkan** untuk: uji berpasangan RQ1 (Wilcoxon butuh protein punya data di kedua bin ≥90 dan 70–90 — protein tanpa salah satu bin: drop dari uji? laporkan berapa?), dan kriteria G1.1 "ordering median menurun monoton di 4 bin pada ≥70% protein" (protein perlu data di 4 bin). Tanpa aturan, hasil bisa dipilih post-hoc. Pre-spesifikasi: definisi "protein UJI-RQ1" = punya ≥10 residu di kedua bin; aturan monotonicity = diuji pada protein dengan ≥1 residu di tiap bin (jumlah dilaporkan); tidak ada imputasi.
4. [SEDANG] **Parameter bootstrap belum lengkap** (jumlah resample B, seed). Tambahkan: B=10.000, seed tetap & dilaporkan; resampling level klaster.
5. [SEDANG] **G.2 (falsifikasi H2/H4) belum operasional**: "tanpa heterogenitas" (H2) dan "<1%" (H4) tanpa definisi. Usulkan: H2 batal bila ρ>0,8 dan batas bawah CI ≥ 0,7; H4 tetap dilaporkan sebagai frekuensi+CI tanpa ambang "batal".
6. [SEDANG] **Uji berpasangan ↔ unit statistik**: Wilcoxon signed-rank pada ringkasan per-protein (error bin ≥90 vs 70–90) valid, tetapi protein hanya boleh menyumbang satu pasangan (sudah terpenuhi); pastikan tidak ada penggunaan residu sebagai n (dokumen sudah melarang; pertahankan di kode).
7. [MINOR] Multiplicity Holm atas 4 endpoint — OK. Kriteria G1.1 (proporsi protein) bersifat deskriptif — beri label "deskriptif" agar tidak masuk family uji.
8. **Precedent baru yang berguna**: [L1] full text memakai aturan eksklusi template >40% identitas >1% chain untuk set uji pasca-cutoff — pola ini dapat dipakai sebagai **sumbu kedua B13** (paparan terhadap data latih): usulkan klasifikasi "cluster-exposed" (klaster sekuens 30–40% RCSB pra-cutoff) memakai klaster sekuens PDB yang sudah tersedia sebagai metadata (tanpa unduhan berat); keputusan implementasi di TOOLS_LOCK/amendment.

### 5.7 Apakah tier T0=30, T1=100, T2=300 berjustifikasi?

**Ya, dengan pembedaan pilot vs klaim yang sudah benar dan eksplisit** (T0 dilarang untuk klaim; G.4). Perhitungan daya T1 (Fisher-z ρ=0,3 → ≈85 → 100) diverifikasi ulang secara aritmetika — benar sebagai aproksimasi korelasi sederhana; **caveat N-efektif klaster (lihat 5.6.2) wajib masuk**. T2=300 untuk 6 strata (~50/sel) wajar. [MINOR] Tambahkan kalimat: "T1 = 100 protein **dengan ≥ X klaster**" (X ditetapkan setelah T0, pre-registered rule).

### 5.8 Data leakage, selection bias, benchmark bias, generalisasi

Register B1–B14 + residual R1–R14 adalah kekuatan dokumen (jujur, spesifik). Temuan:

1. [KRITIS — pra-registrasi] **Prosedur seleksi sampel belum ditetapkan.** §A.1 mendefinisikan populasi target, §A.4 menetapkan *target stratifikasi*, tetapi **tidak ada prosedur enumerasi kandidat & algoritma pemilihan** (frame → filter → urut → kuota → fallback). Akibat: sampel T0/T1 tidak dapat direplikasi; risiko pemilihan oportunistik (sadar/tidak). Koreksi wajib: pre-register frame (mis. query terdefinisi atas PDB/UniProt→AFDB), aturan deduplikasi klaster, kuota per stratum (mis. T0: ≥5 protein per bin mean-pLDDT bila tersedia; T1: proporsional dengan lantai), dan aturan "under-filled stratum dilaporkan, bukan ditutup diam-diam".
2. [KRITIS] **Konflik kelayakan bin rendah vs kriteria eksklusi**: A3.6 mengeksklusi protein mayoritas-IDR (>70%) tetapi A.4 menargetkan bin mean-pLDDT <70/<50 terisi — populasi AFDB∩PDB dengan mean pLDDT <50 dan struktur eksperimen coverage ≥80% kemungkinan **jarang**. Jangan menutup celah ini dengan asumsi: pre-register pengukuran yield (U3) + fallback: target bergeser ke **keterwakilan bin di level residu** (semua protein tetap diambil; klaim strata dilaporkan apa adanya) jika kuota protein-biner tak tercapai.
3. [SEDANG] **7 protein lokal**: jangan dipakai ulang sebagai bagian sampel analisis (sudah dilihat berulang saat desain; semua confident 85–98). Usulkan: label eksplisit "spesimen pembelajaran" dan eksklusi dari set analisis (kecuali laporan QC pipeline). Tidak ada di dokumen → tambah.
4. [SEDANG] Benchmark bias (hanya protein crystallizable/imagable) diakui B1 + pembatasan klaim G1.4 — OK, pertahankan kalimat batasan itu verbatim di semua output eksternal.
5. [MINOR] B6 taksonomi: usulkan minimum reporting (komposisi organisme & SCOP-ish kategori) di T0; tanpa target kuota (tidak realistis), cukup dilaporkan.

### 5.9 Apakah protokol menetapkan keputusan/ambang sebelum hasil?

**Ya untuk hampir semua** (G.1–G.5, D.3 "tidak ada bin digabung setelah melihat hasil", D.4, H.2). Lubang pra-registrasi yang tersisa = daftar 5.6.1–3, 5.6.4–5, 5.8.1–2, 5.4.1–5. Semua dapat ditutup lewat amendment **sebelum** T0 tanpa mengubah desain — dan wajib diberi cap "sebelum melihat hasil apa pun" (benar: belum ada hasil).

### 5.10 Biaya, penyimpanan, lisensi, atribusi, sumber data

- **Realistis.** File kecil (CIF AFDB ≈ 0,1–0,25 MB; PAE JSON ≈ 0,02–0,12 MB; entri PDB CIF ≈ 0,05–2 MB); T0 30 protein ≈ puluhan MB; T2 (300) bahkan dengan alternatif PDB tetap < 1 GB. Unduhan per-file terkontrol (policy §7) memadai; tidak perlu bulk download.
- [MINOR] Tambahkan ke spesifikasi unduh: **format mmCIF koordinat saja** (tanpa structure factor/validation), plus `toolUsed`/`modelCreatedDate`/`latestVersion` dari API AFDB (lihat 5.3 poin 3 dan §8).
- **Lisensi:** AFDB CC BY 4.0 & PDB CC0 1.0 terverifikasi live (§4.4). **Tool:** Foldseek **GPL-3.0**; OpenStructure **LGPL-3.0** (file LICENSE.txt); MMseqs2 **MIT** (per catatan rilis Release 16); **TM-align/US-align: tidak ada pernyataan lisensi di halaman resmi** → wajib diselesaikan sebelum distribusi/publikasi apa pun yang menyertakan tool (praktik umum Zhang group: gratis untuk akademik — harus dikonfirmasi tertulis/di halaman resmi saat TOOLS_LOCK). [SEDANG — bukan blocker T0, blocker publikasi]
- [MINOR] Sitasi/atribusi: update ke Fleming 2025 + [L1] (U7 sebagian terjawab); atribusi PDB "encouraged" (policy sudah lebih ketat — pertahankan).

---

## 6. Review Keputusan K-1 sampai K-7

Pertanyaan & opsi dikutip **persis** dari `PHASE2_DECISION.md` §6 (dan `RESEARCH_PROTOCOL_V1.md` H.2). Reviewer **tidak** menganggap keputusan pemilik sudah diberikan.

### K-1 — `git init` di root audit?
- **Pertanyaan & opsi (asli):** "`git init` di root audit? — Opsi: init / tidak. Rekomendasi audit: **init** (tanpa remote; riwayat dokumen & skrip tertelusur) — keputusan user, temuan F3."
- **Rekomendasi teknis reviewer:** **init** (lokal, tanpa remote), sebelum artefak T0 pertama dibuat; tambahkan `.gitignore` untuk `data/raw` berukuran besar bila diperlukan (keputusan format = pemilik). Manfaat riwayat terutama untuk dokumen + `TOOLS_LOCK.md` + manifest, sesuai temuan F3.
- **Bukti:** baseline F3 (root bukan repo Git); policy §8.8 melarang init tanpa persetujuan — keputusan ini justru persetujuan itu.
- **Risiko bila diambil:** rendah; risiko bila **tidak** diambil: ketertelusuran versi dokumen setelah amendment pertama bergantung pada file terpisah (lebih lemah).
- **Persetujuan eksplisit pemilik?** **Ya** (keputusan pemilik; dilarang otomatis).
- **Dampak ke T0:** tidak ada dampak sains; urutan disarankan: jawab K-1 **sebelum** file T0 pertama dibuat agar semuanya masuk riwayat.

### K-2 — Verifikasi cutoff data latih [L1]
- **Pertanyaan & opsi (asli):** "Verifikasi cutoff data latih [L1] — Opsi: verifikasi resmi / default sementara berlabel. Rekomendasi audit: **verifikasi sebelum dipakai**."
- **Rekomendasi teknis reviewer:** **Terima nilai terverifikasi `2018-04-30`** (untuk entri AFDB monomer F1) dan **catat verifikasinya di `PROTOCOL_AMENDMENTS.md`** (protokol §0.5, policy §5.3, U1) dengan kutipan sumber; tambahkan nuansa dua-cutoff (§4.3) + pencatatan `toolUsed`/`modelCreatedDate` per entri.
- **Bukti:** §4.3 (full text [L1] verbatim; metadata AFDB 7/7; catatan teknis v2.3.0).
- **Risiko bila diambil:** cutoff salah klasifikasi S3 — termitigasi karena nilai kini didukung sumber primer; residual (label pipeline & skenario regenerasi AFDB) didokumentasikan, bukan diasumsikan nol.
- **Persetujuan eksplisit pemilik?** **Ya** — pemilik perlu **meratifikasi** hasil verifikasi ini (review gate memberinya bukti; keputusan tetap miliknya).
- **Dampak ke T0:** S3 (pre/post-cutoff) dapat dijalankan; tanpa ratifikasi, kondisi pra-komitmen `PHASE2_DECISION.md` §7 berlaku (S3 di-drop) — tidak diperlukan lagi karena verifikasi berhasil.

### K-3 — Toolchain metrik + versi (`TOOLS_LOCK.md`)
- **Pertanyaan & opsi (asli):** "Toolchain metrik + versi (`TOOLS_LOCK.md`) — kandidat: TM-align, lDDT-python/foldseek, aligner, MMseqs. Rekomendasi audit: pilih & kunci sebelum metrik pertama."
- **Rekomendasi teknis reviewer (berdasarkan dokumentasi resmi yang dibaca 2026-10-11):**

| Peran | Tool (rekomendasi) | Versi/resmi | Catatan kunci |
|---|---|---|---|
| TM-score (C.2) | **US-align** (utama) atau TM-align | US-align: update resmi terakhir 2026/05/27; TM-align: source update 2022/04/12 (tanpa nomor versi) | US-align = penerus yang dipelihara; pin lewat commit/hash rilis; konvensi TM sama [L17, L18] |
| lDDT (C.3, C.5b) | **OpenStructure 2.12** `ost compare-structures --lddt --local-lddt --bb-lddt` | Docs 2.12 (pernyataan versi di situs resmi) | `--local-lddt` = per-residu (persis kebutuhan C.5b); lisensi LGPL-3.0 |
| RMSD/Superposisi (C.1, C.5a, C.8) | numpy/SVD (implementasi sendiri, Kabsch) **atau** US-align/OST sebagai cross-check | numpy versi dipin di requirements | deterministik; uji dengan rotasi sintetis |
| lDDT alternatif & pencarian/fallback | Foldseek | Release `10-941cd33` (GitHub releases, "Latest") | GPL-3.0; `lddt`/`lddtfull`, `--exact-tmscore`; pakai sebagai **cross-check S7** (implementasi lDDT berbeda) |
| Alignment/mapping model↔PDB | **SIFTS (PDBe) residue-level mapping** sebagai sumber primer + aligner sekuens sebagai verifikasi | API PDBe/SIFTS; aligner: Biopython `PairwiseAligner` (versi dipin) | SIFTS menangani residu termodifikasi & penomoran; aligner = cross-check independen (pola §7.7 policy) |
| Clustering klaster homolog (≤30%) | MMseqs2 `easy-cluster`/`linclust` | Release `18-8cc5c` (GitHub releases, "Latest") | MIT (per catatan rilis); pin `--min-seq-id 0.3` + mode coverage; simpan daftar klaster |
| Domain/disorder (D.5) | InterPro/Pfam (prioritas 1, API resmi) | rilis InterPro dipin saat implementasi | fallback geometrik deterministik: kandidat dipilih & diuji di TOOLS_LOCK (mis. segmentasi berbasis struktur dengan metode publik yang dipin) — **satu metode tetap untuk semua protein** |
| Parsing CIF | gemmi (pustaka) | versi dipin (pip) | alternatif: OST; uji index PAE↔CIF (5.5) |
| Statistik | numpy/scipy (dipin) | versi dipin di requirements | B=10.000, seed tetap |

- **Risiko yang harus ditutup di TOOLS_LOCK:**
  1. **Platform Windows**: sebagian tool (OpenStructure, Foldseek, MMseqs2) lazim didistribusikan untuk Linux/macOS; ketersediaan binary **Windows native tidak terverifikasi** dalam review ini → rencanakan WSL2 atau conda/`mamba` (diverifikasi saat implementasi; bukan di fase ini).
  2. **Lisensi TM-align/US-align tidak dinyatakan** di halaman resmi → selesaikan sebelum publikasi (akademik umumnya bebas).
  3. **Uji determinisme** tiap tool (input sama → output identik) sebelum metrik pertama; output tool mentah disimpan sebagai artefak turunan ber-hash.
- **Persetujuan eksplisit pemilik?** **Ya** (memilih & mengunci; instalasi = fase implementasi).
- **Dampak ke T0:** **tinggi** — tanpa lock, T0 tidak dapat menghitung metrik; disarankan TOOLS_LOCK selesai **sebelum** unduhan dimulai.

### K-4 — Definisi domain & disorder (D.5)
- **Pertanyaan & opsi (asli):** "Definisi domain & disorder (D.5 protokol) — Opsi: UniProt/Pfam prioritas; fallback geometrik deterministik. Rekomendasi audit: kunci sebelum analisis domain."
- **Rekomendasi teknis reviewer:** pertahankan prioritas adalah benar; tambahkan: (a) versi rilis InterPro/Pfam & MobiDB dipin; (b) fallback = **satu** metode geometrik deterministik yang dipin untuk semua protein (bukan pilihan per protein), diuji determinismenya; (c) definisi "disordered/missing eksperimen" untuk A3.6/S5 harus operasional (sumber anotasi + aturan bila kosong). Domain dari **struktur referensi** (anti-sirkularitas) — dipertahankan.
- **Bukti:** D.5; B7/R7; U6 (ketersediaan definisi deterministik diuji di T0).
- **Risiko bila diambil:** hasil RQ3 sensitif terhadap segmentasi (R8 diakui); mitigasi = metode dipin + S8/fold-switching literature [L25–L29] menunjukkan pentingnya.
- **Persetujuan eksplisit pemilik?** Ya (lock); bisa menyusul sebelum titik pakainya (analisis domain), tidak memblokir unduhan/mapping T0.
- **Dampak ke T0:** sedang — T0 menguji **cakupan** definisi (U6) atas 30 protein; keputusan akhir lock = sebelum RQ3 dijalankan.

### K-5 — Tier & anggaran unduhan
- **Pertanyaan & opsi (asli):** "Tier & anggaran unduhan — Opsi: T0 dulu (30). Rekomendasi audit: **T0 dulu**; gate review sebelum T1."
- **Rekomendasi teknis reviewer:** setujui T0=30 dulu; **tetapkan anggaran konkret** (mis. T0 ≤ ±150 MB, format mmCIF + PAE JSON + metadata; tanpa bulk; batas angka final = pemilik); approval per-tier sebagai gate (P3→P4).
- **Bukti:** F.2/F.4; policy §2.2/§7; estimasi ukuran §5.10 (realistis).
- **Risiko bila diambil:** rendah; utama = scope creep volume → dikendalikan angka cap + per-file logging.
- **Persetujuan eksplisit pemilik?** **Ya** (gate unduhan; angka anggaran = keputusan).
- **Dampak ke T0:** **tinggi** — tanpa persetujuan ini tidak ada unduhan (stop condition).

### K-6 — Environment (venv baru di root audit)
- **Pertanyaan & opsi (asli):** "Environment — Opsi: venv baru di root audit. Rekomendasi audit: **ya** (isolasi total dari `ai-biotech`)."
- **Rekomendasi teknis reviewer:** **ya**; Python 3.12.x (menyamai pipeline lokal 3.12.10 — mengurangi risiko perbedaan perilaku pustaka); `requirements.txt` dengan versi **terpin + hash**; tidak ada import silang dari `ai-biotech`; verifikasi ulang platform Windows/WSL untuk tool non-Python (lihat K-3).
- **Bukti:** baseline §3.4 (venv induk 3.12.10, tanpa tool struktur); policy §3.
- **Risiko bila diambil:** rendah; tanpa ini → polusi lingkungan induk (dilarang).
- **Persetujuan eksplisit pemilik?** Ya (bagian paket keputusan).
- **Dampak ke T0:** **tinggi** — prasyarat semua instalasi.

### K-7 — Atribusi & lisensi output publikasi
- **Pertanyaan & opsi (asli):** "Atribusi & lisensi output publikasi — Opsi: kebijakan user. Rekomendasi audit: susun draf atribusi saat laporan T1."
- **Rekomendasi teknis reviewer:** setuju draf disusun di laporan T1; **update pin sitasi sekarang** (informasi baru §4.4): sitasi dataset = Fleming 2025 (`10.1016/j.jmb.2025.168967`) + [L1]; [L39]/[L40] sebagai rujukan historis; AFDB CC BY 4.0 (atribusi EMBL-EBI/DeepMind/UniProt); PDB CC0 1.0 (atribusi praktik baik); lisensi tool (Foldseek GPL-3.0, OST LGPL-3.0, MMseqs2 MIT, TM-align/US-align TBD) diselesaikan sebelum distribusi kode.
- **Bukti:** §4.4 (halaman resmi); policy §4; U7.
- **Risiko bila diambil:** rendah; risiko bila ditunda ke publikasi: sitasi usang (Varadi 2024 tidak lagi di halaman resmi) — sudah diantisipasi U7, kini terverifikasi.
- **Persetujuan eksplisit pemilik?** Tidak wajib untuk T0 (keputusan bentuk = pemilik; verifikasi pin = tugas implementasi).
- **Dampak ke T0:** tidak ada (T0 tidak publikasi); berdampak ke laporan T1+.

---

## 7. Daftar Blocker

### Blocker KRITIS (harus selesai sebelum T0 dimulai)

| # | Blocker | Jenis | Target koreksi |
|---|---|---|---|
| KB-1 | **Prosedur seleksi sampel belum ditetapkan** (frame → filter → kuota → fallback) | Pra-registrasi | Amendment protokol §A.4/B (lampiran sampling baru) |
| KB-2 | **Definisi klaster + jumlah klaster + N-efektif + parameter bootstrap (B, seed)** belum terkunci | Pra-registrasi + TOOLS_LOCK | Amendment §A.4/D.2 + `TOOLS_LOCK.md` |
| KB-3 | **Aturan bin kosong/uji berpasangan/G1.1-G.2** belum dispesifikasi (termasuk wording falsifikasi H2/H4) | Pra-registrasi | Amendment §D.3/D.4/G.1/G.2 |
| KB-4 | **Aturan tepi mapping**: residu termodifikasi (MSE dkk.), isoform (canonical SIFTS), pemilihan chain, model NMR, definisi operasional apo; klarifikasi A3.1 vs B.1.6 | Aturan implementasi | Amendment §B (+TOOLS_LOCK) |
| KB-5 | **`TOOLS_LOCK.md` belum ada** (versi+hash+determinisme+platform; lisensi tool diselesaikan) | Konfigurasi | Dibuat di fase implementasi **sebelum** unduhan/metrik pertama (K-3) |
| KB-6 | **Ratifikasi pemilik**: K-1..K-7 dijawab (minimal K-2 nilai terverifikasi, K-3 lock, K-5 anggaran, K-6 venv) | Keputusan | §6 di atas |

### Blocker NONKRITIS (tidak memblokir T0; harus selesai sebelum titik pakainya)

| # | Item | Tenggat |
|---|---|---|
| NB-1 | Penanda "spesimen pembelajaran" untuk 7 protein lokal (eksklusi dari set analisis) | Amendment saat T0 |
| NB-2 | Pencatatan `toolUsed`/`modelCreatedDate`/`latestVersion` per entri di manifest | Saat implementasi unduh |
| NB-3 | Konflik kelayakan bin rendah vs A3.6 → mekanisme yield T0 + fallback bin level-residu (pre-registered) | Amendment sebelum T0 |
| NB-4 | Sumbu kedua B13 ("cluster-exposed pra-cutoff") via klaster sekuens PDB | Sebelum pelaporan T1 |
| NB-5 | C.5b dipin ke OST `local_lddt`; C.4 GDT dipin/drop; index PAE↔CIF diuji | TOOLS_LOCK |
| NB-6 | Kutipan [L7] kalimat penuh diverifikasi sebelum dipakai verbatim di publikasi | Sebelum publikasi |
| NB-7 | Update policy §4: lisensi terverifikasi live (AFDB CC BY 4.0; PDB CC0 1.0) + pin sitasi Fleming 2025 | Amendment pertama |
| NB-8 | Lisensi TM-align/US-align diselesaikan (halaman resmi tanpa pernyataan) | Sebelum distribusi/publikasi tool |

---

## 8. Koreksi yang Wajib Dilakukan Sebelum T0 (usulan amendment — menunggu persetujuan)

Semua koreksi **tidak mengubah dokumen sumber sekarang**; usulan ini masuk `docs/PROTOCOL_AMENDMENTS.md` hanya setelah persetujuan pemilik (protokol §G.5; policy §9). Tidak ada hasil eksperimen yang dilihat saat penyusunan — penanda "sebelum hasil" berlaku sah.

| # | Dokumen target | Koreksi | Sifat |
|---|---|---|---|
| C-1 | Protokol §A.4 (+ lampiran baru) | Tetapkan prosedur seleksi sampel lengkap: definisi frame (query UniProt→AFDB F1 × PDB), deduplikasi klaster, kuota per strata (T0/T1), aturan tie-break deterministik, aturan under-filled stratum (dilaporkan, bukan ditutup), alur eksklusi satu-per-satu | Wajib (KB-1) |
| C-2 | Protokol §A.4 + §D.2 + TOOLS_LOCK | Kunci: algoritma klaster (MMseqs2, parameter), definisi N-efektif = jumlah klaster; kewajiban melaporkan jumlah klaster; aturan gate T1 bila klaster sedikit; B=10.000 + seed | Wajib (KB-2) |
| C-3 | Protokol §D.3/D.4/G.1/G.2 | Pre-spesifikasi: definisi protein yang masuk uji berpasangan RQ1 (mis. ≥10 residu di kedua bin); aturan monotonicity G1.1 (protein dengan ≥1 residu di tiap bin; jumlah dilaporkan); wording falsifikasi H2 (ρ>0,8 & batas bawah CI ≥0,7) dan H4 (frekuensi+CI, tanpa ambang "batal"); label deskriptif untuk G1.1 | Wajib (KB-3) |
| C-4 | Protokol §B (+§A.5) | Aturan tepi mapping: normalisasi residu termodifikasi (via SIFTS residue mapping; daftar parent standar); isoform = canonical-only; pemilihan chain deterministik; model NMR (model pertama + sensitivitas); definisi operasional apo/holo (HETATM teranotasi dalam radius yang disepakati); klarifikasi A3.1 (larangan = endpoint kompleks, bukan keberadaan multi-chain di entri) | Wajib (KB-4) |
| C-5 | `TOOLS_LOCK.md` (baru) | Isi sesuai tabel K-3: nama tool, versi/commit, hash, cara instalasi (WSL2/conda bila perlu), uji determinisme, lisensi; cross-check lDDT (OST vs Foldseek) pada ≥3 pasang uji | Wajib (KB-5) |
| C-6 | Protokol §0.5 + §B.1.5 + policy §5.3 + `PHASE2_DECISION.md` U1 | Catat cutoff **terverifikasi `2018-04-30`** dengan sumber (full text [L1] verbatim + `toolUsed` AFDB), plus nuansa dua-cutoff (§4.3) dan instruksi pencatatan `toolUsed`/`modelCreatedDate`/`latestVersion` per entri di manifest | Wajib (KB-6, ratifikasi) |
| C-7 | Policy §4 + sitasi | Update: lisensi AFDB CC BY 4.0 & PDB CC0 1.0 (terverifikasi live 2026-10-11); pin sitasi dataset Fleming 2025 (`10.1016/j.jmb.2025.168967`) + [L1]; status Varadi [L39]/[L40] sebagai rujukan historis | Disarankan (NB-7) |
| C-8 | Protokol §A.4/NB-3 | Penanda "spesimen pembelajaran" 7 protein lokal; mekanisme pengukuran yield bin di T0 + fallback bin level-residu | Disarankan (NB-1/NB-3) |
| C-9 | Protokol §C | C.5b → OST `local_lddt`; keputusan C.4 (pin LGA atau drop); catatan uji pemetaan indeks PAE↔CIF; klarifikasi C.5a (domain=global untuk protein satu-domain) | Disarankan (NB-5) |
| C-10 | `LITERATURE_REVIEW.md` (catatan) | Tambahkan catatan U7: sitasi resmi AFDB per 2026-10-11 = Fleming 2025 + [L1] (DOI terverifikasi pipa); kutipan [L7] tetap wajib verifikasi full text sebelum dipakai verbatim; ambang bin pLDDT kini punya dukungan full text [L5] | Disarankan |

---

## 9. Rekomendasi Akhir & Syarat Eksplisit Mengizinkan T0

### **CONDITIONAL GO**

Desain riset layak dan koheren; literatur terverifikasi; integritas artefak utuh; cutoff terjawab; toolchain siap dikunci. T0 **diizinkan hanya bila seluruh syarat berikut terpenuhi** (berurutan):

1. **Persetujuan dokumen.** Pemilik menyetujui kelima dokumen Phase 2 (atau menyetujui dengan amendment C-1..C-10).
2. **Jawaban keputusan §6** — minimal K-2 (ratifikasi cutoff `2018-04-30` terverifikasi), K-3 (menyetujui daftar toolchain untuk dikunci), K-5 (anggaran unduhan T0, angka konkret), K-6 (venv). K-1 diputuskan sebelum file T0 pertama dibuat; K-4/K-7 sebelum titik pakainya.
3. **Koreksi wajib C-1..C-6 diterapkan** ke `PROTOCOL_AMENDMENTS.md` (+ dokumen terkait) — semuanya sebelum unduhan pertama.
4. **`TOOLS_LOCK.md` selesai** dengan uji determinisme lulus (termasuk keputusan platform WSL2/conda bila diperlukan).
5. **Urutan eksekusi dipatuhi:** T0 (30 protein, hanya pipeline) → laporan T0 → gate review pemilik → T1. Tidak melompat; tidak ada klaim dari T0.
6. **Stop-condition dipertahankan:** tanpa butir 1–4, tidak ada unduhan/instalasi/eksekusi; `ai-biotech` tetap read-only.

**Mengapa bukan GO tanpa syarat:** enam blocker kritis pra-registrasi (sampling, klaster, bin, mapping, tools, ratifikasi) adalah syarat *interpretabilitas jujur* hasil pertama — bukan formalitas.

**Mengapa bukan HOLD:** tidak ada temuan yang membatalkan desain; semua blocker dapat diselesaikan sebagai amendment dokumentasi sebelum T0 tanpa mengubah sains; cutoff kini terverifikasi sehingga kondisi pra-komitmen "S3 di-drop" di `PHASE2_DECISION.md` §7 tidak aktif.

**Kondisi turnjadi HOLD:** bila pemilik memilih tidak menerapkan C-1..C-6 → tahan di posisi sekarang (lima dokumen tetap deliverable final yang koheren), karena hasil T0 tanpa pra-registrasi seleksi/klaster/bin tidak akan dapat dipertahankan secara metodologis.

---

## 10. Referensi yang Benar-benar Diverifikasi pada Review Ini (2026-10-11)

**Primer (dibaca/terverifikasi langsung):**

1. 40 sumber [L1]–[L40] — tabel lengkap & status di §4.1 (39 × Europe PMC `resultType=core`; L23 × Crossref). Termasuk full text [L1] (PMC8371605) dan [L5] (PMC8387240).
2. Fleming J, Magana P, Nair S, et al. *AlphaFold Protein Structure Database and 3D-Beacons: New Data and Capabilities.* J Mol Biol (2025). DOI `10.1016/j.jmb.2025.168967`, PMID 40133787 (verifikasi Europe PMC; DOI dari pipa, bukan dari sumber sekunder).
3. DeepMind, *AlphaFold v2.3.0 technical note* (repo `google-deepmind/alphafold`, `docs/technical_note_v2.3.0.md`) — kutipan verbatim §4.3.
4. Halaman resmi AFDB: `https://alphafold.ebi.ac.uk/download` (daftar sitasi + "Data is available for academic and commercial use, under a CC-BY-4.0 licence") dan `License-Disclaimer.pdf` (CC BY 4.0; copyright DeepMind).
5. Metadata AFDB lokal: 7 × `api_<acc>.json` (field `toolUsed` = "AlphaFold Monomer v2.0 pipeline"; `modelCreatedDate` 2025-08-01; `latestVersion` 6) — read-only di `ai-biotech`.
6. RCSB PDB Usage Policies (`https://www.rcsb.org/pages/usage-policy`): kutipan CC0 1.0 (§4.4).
7. Dokumentasi resmi tool: `zhanggroup.org/TM-align/` (update 2022/4/12; tanpa pernyataan lisensi), `zhanggroup.org/US-align/` (update resmi terbaru 2026/05/27; tanpa pernyataan lisensi), `github.com/steineggerlab/foldseek` (GPL-3.0) + releases (Latest: `10-941cd33`; `lddt`/`lddtfull`, `--exact-tmscore`), `openstructure.org` (2.12) + docs `compare-structures` (`--lddt`, `--local-lddt`, `--bb-lddt`) + `LICENSE.txt` (LGPL-3.0), `github.com/soedinglab/MMseqs2/releases` (Latest: `18-8cc5c`; MIT per catatan Release 16).

**Catatan batas:** verifikasi pada level yang dinyatakan §3; tidak ada klaim verifikasi untuk hal yang tidak diperiksa (mis. FAQ AFDB dinamis, ketersediaan binary Windows native tool — keduanya ditandai eksplisit).

---

## Lampiran A — Perintah Verifikasi yang Dapat Direproduksi (read-only)

```bash
# 1. Hash kelima dokumen
cd "D:/project/alphafold-reliability-audit/docs" && sha256sum PHASE1_BASELINE_AUDIT.md LITERATURE_REVIEW.md RESEARCH_PROTOCOL_V1.md DATA_AND_BIAS_POLICY.md PHASE2_DECISION.md

# 2. Verifikasi DOI (pipa Europe PMC; contoh)
export PYTHONIOENCODING=utf-8
"$VENV_INDUK/Scripts/python.exe" "$TMPDIR/source_verify.py" doi 10.1038/s41586-021-03819-2 10.1002/prot.26192 10.1016/j.jmb.2025.168967

# 3. Full text [L1] — cari cutoff (Europe PMC fullTextXML)
#    https://www.ebi.ac.uk/europepmc/webservices/rest/PMC8371605/fullTextXML
#    (cari: "maximum release date of 30 April 2018")

# 4. Status repo induk (read-only)
git -C "D:/project/ai-biotech" status --short && git -C "D:/project/ai-biotech" rev-parse HEAD
```

## Lampiran B — Ketidakpastian Residual yang Tetap Terbuka (tidak ditutup dengan asumsi)

1. Label pipeline "v2.0" pada entri AFDB 2025-08: pernyataan penyedia; tidak ada publikasi DeepMind yang mengonfirmasi ketiadaan retraining monomer secara diam-diam. Mitigasi: kutip "sesuai metadata resmi AFDB" + catat per entri.
2. Ketersediaan binary Windows native (OST/Foldseek/MMseqs2) — verifikasi saat TOOLS_LOCK; kemungkinan WSL2/conda.
3. FAQ resmi AFDB tidak terbaca saat review (situs dinamis) — bila FAQ menyatakan hal berbeda tentang versi model, amendemen C-6 direvisi.
4. Kutipan [L7] bagian "local scale…" — verifikasi full text tertunda (sudah ditandai dokumen; dipertahankan).
5. Volume/angka yield riil per stratum (U3) dan jumlah klaster riil (KB-2) — hanya dapat diukur di T0; aturan fallback sudah diusulkan (C-3/C-8) agar tidak ditutup post-hoc.

---

*Laporan ini adalah artefak review read-only. Kelima dokumen Phase 2 tidak diubah (hash di §2 tetap berlaku). Analisis ini memverifikasi ulang apa yang dapat diverifikasi dan menandai eksplisit apa yang tidak. Verifikasi SHA256 laporan ini dihitung setelah file ditulis dan dilaporkan di sesi (laporan tidak dapat memuat hash dirinya sendiri — konvensi sama dengan `PHASE2_DECISION.md`).*

**STOP CONDITION: menunggu keputusan pemilik proyek. Tidak ada implementasi T0 tanpa persetujuan eksplisit.**
