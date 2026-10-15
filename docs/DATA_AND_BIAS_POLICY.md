# DATA AND BIAS POLICY — AlphaFold Reliability Audit

## Kebijakan Sumber Data, Versioning, Lisensi, Register Bias & Praktik Terlarang

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal** | 2026-10-11 |
| **Status** | SELESAI (kebijakan — mengikat fase implementasi) |
| **Dasar** | `PHASE1_BASELINE_AUDIT.md` (F1–F4), `LITERATURE_REVIEW.md` ([L1]–[L40]), `RESEARCH_PROTOCOL_V1.md` (§A–§H) |
| **Mengikat** | Seluruh fase implementasi riset ini. Pelanggaran terhadap §8 (praktik terlarang) membatalkan klaim hasil. |

---

## 1. Prinsip Umum

1. **Immutability:** file mentah (prediksi & eksperimen) tidak pernah diedit, dipangkas, atau "diperbaiki". Semua transformasi terjadi di lapisan turunan yang dapat diregenerasi.
2. **Traceability:** setiap file data punya: URL sumber, tanggal unduh, versi, SHA256, dan catatan lisensi. Tanpa ini, file tidak boleh dipakai dalam analisis.
3. **No silent changes:** setiap pengecualian, konversi, atau keputusan penyaringan dicatat di manifest dengan alasan. Tidak ada filter diam-diam (warisan pelajaran dari D1–D2 & C1–C2 proyek referensi).
4. **No fabrication:** metrik/angka hanya dari output tool yang dijalankan; tidak ada nilai yang ditulis dari ingatan atau estimasi tanpa label "estimasi".
5. **Isolasi:** seluruh artefak riset berada di `D:\project\alphafold-reliability-audit`. `D:\project\ai-biotech` **read-only** — larangan menulis/mengubah apa pun di sana (aturan user 2026-10-11, dikonfirmasi bersih di baseline §1.2).

---

## 2. Sumber Data Resmi

### 2.1 Sumber prediksi (objek audit)

| Aspek | Ketentuan |
|---|---|
| Sumber | AlphaFold DB resmi (EMBL-EBI & Google DeepMind), `https://alphafold.ebi.ac.uk/` [L39, L40] |
| Entri | Single-chain (F1); versi model dicatat per unduhan (baseline lokal: v6) |
| File per entri | Model CIF + PAE JSON (pola sama dengan Tahap 5 referensi) |
| Alasan memakai AFDB (bukan Server) | Entri AFDB = artifact publik yang diaudit "apa adanya" (tanpa intervensi MSA/pilihan user); Server tetap spesimen pembelajaran, **bukan** sumber dataset (DATASET_PLAN §2) |
| Alternatif lain | **Dilarang di V1:** output AlphaFold Server, model AF2 yang di-sample ulang (multi-seed), AF3 [L6] — menjaga objek audit = distribution publik AFDB |

### 2.2 Sumber eksperimen (ground truth)

| Aspek | Ketentuan |
|---|---|
| Sumber | PDB / wwPDB via API resmi: RCSB PDB data API [L33] dan/atau PDBe/SIFTS (mapping UniProt↔PDB) |
| Metadata wajib per struktur | PDB ID, metode (X-ray/cryo-EM/NMR), resolusi/R-free (bila ada), tanggal rilis, status (current/obsolete), ligan (HETATM), residu missing |
| Pemilihan | Prosedur deterministik §B.1 protokol; primer + alternatif untuk uji sensitivitas |
| Yang dilarang | Menyusun "struktur referensi" dari model/prediksi; memakai struktur AlphaFold yang sudah ada di RCSB (computed models [L33]) sebagai ground truth |
| Volume | Unduhan terkontrol per file (CIF/PDB format kecil); **bukan bulk download, bukan scraping**. Batas volume disetujui user sebelum fase unduh (K-5 protokol) |

### 2.3 Larangan pencampuran peran

Peran dua sumber dipisahkan tegas (warisan DATASET_PLAN §2):
- AFDB = prediksi yang diaudit.
- PDB = referensi eksperimen.
- AlphaFold Server output (ubiquitin, dll.) = **bahan pembelajaran dokumentasi saja** — tidak boleh masuk dataset audit.

---

## 3. Isolasi & Struktur Kerja (diusulkan; dibuat pada fase implementasi)

```
D:\project\alphafold-reliability-audit\
├── docs\                ← dokumen (fase 2 — sudah ada: 5 dokumen)
├── data\                ← (fase implementasi) raw/ (immutable) + metadata/
├── results\             ← (fase implementasi) output turunan + manifest
├── src\                 ← (fase implementasi) script analisis + test
└── .venv\               ← (fase implementasi) environment terpisah
```

- Environment **terpisah** dari `ai-biotech` (keputusan K-6). Tidak ada import silang file dari repo induk dalam pipeline (referensi silang hanya di level dokumen).
- File mentah di `data/raw/` diberi README yang **akurat** (pelajaran D2/F1: folder kosong/tidak kosong harus sesuai pernyataan).
- `download_log.txt`-style log (timestamp lokal) hanya untuk log operasional; **output analisis tidak boleh memuat timestamp/path absolut** (aturan determinisme proyek referensi, `PROJECT_STATUS.md` §14).

---

## 4. Lisensi & Atribusi

| Data | Lisensi/status | Kewajiban |
|---|---|---|
| AlphaFold DB | **CC BY 4.0** (pernyataan resmi situs, dicek 2026-10-05 di DATASET_PLAN §4) | Atribusi **EMBL-EBI, Google DeepMind, UniProt**; sitasi [L39]/[L40]; cek halaman sitasi resmi (yang menyebut paper dataset terbaru) saat publikasi |
| PDB | Data tersedia bebas untuk riset (wwPDB) | Atribusi RCSB PDB/wwPDB + PDB ID + (bila relevan) depositor/paper struktur |
| Output riset (dokumen/kode/angka) | Kebijakan user | Default: dokumen kerja internal berbahasa Indonesia; keputusan lisensi publikasi = user |

Catatan kejujuran: klaim lisensi di atas bersumber dari dokumen referensi (dicatat 2026-10-05); **tidak dicek ulang ke server live pada fase ini** (baseline §5). Sebelum publikasi eksternal, verifikasi ulang ke halaman resmi.

---

## 5. Versioning & Freeze

1. **Pencatatan versi di tiga titik:** (a) versi AFDB saat unduh (mis. v6), (b) tanggal query API, (c) SHA256 file. Perubahan versi AFDB di masa depan tidak menimpa dataset yang sudah dibekukan — dataset baru = dataset baru, dengan hash baru (pelajaran C6).
2. **PDB revision:** sebelum dianalisis, cek status entri (current/obsolete). Struktur obsolete/superseded diganti dengan versi pengganti dan dicatat (A3.5 protokol).
3. **Cutoff data latih [L1]:** tanggal cutoff resmi **wajib diverifikasi dari full text/supplementary paper [L1] sebelum filter tanggal dipakai** (nilai default sementara 2018-04-30 diberi tanda TBD di protokol §0.5). Dilarang mengisi nilai ini dari ingatan.
4. **Freeze manifest sebelum analisis:** manifest pencocokan (§B.1 protokol) dibekukan (hash) sebelum metrik dihitung; perubahan setelahnya = amendment tercatat.
5. **Freeze hasil:** hasil final di-hash; laporan menyertakan hash (kebiasaan proyek referensi §10.4).

---

## 6. Register Bias (lengkap; ringkasan eksekusi ada di protokol §E)

Legenda mitigasi: **M-** = aksi di protokol; **R-** = residual risk yang diakui.

| # | Bias / masalah | Mekanisme | Dampak | Mitigasi | Residual risk |
|---|---|---|---|---|---|
| **B1** | Bias seleksi eksperimen | Hanya protein crystallizable/imagable yang ada di PDB ([L1] ~100 ribu vs miliaran sekuens) | Set ter-audit tidak mewakili proteom | Klaim dibatasi ke "protein yang punya struktur eksperimen"; stratifikasi panjang/fungsi | R1: generalisasi ke proteom penuh tidak boleh diklaim |
| **B2** | Leakage / memorization | Prediksi pada homolog data latih bisa sukses karena memorization [L24, L26, L38] | Overestimate generalisasi | Klaster homolog 30%; strata pre/post-cutoff (S3); laporan terpisah | R2: daftar data latih AF2 tidak publik penuh — stratifikasi hanya proxy |
| **B3** | Ground truth tidak sempurna | Struktur eksperimen pun punya error/model bias [L30] | Sebagian "error audit" milik referensi | Metadata resolusi/R-free; sensitivitas S1/S2 | R3: tanpa peta eksperimen, error referensi tidak bisa diukur langsung |
| **B4** | Konformasi tunggal | Prediksi & referensi = dua snapshot; state bisa berbeda [L25, L27–L29, L31] | Selisih konformasi terbaca sebagai "error" | Label state per protein; S4 apo-vs-holo; bahasa laporan "terhadap state X" | R4: state in-solution/in-cell tidak terwakili |
| **B5** | Model tanpa ligan/ion/kovalen | AFDB menghapus HETATM [L14]; konkret di dataset lokal: heme, Cu/Zn, kromofor GFP (C12) | Pocket kimiawi & efek ligan tidak tertangkap | Endpoint pocket/ligan ditunda & dinyatakan keluar lingkup (§C.13 protokol) | R5: audit tidak menjawab "pocket akurat?" — pertanyaan terpisah [L16] |
| **B6** | Bias taksonomi/fungsi | Dataset mudah terjebak manusia-globular (dataset lokal: 5/7 manusia; DATASET_PLAN §6) | Stratifikasi tidak seimbang | Stratifikasi sampling; laporan komposisi aktual | R6: komposisi populasi AFDB vs PDB tetap tidak identik |
| **B7** | pLDDT rendah ≠ error | pLDDT rendah sering = disorder/fleksibilitas [L10, L32] | Salah tafsir low-bin sebagai kegagalan | S5 (residu disorder eksperimen dipisah); bahasa laporan dipisah | R7: definisi disorder eksperimen juga heuristik |
| **B8** | PAE asimetris | |PAE − PAEᵀ| ≈ 19–24 Å (7/7 protein lokal, C8) | Interpretasi arah blok keliru | Mean dua arah di C.6/C.7; uji di QC | R8: definisi "cross-domain block" bergantung segmentasi domain (D.5) |
| **B9** | Pseudoreplikasi residu | 1098 residu ≠ 1098 sampel | CI terlalu sempit, signifikansi palsu | Unit = protein/klaster (klaim F; §A.4, D.1–D.3 protokol) | R9: — |
| **B10** | Multiplicity | Banyak uji dari banyak sel | False positive | Holm pada 4 primer; label eksploratori (§D.4) | R10: sensitivitas tetap eksploratori by design |
| **B11** | Anomali penomoran sekuens | Konstruk PDB ≠ sekuens UniProt kanonik eksak (mis. RNase1 prekursor 1–150 vs mature 124; C11) | Alignment salah → error palsu | A2.5 (wild-type, region colinear), flag indel (§B.2), eksklusi tercatat | R11: isoform/variant alami di PDB bisa lolos filter |
| **B12** | Granularitas pLDDT AFDB | CIF AFDB menulis pLDDT per-residu ke semua atom → kolom atom redundan (C2) | Multikolinearitas jika dipakai ML | V1 tidak memakai fitur atom; hanya 1 kanal pLDDT (C.10) | R12: — (V1) |
| **B13** | Temporal confusion | Struktur PDB rilis setelah cutoff AF2 **bukan** otomatis "untuk generalisasi" jika homolognya ada di data latih | Klaim "blind" berlebihan | Dua sumbu terpisah & dilaporkan keduanya: (i) rilis PDB vs cutoff [L15 pola], (ii) kemiripan sekuens ke PDB pra-cutoff (klaster) | R13: proxy homolog ≠ definisi resmi data latih (sama dengan R2) |
| **B14** | Pilihan struktur referensi | Satu protein bisa punya puluhan entri PDB | Hasil bergantung pemilihan | Pemilihan deterministik + S8 (struktur alternatif) | R14: pemilihan terbaik menurut resolusi ≠ paling relevan biologis |

---

## 7. Aturan Penanganan Data (fase implementasi)

1. **Unduh:** satu entri pada satu waktu; verify HTTP 200, ukuran, SHA256; simpan log; manifest diperbarui per file (pola identik Tahap 5 referensi — pola terbukti).
2. **Klasifikasi folder:** `raw/` immutable; turunan (konversi, mapping, metrik) di `results/` dengan referensi hash sumber.
3. **Konversi format** (CIF↔PDB dsb.) hanya sebagai artifact turunan; file raw tetap asli.
4. **Eksepsi & pengecualian:** setiap baris yang di-exclude dari analisis wajib muncul di tabel eksklusi dengan alasan (tidak boleh hilang dari pandangan).
5. **Imputasi:** dilarang. Nilai missing = missing (dilaporkan), tidak diisi. (Warisan aturan C1/C2: kolom bermasalah dikeluarkan/dinyatakan, bukan didefinisikan ulang diam-diam.)
6. **Determinisme output:** tanpa timestamp/seed acak/path absolut di output data; teks CSV = LF + UTF-8 (aturan proyek referensi §14).
7. **Cross-check silang:** setiap angka ringkasan dihitung dari file mentah oleh script yang berbeda dari script produksi (pola test suite referensi) — berlaku untuk T1+.

---

## 8. Praktik yang DILARANG (mengikat seluruh fase)

1. Menulis/mengubah apa pun di `D:\project\ai-biotech` (read-only; termasuk "sekadar merapikan").
2. Mengunduh dataset volume besar, scraping massal, atau memakai mirror tidak resmi.
3. Mengubah/mengedit file raw; menghapus residu "mengganggu"; menyesuaikan struktur referensi agar cocok dengan prediksi.
4. Memilih/membuang protein setelah melihat arah hasil (cherry-picking); menggabungkan/mengganti bin pLDDT setelah data dilihat.
5. Memakai metrik tool tanpa versi terkunci; mencampur versi tool dalam satu tabel hasil.
6. Menyajikan estimasi/ingatan sebagai fakta (termasuk DOI, angka cutoff, statistik). Semua DOI dari `LITERATURE_REVIEW.md`; nilai cutoff dari verifikasi full text.
7. Mengklaim generalisasi melebihi §6 (proteom penuh, sel, aplikasi obat) atau "pertama di dunia".
8. Komit/push ke remote apa pun, atau `git init` di root audit, **tanpa persetujuan eksplisit user** (temuan F3 terbuka).
9. Menjalankan tool/unduhan pada fase ini (fase dokumen): Phase 2 = dokumen saja.

---

## 9. Amendment & Audit Trail

- Perubahan kebijakan ini setelah tanggal = dicatat di `docs/PROTOCOL_AMENDMENTS.md` (dibuat saat perubahan pertama): tanggal, bagian yang diubah, alasan, status sebelum/sesudah melihat hasil.
- Semua dokumen fase ini di-hash (SHA256) pada laporan akhir Phase 2; perubahan berkas setelah hash = versi baru dengan penanda.

---

*Kebijakan ini disusun dari bukti aktual (`PHASE1_BASELINE_AUDIT.md`) dan 40 sumber terverifikasi (`LITERATURE_REVIEW.md`). Tidak ada kebijakan yang mengasumsikan kondisi server/data yang belum diverifikasi — semua item UNKNOWN ditandai eksplisit (mis. cutoff [L1], lisensi live).*
