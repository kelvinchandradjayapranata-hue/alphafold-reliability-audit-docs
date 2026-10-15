# TOOLS_LOCK — AlphaFold Reliability Audit

## Penguncian Toolchain Metrik & Mapping (Fase Persiapan 2B — RENCANA)

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal** | 2026-10-11 |
| **Status** | **RENCANA — belum ada instalasi, belum ada pengujian.** Status akan berubah menjadi `LOCKED` pada fase implementasi setelah seluruh uji determinisme (§6) lulus dan bagian §9 terisi. |
| **Aturan mengikat** | Protokol §C ("semua definisi tool dikunci sebelum skor pertama dihitung") + amendemen C-5: **tidak ada metrik yang boleh dihitung sebelum status file ini = LOCKED**. |
| **Dasar** | `docs/PHASE2_REVIEW_GATE.md` §5.5/§5.10/§6 K-3; informasi versi/lisensi diverifikasi dari dokumentasi resmi pada 2026-10-11 (review gate). |
| **Batas** | Tidak ada instalasi/unduhan/eksekusi dalam penyusunan file ini. Nilai bertanda `UNVERIFIED` **tidak boleh ditebak** — wajib diverifikasi saat implementasi. |
| **Perhatian** | Semua versi "terbaru per observasi 2026-10-11" bersifat snapshot; **wajib dicek ulang saat instalasi** (rilis bisa berubah). Jika berubah → pin ke versi yang sama dengan yang diuji, bukan otomatis ke yang terbaru. |

---

## 1. Prinsip penguncian

1. **Satu tool, satu versi, satu hash** — tidak ada pencampuran versi dalam satu tabel hasil (policy §8.5).
2. **Output mentah disimpan** sebagai artefak turunan + hash; metrik dihitung dari output mentah tersebut.
3. **Setiap flag non-default dicatat** di bagian §3; default resmi hanya dipakai bila tercatat di sini.
4. **Determinisme wajib diuji** (§6) sebelum metrik pertama; hasil uji dicatat di §9.
5. **Lisensi diperiksa sebelum distribusi** (bukan sebelum perhitungan) — kecuali tool inti yang lisensinya masih `UNVERIFIED` → keputusan pemilik (D-11/K-3).
6. **Isolasi**: seluruh tool dijalankan dari root audit; tidak ada import/file dari `ai-biotech` dalam pipeline analisis (policy §3).

---

## 2. Tabel utama toolchain

| Peran (metrik) | Tool | Versi/kandidat (observasi 2026-10-11) | Lisensi | Dokumentasi resmi | Status verifikasi |
|---|---|---|---|---|---|
| TM-score (C.2) | **US-align** (utama) | Halaman resmi menyatakan update terbaru **2026/05/27**; nomor versi tidak dinyatakan di halaman → pin via arsip+hash saat instalasi | **Tidak dinyatakan di halaman resmi — UNVERIFIED** | `zhanggroup.org/US-align/` | Halaman OK; nomor versi & lisensi UNVERIFIED |
| TM-score (alternatif/legacy) | TM-align | Source update **2022/04/12**, changelog 2022/04/15 (per halaman resmi); tanpa nomor versi → pin via arsip+hash | **Tidak dinyatakan di halaman resmi — UNVERIFIED** | `zhanggroup.org/TM-align/` | idem |
| lDDT global & per-residu (C.3, C.5b) | **OpenStructure** | **2.12** (pernyataan versi di situs resmi) | **LGPL-3.0** (file LICENSE.txt — terverifikasi) | `openstructure.org`; docs `compare-structures` | Versi & lisensi OK; perintah `--lddt --local-lddt --bb-lddt` terverifikasi dari docs |
| lDDT cross-check (S7) | **Foldseek** | Halaman rilis GitHub: Latest `10-941cd33` (observasi 2026-10-11) | **GPL-3.0** (terverifikasi) | `github.com/steineggerlab/foldseek` + wiki | Rilis & lisensi OK; detail flag dikutip ulang saat instalasi |
| RMSD Kabsch (C.1, C.5a, C.8) | Implementasi sendiri (**numpy** SVD) + cross-check US-align/OST | numpy dipin saat instalasi (venv) — UNVERIFIED nomor | numpy: BSD (standard) — verifikasi saat instalasi | numpy docs; dasar metode [L23] | Rencana; determinisme diuji §6 |
| Pemetaan model↔PDB | **SIFTS (PDBe)** sebagai sumber primer + **Biopython** `PairwiseAligner` sebagai cross-check | Biopython dipin saat instalasi — UNVERIFIED nomor | Biopython: lisensi Biopython — verifikasi saat instalasi | PDBe SIFTS docs; Biopython docs | **Ketersediaan endpoint SIFTS belum diuji → UNVERIFIED** |
| Metadata PDB | **RCSB Data API** | — (API, tanpa versi tool; respons disimpan) | Data: CC0 1.0 (terverifikasi review gate §4.4) | RCSB PDB Data API docs [L33] | OK (kebijakan) |
| Klaster homolog (C-2/D-5) | **MMseqs2** `easy-cluster` | Halaman rilis GitHub: Latest `18-8cc5c` (observasi 2026-10-11) | **MIT** (catatan rilis Release 16 — terverifikasi) | `github.com/soedinglab/MMseqs2` | Rilis & lisensi OK; parameter coverage (D-5) menunggu keputusan pemilik |
| Parsing CIF & PAE QC (C-9.4) | **gemmi** | Dipin saat instalasi — UNVERIFIED nomor | Belum diverifikasi di review — UNVERIFIED | gemmi docs | Rencana |
| Statistik (D) | **numpy + scipy** | Dipin saat instalasi — UNVERIFIED nomor | BSD — verifikasi saat instalasi | docs resmi | Rencana |
| Domain & disorder (D.5, K-4) | **InterPro/Pfam API** (prioritas 1) | Rilis dipin saat implementasi — UNVERIFIED | — | `ebi.ac.uk/interpro` | Metode = keputusan K-4; detail UNVERIFIED |
| Bahasa | **Python 3.12.x** (menyamai pipeline lokal 3.12.10) | 3.12.x | PSF | docs python.org | Rencana |

**Catatan penting T0:** untuk T0 (pilot pipeline, tanpa klaim), toolchain minimum = parsing CIF (gemmi), RMSD Kabsch (numpy), lDDT (OpenStructure), TM-score (US-align), mapping (SIFTS+Biopython), klaster (MMseqs2). PAE (C.6/C.7) hanya diperlukan bila T0 mencakup uji pipeline PAE — tetap wajib lulus QC indeks (C-9.4) sebelum dipakai.

---

## 3. Detail per tool

### 3.1 US-align (TM-score, C.2; cross-check RMSD C.1)
- **Fungsi:** TM-score [L17] dengan konvensi [L18]; alternatif pemelihara dari TM-align (aktif diperbarui, update halaman 2026/05/27).
- **Alasan pemilihan:** penerus yang dipelihara; mengurangi risiko versi beku — namun **pin tetap memakai arsip+hash**, bukan "selalu terbaru".
- **Sumber:** `zhanggroup.org/US-align/` (halaman + arsip unduhan resmi).
- **Verifikasi versi & integritas:** unduh arsip dari halaman resmi; catat URL, tanggal, SHA256 arsip; bila halaman menyediakan checksum resmi → bandingkan; jika tidak → hash arsip dicatat apa adanya. Output versi tool (bila ada) dicatat — **perintah cek versi persis: UNVERIFIED, dikutip dari dokumentasi saat instalasi**.
- **Flags rencana:** default resmi tool untuk mode struktural (tanpa `-byresi` dsb. kecuali dicatat); semua flag dicatat di §9 saat LOCK.
- **Lisensi:** tidak dinyatakan di halaman resmi → **UNVERIFIED** (lihat §7).

### 3.2 TM-align (alternatif legacy)
- Sama seperti 3.1; source update halaman 2022/04/12. Dipakai hanya bila US-align gagal diverifikasi (fallback terkunci), atau sebagai cross-check TM (S7).

### 3.3 OpenStructure 2.12 (lDDT C.3, per-residu C.5b, backbone lDDT)
- **Fungsi:** lDDT bebas superposisi [L19]; `local_lddt` per-residu untuk C.5b (amendemen C-9).
- **Perintah rencana (terverifikasi dari docs review gate):** `ost compare-structures -m <model> -r <reference> --lddt --local-lddt --bb-lddt`.
- **Verifikasi versi & integritas:** catat cara instalasi (resmi) + hash paket/arsip; perintah versi persis — **UNVERIFIED, dikutip saat instalasi**.
- **Lisensi:** LGPL-3.0 (LICENSE.txt pada paket — terverifikasi).

### 3.4 Foldseek (cross-check lDDT, S7; alternatif TM)
- **Fungsi:** `lddt`/`lddtfull` + `alntmscore`/`qtmscore`/`ttmscore` (output terverifikasi dari dokumentasi review gate); opsi `--exact-tmscore`.
- **Alasan:** implementasi lDDT berbeda dari OST → cross-check positif (S7); GPL-3.0 jelas.
- **Verifikasi:** unduh rilis GitHub `steineggerlab/foldseek` (asset resmi), catat tag (`10-941cd33` = observasi 2026-10-11), SHA256 asset; perintah versi persis — dikutip saat instalasi.
- **Flags rencana:** `--exact-tmscore` bila TM dipakai dari foldseek; format output disimpan.

### 3.5 MMseqs2 (klaster homolog, C-2)
- **Fungsi:** `easy-cluster` dengan `--min-seq-id 0.3` + coverage timbal-balik **[D-5 — usulan 0,8; keputusan pemilik]** (mode coverage persis diverifikasi dari dokumentasi MMseqs2 saat penguncian).
- **Verifikasi:** rilis GitHub `soedinglab/MMseqs2` (tag observasi `18-8cc5c`), SHA256 asset. Lisensi MIT (catatan rilis Release 16).
- **Determinisme:** uji multi-thread (§6 TD-5) — hasil klaster harus identik himpunan.

### 3.6 SIFTS/PDBe + Biopython (mapping)
- **Fungsi:** pemetaan residu-level UniProt↔PDB (primer, termasuk penanganan residu termodifikasi & penomoran — amendemen C-4); Biopython `PairwiseAligner` sebagai verifikasi independen (pola cross-check policy §7.7).
- **Status:** **endpoint SIFTS belum diuji pada fase ini** → liveness & format respons = UNVERIFIED. Saat implementasi: uji 1–2 protein; bila endpoint tidak tersedia → **escalate ke pemilik, bukan fallback diam-diam** (amendemen C-4).
- **Metadata PDB:** RCSB Data API (metode, resolusi, tanggal rilis, ligan, status current/obsolete) — respons disimpan verbatim.

### 3.7 gemmi (CIF) + numpy/scipy (statistik & Kabsch)
- **Fungsi:** parsing CIF AFDB/PDB (pLDDT dari kolom B-factor, penomoran residu); Kabsch SVD (C.1/C.5a/C.8) [L23]; statistik §D (Wilcoxon, Spearman, Wilson, Holm, bootstrap klaster).
- **Verifikasi:** versi paket Python dipin di `requirements.txt` + hash (pip `--require-hashes`).
- **Lisensi:** numpy/scipy BSD; gemmi **UNVERIFIED** (cek saat instalasi).

### 3.8 Domain & disorder (K-4 — metode belum diputuskan)
- Prioritas 1 InterPro/Pfam (API resmi, rilis dipin saat implementasi); fallback geometrik = **satu metode deterministik yang dipin untuk semua protein** (kandidat dipilih & diuji saat TOOLS_LOCK diisi). Tidak ada metode "per protein". Pelaksanaan: mengikuti keputusan K-4.

---

## 4. Environment & dependensi

| Aspek | Ketentuan |
|---|---|
| Python | 3.12.x, venv baru di `D:\project\alphafold-reliability-audit\.venv` (K-6; menggantikan struktur §3 policy — dibuat saat implementasi) |
| Paket Python | `requirements.txt` versi persis + hash; instalasi `--require-hashes`; tanpa import silang dari `ai-biotech` |
| Tool non-Python | US-align/TM-align (arsip resmi), OpenStructure 2.12, Foldseek, MMseqs2 |
| Platform | Mesin audit = Windows 10 (10.0.19045). **Ketersediaan binary Windows native untuk OpenStructure/Foldseek/MMseqs2 TIDAK terverifikasi** → kontingensi: WSL2 atau distribusi conda-forge — **keputusan platform D-11 (bagian K-3)**. Verifikasi ketersediaan dilakukan saat instalasi (bukan fase ini). |
| Isolasi | Semua proses dari root audit; `ai-biotech` read-only mutlak (policy §8.1); venv induk tidak dipakai oleh pipeline analisis |

---

## 5. Prosedur integritas instalasi (per tool)

1. Catat: nama, sumber URL resmi, tanggal unduh, tag/versi, SHA256 arsip.
2. Bandingkan dengan checksum resmi **bila halaman resmi menyediakan**; bila tidak → simpan hash observasi + catatan "checksum resmi tidak tersedia".
3. Ekstrak/instal ke lokasi terkontrol (mis. `tools/` di root audit atau WSL) — lokasi dicatat.
4. Jalankan perintah versi tool (bila didokumentasikan) → output disimpan di §9.
5. Verifikasi fungsional minimal 1 input identitas: skor terhadap diri sendiri (TM=1,000; lDDT=1,000; RMSD=0,000) sebelum dipakai (§6 TD-3).

---

## 6. Uji determinisme (WAJIB lulus sebelum T0; hasil dicatat di §9)

| ID | Tool | Uji | Kriteria lulus (usulan — bagian persetujuan K-3) |
|---|---|---|---|
| TD-1 | Semua | Jalankan 2× pada input identik → bandingkan output | Output numerik identik; bila tool paralel non-deterministik → pakai mode 1-thread & catat |
| TD-2 | RMSD/TM/lDDT | Transformasi kaku acak (rotasi+translasi, seed tetap) pada model → ukur ulang | RMSD = 0 ± 1e-6 Å; TM = 1,000 ± 1e-4; lDDT = 1,000 ± 1e-6 |
| TD-3 | RMSD/TM/lDDT | Model vs salinan dirinya | RMSD 0; TM 1,000; lDDT 1,000 |
| TD-4 | OST vs Foldseek | lDDT pada ≥3 pasang uji (2 di antaranya = spesimen pembelajaran) | Selisih dilaporkan; bila \|Δ\| > 0,05 → investigasi wajib sebelum dipakai (usulan ambang, bagian K-3) |
| TD-5 | MMseqs2/Foldseek | 1 thread vs N thread (flag deterministik) | **Himpunan** klaster identik (bukan urutan berkas) |
| TD-6 | gemmi + PAE JSON | QC indeks PAE↔CIF (C-9.4) pada ≥2 protein (spesimen pembelajaran) | Jumlah baris matriks = panjang sekuens; spot-check ≥3 residu cocok |
| TD-7 | SIFTS/Biopython | Uji mapping pada 1–2 protein (termasuk 1 dengan residu termodifikasi bila tersedia) | Mapping dihasilkan & dilaporkan; kegagalan endpoint = escalate (bukan fallback diam-diam) |
| TD-8 | Pipeline (script) | 2 run penuh set QC kecil → bandingkan | Byte-identical (protokol §D.7; tanpa timestamp/path absolut di output) |

---

## 7. Lisensi — status & yang perlu diselesaikan

| Tool | Lisensi | Status | Tindakan |
|---|---|---|---|
| OpenStructure | LGPL-3.0 | Terverifikasi (LICENSE.txt) | — |
| Foldseek | GPL-3.0 | Terverifikasi | Catat implikasi distribusi bila kode audit didistribusikan |
| MMseqs2 | MIT | Terverifikasi (Release 16 notes) | — |
| US-align / TM-align | Tidak dinyatakan di halaman resmi | **UNVERIFIED** | Selesaikan sebelum distribusi/publikasi yang menyertakan tool (halaman resmi/persetujuan tertulis) |
| numpy/scipy | BSD | Standar — verifikasi saat instalasi | — |
| gemmi, Biopython | Belum diverifikasi | **UNVERIFIED** | Verifikasi saat instalasi |
| AFDB data | CC BY 4.0 | Terverifikasi (review gate §4.4) | Atribusi per amendemen C-7 |
| PDB data | CC0 1.0 | Terverifikasi (review gate §4.4) | Atribusi praktik baik |

---

## 8. Register UNVERIFIED (wajib diselesaikan; dilarang menebak)

| # | Item | Cara menyelesaikan | Kapan |
|---|---|---|---|
| U-1 | Nomor versi US-align/TM-align (halaman resmi tanpa nomor) | Pin arsip+hash; catat tanggal halaman | Saat instalasi |
| U-2 | Lisensi US-align/TM-align | Cek halaman resmi/permintaan resmi | Sebelum publikasi/distribusi |
| U-3 | Perintah cek versi persis tiap tool (OST/Foldseek/MMseqs2/US-align) | Kutip dari dokumentasi resmi | Saat instalasi |
| U-4 | Ketersediaan binary Windows native (OST/Foldseek/MMseqs2) | Uji saat instalasi → keputusan D-11 (WSL2/conda bila perlu) | Saat instalasi |
| U-5 | Liveness & format respons SIFTS/PDBe | Uji endpoint pada 1–2 protein | Sebelum mapping T0 |
| U-6 | Versi paket Python (numpy/scipy/gemmi/Biopython) & lisensi gemmi/Biopython | Pin saat instalasi (requirements + hash) | Saat instalasi |
| U-7 | Ketersediaan klaster sekuens PDB pra-cutoff (untuk C-11/NB-4) | Cek saat implementasi | Sebelum laporan T1 |
| U-8 | File PAE JSON lokal tersedia/tidak (untuk TD-6) | Inventaris lokal read-only saat implementasi | Saat instalasi |

---

## 9. Bagian LOCKED (kosong — diisi pada fase implementasi)

> Diisi hanya setelah: (1) seluruh uji §6 lulus, (2) keputusan K-3 & D-11 diterima, (3) instalasi terverifikasi §5. Format:

| Aspek | Nilai (diisi saat LOCK) |
|---|---|
| Versi final + hash tiap tool | — |
| Output perintah versi | — |
| Lokasi instalasi (venv/WSL/conda) | — |
| Hasil TD-1..TD-8 (lulus/gagal + bukti) | — |
| `requirements.txt` hash | — |
| Tanggal LOCK + penanda persetujuan | — |

Setelah diisi: file di-hash (SHA256) dan dilaporkan; perubahan setelah itu = amendemen tercatat.

---

## 10. Keputusan pemilik terkait file ini

- **K-3** (menyetujui daftar toolchain ini untuk dikunci) — termasuk persetujuan usulan ambang QC §6 (TD-4 0,05).
- **D-11** (platform: Windows native vs WSL2/conda) — setelah hasil U-4.
- **D-5** (parameter coverage klaster MMseqs2) — dicatat di sini saat LOCK.
- **D-6 / D-6b** (B, seed bootstrap; ambang klaster minimum) — dicatat di sini saat LOCK.

**STOP:** file ini adalah rencana. Tidak ada instalasi/unduhan/eksekusi sampai persetujuan eksplisit pemilik.
