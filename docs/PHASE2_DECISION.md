# PHASE 2 — DECISION DOCUMENT

## Keputusan Kelayakan: Lanjut ke Implementasi atau Tidak?

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal** | 2026-10-11 |
| **Status** | SELESAI — keputusan diajukan ke user |
| **Dokumen dasar** | `PHASE1_BASELINE_AUDIT.md`, `LITERATURE_REVIEW.md` (40 sumber), `RESEARCH_PROTOCOL_V1.md` (A–H), `DATA_AND_BIAS_POLICY.md` |
| **Sifat dokumen** | Jujur-by-design: memuat juga yang *tidak* didukung literatur dan yang *belum* diketahui. |

---

## 1. Ringkasan Eksekutif & Rekomendasi

1. **RQ terdefinisi baik dan dapat dioperasionalkan** menjadi 4 sub-pertanyaan terukur (RQ1–RQ4, protokol §0.2) dengan unit analisis yang benar (protein/klaster, bukan residu).
2. **RQ didukung literatur sebagai pertanyaan yang bermakna:** ada bukti kuat pLDDT ↔ error lokal ([L1, L11–L13]) sekaligus bukti eksplisit bahwa prediksi ber-confidence sangat tinggi dapat menyimpang secara global/domain ([L7]) dan bahwa kesuksesan dapat terkontaminasi memorization ([L26]) — artinya pertanyaan "seberapa andal" **belum terjawab tuntas**, bukan pertanyaan kosong.
3. **Tidak ada klaim kebaruan besar.** Kontribusi yang jujur: replikasi-terkontrol + audit kalibrasi + protokol terbuka + register bias (§5). Klaim "pertama di dunia" **tidak** dibuat.
4. **Semua basis data & tool yang dibutuhkan tersedia publik** (AFDB CC BY 4.0; PDB via API resmi) — tidak ada blocker akses data.
5. **Blocker yang tersisa bersifat keputusan, bukan sains:** 7 keputusan (K-1..K-7, §6) — terutama verifikasi cutoff data latih dari full text [L1] dan pemilihan toolchain — semuanya diselesaikan **sebelum** eksekusi, tanpa mengubah desain.
6. **Risiko utama yang diakui:** RQ3 (domain) bisa turun menjadi eksploratori bila protein multidomain di T1 sedikit (F.3); jejak kecil relatif terhadap benchmark besar (Akdel [L8], Yin [L34]) sehingga nilai terletak pada kualitas kontrol bias, bukan skala.
7. **Rekomendasi: GO KONDISIONAL** — lanjut ke implementasi (mulai T0 pilot) **hanya setelah** user menyetujui 5 dokumen Phase 2 dan menjawab keputusan §6. Tanpa persetujuan: status tetap HOLD pada posisi sekarang (dokumen selesai, nol eksekusi).

---

## 2. Apakah RQ Terdefinisi Baik?

### 2.1 Uji kelayakan RQ (diuji terhadap dokumen, bukan asumsi)

| Kriteria | Penilaian | Bukti |
|---|---|---|
| Spesifik & terukur? | **Ya** | RQ1–RQ4 dipetakan ke metrik berdefinisi (C.1–C.13) dengan unit & tool terkunci (§C, §D.4 protokol) |
| Falsifiable? | **Ya** | Setiap hipotesis H1–H4 punya kondisi pembantah eksplisit (§G.2 protokol) |
| Dapat dijawab dengan sumber daya yang ada? | **Ya, bertier** | T0 (30) untuk pipeline; T1 (100) untuk klaim; tidak butuh training/model ML (§F protokol) |
| Tidak duplikat yang trivial? | **Cukup** | Benchmark aplikasi besar ada ([L8, L34, L35]); audit kalibrasi terkontrol yang menjadi niche ini belum ditemukan dalam literatur yang diverifikasi (§7 LITERATURE_REVIEW) |
| Risiko "IQ test" (sudah jelas jawabannya)? | **Dikelola** | Hipotesis diarahkan pada *magnitude & bentuk kegagalan*, bukan hanya arah korelasi yang sudah diketahui |

### 2.2 Batas tegas RQ ini (agar tidak melebar)

RQ ini **bukan**: (a) penilaian utilitas obat/docking ([L16, L35, L37]); (b) prediksi fungsi/biologi baru; (c) pengukuran dinamika protein ([L31]); (d) benchmark penuh proteom ([L8]); (e) penilaian AF3 ([L6]); (f) training apa pun. Semua yang di luar ini tetap di luar lingkup sampai ada dokumen baru.

### 2.3 Catatan kejujuran tentang judul

Judul proyek "Reliability Audit" mengandung kata "audit" — dipakai dalam arti **audit metodologis-terukur** (mengukur kalibrasi confidence terhadap ground truth), bukan audit finansial/kepatuhan. Ini diklarifikasi di laporan awal agar tidak salah tafsir.

---

## 3. Apa yang Didukung Literatur (per komponen desain)

| Komponen desain | Dukungan | Sumber | Catatan |
|---|---|---|---|
| pLDDT bermakna sebagai sinyal akurasi lokal | KUAT | [L1, L11, L12, L13] | Dasar RQ1 dapat diinterpretasi |
| Error global ≠ error lokal; domain orientation = mode kegagalan | KUAT (kualitatif) | [L7]; konsisten [L11, L19] | Magnitude belum diketahui — menjadi hasil audit |
| PAE sebagai sinyal orientasi domain | SEDANG | [L7, L9] (dan definisi AFDB [L39]) | RQ3 layak; validitas empirisnya belum besar-literatur → kontribusi potensial |
| Konformasi & disorder sebagai perancu | KUAT | [L10, L25, L27, L28, L29, L32] | Wajib dikontrol (B4, B7) |
| Leakage/memorization sebagai ancaman validitas | KUAT | [L24, L26, L38] | Wajib stratifikasi (B2, B13) |
| Ground truth eksperimen tidak sempurna | KUAT | [L30] + metadata PDB [L33] | Wajib sensitivitas resolusi (S1–S2) |
| Metrik yang dipilih (TM [L17, L18], lDDT [L19], RMSD [L23]) | KUAT (standar komunitas) | — | Toolchain belum ada di mesin (baseline §3.4) → K-3 |
| Pilot 30 hanya untuk pipeline | Sesuai praktik (prompt) + logika statistik | §F protokol (perhitungan daya) | 30 ≈ deteksi ρ 0.5; 100 ≈ ρ 0.3 |
| AFDB tanpa ligan = batas struktural | KUAT | [L14] + C12 lokal | Pocket keluar dari endpoint V1 (jujur) |

---

## 4. Yang Masih TIDAK Diketahui (UNKNOWN — wajib diselesaikan/dinyatakan sebelum/ sewaktu implementasi)

| # | UNKNOWN | Mengapa penting | Cara menyelesaikan | Kapan |
|---|---|---|---|---|
| U1 | **Tanggal cutoff data latih AF2 yang definitif** | Menentukan stratifikasi pre/post-cutoff (S3, B2/B13) | Verifikasi full text [L1] + dokumentasi resmi | Sebelum filter tanggal dipakai (K-2) |
| U2 | **Ketersediaan tool metrik di lingkungan baru** (TM-align, lDDT, aligner, clustering) | Semua endpoint bergantung padanya | Pilih, instal, uji determinisme → `TOOLS_LOCK.md` | Fase implementasi (K-3) |
| U3 | **Berapa protein yang lolos §A.2–A.3 per tier (yield riil)** | Menentukan apakah T1 tercapai | Ukur di T0; laporan yield | T0 |
| U4 | **Jumlah protein multidomain di T1** | Menentukan RQ3 primer vs eksploratori | Hitung di T0/T1 (F.3) | T0 |
| U5 | **Versi live AFDB saat unduh (post-v6?)** | Konsistensi dengan dataset lokal v6 | Query API saat unduh; catat versi | Fase unduh |
| U6 | **Ketersediaan definisi domain & disorder yang deterministik untuk semua protein terpilih** | RQ3, S5 | Uji cakupan di T0 | T0 |
| U7 | **Lisensi & sitasi final untuk publikasi** (halaman sitasi AFDB yang menyebut paper dataset terbaru) | Kewajiban atribusi | Cek halaman resmi saat publikasi (dicatat di policy §4) | Sebelum publikasi |
| U8 | **Anggaran volume unduhan PDB & format** | Batas operasional | Tentukan per tier (K-5) | Sebelum unduh |

**Bukan UNKNOWN (sudah terverifikasi):** integritas dataset lokal (14/14 + 9/9 hash MATCH, baseline §3); status repo induk (bersih); ketiadaan struktur eksperimen lokal (0 file PDB); ketiadaan tool perbandingan; isi & angka 4 dokumen referensi.

---

## 5. Kandidat Kontribusi Ilmiah (jujur — kecil tapi nyata)

1. **Audit kalibrasi pLDDT/PAE terukur pada set terkontrol-bias baru** — angka kalibrasi (error lokal per bin pLDDT; frekuensi high-confidence failure; validitas PAE cross-domain) pada populasi AFDB-vs-PDB dengan klaster homolog & strata tanggal rilis. Nilai: *estimasi empiris baru pada set baru*, bukan metode baru.
2. **Protokol pra-registrasi + toolchain-terkunci + register bias yang dapat dipakai ulang** — template untuk audit serupa (termasuk jalur kegagalan yang didokumentasikan: 5 DOI salah tertangkap, pemeriksaan D1–D4/F1–F4).
3. **Kuantifikasi awal "biaya konformasi" (apo vs holo) pada pasangan AFDB–PDB** (S4) sebagai hasil sekunder — pertanyaan kecil yang laporan [L27] pancing.

**Yang secara eksplisit BUKAN kontribusi klaim:** metode prediksi baru; peningkatan AF2; klaim "pertama"; klaim utilitas drug discovery ([L16, L35, L37] sudah menunjukkan pemisahan akurasi vs utilitas); hukum umum tentang AI (N kecil, desain observasional).

---

## 6. Blocker Sebelum Implementasi (keputusan user)

Semua blocker bersifat keputusan/konfigurasi — tidak ada blocker ilmiah yang membatalkan desain.

| Kode | Keputusan | Opsi | Rekomendasi audit |
|---|---|---|---|
| K-1 | `git init` di root audit? | init / tidak | **init** (tanpa remote; riwayat dokumen & skrip tertelusur) — keputusan user, temuan F3 |
| K-2 | Verifikasi cutoff data latih [L1] | verifikasi resmi / default sementara berlabel | **verifikasi sebelum dipakai** |
| K-3 | Toolchain metrik + versi (`TOOLS_LOCK.md`) | kandidat: TM-align, lDDT-python/foldseek, aligner, MMseqs | pilih & kunci sebelum metrik pertama |
| K-4 | Definisi domain & disorder (D.5 protokol) | UniProt/Pfam prioritas; fallback geometrik deterministik | kunci sebelum analisis domain |
| K-5 | Tier & anggaran unduhan | T0 dulu (30) | **T0 dulu**; gate review sebelum T1 |
| K-6 | Environment | venv baru di root audit | **ya** (isolasi total dari `ai-biotech`) |
| K-7 | Atribusi & lisensi output publikasi | kebijakan user | susun draf atribusi saat laporan T1 |

---

## 7. Rekomendasi Akhir

### **GO KONDISIONAL** untuk fase implementasi (T0 pilot) — dengan 4 syarat:

1. **Review & persetujuan** kelima dokumen Phase 2 oleh user (baseline + 4 dokumen ini).
2. **Jawaban keputusan §6** — minimal K-2, K-3, K-5, K-6 (K-1 bebas; K-4, K-7 menyusul sebelum titik pakainya).
3. **Urutan eksekusi dipatuhi:** T0 (30 protein) → *laporan T0* → gate review user → T1; tidak melompat.
4. **Stop-condition dipertahankan:** tanpa persetujuan, tidak ada unduhan/instalasi/eksekusi; `ai-biotech` tetap read-only.

### Mengapa bukan HOLD:

- RQ bermakna & operasional; data & lisensi aman; beban kerja bertier; risiko utama (leakage, konformasi, ground truth) sudah punya mitigasi tertulis; dan kalaupun T1 gagal tercapai, **laporan teknis berskala N apa adanya tetap bernilai** (§G.4 protokol).

### Mengapa bukan GO tanpa syarat:

- 7 keputusan terbuka (§6) dan 8 UNKNOWN (§4) — tanpa menyelesaikannya, hasil pertama tidak dapat diinterpretasi jujur (terutama cutoff & toolchain).

### Kondisi berubah menjadi HOLD/STOP (pra-komitmen):

- Jika K-2 (cutoff) tidak dapat diverifikasi dari sumber resmi → seluruh analisis strata S3 di-drop dan B2/B13 diperlakukan sebagai keterbatasan tak-termigasi (protokol tetap jalan dengan penanda).
- Jika yield T0 < 20 protein valid → desain pencocokan direvisi sebelum T1 (laporan T0 memuat usulan).
- Jika user menilai nilai ilmiah tidak sepadan → STOP pada posisi sekarang: 5 dokumen tetap menjadi deliverable final yang koheren.

---

## 8. Kepatuhan Aturan Phase 2 (self-audit)

| Aturan (Keputusan user 2026-10-11) | Status |
|---|---|
| Semua artefak di `D:\project\alphafold-reliability-audit` | **PATUH** — 5 dokumen, semua di `docs/` root audit |
| Tidak ada file baru di `D:\project\ai-biotech` | **PATUH** — diverifikasi ulang di akhir (git status) |
| Tidak mencari/mengarang laporan Phase 1 | **PATUH** — digantikan baseline aktual (F4) |
| Baseline Phase 1 dibuat dari verifikasi aktual | **PATUH** — `PHASE1_BASELINE_AUDIT.md` |
| Tidak auto-init Git / remote | **PATUH** — F3 dilaporkan, keputusan diserahkan |
| Tidak unduh dataset / instal dependensi / eksperimen | **PATUH** — nol unduhan data, nol instalasi, nol eksekusi eksperimen (verifikasi lisensi = baca dokumen) |
| Tidak commit/push | **PATUH** |
| Sitasi tanpa fabrikasi | **PATUH** — 40 sumber terverifikasi; 5 DOI salah tertangkap & dikoreksi |
| Stop & lapor bila blocker | **PATUH** — §6/§7 |

---

## 9. Langkah Berikutnya (menunggu user)

1. User membaca 5 dokumen & mengajukan koreksi.
2. User menjawab keputusan K-1..K-7 (§6).
3. Jika GO: mulai fase implementasi dari **T0 pilot** dengan urutan tetap: K-2/K-3/K-6 → `TOOLS_LOCK.md` → unduh terkontrol → mapping → metrik → laporan T0.
4. Jika HOLD/STOP: dokumen Phase 2 menjadi arsip final; tidak ada aksi lanjutan.

---

*Akhir rantai dokumen Phase 2. Verifikasi silang & SHA256 kelima dokumen terdapat pada lampiran di bawah (hash dihitung setelah dokumen final).*

---

## Lampiran — Verifikasi Silang & SHA256 Dokumen Phase 2

**Pemeriksaan konsistensi (2026-10-11):** angka bersama antar dokumen diperiksa terarah (grep) dan cocok: 1.098 baris residu (26 fitur/29 kolom) — baseline & policy; 1.469.156 byte — baseline (satu-satunya tempat klaim total); mean pLDDT 85.241–97.932 (baseline) vs rentang pembulatan "85–98" (protokol) — konsisten; cutoff default 2018-04-30 dengan status TBD — protokol §0.5 & policy §5; tier T0/T1/T2 = 30/100/300 — protokol §F & dokumen ini §6/§7; rujukan [L1]–[L40] seluruhnya berada dalam daftar `LITERATURE_REVIEW.md`; rujukan metrik C.x dan §-silang (C.12/C.13, F3, K-1..K-7, C1–C12/D1–D4) sudah selaras setelah koreksi. Tidak ada kontradiksi angka yang tersisa.

**SHA256 dokumen (dihitung 2026-10-11, dari disk):**

| Dokumen | SHA256 |
|---|---|
| `docs/PHASE1_BASELINE_AUDIT.md` | `da5adfb9087be790f97b39115c63b0fc198b164529dbb5bac61467b576fa0430` |
| `docs/LITERATURE_REVIEW.md` | `471b24cc3c2edf3482dc4a55a2484f12db42efb383aea55ffb7279a67e5fd069` |
| `docs/RESEARCH_PROTOCOL_V1.md` | `71e665e64697c269eef80437b243409ccff5f5d07b6db3df5ec8da0324ac1536` |
| `docs/DATA_AND_BIAS_POLICY.md` | `fa08e40c4817748770ad353b460e5356fe5ce90773dd16d6e74c864827320b6f` |
| `docs/PHASE2_DECISION.md` (dokumen ini) | dihitung setelah lampiran ini ditambahkan — nilainya dilaporkan di laporan sesi (hash tidak dapat memuat dirinya sendiri) |

**Catatan:** jika salah satu file di atas diubah setelah tanggal ini, hash di tabel ini menjadi tidak berlaku — perubahan wajib melalui `PROTOCOL_AMENDMENTS.md` (protokol §G.5; policy §9).
