# PHASE 1 — BASELINE AUDIT
## Kondisi Proyek & Dataset pada Awal "AlphaFold Reliability Audit"

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal audit** | 2026-10-11 (seluruh pemeriksaan dijalankan pada tanggal ini, waktu lokal +0700) |
| **Objek audit** | Proyek riset `D:\project\ai-biotech` (read-only) — commit `b942843` |
| **Metode** | Pemeriksaan read-only. Tidak ada file proyek referensi yang dibuat/diubah/dihapus. Tidak ada unduhan, instalasi, eksperimen, atau operasi git yang mengubah state. |
| **Status** | **SELESAI** — hasil audit baseline Phase 1 |

---

## 0. Ringkasan Eksekutif

1. **Root audit** `D:\project\alphafold-reliability-audit` ada, namun **bukan repository Git** dan sebelum audit ini hanya berisi `.claude/`. Sesuai aturan fase, Git **tidak** diinisialisasi otomatis; kondisi ini dilaporkan (temuan F3, §6).
2. **Laporan "audit Phase 1"** yang dirujuk oleh prompt Phase 2 ternyata **tidak pernah ada** di mesin ini (dilaporkan terpisah, dikonfirmasi user 2026-10-11). Dokumen ini menggantikannya sebagai baseline Phase 1.
3. **Integritas dataset inti terverifikasi penuh secara lokal:** 14/14 file mentah AlphaFold DB (v6) dan 9/9 artefak hash top-level **cocok persis** dengan nilai yang tercatat di `PROJECT_STATUS.md` §10.4 dan `TAHAP_5` §16. Total 14 file mentah = **1.469.156 byte** persis seperti terdokumentasi.
4. **Proyek referensi bersih & sinkron:** `ai-biotech` pada `main @ b942843`, working tree bersih sebelum dan sesudah audit, sinkron `origin/main`, remote publik diverifikasi via API GitHub anonim (0 issues).
5. **Gap utama yang menjadi fokus riset ini:** di seluruh proyek **tidak ada satu pun struktur eksperimen** (PDB: 0 file `.pdb`/`.ent`/`.mmtf`), tidak ada RMSD/TM-score/lDDT, dan **tidak ada tool perbandingan struktur yang terpasang**. Prediksi AlphaFold belum pernah dibandingkan dengan ground truth eksperimen — ini persis yang dinyatakan sebagai keterbatasan di dokumen proyek.
6. **Semua yang bisa diverifikasi dari file lokal cocok dengan dokumentasi proyek.** Yang **tidak** diverifikasi ulang (dan tidak diklaim) dicantumkan eksplisit di §5 — terutama: test suite (72 check) tidak dijalankan ulang pada fase ini.

---

## 1. Kondisi Root Audit & Repositori

### 1.1 Root audit — `D:\project\alphafold-reliability-audit`

| Pemeriksaan | Hasil |
|---|---|
| Keberadaan folder | ADA — dibuat 2026-10-11 11:53 (+0700) |
| Isi sebelum audit | hanya `.claude/` (tidak ada `docs/`, tidak ada artefak proyek) |
| Status Git | **BUKAN repository Git** (`fatal: not a git repository` — `git rev-parse --is-inside-work-tree`) |
| Repository remote | TIDAK ADA |
| Tindakan | sesuai aturan: **tidak diinisialisasi otomatis**. Keputusan perlu/tidaknya Git di root audit diserahkan ke user (temuan F3) |
| `docs/` | dibuat oleh audit ini (2026-10-11 12:09) sebagai lokasi seluruh dokumen fase ini |

Catatan: path yang tertulis di prompt awal Phase 2 (`D:\project\ai-biotech\alphafold-reliability-audit`) **tidak ada** (`git -C` → *No such file or directory*; pencarian `find` → nol hasil). Sudah diklarifikasi dan diputuskan user pada 2026-10-11: **root audit = `D:\project\alphafold-reliability-audit`**, dan `D:\project\ai-biotech` murni read-only.

### 1.2 Proyek referensi — `D:\project\ai-biotech` (READ-ONLY)

| Pemeriksaan | Hasil |
|---|---|
| Branch aktif | `main` |
| HEAD | `b942843d4cc138873fa937c28f08381e109306ce` — "Improve AI4Bio research documentation" (2026-10-10 11:37 +0700) |
| Working tree | **bersih** (`git status --short` kosong — diperiksa sebelum DAN sesudah seluruh audit; tidak berubah) |
| Sinkronisasi | HEAD == `origin/main` (tidak ada commit lokal yang belum di-push, tidak ada yang tertinggal di remote) |
| Branch/tag/stash | hanya `main`; 0 tag; 0 stash |
| Riwayat | 6 commit (2026-10-09 s.d. 2026-10-10) |
| Remote | `https://github.com/kelvinchandradjayapranata-hue/ai-biotech.git` |

Verifikasi GitHub via API anonim (2026-10-11):

| Field | Nilai |
|---|---|
| `private` | false (publik) |
| `default_branch` | `main` |
| `pushed_at` | 2026-10-10T04:37:25Z (= commit `b942843` waktu lokal +0700) |
| `archived` | false |
| Issues (state=all) | **0** — tidak ada laporan tersembunyi di GitHub |

### 1.3 Catatan tentang "Laporan Phase 1" yang dirujuk prompt

Prompt Phase 2 meminta membaca "laporan audit Phase 1". Pencarian menyeluruh (seluruh file di `D:\project`, Downloads/Documents/Desktop, seluruh transkrip sesi Claude Code di mesin ini, memory project, git refs, GitHub issues) menunjukkan laporan tersebut **tidak pernah ada** — frasa "AlphaFold Reliability Audit" pertama kali muncul di prompt user pada 2026-10-11, dan `docs/` ai-biotech tidak memuat dokumen audit reliabilitas. User mengonfirmasi: *"Laporan Phase 1 memang belum pernah dibuat. Jangan mencari atau mengarang laporan yang tidak ada."* Dokumen ini adalah **baseline Phase 1** yang disusun sebagai gantinya, dari verifikasi aktual.

---

## 2. Dokumen Referensi yang Digunakan (dibaca penuh 2026-10-11)

| Dokumen | mtime | Ukuran | Isi pokok (terverifikasi ada) |
|---|---|---|---|
| `docs/DATASET_PLAN.md` | 2026-10-05 14:32 | 10.701 B | Rencana dataset Tahap 4: pemilihan 7 kandidat, lisensi CC BY 4.0, pemisahan peran AlphaFold Server vs AlphaFold DB |
| `docs/TAHAP_5_AFDB_DOWNLOAD_REPORT.md` | 2026-10-05 17:02 | 7.541 B | Unduhan terkontrol 14 file (v6), verifikasi checksum/sekuens/PAE, detail per protein + SHA256 |
| `docs/TAHAP_6_MULTI_PROTEIN_FEATURE_REPORT.md` | 2026-10-05 17:19 | 8.726 B | Ekstraksi fitur multi-protein: 1098 × 29, skema 26 fitur, eksklusi 2 kolom contact, test 24/24 |
| `docs/PROJECT_STATUS.md` | 2026-10-05 17:44 | 31.693 B | Single source of truth status Stage 1–6: klaim terbukti vs belum (A–G), concern C1–C12, discrepancy register D1–D4, SHA256 §10.4 |

Ringkasan status yang dinyatakan dokumen (klaim dokumen — lihat §5 untuk batas verifikasi):

- Stage 1–6 selesai; pipeline data tervalidasi; **belum ada ML, belum ada label biologis**.
- Dataset: 7 protein AFDB v6, 1098 baris residu × 29 kolom (3 identifier + 26 fitur).
- Test suite: Stage 3 = 20/20, Stage 4 = 12/12, Stage 5 = 16/16, Stage 6 = 24/24 (total 72 check) — semuanya PASS **menurut dokumen**.
- Stage 6 berstatus "menunggu review user" sejak 2026-10-05.

---

## 3. Dataset & Artefak — Verifikasi Aktual (2026-10-11)

### 3.1 File mentah AlphaFold DB — 14/14 SHA256 dihitung ulang: COCOK

Semua nilai dihitung ulang dari file di disk pada 2026-10-11 dan dibandingkan dengan `data/alphafold_db/metadata/download_checksums.csv` (14 baris, `http_status=200`, `afdb_version=6`, `download_status=OK`).

| # | File (`data/alphafold_db/raw/`) | Bytes | SHA256 (recompute 2026-10-11) | vs tercatat |
|---|---|---|---|---|
| 1 | `AF-P61626-F1-model_v6.cif` | 147.439 | `8444ea54dc496d7a8107257040ca7d12476f0508a943b17890b846ecd1e2b99e` | MATCH |
| 2 | `AF-P61626-F1-predicted_aligned_error_v6.json` | 48.377 | `b3fba0b90f6a79cfa60952a2243a1410cc108438ab998bb50de620014ad26902` | MATCH |
| 3 | `AF-P02144-F1-model_v6.cif` | 151.949 | `5fe127f5047a9bda4eb96759c444b9f12b447b24ec49e82bdae32ea768803d23` | MATCH |
| 4 | `AF-P02144-F1-predicted_aligned_error_v6.json` | 48.352 | `d32c10446d18a6b375af31e19dceb774ffb81ab6354318e8b58b374742e762af` | MATCH |
| 5 | `AF-P99999-F1-model_v6.cif` | 108.338 | `1b48d4bc17398c7e4f54d18b1c401fedc099aea22adbb8b6e943755acd8e3605` | MATCH |
| 6 | `AF-P99999-F1-predicted_aligned_error_v6.json` | 22.497 | `b422bfda4884d8c758d00488274ac46e505f913c27f686e052f9cbdc6c3a9faa` | MATCH |
| 7 | `AF-P0DP23-F1-model_v6.cif` | 148.336 | `e698114902202d87b9cf6ea7656fbf433db61571d085593905885de6389b475f` | MATCH |
| 8 | `AF-P0DP23-F1-predicted_aligned_error_v6.json` | 57.011 | `44c3f6c438ee29fe9d6c7da72e916904f3103c5471aae93747a586f7d1b49652` | MATCH |
| 9 | `AF-P00441-F1-model_v6.cif` | 144.236 | `fd186af7f5fdd9214cee40653bed71760fa53f0922d3438f9ce62e74c64f0edb` | MATCH |
| 10 | `AF-P00441-F1-predicted_aligned_error_v6.json` | 48.214 | `ab6998cc394243b3ad7d5ed29b9ce6ea5b9a65c38584d8d8b98c77bac8e5de90` | MATCH |
| 11 | `AF-P61823-F1-model_v6.cif` | 145.832 | `9f20e86e00ceb3f6d2020cf3ac4b5c17a340be5a2b56dd89e20b9edb9a7dd9b5` | MATCH |
| 12 | `AF-P61823-F1-predicted_aligned_error_v6.json` | 51.759 | `b3258e465f4edb5f3b9a1db02547927776ec28579dadd5c3e05a47c3b5173b4f` | MATCH |
| 13 | `AF-P42212-F1-model_v6.cif` | 230.531 | `20fee8dd4053d5aa1dc43acd68810faccf68b942f41220d375bfa6307daaa6d2` | MATCH |
| 14 | `AF-P42212-F1-predicted_aligned_error_v6.json` | 116.285 | `b2838a1e0d38a31e133bec1f176c54578410f4b8a7b009b648b0c621a682ee88` | MATCH |

**Total 14 file = 1.469.156 byte — sama persis dengan angka terdokumentasi.** Ukuran per file juga cocok dengan kolom `file_size_bytes`.

Metadata API tersimpan (7 `api_<acc>.json`); spot-check `api_P61626.json`: `entryId=AF-P61626-F1`, `latestVersion=6`, `isComplex=false` — konsisten dengan dokumentasi. (Tanggal query asli: 2026-10-05; **tidak** di-query ulang ke server AFDB pada fase ini.)

### 3.2 Artefak hash top-level — 9/9 COCOK dengan `PROJECT_STATUS.md` §10.4

| File | SHA256 (recompute 2026-10-11) | vs tercatat |
|---|---|---|
| `results/dataset/multi_protein_residue_features.csv` | `390462a42d8befe5588b1019d51b4c4282433b0f712c4377e5191883983d3c1d` | MATCH |
| `results/dataset/multi_protein_summary.csv` | `111a9569ebf8990eeca19673215f1415e828cc00fd2f6c20a16e9048252660e1` | MATCH |
| `results/dataset/multi_protein_structure_features.json` | `7481765648cbd3b5c340cbacadc79635756d4867254b1003b335680a136587bc` | MATCH |
| `results/dataset/multi_protein_plddt_overview.png` | `365d29134a1ab55f5e3464e1155a89a2f04b747adbc7a762521f2d1b5ab23fc5` | MATCH |
| `results/dataset/multi_protein_pae_overview.png` | `3e0d1ef035271453d6aba4e54367df6ce0c41dad53693fe1444448a2b3ad77b0` | MATCH |
| `results/dataset/dataset_download_manifest.csv` | `67ed8a389d29782f0f67edbb49c1a95385fddca00e3f3f9d20db3b4f1d16c3a8` | MATCH |
| `structures/ubiquitin_alphafold.cif` | `adc6c6e825893e57590c5c5a2921875c9f06d8279025091b38fc88ad2968aa5d` | MATCH |
| `data/ubiquitin_sequence.fasta` | `35100b2cfcb5300b6f969e5b2393ef1c9f644d73f7ac23e49a426d1ec238cff1` | MATCH |
| `data/alphafold_downloads/fold_2026_10_05_10_26/fold_2026_10_05_10_26_full_data_0.json` | `09178e510be8f5176bb3aeac92935e0f33d2a238346e073fd0668f6a687da61d` | MATCH |

### 3.3 Bentuk tabel data (spot-verifikasi langsung dari file)

| File | Hasil hitung aktual | Kesesuaian |
|---|---|---|
| `multi_protein_residue_features.csv` | 1.099 baris (= 1 header + **1.098 data**), **29 kolom** | sesuai (1098 × 29) |
| Distribusi baris per protein | P42212=238, P00441=154, P02144=154, P61823=150, P0DP23=149, P61626=148, P99999=105 | **Σ = 1.098** persis |
| `summary` (7 protein) | Semua `status=PASS`; mean pLDDT 85.241 (P0DP23) – 97.932 (P99999); mean PAE 2.059 – 13.281 Å | konsisten dengan README/TAHAP_6 |
| `ubiquitin_residue_features.csv` | 77 baris (= 1 header + **76 data**), **28 kolom** | sesuai (76 × 28) |
| `dataset_manifest.csv` (Stage 4) | 7 baris berstatus `planned_not_downloaded` | lihat temuan F2 |
| `dataset_download_manifest.csv` (Stage 5) | 7 baris berstatus `DOWNLOADED_VALIDATED` | lihat temuan F2 |

> Catatan: nilai fitur **tidak** dihitung ulang dari matriks mentah pada fase ini (lihat §5).

### 3.4 Lingkungan eksekusi (terverifikasi ada)

| Item | Nilai |
|---|---|
| Python (`.venv`) | 3.12.10 |
| Paket terpasang (read-only `pip list`) | biopython 1.88, pandas 3.0.6, numpy 2.5.3, matplotlib 3.11.2, ipykernel 7.4.0, nbclient 0.11.0 |
| Framework ML | **tidak ada** (tanpa PyTorch/TensorFlow/CUDA — sesuai klaim dokumen) |
| Tool perbandingan struktur | **tidak terpasang**: TMscore, TM-align, foldseek, USalign, lddt → semua *not found* di PATH |

### 3.5 Data pendukung lain yang ada

- `data/alphafold_downloads/fold_2026_10_05_10_26/`: output AlphaFold Server (5 model CIF, 5 `full_data`, 5 `summary_confidences`, `job_request.json`, `msas/`, `templates/`, `terms_of_use.md`). Peran per dokumen: **spesimen pembelajaran, bukan training data**; folder ini git-ignored di repo.
- `notebooks/01_first_protein_analysis.ipynb` + `results/01_first_protein_analysis.executed.ipynb`: notebook konsep; tidak diverifikasi isinya pada fase ini.

---

## 4. Yang TIDAK Ada (penting untuk desain fase riset)

Hasil pencarian langsung di seluruh `D:\project\ai-biotech` (2026-10-11):

| Yang dicari | Hasil | Implikasi |
|---|---|---|
| Struktur eksperimen (`.pdb`, `.pdb.gz`, `.ent`, `.mmtf`) | **0 file** | Perbandingan prediksi vs eksperimen belum pernah dilakukan dan **tidak mungkin** dilakukan dari artefak lokal saat ini |
| File/artefak RMSD, TM-score, lDDT, pocket metrics | tidak ada | Belum ada satu pun pengukuran kesesuaian struktur |
| Label biologis (y) | tidak ada | Bukan blocker untuk RQ audit ini (RQ = akurasi struktur vs pLDDT/PAE, bukan prediksi fungsi) |
| Model ML / training artifacts | tidak ada | Sesuai klaim dokumen (Stage 7 belum dimulai) |
| Referensi PDB ID / pemetaan UniProt → PDB | tidak ada | Pencocokan model↔eksperimen harus dimulai dari nol di fase riset |

---

## 5. Pembatas Bukti: VERIFIED vs TIDAK Diverifikasi Ulang

**Diverifikasi aktual pada 2026-10-11 (fakta):**

1. Keberadaan, ukuran, dan SHA256 seluruh 14 file dataset mentah + 9 artefak top-level (recompute lokal — semuanya MATCH).
2. Bentuk tabel CSV (jumlah baris/kolom; distribusi baris per protein; isi summary).
3. Kondisi git `ai-biotech` (bersih, sinkron) dan status publik repo via API GitHub.
4. Keberadaan/ketiadaan file PDB di proyek (0).
5. Lingkungan Python + daftar paket terpasang; ketiadaan tool perbandingan struktur.
6. Isi aktual keempat dokumen referensi (dibaca penuh).

**TIDAK diverifikasi ulang pada fase ini (disebut sebagai klaim dokumen, bukan hasil audit):**

1. **Test suite (72 check) tidak dijalankan** — aturan fase melarang menjalankan eksperimen. Status PASS (20/20, 12/12, 16/16, 24/24) adalah klaim tertulis di dokumen.
2. **Identitas sekuens CIF ↔ UniProt** (7/7 MATCH), checksum MD5 `sequenceChecksum` dan CRC64 UniProt — tidak diulang (butuh parsing file). Klaim: TAHAP_5 §11.
3. **Rekalkulasi nilai fitur dari matriks mentah** (PAE, rekonsiliasi kolom) — tidak diulang. Klaim: test Stage 6.
4. **Reproducibility byte-identical antar-run** — tidak diuji ulang (butuh menjalankan extractor). Klaim: TAHAP_6 §9.
5. **Kondisi live server AlphaFold DB** — tidak di-query pada fase ini. Versi v6 adalah kondisi **per 2026-10-05** menurut metadata lokal; versi live hari ini = UNKNOWN.
6. **Klaim lisensi** (CC BY 4.0, atribusi EMBL-EBI/DeepMind/UniProt) — dari dokumen; tidak dicek ke sumber live.
7. **Nilai referensi di dokumen** (mis. pLDDT mean, nilai PAE per protein) — dibaca dari file lokal yang hash-nya cocok; bukan hasil komputasi independen baru.

**Implikasi:** hash cocok ⇒ file lokal **identik dengan yang tercatat dan divalidasi pada 2026-10-05**. Ini jaminan integritas & ketertelusuran file — **bukan** jaminan kebenaran ilmiah isinya, dan **bukan** validasi ulang terhadap struktur eksperimen.

---

## 6. Register Temuan (Findings)

| # | Temuan | Bukti (2026-10-11) | Tingkat | Status |
|---|---|---|---|---|
| F1 | `data/alphafold_db/raw/README.txt` masih menyatakan *"Folder ini KOSONG dengan sengaja"*, padahal folder berisi 14 file dataset + README itu sendiri | Teks README (dibaca aktual) vs isi folder (15 entri) | Rendah — anomali dokumentasi | Terkonfirmasi ulang; reproduksi temuan D2 `PROJECT_STATUS.md`. **Tidak diperbaiki** (read-only). |
| F2 | Dua manifest dengan status berbeda: Stage 4 `dataset_manifest.csv` = `planned_not_downloaded` (7 baris) vs Stage 5 `dataset_download_manifest.csv` = `DOWNLOADED_VALIDATED` (7 baris) | Kedua file dihitung actual | Sedang — berpotensi menyesatkan pembaca | Terkonfirmasi ulang; reproduksi temuan D1. **Tidak diubah** (read-only). |
| F3 | Root audit `D:\project\alphafold-reliability-audit` **bukan** repository Git (tanpa branch/remote/riwayat) | `git rev-parse` → fatal: not a git repository | Informasional — keputusan user | Dilaporkan; **tidak** diinisialisasi (aturan). Perlu keputusan: init atau tidak. |
| F4 | "Laporan audit Phase 1" yang dirujuk prompt Phase 2 tidak pernah ada; path prompt `D:\project\ai-biotech\alphafold-reliability-audit` juga tidak ada | Pencarian menyeluruh 2026-10-11 + konfirmasi user | Tinggi — blocker yang sudah diselesaikan | **Selesai**: user memutuskan root audit = `D:\project\alphafold-reliability-audit`; dokumen ini menggantikan laporan Phase 1. |

---

## 7. Implikasi Langsung untuk Phase 2 (Desain Riset)

1. **Ground truth harus datang dari luar proyek.** Tidak ada struktur eksperimen lokal; protokol riset harus mendefinisikan sumber eksperimen (PDB/RCSB), kriteria pencocokan, dan prosedur unduhan terkontrol — dieksekusi **hanya setelah protokol disetujui** (bukan pada fase ini).
2. **Set lokal (7 protein) terlalu kecil & homogen untuk klaim** — mean pLDDT 85–98 (seluruhnya "confident"), tanpa protein membran/disordered besar, semua monomer. Baseline lokal tidak bisa menjawab RQ; dibutuhkan set benchmark baru.
3. **RQ terfokus pada confidence↔error** ⇒ desain harus sengaja memasukkan protein dengan rentang pLDDT lebar (termasuk low-confidence) agar hubungan pLDDT/PAE–error bisa diuji, bukan hanya dideskripsikan pada kasus mudah.
4. **Tooling belum ada** — tool TM-score/lDDT/RMSD harus dipilih, versi dikunci, dan dijelaskan di protokol; instalasi adalah keputusan fase implementasi.
5. **Pilot 30 protein** (usulan prompt) hanya untuk menguji pipeline; ukuran sampel untuk klaim harus dijustifikasi terpisah (§ protokol).
6. **Root audit belum ber-Git** — jika riwayat perubahan dokumen ingin tertelusur, perlu keputusan user untuk `git init` di root audit (bukan di proyek induk).

---

## 8. Lampiran — Perintah Verifikasi (dapat direproduksi)

```bash
# 1. Kondisi git proyek referensi (read-only)
git -C "D:/project/ai-biotech" status --short && git -C "D:/project/ai-biotech" rev-parse HEAD

# 2. Recompute SHA256 14 file mentah vs download_checksums.csv
cd "D:/project/ai-biotech" && ./.venv/Scripts/python.exe - <<'EOF'
import hashlib, csv, os
base = os.path.join("data", "alphafold_db")
rows = list(csv.DictReader(open(os.path.join(base,"metadata","download_checksums.csv"), newline="", encoding="utf-8")))
for r in rows:
    b = open(os.path.join(base,"raw",r["filename"]),"rb").read()
    print(r["filename"], len(b), "OK" if hashlib.sha256(b).hexdigest()==r["sha256"] else "MISMATCH")
EOF

# 3. Bentuk tabel data
head -1 results/dataset/multi_protein_residue_features.csv | awk -F',' '{print NF" cols"}'
wc -l results/dataset/multi_protein_residue_features.csv

# 4. Pencarian struktur eksperimen (harus 0 hasil)
find . -iname "*.pdb*" -o -iname "*.mmtf" -o -iname "*.ent" | grep -v .venv | grep -v .git
```

---

*Dokumen ini adalah artefak audit read-only. Tidak ada perubahan apa pun yang dilakukan pada `D:\project\ai-biotech` maupun isinya selama penyusunan dokumen ini — diverifikasi dengan `git status --short` kosong sebelum dan sesudah audit.*
