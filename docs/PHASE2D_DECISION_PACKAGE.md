# PHASE2D — PRE-REGISTRATION DECISION PACKAGE

## Paket Keputusan Final Sebelum Implementasi T0 — AlphaFold Reliability Audit

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal** | 2026-10-11 |
| **Status** | MENUNGGU KEPUTUSAN PEMILIK — kecuali **D-2, D-3, D-4 (diputuskan pemilik 2026-10-11)** dan **D-5 = 0,80 — praregistrasi; 2026-10-11** (beserta **persetujuan prinsip bersyarat atas elemen C-2(1)**; **bukan** adopsi formal entri C-2), **ambang identitas operasional = ≥30% inklusif — tepat 30,000% termasuk** (keputusan tersendiri; diputuskan pemilik 2026-10-11; konvensi operasional praregistrasi — bukan klaim kebenaran biologis; berlaku setelah adopsi formal entri C-2), serta **D-6 = B 10.000 / seed tetap 20261011** dan **D-6b = X 30 (cakupan gate: T1)** (diputuskan pemilik 2026-10-11; formula operasional sisi graf: WORKING DRAFT / UNRESOLVED) — dokumen ini **tidak menganggap rekomendasi lain sebagai persetujuan** |
| **Dasar** | `PHASE2_DECISION.md` §6/§7; `RESEARCH_PROTOCOL_V1.md` (A–H); `DATA_AND_BIAS_POLICY.md` (§4–§9); `PHASE2_REVIEW_GATE.md` (§4–§9); `PROTOCOL_AMENDMENTS.md` (C-1..C-11); `TOOLS_LOCK.md`; `OWNER_DECISIONS.md` (K-1..K-7, D-1..D-12) |
| **Batasan** | Tidak ada T0, unduhan, instalasi, tes, `git init`, atau perubahan pada dokumen mana pun / `ai-biotech`. Satu-satunya file baru = dokumen ini. |
| **Pra-registrasi** | Seluruh angka/keputusan di bawah diajukan **sebelum ada hasil apa pun** (belum ada data hasil di proyek). |

**Label sumber (dipakai ketat di seluruh dokumen):**

| Tag | Arti |
|---|---|
| `[DOK-BEKU]` | Didukung eksplisit oleh dokumen beku (protokol/policy/decision) — angka/opsi dari dokumen asli |
| `[REVIEW-GATE]` | Fakta yang **sudah diverifikasi** dalam review gate (tanggal 2026-10-11) |
| `[USULAN-CLAUDE]` | Usulan reviewer — **tanpa** dukungan nilai di dokumen; wajib keputusan pemilik |
| `[UNVERIFIED]` | Belum dapat diverifikasi; dilarang ditebak |
| `[BELUM-DIJALANKAN]` | Tes/prosedur yang belum dieksekusi (hanya rencana) |

---

## 1. Keputusan utama K-1 sampai K-7

### K-1 — `git init` di root audit?

| Aspek | Isi |
|---|---|
| **Pertanyaan asli `[DOK-BEKU]`** | "`git init` di root audit?" (`PHASE2_DECISION.md` §6) |
| **Opsi asli `[DOK-BEKU]`** | init / tidak |
| **Rekomendasi audit asli `[DOK-BEKU]`** | "init (tanpa remote; riwayat dokumen & skrip tertelusur) — keputusan user, temuan F3" |
| **Rekomendasi Claude `[USULAN-CLAUDE]`** | init tanpa remote; dilaksanakan sebelum artefak T0 pertama dibuat |
| **Konsekuensi metodologis** | *init* → riwayat versi dokumen/amendemen/`TOOLS_LOCK`/skrip; hash tetap dipakai sebagai pelengkap. *tidak* → ketertelusuran hanya via hash + file amendemen (lebih lemah tapi tetap sah). |
| **Konsekuensi operasional** | Policy §8.8: `git init` dilarang tanpa persetujuan eksplisit — keputusan ini adalah izinnya. Tidak ada remote dalam opsi yang direkomendasikan. |
| **Dependensi & batas waktu** | Tidak bergantung keputusan lain; batas: sebelum artefak T0 pertama. |
| **Status** | **OWNER DECISION REQUIRED** |

### K-2 — Verifikasi cutoff data latih [L1]

| Aspek | Isi |
|---|---|
| **Pertanyaan asli `[DOK-BEKU]`** | "Verifikasi cutoff data latih [L1]" (`PHASE2_DECISION.md` §6) |
| **Opsi asli `[DOK-BEKU]`** | verifikasi resmi / default sementara berlabel |
| **Rekomendasi audit asli `[DOK-BEKU]`** | "verifikasi sebelum dipakai" |
| **Rekomendasi Claude `[USULAN-CLAUDE]`** | ratifikasi nilai terverifikasi `2018-04-30` untuk entri AFDB monomer |
| **Bukti yang sudah ada `[REVIEW-GATE]`** | Full text [L1] verbatim ("maximum release date of 30 April 2018"); metadata AFDB 7/7 entri lokal (`toolUsed: "AlphaFold Monomer v2.0 pipeline"`); catatan teknis v2.3.0 (cutoff `2021-09-30` hanya bobot Multimer) — `PHASE2_REVIEW_GATE.md` §4.3 |
| **Konsekuensi metodologis** | *Ratifikasi* → strata S3 (pre/post-cutoff) berjalan; amendemen C-6 dicatat; ketidakpastian residual (label pipeline = pernyataan penyedia; potensi regenerasi AFDB) tetap dilaporkan. *Tidak* → kondisi pra-komitmen `PHASE2_DECISION.md` §7 aktif: S3 di-drop; B2/B13 = keterbatasan tak-termigasi. |
| **Konsekuensi operasional** | Filter tanggal §B.1 langkah 5 hanya boleh dijalankan setelah ini; manifest wajib mencatat `toolUsed`/`modelCreatedDate`/`latestVersion` (C-6(3)). |
| **Dependensi & batas waktu** | Prasyarat D-3 (sel strata pre/post) dan C-6; batas: sebelum T0. |
| **Status** | **OWNER DECISION REQUIRED** |

### K-3 — Toolchain metrik + versi (`TOOLS_LOCK.md`)

| Aspek | Isi |
|---|---|
| **Pertanyaan asli `[DOK-BEKU]`** | "Toolchain metrik + versi (`TOOLS_LOCK.md`)" (`PHASE2_DECISION.md` §6) |
| **Opsi asli `[DOK-BEKU]`** | "kandidat: TM-align, lDDT-python/foldseek, aligner, MMseqs" |
| **Rekomendasi audit asli `[DOK-BEKU]`** | "pilih & kunci sebelum metrik pertama" |
| **Rekomendasi Claude `[USULAN-CLAUDE]`** | setujui daftar `TOOLS_LOCK.md` §2 (US-align utama/TM-align legacy; OpenStructure 2.12; Foldseek cross-check; MMseqs2; SIFTS+Biopython; gemmi; numpy/scipy; Python 3.12.x) + usulan ambang QC TD-4 = 0,05 |
| **Konsekuensi metodologis** | *Setujui* → instalasi + TD-1..TD-8 di fase implementasi; `RENCANA → LOCKED` hanya setelah lulus. *Ubah kandidat* → beban verifikasi sendiri naik; determinisme + cross-check tetap wajib. |
| **Konsekuensi operasional** | Termasuk keputusan turunan D-11 (platform) setelah verifikasi U-4; tanpa `LOCKED` tidak ada metrik/mapping. |
| **Dependensi & batas waktu** | Prasyarat instalasi; butuh resolusi `UNVERIFIED` (U-1..U-8) saat implementasi; batas: sebelum T0. |
| **Status** | **OWNER DECISION REQUIRED** + **TECHNICAL VERIFICATION REQUIRED** (U-1..U-8) |

### K-4 — Definisi domain & disorder (protokol §D.5)

| Aspek | Isi |
|---|---|
| **Pertanyaan asli `[DOK-BEKU]`** | "Definisi domain & disorder (D.5 protokol)" (`PHASE2_DECISION.md` §6) |
| **Opsi asli `[DOK-BEKU]`** | "UniProt/Pfam prioritas; fallback geometrik deterministik" |
| **Rekomendasi audit asli `[DOK-BEKU]`** | "kunci sebelum analisis domain" |
| **Rekomendasi Claude `[USULAN-CLAUDE]`** | pertahankan prioritas 1 (InterPro/Pfam); fallback = satu metode geometrik deterministik dipin untuk semua protein; waktu keputusan boleh menyusul — kandidat fallback sebelum laporan T0 (uji cakupan U6), penguncian sebelum analisis RQ3 |
| **Konsekuensi metodologis** | *Prioritas 1 saja* → protein tanpa anotasi domain keluar dari RQ3; risiko §F.3 (<20 multidomain → RQ3 eksploratori). *Dengan fallback* → cakupan lebih besar; ketergantungan segmentasi (R8) diakui. Domain dihitung dari struktur referensi (anti-sirkularitas) — sudah benar di §D.5. |
| **Konsekuensi operasional** | Pilihan metode fallback konkret = setelah U6; jika memilih pin tool tambahan → masuk `TOOLS_LOCK`. |
| **Dependensi & batas waktu** | Tidak memblokir T0; blokir analisis domain/RQ3. |
| **Status** | **OWNER DECISION REQUIRED** (NOT BLOCKING T0) |

### K-5 — Tier & anggaran unduhan

| Aspek | Isi |
|---|---|
| **Pertanyaan asli `[DOK-BEKU]`** | "Tier & anggaran unduhan" (`PHASE2_DECISION.md` §6) |
| **Opsi asli `[DOK-BEKU]`** | "T0 dulu (30)" |
| **Rekomendasi audit asli `[DOK-BEKU]`** | "T0 dulu; gate review sebelum T1" |
| **Rekomendasi Claude `[USULAN-CLAUDE]`** | T0=30 dulu + tetapkan angka anggaran (review gate §6 K-5 mengusulkan ≤ ±150 MB; angka final = D-12) |
| **Konsekuensi metodologis** | T0 = pilot tanpa klaim (§F.2/G.4); gate review setelah laporan T0 sebelum T1. |
| **Konsekuensi operasional** | Angka anggaran = gate unduhan (policy §2.2; stop condition). Estimasi ukuran: review gate §5.10 (T0 ≈ puluhan MB). |
| **Dependensi & batas waktu** | D-12 (angka) = bagian dari keputusan ini; batas: sebelum unduhan pertama. |
| **Status** | **OWNER DECISION REQUIRED** |

### K-6 — Environment

| Aspek | Isi |
|---|---|
| **Pertanyaan asli `[DOK-BEKU]`** | "Environment" (`PHASE2_DECISION.md` §6) |
| **Opsi asli `[DOK-BEKU]`** | "venv baru di root audit" |
| **Rekomendasi audit asli `[DOK-BEKU]`** | "ya (isolasi total dari `ai-biotech`)" |
| **Rekomendasi Claude `[USULAN-CLAUDE]`** | ya — `D:\project\alphafold-reliability-audit\.venv`, Python 3.12.x (menyamai lokal 3.12.10) |
| **Konsekuensi metodologis** | Isolasi mutlak (policy §1.5/§3); prasyarat determinisme output (§D.7). |
| **Konsekuensi operasional** | `requirements.txt` versi+hash; tanpa isolasi → risiko polusi lingkungan induk (dilarang). |
| **Dependensi & batas waktu** | Prasyarat semua instalasi; batas: sebelum T0. |
| **Status** | **OWNER DECISION REQUIRED** |

### K-7 — Atribusi & lisensi output publikasi

| Aspek | Isi |
|---|---|
| **Pertanyaan asli `[DOK-BEKU]`** | "Atribusi & lisensi output publikasi" (`PHASE2_DECISION.md` §6) |
| **Opsi asli `[DOK-BEKU]`** | "kebijakan user" (catatan: protokol §H.2 menulis opsi berbeda — konflik #1, §5 di bawah) |
| **Rekomendasi audit asli `[DOK-BEKU]`** | "susun draf atribusi saat laporan T1" |
| **Rekomendasi Claude `[USULAN-CLAUDE]`** | draf saat laporan T1; pin sitasi diperbarui: Fleming 2025 (`10.1016/j.jmb.2025.168967` `[REVIEW-GATE]`) + [L1]; lisensi tool US-align/TM-align diselesaikan sebelum distribusi |
| **Konsekuensi metodologis** | Tidak memengaruhi angka; memengaruhi kepatuhan atribusi publikasi. |
| **Konsekuensi operasional** | Wajib sebelum publikasi eksternal/distribusi kode; tidak memblokir T0. |
| **Dependensi & batas waktu** | Resolusi lisensi U-2 (konteks K-3); batas: sebelum publikasi. |
| **Status** | **NOT BLOCKING T0** (kebijakan tetap OWNER DECISION) |

---

## 2. Keputusan turunan D-1 sampai D-12

### D-1 — Arah enumerasi frame sampling
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-1/A9.1]`:** (a) PDB-initiated (PDB→SIFTS→AFDB); (b) AFDB-initiated.
- **Rekomendasi Claude:** (a). *Alasan:* tractable (metadata PDB → SIFTS), berjangkar ground truth, tanpa unduhan struktur saat enumerasi.
- **Konsekuensi:** (a) deterministik & murah, tercatat verbatim; (b) enumerasi populasi jauh lebih besar → lebih rawan tidak tuntas. Keduanya menargetkan himpunan yang sama (butuh AFDB **dan** PDB); beda pada kelengkapan & biaya prosedur.
- **Dependensi & batas waktu:** langkah pertama sampling; batas: sebelum T0.
- **Status: OWNER DECISION REQUIRED**

### D-2 — Aturan independensi penarikan
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-1/A9.2]`:** (A) maks 1 protein/klaster di set analisis; (B) boleh >1 protein/klaster, andalkan CI cluster-robust.
- **Rekomendasi Claude:** (A). *Alasan:* interpretasi N-efektif paling bersih; mencegah dominasi satu klaster. Cluster-robust CI dilaporkan apa pun pilihannya (C-2).
- **Konsekuensi:** (A) pool per sel strata lebih tipis (dampak yield), independensi terkuat; (B) pool lebih besar tetapi N-efektif < N; klaim wajib menyebut jumlah klaster.
- **Dependensi & batas waktu:** sebelum T0. **Status: DIPUTUSKAN PEMILIK (2026-10-11) — opsi (A):** maksimal satu protein per klaster pada set analisis utama. *Batas:* definisi klaster/ambang identitas/coverage tidak diubah — parameter teknis C-2 ([D-5], tool/versi) tetap usulan.

### D-3 — Kuota strata T0 & T1
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-1/A9.3]`:** angka bebas (ditetapkan pemilik). Usulan tercantum: T0 ≥5/bin mean-pLDDT (bila tersedia), ≥5 multidomain, ≥5/≥5 pre/post-cutoff, ≥3 NMR (bila tersedia); T1 ≥15/bin, ≥30 multidomain, ≥15/≥15 pre-post.
- **Rekomendasi Claude:** nilai di atas. *Alasan:* mengisi sel RQ1/RQ3/S3; ≥30 multidomain T1 agar ambang F.3 (≥20 → RQ3 primer) realistis. Telaah rinci: §3.1.
- **Konsekuensi:** naik → sel lebih kuat tapi risiko under-fill naik (ditangani A9.4 — **bukan** dengan melonggarkan §A.2–A.3); turun → sel tipis, RQ3/S3 lemah.
- **Dependensi & batas waktu:** sel pre/post bergantung K-2; batas: sebelum T0. **Status: DIPUTUSKAN PEMILIK (2026-10-11) — Opsi 1 (angka target):** T0 ≥5/bin mean-pLDDT, multidomain ≥5, NMR target 3 bila tersedia; T1 ≥15/bin, multidomain ≥30, NMR aktual tanpa target numerik; pre/post ≥5/≥15 **hanya bila K-2 diratifikasi**. *Batas:* target bukan jaminan ketercapaian; kelayakan belum diverifikasi (sampling frame & diversitas klaster); K-2 tetap terbuka — sel pre/post belum aktif.

### D-4 — Aturan under-filled stratum + fallback bin level-residu
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-1/A9.4, C-8(2)]`:** (a) adopsi teks A9.4 (laporkan apa adanya + fallback pelengkap level-residu); (b) tanpa fallback (hanya laporkan).
- **Rekomendasi Claude:** (a). *Alasan:* jalur deviasi terkunci sebelum hasil → mencegah kesan post-hoc; fallback tetap pelengkap, bukan pengganti analisis protein.
- **Konsekuensi:** (a) penanganan kuota tak terpenuhi transparan & pra-registered; (b) klaim strata rawan dipertanyakan bila sel kosong.
- **Dependensi & batas waktu:** sebelum T0. **Status: DIPUTUSKAN PEMILIK (2026-10-11) — opsi (a):** laporkan jumlah aktual & kekurangan secara transparan; lanjutkan hanya sesuai protokol; klaim dibatasi sampel aktual (§G.4); fallback hanya sejauh mekanisme, kondisi pemicu, dan prosedur yang sudah ditetapkan di A9.4 sebelum hasil diketahui — tidak diperluas/diubah dari hasil aktual.

### D-5 — Coverage klaster (MMseqs2)
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-2(1)]`:** coverage timbal-balik 0,7 / 0,8 / 1,0 (identitas ≥30% **dari dokumen beku** §0.5/§A.4).
- **Rekomendasi Claude:** 0,8 — nilai ini **usulan murni**, tanpa dukungan nilai di dokumen. Telaah rinci: §3.2.
- **Konsekuensi:** lebih rendah → klaster besar, unit independen menyusut (N-efektif turun, gate D-6b lebih sulit); lebih tinggi → klaster kecil, N naik, risiko menyatukan protein yang hanya berbagi satu domain berkurang tapi definisi "homolog" menyempit.
- **Dependensi & batas waktu:** bergantung K-3 (MMseqs2 dikunci); batas: sebelum T0. **Status: DIPUTUSKAN PEMILIK (2026-10-11) — 0,80:** praregistrasi; bukan validasi empiris; tidak ada bukti yang membedakan 0,7/0,8/1,0. Elemen C-2(1): **disetujui secara prinsip dengan syarat ketat — bukan adopsi formal entri C-2**; pelaksanaan clustering hanya bila V1–V3 lulus; adopsi formal — termasuk slot D-6/D-6b — tetap menunggu §7 butir 2; definisi sisi graf belum final sampai ambang identitas diputuskan. Ambang identitas operasional: **DIPUTUSKAN PEMILIK (2026-10-11) — ≥30% inklusif:** tepat 30,000% termasuk; konvensi operasional praregistrasi — bukan klaim kebenaran biologis; **berlaku setelah adopsi formal entri C-2 melalui mekanisme amendment** (ketidakkonsistenan tekstual §0.5/§A.4/§D.2 dicatat — belum dikoreksi; dokumen beku tidak diubah). Target definisi sisi graf (diputuskan pemilik — **definisi baru**, bukan penafsiran ulang teks beku): komponen terhubung pada graf tak berarah; identitas & coverage timbal-balik terpenuhi pada kedua arah; singleton = satu komponen; himpunan simpul = set analisis — **belum menjadi adopsi formal entri C-2**; formula operasional (identity/alignment/gap/denominator/evaluasi dua arah) & semantik tool: **WORKING DRAFT / UNRESOLVED** — tidak dikunci sebelum spesifikasi eksplisit & non-ambigu dan V1 lulus; ketergantungan semantik MMseqs2 menunggu V1. Izin eksekusi clustering dan T0 tetap tunduk pada gate protokol (`PHASE2_REVIEW_GATE.md` §9; **T0 HOLD**); bila verifikasi gagal → **hentikan dan ajukan pilihan kepada pemilik**.

### D-6 — Bootstrap B, seed, ambang klaster minimum
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-2(3)(4)]`:** B (usulan 10.000); seed tetap (direkam); ambang klaster T1 X (usulan 30).
- **Rekomendasi Claude:** B=10.000, seed tetap direkam, X=30. Telaah rinci: §3.3.
- **Konsekuensi:** B kecil → CI tidak stabil; X tinggi → gate T1 menahan klaim lebih sering; X rendah → CI aproksimatif berisiko dipakai seolah presisi.
- **Dependensi & batas waktu:** pra-registrasi; X mengikat gate T1 (§G.4); batas: sebelum T0. **Status: DIPUTUSKAN PEMILIK (2026-10-11) — B=10.000; seed tetap = 20261011; X=30:** pra-registrasi (bukan validasi empiris). Definisi bootstrap & unit resampling: resample **klaster** dengan penggantian; setiap klaster membawa seluruh protein anggotanya; unit = klaster homolog — konsisten dengan protokol §D.2 (resampling level klaster); dokumen beku tidak menetapkan B/seed (review gate §5.6.4) — tidak ada pertentangan. Interpretasi & cakupan gate (eksplisit): pemicu = jumlah klaster riil **di T1** < 30; konsekuensi (per draf C-2(4) & review gate §5.6.2): CI untuk **klaim primer pada T1** dinyatakan "aproksimatif (jumlah klaster kecil)"/butuh T2, atau tambah protein; kalimat klaim §G.1 menyebut angka jumlah klaster (per draf C-2(4)). **Cakupan = T1 — tidak diperluas** ke CI/endpoint lain **tanpa keputusan pemilik tersendiri**; frasa "seluruh CI" pada draf C-2(4) (belum diadopsi) tidak dibaca sebagai perluasan otomatis. Jumlah klaster dilaporkan T0/T1 (C-2(2)).

### D-7 — Minimum residu per bin (uji RQ1 & monotonisitas)
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-3(1)(2)]`:** K uji berpasangan (usulan 10 residu/bin); monotonisitas ≥1 residu/bin.
- **Rekomendasi Claude:** sesuai usulan. Telaah rinci: §3.4.
- **Konsekuensi:** K tinggi → banyak protein gugur dari uji (N uji turun); K rendah → median bising.
- **Dependensi & batas waktu:** sebelum T0. **Status: OWNER DECISION REQUIRED**

### D-8 — Ambang falsifikasi H2 & perlakuan H4
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-3(4)(5)]`:** (a) H2: ρ>0,8 **dan** batas bawah CI ≥0,7; H4 tanpa ambang "batal" + frasa "jarang" hanya bila batas atas CI <1%; (b) tanpa ambang numerik (deskriptif murni). *Catatan: ρ>0,8 dan "<1%" berasal dari protokol §G.2 `[DOK-BEKU]`; syarat batas-bawah-CI dan relokasi klausa H4 = usulan.*
- **Rekomendasi Claude:** (a). Telaah rinci: §3.5.
- **Konsekuensi:** menentukan kapan boleh ditulis "ekspektasi terbantah" — **pilihan ilmiah milik pemilik**; tidak ada default yang diberlakukan.
- **Dependensi & batas waktu:** sebelum T0. **Status: OWNER DECISION REQUIRED**

### D-9 — Definisi operasional apo/holo
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-4(5)]`:** radius HETATM (usulan 5 Å) + daftar pengecualian direkam (air/ion buffer/krioprotektan); primer = apo bila tersedia. *Latar: §B.1.6 `[DOK-BEKU]` hanya menyebut "state biologisnya paling netral (default: apo)".*
- **Rekomendasi Claude:** sesuai usulan. Telaah rinci: §3.6.
- **Konsekuensi:** radius mengubah label apo/holo → memengaruhi S4 ("biaya konformasi") dan bahasa klaim per protein (B4).
- **Dependensi & batas waktu:** sebelum T0. **Status: OWNER DECISION REQUIRED**

### D-10 — GDT_TS (C.4)
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, C-9(3)]`:** (a) drop dari output V1; (b) pin LGA versi terkunci + jalankan.
- **Rekomendasi Claude:** (a). *Alasan:* C.4 `[DOK-BEKU]` sudah berstatus "pelengkap, bukan endpoint primer"; menghindari mengunci tool legacy tanpa kebutuhan.
- **Konsekuensi:** *drop* → konteks CASP hilang (kecil; bukan endpoint §D.4); *pin* → beban penguncian + verifikasi lisensi/maintenance LGA (masuk `TOOLS_LOCK`).
- **Dependensi & batas waktu:** tidak memblokir eksekusi T0; tenggat: sebelum laporan T0. **Status: OWNER DECISION REQUIRED (NOT BLOCKING T0)**

### D-11 — Platform eksekusi tool
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, TOOLS_LOCK §4]`:** (a) Windows native bila tersedia; (b) WSL2; (c) conda-forge.
- **Rekomendasi Claude:** putuskan **setelah verifikasi U-4** — status ketersediaan `[UNVERIFIED]`; dilarang ditebak.
- **Konsekuensi:** (a) paling sederhana bila tersedia; (b) dukungan tool paling luas, +lapisan WSL2 (path & reproduksibilitas dicatat); (c) paket terkelola, cakupan terbatas.
- **Dependensi & batas waktu:** prasyarat instalasi; batas: awal implementasi sebelum instalasi. **Status: TECHNICAL VERIFICATION REQUIRED → OWNER DECISION**

### D-12 — Angka anggaran unduhan T0
- **Pertanyaan/opsi asli `[USULAN-CLAUDE, K-5/review gate §6]`:** angka bebas (ditetapkan pemilik). Usulan tercantum: ≤150 MB; format mmCIF + PAE JSON + metadata.
- **Rekomendasi Claude:** ≤150 MB. *Alasan:* batas longgar atas kebutuhan riil (estimasi T0 ≈ puluhan MB `[REVIEW-GATE §5.10]`), mencegah scope creep.
- **Konsekuensi:** terlalu kecil → unduhan terbagi; terlalu besar → proteksi melemah.
- **Dependensi & batas waktu:** bagian K-5; batas: sebelum unduhan pertama. **Status: OWNER DECISION REQUIRED**

---

## 3. Telaah mendalam keputusan ilmiah berisiko

> Prinsip bagian ini: setiap angka dipisahkan tegas antara **didukung dokumen** vs **usulan Claude**. Tidak ada angka yang disebut "standar universal" tanpa bukti; tidak ada analisis/hasil pengujian baru yang diciptakan.

### 3.1 D-3 — Kuota strata T0/T1

- **Didukung dokumen `[DOK-BEKU]`:** struktur strata (§A.4: bin pLDDT rendah <70/<50, panjang, metode, tanggal pre/post "terisi"); T1=100 (§F.2); ambang RQ3 ≥20 multidomain (§F.3); T0=30 (§F.2); yield riil = U3 (diukur di T0, belum diketahui).
- **Hanya usulan `[USULAN-CLAUDE]`:** semua angka kuota (≥5/≥15/≥30/≥3/≥5/≥5).
- **Jika angka diubah:** dinaikkan → lebih banyak sel terisi untuk S1–S6/RQ3, tetapi risiko under-fill naik (ditangani A9.4/D-4, bukan dengan melonggarkan §A.2–A.3); diturunkan → sel tipis, CI per-strata lemah, RQ3 lebih mungkin turun ke eksploratori (§F.3).
- **Catatan teknis:** kuota antar-sel dapat tumpang tindih (satu protein bisa memenuhi >1 sel) — angka bukan penjumlahan; total dibatasi T1=100 (§F.2). Kelayakan riil hanya terukur di T0 (U3).
- **Kejujuran:** angka ini **bukan standar universal**; ia menggabungkan struktur dokumen + pilihan kehati-hatian reviewer.

### 3.2 D-5 — Coverage klaster 0,7 / 0,8 / 1,0

- **Didukung dokumen `[DOK-BEKU]`:** ambang identitas >30% (§0.5, §A.4 — threshold dari dokumen, bukan klaim standar universal); kebutuhan klaster sebagai unit statistik (§D.2; konteks [L24]); tool kandidat MMseqs2 (§K-3/`TOOLS_LOCK`).
- **Hanya usulan `[USULAN-CLAUDE]`:** parameter coverage timbal-balik itu sendiri + nilai 0,8; juga implikasi algoritmik (komponen terhubung — dokumen asli tidak menetapkan algoritma klaster).
- **Jika angka diubah:** 0,7 → klaster lebih besar (protein yang berbagi satu domain bisa menyatu) → unit independen menyusut, CI lebih lebar, gate klaster D-6b lebih sulit dilalui; 1,0 → hanya yang tercakup penuh yang menyatu → klaster kecil, N-efektif naik, definisi "berbagi klaster" menyempit.
- **Kejujuran:** tidak ada dokumen eksternal proyek ini yang menetapkan coverage; **bukan standar universal**.

### 3.3 D-6 — Bootstrap B, seed, minimum klaster

- **Didukung dokumen `[DOK-BEKU]`:** keharusan cluster-robust bootstrap (§D.2); gate klaim T1 (§G.4). Review gate §5.6.4 mencatat B/seed **tidak dispesifikasi** di dokumen beku (celah yang kini diusulkan ditutup).
- **Hanya usulan `[USULAN-CLAUDE]`:** B=10.000; nilai seed; X=30.
- **Jika angka diubah:** B kecil (mis. 1.000) → CI kurang stabil; B besar (mis. 20.000) → biaya komputasi naik (kecil), stabilitas naik. Seed: nilai apa pun yang **tetap dan direkam** menghasilkan hasil reproducible — mengganti seed mengganti undian, bukan mengubah prosedur. X: dinaikkan (mis. 50) → klaim T1 lebih sering ditahan/diberi label aproksimatif; diturunkan (mis. 20) → risiko CI terlalu sempit dipakai sebagai presisi.
- **Kejujuran:** tidak ada nilai standar universal untuk B/seed/X dalam dokumen; ini murni pilihan prosedural pra-registrasi.

### 3.4 D-7 — Minimum residu per bin

- **Didukung dokumen `[DOK-BEKU]`:** uji berpasangan pada ringkasan per-protein (§D.4 RQ1); kriteria "≥70% protein" (§G1.1); larangan pseudoreplikasi residu (§A.4, §D.1).
- **Hanya usulan `[USULAN-CLAUDE]`:** K=10 (paired); ≥1 untuk monotonisitas.
- **Jika angka diubah:** K dinaikkan (mis. 20) → protein eligible menyusut → N uji turun (interaksi dengan daya §F.2); K diturunkan (mis. 5) → median per-bin bising, klaim rapuh.
- **Kejujuran:** K=10 **bukan standar universal**; ia pilihan pragmatis yang juga tunduk pada yield riil (U3).

### 3.5 D-8 — Ambang falsifikasi H2/H4

- **Didukung dokumen `[DOK-BEKU]`:** kalimat H2 di §G.2 sudah memuat "ρ>0,8"; kalimat H4 memuat "<1%" — **tetapi** frasa "tanpa heterogenitas" tidak operasional (review gate §5.6.5). G1.2: RQ4 tanpa ambang lulus/gagal.
- **Hanya usulan `[USULAN-CLAUDE]`:** syarat tambahan batas bawah CI ≥0,7 untuk H2; relokasi "<1%" menjadi syarat frasa deskriptif "sangat jarang" (bukan klausa falsifikasi); opsi (b) deskriptif murni.
- **Jika angka diubah:** ambang H2 diperketat (mis. ρ>0,9) → H2 hampir tidak dapat terbantah (kalimat falsifikasi kehilangan daya); dilonggarkan (mis. ρ>0,7) → lebih mudah menyatakan H2 terbantah (hati-hati: klaim lebih agresif). Syarat batas-bawah-CI mencegah "terbantah" karena ρ titik kebetulan tinggi.
- **Kejujuran:** angka penentu interpretasi — keputusan ilmiah penuh pemilik; tidak ada default.

### 3.6 D-9 — Definisi apo/holo & radius 5 Å

- **Didukung dokumen `[DOK-BEKU]`:** §B.1.6 (default apo; holo sebagai keterbatasan; S4 mengukur "biaya konformasi"); B4/R4 (konformasi sebagai perancu).
- **Hanya usulan `[USULAN-CLAUDE]`:** radius 5 Å; daftar pengecualian (air/ion buffer/krioprotektan) + pencatatannya.
- **Jika angka diubah:** radius kecil (mis. 4 Å) → lebih banyak `apo`; radius besar (mis. 6 Å) → lebih banyak `holo`; label memengaruhi struktur mana yang jadi primer dan bagaimana hasil dibaca "terhadap state X".
- **Kejujuran:** 5 Å **bukan standar universal** dalam dokumen; tidak ada angka lain yang didukung dokumen.

### 3.7 K-3 — Ambang QC selisih lDDT OpenStructure vs Foldseek = 0,05

- **Didukung dokumen `[DOK-BEKU]`/`[USULAN-CLAUDE]` review gate:** kewajiban cross-check antar implementasi (S7 §D.6; review gate C-5 "cross-check lDDT pada ≥3 pasang"); **ambang numerik 0,05 = usulan murni Claude** (`TOOLS_LOCK` §6 TD-4), tanpa dukungan nilai di dokumen mana pun.
- **Sifat ambang:** hanya **pemicu investigasi** — tidak mengubah angka yang dilaporkan; jika selisih melewati ambang, kedua hasil diselidiki sebelum dipakai.
- **Jika angka diubah:** diturunkan (mis. 0,02) → lebih sering memicu investigasi (lebih konservatif); dinaikkan (mis. 0,10) → lebih longgar.
- **Kejujuran:** bukan standar universal; alternatif yang sah = tanpa ambang numerik (selalu laporkan selisih, investigasi berdasarkan pertimbangan) — pembaca dapat memilih ini sebagai opsi (b).

---

## 4. Tabel keputusan ringkas

> Kolom **Jawaban pemilik** hanya diisi untuk keputusan yang telah benar-benar ditetapkan pemilik (saat ini: **D-2, D-3, D-4, D-5** — D-2..D-4 2026-10-11; D-5 2026-10-11 = nilai coverage + persetujuan prinsip bersyarat atas elemen C-2(1) — **bukan** adopsi formal entri C-2; **D-6 + D-6b + ambang identitas operasional — 2026-10-11**); selain itu dikosongkan — tidak ada pilihan yang dibuat atas nama pemilik.

### 4.1 K-1..K-7

| ID | Opsi | Rekomendasi Claude | Konsekuensi | Jawaban pemilik |
|---|---|---|---|---|
| K-1 | init / tidak | init tanpa remote | init → riwayat penuh; tidak → hash+amendemen saja | |
| K-2 | verifikasi resmi / default berlabel | ratifikasi 2018-04-30 (bukti §4.3 review gate) | ratifikasi → S3 hidup; tidak → S3 drop (pra-komitmen §7) | |
| K-3 | setujui daftar toolchain / ubah kandidat | setujui (dengan UNVERIFIED diselesaikan saat instalasi) | tanpa lock → tidak ada metrik | |
| K-4 | P1 saja / P1+fallback | P1 + satu fallback deterministik; kandidat sebelum laporan T0 | P1 saja → RQ3 N mengecil (risiko eksploratori §F.3) | |
| K-5 | T0 dulu / langsung besar | T0 dulu + anggaran angka (D-12) | gate review sebelum T1 | |
| K-6 | venv di root audit | ya (Python 3.12.x) | isolasi mutlak; prasyarat instalasi | |
| K-7 | kebijakan user | draf saat laporan T1; pin sitasi Fleming 2025 + [L1] | tidak memblokir T0; wajib sebelum publikasi | |

### 4.2 D-1..D-12

| ID | Opsi | Rekomendasi Claude | Konsekuensi | Jawaban pemilik |
|---|---|---|---|---|
| D-1 | (a) PDB-initiated; (b) AFDB-initiated | (a) | (a) deterministik/murah; (b) rawan tidak tuntas | |
| D-2 | (A) maks 1/klaster; (B) >1 + CI robust | (A) | (A) independensi kuat, pool tipis; (B) N-efektif<N | **(A) — diputuskan 2026-10-11** |
| D-3 | angka bebas; usulan T0 ≥5/bin, ≥5 MD, ≥5/≥5, ≥3 NMR; T1 ≥15/bin, ≥30 MD, ≥15/≥15 | sesuai usulan | naik → risiko under-fill; turun → sel tipis | **Opsi 1 — diputuskan 2026-10-11** (angka target; pre/post kondisional K-2) |
| D-4 | (a) teks A9.4 + fallback; (b) tanpa fallback | (a) | (a) jalur deviasi pra-registered; (b) rawan post-hoc | **(a) — diputuskan 2026-10-11** |
| D-5 | 0,7 / 0,8 / 1,0 | 0,8 | rendah → klaster besar, N-efektif turun; tinggi → menyempit | **0,80 — diputuskan pemilik 2026-10-11** (praregistrasi; bukan validasi empiris); elemen C-2(1): persetujuan prinsip bersyarat — adopsi formal entri C-2 tetap menunggu §7 butir 2; **ambang identitas: ≥30% inklusif — diputuskan pemilik 2026-10-11** (berlaku setelah adopsi formal entri C-2); formula operasional: WORKING DRAFT / UNRESOLVED (V1) |
| D-6 | B (10.000) / seed / X (30) | sesuai usulan | B kecil → CI tak stabil; X tinggi → klaim sering ditahan | **B=10.000; seed tetap = 20261011; X=30 (D-6b; cakupan gate: T1) — diputuskan pemilik 2026-10-11** |
| D-7 | K paired (10) / monot. ≥1 | sesuai usulan | K tinggi → N uji turun; K rendah → bising | |
| D-8 | (a) ρ>0,8 & lb≥0,7 + H4 deskriptif (jarang bila ub CI<1%); (b) deskriptif murni | (a) | menentukan kapan "terbantah" boleh ditulis | |
| D-9 | radius 5 Å + daftar pengecualian | sesuai usulan | radius → label apo/holo → S4 & klaim state | |
| D-10 | (a) drop V1; (b) pin LGA | (a) | (a) konteks CASP hilang (kecil); (b) beban lock tool legacy | |
| D-11 | (a) Windows native; (b) WSL2; (c) conda | putuskan setelah U-4 | menentukan jalur instalasi & reproduksibilitas | |
| D-12 | angka bebas; usulan ≤150 MB | ≤150 MB | kecil → unduhan terbagi; besar → proteksi lemah | |

---

## 5. Konflik antar-dokumen yang terbuka + teks amendemen yang diperlukan

*Tidak ada dokumen beku yang diubah. Teks di bawah = usulan untuk `PROTOCOL_AMENDMENTS.md` setelah persetujuan.*

| # | Konflik (referensi) | Teks amendemen yang diperlukan |
|---|---|---|
| 1 | Bunyi tabel K berbeda: `RESEARCH_PROTOCOL_V1.md` §H.2 vs `PHASE2_DECISION.md` §6 (mis. K-7: "Sitasi & lisensi output | CC BY 4.0…" vs "Atribusi & lisensi output publikasi | kebijakan user") | **Usulan catatan amendemen (baru, kecil):** "Tabel K pada protokol §H.2 adalah deskripsi awal pra-rekomendasi; **bunyi kanonik keputusan = `PHASE2_DECISION.md` §6** (per instruksi pemilik pada review gate). Tidak ada keputusan yang dibaca dari §H.2 bila berbeda. Rujukan silang ini dicatat agar tidak ada dua daftar keputusan yang bersaing." |
| 2 | A3.1 (multi-chain dikecualikan) vs B.1.6 (catat oligomeric state) — `RESEARCH_PROTOCOL_V1.md` §A.3 vs §B.1.6 | Sudah disusun: **`PROTOCOL_AMENDMENTS.md` C-4(3)** (klarifikasi: larangan = endpoint kompleks, bukan keberadaan multi-chain + aturan pemilihan chain deterministik). |
| 3 | Cutoff "TBD 2018-04-30" vs bukti terverifikasi — §0.5/§B.1.5; policy §5.3 vs `PHASE2_REVIEW_GATE.md` §4.3 | Sudah disusun: **C-6** (nilai terverifikasi + nuansa dua-cutoff + field manifest `toolUsed`/`modelCreatedDate`/`latestVersion`). |
| 4 | Policy §4 "tidak dicek ulang ke server live" vs verifikasi live — `DATA_AND_BIAS_POLICY.md` §4 vs review gate §4.4 | Sudah disusun: **C-7(1)(5)** (CC BY 4.0 & CC0 kutipan resmi + status terverifikasi 2026-10-11). |
| 5 | Sitasi [L39]/[L40] vs sitasi resmi baru — policy §4 vs review gate §4.4 | Sudah disusun: **C-7(2)** + **C-10(1)** (pin Fleming 2025 + [L1]; [L39]/[L40] historis). |
| 6 | A3.6 (>70% IDR) vs A.4 (bin low-pLDDT terisi) — §A.3 vs §A.4 | Sudah disusun: **C-1 A9.4 (D-4)** + **C-8(2)** (mekanisme yield + fallback bin level-residu, pra-registered). |
| 7 | Power T1 N=protein vs CI level klaster — §F.2 vs §D.2 | Sudah disusun: **C-2(2)(4)** (N-efektif = klaster; laporan jumlah klaster; gate D-6b). |
| 8 | §G.2 (H2 "tanpa heterogenitas", H4 "<1%") + §D.3/§D.4 tanpa aturan bin kosong — §G.2, §D.3–D.4 | Sudah disusun: **C-3(1)–(5)** (D-7, D-8). |
| 9 | C.4 GDT tanpa keputusan pin; C.5b "kontribusi lDDT" tanpa implementasi — §C | Sudah disusun: **C-9(1)(3)** (local_lddt; GDT = D-10). |
| 10 | B13 sumbu-ii tanpa definisi operasional — `DATA_AND_BIAS_POLICY.md` §6 B13 | Sudah disusun: **C-11** (dari NB-4). |

---

## 6. Validasi kesiapan teknis tanpa eksekusi

### 6.1 Sudah diverifikasi (dokumentasi — `[REVIEW-GATE]`)

| Item | Bukti |
|---|---|
| OpenStructure 2.12 + lisensi LGPL-3.0 + perintah `--lddt --local-lddt --bb-lddt` | Halaman/dokumen resmi (review gate §6 K-3) |
| Foldseek tag `10-941cd33` + GPL-3.0 | GitHub releases + lisensi |
| MMseqs2 tag `18-8cc5c` + MIT | GitHub releases + catatan Release 16 |
| AFDB CC BY 4.0; PDB CC0 1.0 | Halaman resmi (review gate §4.4) |
| Cutoff 2018-04-30 (full text [L1] + metadata AFDB) | Review gate §4.3 |
| Fleming 2025 DOI `10.1016/j.jmb.2025.168967` | Pipa Europe PMC |

### 6.2 Belum diverifikasi / belum dijalankan (WAJIB sebelum titik pakainya)

| # | Prasyarat | Status hari ini | Cara verifikasi (fase implementasi) | Dibutuhkan untuk |
|---|---|---|---|---|
| U-2 | Lisensi US-align/TM-align | **Belum** — halaman resmi tidak memuat pernyataan lisensi (`TOOLS_LOCK` §7) | Periksa ulang halaman resmi/persetujuan tertulis | Sebelum publikasi/distribusi |
| U-4 | Binary Windows native OST/Foldseek/MMseqs2 | **Belum diuji** — ketersediaan tidak terverifikasi dalam review | Uji di lingkungan implementasi → dasar keputusan D-11 | Sebelum instalasi |
| U-6 | Versi dependensi Python yang dapat dipasang (numpy/scipy/gemmi/Biopython) | **Belum** — pin saat instalasi | `pip` plan + versi+hash di `requirements.txt`; uji import | Sebelum LOCK |
| U-5 | Liveness & format SIFTS | **Belum** | Uji endpoint pada 1–2 protein | Sebelum mapping T0 |
| U-8 | Ketersediaan file PAE pada dataset lokal | **Belum** — inventaris lokal read-only belum dilakukan | Cek file PAE di dataset lokal | Sebelum TD-6 |
| U-1/U-3 | Nomor versi US-align (pin via hash) + perintah cek versi persis tiap tool | **Belum** | Kutip dari dokumentasi resmi saat instalasi | Saat LOCK |
| TD-1..TD-8 | Uji determinisme | **BELUM DIJALANKAN** — hanya rencana (`TOOLS_LOCK` §6) | Eksekusi deterministik: input sama dijalankan 2× (TD-1); mode 1-thread bila tool paralel (TD-5); seed tetap untuk transformasi kaku (TD-2); output mentah disimpan + hash; TD-6/TD-7 memakai spesimen pembelajaran lokal tanpa unduhan baru | Sebelum `TOOLS_LOCK = LOCKED` |
| U-7 | Klaster sekuens PDB pra-cutoff (C-11) | **Belum** | Cek ketersediaan saat implementasi | Sebelum laporan T1 |

**Tidak ada** instalasi, pengujian, atau eksekusi yang diklaim dilakukan pada dokumen ini — seluruh status di atas adalah rencana/prasyarat.

---

## 7. Ringkasan keputusan dalam urutan dependensi

1. **K-2** (ratifikasi cutoff) — mengaktifkan C-6 & menentukan sel pre/post D-3.
2. **Persetujuan amendemen C-1..C-6 + nilai D-1, D-7..D-9** (D-2..D-5 telah diputuskan pemilik — D-2..D-4 2026-10-11; D-5 2026-10-11; **D-6/D-6b + ambang identitas operasional diputuskan pemilik 2026-10-11**; elemen C-2(1): persetujuan prinsip bersyarat V1–V3 tercatat — **entri C-2 tetap menunggu adopsi penuh di butir ini, yang tersisa: spesifikasi formula operasional non-ambigu (WORKING DRAFT / UNRESOLVED) + V1–V3**; V1–V3 = verifikasi teknis tahap K-3/`TOOLS_LOCK`, bukan keputusan pemilik) — mengikat mesin sampling, statistik, dan aturan mapping sebelum eksekusi apa pun.
3. **K-6** (venv) + **K-3** (daftar toolchain) — prasyarat instalasi; lalu **D-11** setelah verifikasi U-4.
4. **K-5 + D-12** (tier + angka anggaran) — gerbang unduhan.
5. **K-1** (git init) — sebelum artefak T0 pertama.
6. **D-10** (tenggat: laporan T0) · **K-4** (tenggat: analisis domain; kandidat fallback sebelum laporan T0).
7. **K-7** (tenggat: publikasi).

**Urutan eksekusi setelah persetujuan:** instalasi + verifikasi U-1..U-8 → TD-1..TD-8 lulus → `TOOLS_LOCK = LOCKED` (di-hash) → enumerasi frame (metadata) → sampling A9 → freeze manifest → unduhan terkontrol (anggaran D-12) → mapping → metrik → laporan T0 → **gate review pemilik sebelum T1**.

---

## 8. Pemeriksaan akhir internal

| # | Pemeriksaan | Hasil |
|---|---|---|
| 1 | K-1..K-7 dan D-1..D-12 tercakup | Ya — §1, §2, §4 |
| 2 | Semua angka/opsi memiliki rujukan sumber | Ya — tag `[DOK-BEKU]`/`[REVIEW-GATE]`/`[USULAN-CLAUDE]` per item |
| 3 | Usulan dibedakan dari fakta & keputusan | Ya — label eksplisit; kolom "Jawaban pemilik" kosong kecuali D-2..D-6 + D-6b + ambang identitas operasional (D-2..D-4 2026-10-11; D-5 2026-10-11 = nilai coverage + persetujuan prinsip bersyarat elemen C-2(1); **D-6/D-6b/ambang identitas — 2026-10-11** — **adopsi formal entri C-2 tetap menunggu §7 butir 2**) |
| 4 | Tidak ada klaim tes/instalasi belum dilakukan | Ya — §6.2 memisahkan "belum dijalankan" |
| 5 | Ukuran byte & SHA256 file ini | Dilaporkan di sesi setelah penulisan (hash tidak dapat memuat dirinya sendiri) |
| 6 | File sumber tidak berubah | Diverifikasi ulang via SHA256 setelah penulisan dokumen ini (9 file `docs/`) |

**STOP:** menunggu keputusan eksplisit pemilik. Tidak ada implementasi, instalasi, unduhan, atau T0.
