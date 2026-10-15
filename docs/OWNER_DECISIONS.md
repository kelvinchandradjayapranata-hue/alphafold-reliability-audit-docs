# OWNER DECISIONS — AlphaFold Reliability Audit

## Konsolidasi Keputusan Pemilik Sebelum T0 (Fase Persiapan 2B)

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal** | 2026-10-11 |
| **Status** | MENUNGGU KEPUTUSAN PEMILIK — kecuali **D-2 (opsi (A)), D-3 (Opsi 1 — angka target), D-4 (opsi (a))** (semuanya 2026-10-11), **D-5 (0,80 — praregistrasi; bukan validasi empiris; 2026-10-11)**, dan **persetujuan prinsip bersyarat atas elemen C-2(1)** (2026-10-11; **bukan** adopsi formal entri C-2) — lihat §2. **Adopsi formal C-1..C-6 tetap menunggu; D-6/D-6b diputuskan pemilik 2026-10-11 (B=10.000; seed tetap 20261011; X=30 — cakupan gate T1); ambang identitas operasional diputuskan pemilik 2026-10-11: ≥30% inklusif — tepat 30,000% termasuk (konvensi operasional praregistrasi — bukan klaim kebenaran biologis; berlaku setelah adopsi formal entri C-2 melalui mekanisme amendment); formula operasional sisi graf: WORKING DRAFT / UNRESOLVED (spesifikasi eksplisit & V1 menyusul).** Dokumen ini **tidak memilih apa pun atas nama pemilik** |
| **Dasar** | `PHASE2_DECISION.md` §6 (bunyi asli K-1..K-7), `PHASE2_REVIEW_GATE.md` §6/§7/§9, `PROTOCOL_AMENDMENTS.md` (keputusan turunan D-1..D-12), `RESEARCH_PROTOCOL_V1.md` §H.2 |
| **Cara pakai** | Bagian §1 = keputusan utama (K). Bagian §2 = keputusan turunan yang muncul dari amendemen (D). Bagian §3 = konsistensi & konflik. Bagian §4 = urutan penyelesaian. Bagian §5 = checklist ringkas. |
| **Catatan** | Kolom "usulan" = rekomendasi berbasis bukti (bukan keputusan). Semua angka baru berstatus usulan sampai pemilik menetapkannya. |

---

## 1. Keputusan utama — K-1 sampai K-7

*Bunyi pertanyaan, opsi, dan rekomendasi audit dikutip persis dari `PHASE2_DECISION.md` §6.*

### K-1 — `git init` di root audit?
> **Bunyi asli:** "`git init` di root audit? — Opsi: init / tidak. Rekomendasi audit: **init** (tanpa remote; riwayat dokumen & skrip tertelusur) — keputusan user, temuan F3."

- **Konteks:** baseline F3: root audit bukan repo Git. Policy §8.8 melarang `git init` tanpa persetujuan eksplisit.
- **Opsi:**
  - *init (tanpa remote)*: dokumen/amendemen/`TOOLS_LOCK`/skrip/temuan punya riwayat versi; audit trail lebih kuat daripada mengandalkan hash per file.
  - *tidak*: semua ketertelusuran lewat hash + file amendemen; lebih sederhana tetapi riwayat perubahan antar-hash tidak terlihat.
- **Konsekuensi metodologis:** tidak memengaruhi sains; memengaruhi kekuatan audit trail (review gate mengharapkan ini selesai **sebelum artefak T0 pertama dibuat** bila dipilih).
- **Bukti pendukung:** F3 (baseline); review gate §6 K-1. Catatan: `git init` akan menulis di root audit (bukan `ai-biotech`) — diizinkan hanya setelah persetujuan.
- **Perlu sebelum T0?** Ya — sebaiknya sebelum file T0 pertama dibuat (bukan sebelum unduhan).
- **Yang perlu pemilik nyatakan:** "init tanpa remote" / "tidak" (+ bila init: kapan — sekarang atau saat fase implementasi mulai).

### K-2 — Verifikasi cutoff data latih [L1]
> **Bunyi asli:** "Verifikasi cutoff data latih [L1] — Opsi: verifikasi resmi / default sementara berlabel. Rekomendasi audit: **verifikasi sebelum dipakai**."

- **Konteks:** nilai cutoff menentukan stratifikasi S3 + B2/B13; dilarang diisi dari ingatan (protokol §0.5, policy §5.3). Verifikasi **sudah dilakukan** oleh review gate: full text [L1] verbatim ("maximum release date of 30 April 2018") + metadata AFDB (`toolUsed: "AlphaFold Monomer v2.0 pipeline"`, 7/7 entri lokal) + catatan teknis v2.3.0 (cutoff 2021-09-30 hanya untuk bobot Multimer).
- **Opsi:**
  - *ratifikasi nilai terverifikasi 2018-04-30*: S3 berjalan; amendemen C-6 dicatat; dua-cutoff + pencatatan `toolUsed` per entri ikut disetujui.
  - *tidak meratifikasi / tetap default berlabel*: kondisi pra-komitmen `PHASE2_DECISION.md` §7 aktif — S3 di-drop; B2/B13 menjadi keterbatasan tak-termigasi (tertulis).
- **Konsekuensi metodologis:** menentukan apakah strata pre/post-cutoff menjadi bagian hasil; nuansa (label pipeline = pernyataan penyedia; AFDB dapat meregenerasi entri) tetap dicatat sebagai ketidakpastian residual (review gate §4.3).
- **Perlu sebelum T0?** Ya — filter tanggal (§B.1 langkah 5) dan klasifikasi strata memakainya; amendemen C-6 adalah `[WAJIB-T0]`.
- **Yang perlu pemilik nyatakan:** ratifikasi "cutoff 2018-04-30 (untuk entri monomer AFDB; bukti review gate §4.3)" — ya/tidak.

### K-3 — Toolchain metrik + versi (`TOOLS_LOCK.md`)
> **Bunyi asli:** "Toolchain metrik + versi (`TOOLS_LOCK.md`) — Opsi: kandidat: TM-align, lDDT-python/foldseek, aligner, MMseqs. Rekomendasi audit: pilih & kunci sebelum metrik pertama."

- **Konteks:** semua endpoint bergantung toolchain (U2); policy §8.5 melarang metrik tanpa versi terkunci. Kandidat konkret + status lisensi sudah disiapkan di `TOOLS_LOCK.md` (fase 2B) berdasarkan dokumentasi resmi.
- **Opsi:**
  - *setujui daftar `TOOLS_LOCK.md`* (US-align/TM-align; OpenStructure 2.12; Foldseek; MMseqs2; SIFTS+Biopython; gemmi; numpy/scipy) → instalasi & pengujian determinisme dijalankan di fase implementasi; status file berubah `RENCANA → LOCKED` setelah TD-1..TD-8 lulus.
  - *ubah kandidat* (mis. lDDT implementasi sendiri, tanpa OST) → konsekuensi: menambah beban verifikasi sendiri; tetap wajib uji determinisme + cross-check.
- **Konsekuensi metodologis:** menentukan angka yang dilaporkan; mencampur versi = pelanggaran. Termasuk keputusan platform (D-11: Windows native vs WSL2/conda) setelah hasil verifikasi ketersediaan (U-4).
- **Item `UNVERIFIED` yang tetap terbuka (tidak boleh ditebak):** lisensi US-align/TM-align; ketersediaan binary Windows native OST/Foldseek/MMseqs2; liveness SIFTS; versi persis paket Python (lihat `TOOLS_LOCK.md` §8).
- **Perlu sebelum T0?** Ya — tidak ada metrik/mapping tanpa tool terkunci.
- **Yang perlu pemilik nyatakan:** approve daftar toolchain `TOOLS_LOCK.md` (ya/tidak + perubahan); saat atau sebelum instalasi.

### K-4 — Definisi domain & disorder (protokol §D.5)
> **Bunyi asli:** "Definisi domain & disorder (D.5 protokol) — Opsi: UniProt/Pfam prioritas; fallback geometrik deterministik. Rekomendasi audit: kunci sebelum analisis domain."

- **Konteks:** RQ3 memakai RQ3-eligibility (≥2 domain); S5 memakai anotasi disorder; U6 = ketersediaan definisi deterministik diuji di T0. Sumber: protokol §D.5 (prioritas 1 UniProt/Pfam; prioritas 2 geometrik deterministik, dari **struktur referensi**).
- **Opsi:**
  - *prioritas 1 saja* (InterPro/Pfam) untuk semua protein; protein tanpa anotasi domain → keluar dari RQ3.
  - *prioritas 1 + fallback geometrik* (satu metode deterministik dipin untuk semua protein) — memperbesar cakupan tetapi menambah ketergantungan pada metode fallback (R8 diakui).
- **Konsekuensi metodologis:** menentukan N multidomain (F.3: <20 di T1 → RQ3 eksploratori); memengaruhi S5 (pemisahan disorder).
- **Perlu sebelum T0?** Untuk perhitungan domain: tidak (boleh menyusul sebelum analisis domain/RQ3). Untuk uji cakupan U6 di T0: identifikasi kandidat fallback dianjurkan **sebelum laporan T0**.
- **Yang perlu pemilik nyatakan:** pilihan kombinasi prioritas + metode fallback (atau menunda keputusan sampai hasil cakupan T0).

### K-5 — Tier & anggaran unduhan
> **Bunyi asli:** "Tier & anggaran unduhan — Opsi: T0 dulu (30). Rekomendasi audit: **T0 dulu**; gate review sebelum T1."

- **Konteks:** U8/§H.2: batas volume wajib disetujui sebelum fase unduh. Estimasi ukuran (review gate §5.10): T0 ≈ puluhan MB; wajar dengan anggaran kecil.
- **Opsi:**
  - *T0 dulu (30 protein) + anggaran angka* (usulan review gate: ≤ ±150 MB untuk T0; format mmCIF + PAE JSON + metadata; tanpa bulk) → gate review setelah laporan T0 sebelum T1.
  - *langsung tier lebih besar*: bertentangan dengan rekomendasi audit & desain bertier (§F.2).
- **Konsekuensi metodologis:** anggaran angka = keputusan pemilik (policy §2.2); urutan gate melindungi dari pemborosan & scope creep.
- **Perlu sebelum T0?** Ya — ini gate unduhan (stop condition).
- **Yang perlu pemilik nyatakan:** setujui "T0=30 dulu" + angka anggaran (usulan ≤150 MB atau angka lain).

### K-6 — Environment (venv baru di root audit)
> **Bunyi asli:** "Environment — Opsi: venv baru di root audit. Rekomendasi audit: **ya** (isolasi total dari `ai-biotech`)."

- **Konteks:** policy §1.5/§3 (isolasi mutlak); protokol §D.7. Python 3.12.x disarankan (menyamai pipeline lokal 3.12.10).
- **Opsi:** *venv di `D:\project\alphafold-reliability-audit\.venv`* (ya) / *lain* (mis. conda env — bila D-11 memilih WSL2/conda, environment Python tetap di root audit).
- **Konsekuensi metodologis:** prasyarat semua instalasi & determinisme; tanpa ini tidak ada T0.
- **Perlu sebelum T0?** Ya.
- **Yang perlu pemilik nyatakan:** ya/tidak (+ lokasi, bila berbeda).

### K-7 — Atribusi & lisensi output publikasi
> **Bunyi asli:** "Atribusi & lisensi output publikasi — Opsi: kebijakan user. Rekomendasi audit: susun draf atribusi saat laporan T1."

- **Konteks:** U7 terjawab sebagian: sitasi resmi AFDB kini = Fleming et al. 2025 (DOI `10.1016/j.jmb.2025.168967`, PMID 40133787 — terverifikasi) + [L1] (review gate §4.4); lisensi AFDB CC BY 4.0 & PDB CC0 1.0 terverifikasi live.
- **Opsi:** *susun draf saat laporan T1* (rekomendasi) / *tetapkan format sekarang* (mis. gaya sitasi laporan).
- **Konsekuensi metodologis:** tidak memengaruhi T0; memengaruhi kepatuhan atribusi publikasi eksternal (termasuk lisensi tool US-align/TM-align yang masih `UNVERIFIED`).
- **Perlu sebelum T0?** Tidak — sebelum publikasi eksternal apa pun.
- **Yang perlu pemilik nyatakan:** kapan draf atribusi dibuat + kebijakan lisensi output riset (saat ini: dokumen kerja internal; lisensi publikasi = keputusan pemilik).
- **Status 2026-10-15:** lisensi **dokumentasi asli proyek** ditetapkan **CC BY 4.0** (keputusan `A-4`) — butir lisensi dokumentasi pada `A-3` §4 butir (i) selesai. Cakupan `K-7` lainnya (kebijakan atribusi output riset, lisensi tool `UNVERIFIED` mis. US-align/TM-align) **tetap terbuka**.

---

## 2. Keputusan turunan dari amendemen (D-1..D-12)

*Muncul dari `PROTOCOL_AMENDMENTS.md`; nilai ditetapkan pemilik (semua tetap usulan sampai diputuskan — status per log di bawah tabel). "Blokir T0" = harus diputuskan sebelum T0 dimulai.*

| ID | Keputusan | Opsi | Usulan (bukan keputusan) | Blokir T0 | Ref |
|---|---|---|---|---|---|
| **D-1** | Arah enumerasi frame sampling | (a) PDB-initiated (PDB→SIFTS→AFDB); (b) AFDB-initiated | (a) — tractable; metadata saja; ground-truth anchored | Ya | C-1 A9.1 |
| **D-2** | Aturan independensi penarikan | (A) maks 1 protein/klaster di set analisis; (B) boleh >1, andalkan CI cluster-robust | **(A) — DIPUTUSKAN PEMILIK 2026-10-11** (N-efektif = N) | Ya | C-1 A9.2 |
| **D-3** | Kuota strata T0 & T1 | angka bebas; usulan T0: ≥5/bin mean-pLDDT, ≥5 multidomain, ≥5/≥5 pre-post, ≥3 NMR; T1: ≥15/bin, ≥30 multidomain, ≥15/≥15 pre-post | **DIPUTUSKAN PEMILIK 2026-10-11 — Opsi 1 (angka target)**; pre/post kondisional ratifikasi K-2 | Ya | C-1 A9.3 |
| **D-4** | Aturan under-filled stratum + fallback bin level-residu | (a) adopsi teks A9.4 (laporkan apa adanya + fallback pelengkap); (b) tanpa fallback (hanya laporkan) | **(a) — DIPUTUSKAN PEMILIK 2026-10-11** (fallback pre-registered mencegah post-hoc; eksekusi terbatas pada teks A9.4) | Ya | C-1 A9.4, C-8 |
| **D-5** | Coverage klaster (MMseqs2) | coverage timbal-balik: 0,7 / **0,8** / 1,0 (identitas: keputusan tersendiri — diputuskan pemilik 2026-10-11: ≥30% inklusif — tepat 30,000% termasuk; berlaku setelah adopsi formal entri C-2; lihat blok keputusan entri C-2) | **0,8 — DIPUTUSKAN PEMILIK 2026-10-11** (praregistrasi; bukan validasi empiris) | Ya | C-2(1) |
| **D-6** | Bootstrap: B, seed, ambang klaster minimum | B (usulan 10.000); seed tetap (usulan direkam, mis. turunan tanggal protokol); ambang klaster T1 (usulan 30) | **B=10.000; seed tetap = 20261011; ambang klaster T1 (D-6b) X=30 — DIPUTUSKAN PEMILIK 2026-10-11** (pra-registrasi; bukan validasi empiris; cakupan gate D-6b: T1) | Ya (pra-registrasi) | C-2(3)(4) |
| **D-7** | Min residu per bin untuk uji RQ1 & monotonisitas | K uji berpasangan (usulan 10/bin); monotonisitas: ≥1/bin | seperti opsi | Ya | C-3(1)(2) |
| **D-8** | Ambang falsifikasi H2 & perlakuan H4 | (a) H2: ρ>0,8 & lb CI ≥0,7; H4 tanpa ambang batal + frasa "jarang" bila batas atas CI <1%; (b) tanpa ambang numerik (deskriptif murni) | (a) | Ya | C-3(4)(5) |
| **D-9** | Definisi operasional apo/holo | radius HETATM (usulan 5 Å) + daftar pengecualian (air/ion buffer/krioprotektan, direkam); primer = apo bila ada | seperti opsi | Ya | C-4(5) |
| **D-10** | GDT_TS (C.4) | (a) drop dari output V1; (b) pin LGA versi terkunci + jalankan | (a) | Tidak (sebelum laporan T0) | C-9(3) |
| **D-11** | Platform eksekusi tool | (a) Windows native bila tersedia; (b) WSL2; (c) conda-forge | putuskan setelah verifikasi U-4 saat instalasi | Ya (prasyarat instalasi) | TOOLS_LOCK §4 |
| **D-12** | Angka anggaran unduhan T0 | bebas (usulan ≤150 MB; format mmCIF + PAE JSON + metadata) | ≤150 MB | Ya | K-5, policy §2.2 |

*Konvensi timestamp: setiap timestamp pada log keputusan ini — dan pada heading blok keputusan terkait di `PROTOCOL_AMENDMENTS.md` — dinyatakan dalam **UTC** dan disertai label `UTC`.*

**Log keputusan pemilik — 2026-10-11:** **D-2 = opsi (A) — maksimal satu protein per klaster pada set analisis utama** (satu klaster menyumbang maksimal satu protein; N-efektif = jumlah protein = jumlah klaster). Batas: keputusan ini **tidak** mengubah definisi klaster, ambang identitas (>30% — dokumen asli §0.5/§A.4), maupun parameter teknis yang masih usulan (coverage [D-5], tool & versi [K-3], bootstrap B/seed/gate [D-6]). Penerapan teks: `PROTOCOL_AMENDMENTS.md` C-1/A9.2. **Keputusan terbuka berikutnya yang disarankan: D-3.**

**Log keputusan pemilik — 2026-10-11 (06:15 UTC):** **D-3 = Opsi 1 (angka target):** T0 (30 protein): minimal 5/bin mean-pLDDT, multidomain ≥5, NMR target 3 bila tersedia; T1 (100 protein): minimal 15/bin, multidomain ≥30, NMR dilaporkan aktual tanpa target numerik; pre/post-cutoff ≥5 (T0) / ≥15 (T1) **hanya bila K-2 diratifikasi** — bila tidak, aturan S3 yang berlaku dijalankan. Target **bukan jaminan ketercapaian**; kelayakan kuota **belum terbukti** (sampling frame & diversitas klaster belum diverifikasi). **D-4 = opsi (a):** laporkan jumlah aktual & kekurangan secara transparan; lanjutkan hanya sesuai protokol; klaim dibatasi sampel aktual (§G.4); fallback (A9.4(d)) hanya sejauh mekanisme, kondisi pemicu, dan prosedur yang sudah ditetapkan sebelum hasil diketahui — tidak diperluas/diubah dari hasil aktual. **K-2 tetap belum diratifikasi — sel pre/post belum aktif.** Penerapan teks: `PROTOCOL_AMENDMENTS.md` C-1/A9.3–A9.4. **Keputusan terbuka berikutnya yang disarankan: D-5.**

**Log keputusan pemilik — 2026-10-11 (07:54 UTC):** **D-5 = 0,80** (coverage alignment timbal-balik; keputusan **praregistrasi** — bukan hasil validasi empiris; tidak ada angka pembanding di dokumen beku). **Elemen C-2(1) — disetujui secara prinsip dengan syarat ketat (bukan adopsi formal entri):** definisi yang **ditargetkan** = komponen terhubung dari graf pasangan yang memenuhi ambang identitas (keputusan tersendiri — lihat kalimat berikutnya) dan coverage ≥ D-5; definisi **belum final/mengikat** sampai ambang identitas diputuskan dan entri diadopsi penuh; implementasi = kandidat `MMseqs2 easy-cluster`; kesetaraan keluaran ↔ definisi **tidak diklaim** — status **BELUM DIVERIFIKASI**; pelaksanaan clustering & T0 hanya bila ambang identitas ditetapkan, entri C-2 diadopsi formal, dan **V1–V3 lulus**; gagal → hentikan & ajukan pilihan ke pemilik (tanpa penggantian algoritme otomatis). Status entri C-2 tetap **"USULAN — BELUM BERLAKU"**; adopsi formal menunggu `PHASE2D_DECISION_PACKAGE.md` §7 butir 2 (slot D-6/D-6b tetap `OWNER DECISION REQUIRED`). **Ambang identitas operasional (≥30% inklusif vs >30% strict) = keputusan tersendiri, masih terbuka** — kedua formulasi **tidak disamakan** pada batas tepat 30%; konflik §0.5/§A.4/§D.2 dicatat tanpa mengubah dokumen beku. **K-2 tetap belum diratifikasi — sel pre/post belum aktif.** **Mencatat log ini bukan izin eksekusi** (tanpa tes/clustering/T0). **T0 tetap HOLD.** Keputusan terbuka berikutnya yang disarankan: ambang identitas (bila belum ditetapkan), lalu D-6/D-6b. Penerapan teks: `PROTOCOL_AMENDMENTS.md` entri C-2 (blok keputusan); riwayat usulan lama dipertahankan.

**Log keputusan pemilik — 2026-10-11 (09:12 UTC):** **Ambang identitas operasional = ≥30% inklusif — tepat 30,000% termasuk** (konvensi operasional praregistrasi; **bukan klaim kebenaran biologis**; ketidakkonsistenan tekstual dokumen beku pada batas ini (§0.5/§A.4/§D.2) **dicatat — belum dikoreksi**; dokumen beku tidak diubah; rumusan operasional tunggal **berlaku setelah adopsi formal entri C-2 melalui mekanisme amendment**; realisasi batas eksak diverifikasi V1 — gagal → berhenti & ajukan pilihan ke pemilik, definisi tidak diubah agar cocok dengan tool). **Target definisi klaster (untuk adopsi C-2):** komponen terhubung pada graf tak berarah; sisi hanya bila kriteria identitas & coverage timbal-balik terpenuhi **pada kedua arah**; **singleton = satu komponen**; **himpunan simpul = set analisis yang sedang dievaluasi** — **definisi baru** yang berlaku **setelah adopsi formal entri C-2**; bukan penafsiran ulang otomatis teks beku. **Formula operasional (identity, alignment, gap, denominator coverage, evaluasi dua arah) & semantik tool: WORKING DRAFT / UNRESOLVED** — tidak dikunci sebelum spesifikasi eksplisit & non-ambigu (cukup untuk reference implementation) **dan V1 lulus**; ketergantungan semantik MMseqs2 menunggu V1. **D-6 = B 10.000; seed tetap 20261011** (unit resampling = klaster; definisi per C-2(3); konsisten dokumen beku §D.2/§D.7). **D-6b = X 30** (**cakupan gate: T1** — per §G.4 & teks C-2(4)/review gate §5.6.2; tidak diperluas ke CI/endpoint lain tanpa keputusan pemilik tersendiri; bukan validasi empiris). **Mencatat log ini bukan izin eksekusi** — V1–V3 belum dijalankan/belum diotorisasi; adopsi formal entri C-2 tetap menunggu §7 butir 2; **T0 tetap HOLD.** Penerapan teks: `PROTOCOL_AMENDMENTS.md` entri C-2 (blok keputusan (f)–(k)); riwayat usulan lama dipertahankan.

**Log keputusan pemilik — 2026-10-15 (UTC; jam tidak dicatat — tidak ditetapkan pada saat persetujuan):** **A-1 — Retensi studi kasus eksploratif myoglobin.** ID **`A-1`** membuka **seri administratif** baru; ia **bukan** bagian seri K (keputusan utama) maupun D (keputusan turunan dari amendemen) — keputusan ini tidak berasal dari entri amendemen mana pun dan **tidak memasok teks pengganti** protokol/policy. Tidak ada tabrakan ID: K-1..K-7 dan D-1..D-12 sudah terpakai; lampiran protokol memakai notasi bertitik (mis. §A.9). **Keputusan ini tidak memengaruhi hitungan "Keputusan turunan (12)" di §5.**

**(1) Yang diputuskan.** Hasil studi kasus myoglobin **dipertahankan** sebagai **studi eksploratif non-praregistrasi**, terpisah dari T0. Eksekusi Colab **2026-10-15** dicatat sebagai **eksekusi pada fase dokumentasi tanpa catatan otorisasi yang ditemukan** pada `docs/`. **Tidak ada otorisasi retroaktif yang diberikan atau disimpulkan.**

**(2) Batas yang mengikat.** **T0 tetap HOLD.** Keputusan ini **tidak memenuhi** prasyarat T0, **tidak mengubah** endpoint, **tidak mengadopsi** C-2, dan **tidak mengizinkan** eksekusi lanjutan. C-2 tetap `USULAN — BELUM BERLAKU`; V1–V3 belum dijalankan/belum diotorisasi; K-2 belum diratifikasi.

**(3) Batas klaim hasil.** RMSD yang diperoleh berlaku **hanya** sebagai temuan studi kasus **satu** model myoglobin (`AF-P02185-F1`) dibandingkan **tiga** struktur eksperimen terkait (1BZ6, 1BZP, 1BZR). Ketiga nilai **bukan validasi independen** dan **tidak** mewakili akurasi AlphaFold DB secara umum. Status **N-terminal metionin tetap UNRESOLVED/UNVERIFIED**. **pLDDT bukan pengukuran akurasi eksperimental.**

**(4) Yang tetap belum terverifikasi.** **Provenance**, **versi tool** yang dipakai, dan **kelayakan publikasi** tetap belum terverifikasi.

**(5) Batas pakai hasil.** Studi kasus **tidak memenuhi** ketentuan praregistrasi dan **tidak** dapat dipakai sebagai dasar klaim protokol. Artefak tetap di luar Git dan tidak dianggap terbit.

**(6) Pemisahan dari penilaian kepatuhan.** Keputusan retensi hasil pada butir (1) **berdiri sendiri** dari penilaian kepatuhan eksekusi. Penilaian kepatuhan atas eksekusi 2026-10-15 terhadap `DATA_AND_BIAS_POLICY.md` §8 **tidak dibuat** di entri ini; disposisinya dicatat terpisah di `PROTOCOL_AMENDMENTS.md`.

**(7) Status sebelum/sesudah melihat hasil (wajib — protokol §G.5; policy §9).** Keputusan retensi ini diambil **setelah** hasil studi kasus terlihat. Fakta tersebut dicatat apa adanya; entri ini **tidak menyimpulkan** status kepatuhan pra-registrasi (lihat butir 6).

**(8) Dampak dokumen.** **Tidak ada** dokumen beku yang diubah. Kelima hash dokumen Phase 2 tetap berlaku; verifikasi ulang dilaporkan di sesi.

**Log keputusan pemilik — 2026-10-15 (UTC; jam tidak dicatat — tidak ditetapkan pada saat keputusan dicatat):** **A-2 — Penetapan status kepatuhan eksekusi 2026-10-15 terhadap `DATA_AND_BIAS_POLICY.md` §8.9.** ID `A-2` melanjutkan seri administratif yang dibuka `A-1`; ia **tidak memasok teks pengganti** protokol/policy dan **tidak** mengubah hitungan "Keputusan turunan (12)" di §5.

**Rumusan keputusan (dicatat apa adanya dari pemilik):** *"Berdasarkan catatan yang tersedia, eksekusi Colab pada 15 Oktober 2026 tidak sesuai dengan ketentuan fase dokumentasi dalam `DATA_AND_BIAS_POLICY.md` §8.9, yang membatasi Phase 2 pada dokumentasi saja. Tidak ditemukan catatan otorisasi dalam `docs/`. Penetapan ini tidak menyimpulkan niat pelaksana maupun meniadakan kemungkinan adanya bukti di luar dokumentasi yang diperiksa. Hasil myoglobin tetap dipertahankan sebagai studi kasus eksploratif non-praregistrasi. Tidak ada otorisasi retroaktif dan T0 tetap HOLD."*

**Konteks penetapan.** Butir `A-1`(6) menyatakan penilaian kepatuhan **tidak dibuat** pada entri itu dan menunggu keputusan pemilik tersendiri; keputusan tersendiri tersebut **diambil di sini**. Sejalan dengan `A-1`(7), penetapan ini dibuat **setelah** hasil studi kasus terlihat (wajib — protokol §G.5; policy §9).

**Yang tidak berubah.** Disposisi retensi `A-1` tetap berlaku. **T0 tetap HOLD**; C-2 tetap `USULAN — BELUM BERLAKU`; V1–V3 belum dijalankan/belum diotorisasi; K-2 belum diratifikasi. **Tidak ada otorisasi** — retroaktif maupun untuk eksekusi lanjutan.

**Dampak dokumen.** Tidak ada dokumen beku yang diubah. `docs/PROTOCOL_AMENDMENTS.md` menerima satu **catatan silang** di bagian akhir agar butir (5) entri "Disposisi Eksekusi … 2026-10-15" tidak terbaca sebagai status yang masih berlaku; entri tersebut sendiri **tidak diubah**.

**Log keputusan pemilik — 2026-10-15 (UTC; jam tidak dicatat — tidak ditetapkan pada saat keputusan dicatat):** **A-3 — Arah publikasi artefak: `derived artifacts only`, bersyarat.** ID `A-3` melanjutkan seri administratif yang dibuka `A-1`; ia **tidak memasok teks pengganti** protokol/policy dan **tidak mengubah** hitungan "Keputusan turunan (12)" di §5. Keputusan ini **tidak menulis ulang** `A-1` maupun `A-2`.

**(1) Yang diputuskan.** Arah publikasi artefak repositori ini ditetapkan **hanya untuk artefak turunan (derived artifacts)**, dan hanya **setelah syarat pada butir (4) terpenuhi**. Keputusan ini **bukan** izin menerbitkan, **bukan** izin `push`, dan **bukan** persetujuan atas paket publikasi tertentu — ia menetapkan **arah dan batas**, bukan pelaksanaan.

**(2) Batas isi yang mengikat.** (a) **CIF mentah tidak dipublikasikan** — baik model AlphaFold DB (`AF-P02185-F1.cif`; CC-BY-4.0 dengan pernyataan hak cipta DeepMind, wajib atribusi) maupun struktur eksperimen RCSB (1BZ6/1BZP/1BZR; tidak memuat blok lisensi di dalam berkas). (b) **Aset gambar yang sumber/lisensinya belum jelas tidak dipublikasikan** — termasuk `image/Protein Structure Analysis Dashboard.png`. (c) **Klaim RMSD sebagai hasil verifikasi independen tidak dipublikasikan.** (d) Status angka dipertahankan: **`reported, not independently verified`** — tanpa penghitungan ulang.

**(3) Status kesiapan.** **Belum ada artefak yang dinyatakan siap diterbitkan.** Daftar kandidat (13 berkas turunan, bersyarat), berkas yang ditahan (21 berkas beserta alasan), hasil pemeriksaan dependensi manifest, dan blocker dicatat di `docs/PUBLICATION_STATUS.md` (dokumen baru pada tanggal ini). Tidak ada berkas yang di-staging, di-commit, atau di-push bersamaan dengan keputusan ini.

**(4) Syarat yang belum terpenuhi.** (i) kebijakan lisensi & atribusi output (**K-7**) belum ditetapkan — K-7 tetap terbuka; (ii) **provenance belum terverifikasi** — kecocokan hash lokal terhadap manifest lokal bukan bukti asal-usul eksekusi; (iii) penerbitan hasil eksekusi yang **ditetapkan tidak sesuai `§8.9`** (`A-2`) belum memperoleh penetapan tersendiri — `A-3` menetapkan arah, **bukan** menyatakan penghalang kepatuhan selesai; (iv) **versi tool & parameter tidak tercatat**; (v) penanganan `.gitignore` untuk `*.csv` (12 dari 13 kandidat tertutup pola) belum diputuskan; (vi) **disposisi duplikat byte-identik** belum diputuskan; (vii) label `verified`/`independent`/`recomputed` pada artefak belum diklarifikasi. Rincian per syarat: `docs/PUBLICATION_STATUS.md` §4.

**(5) Yang tidak berubah.** **T0 tetap HOLD**; prasyarat CONDITIONAL GO tetap belum terpenuhi. `A-1` (retensi studi kasus) dan `A-2` (penetapan ketidaksesuaian `§8.9`) tetap berlaku. **Tidak ada otorisasi retroaktif**, dan tidak ada otorisasi untuk eksekusi, unduhan, instalasi, penghitungan ulang RMSD, atau operasi Git. C-2 tetap `USULAN — BELUM BERLAKU`; V1–V3 belum dijalankan/belum diotorisasi; K-2 belum diratifikasi.

**(6) Batas operasi Git.** Selama keputusan ini berlaku: **`git add -A` dilarang**; staging hanya boleh atas **daftar berkas eksplisit** yang tercatat; `Downloads/` (**25,01 GiB**; 3.669 berkas tidak tertutup `.gitignore`) tidak boleh masuk staging; tidak ada penghapusan atau pemindahan berkas yang dijalankan atas keputusan ini.

**(7) Status sebelum/sesudah melihat hasil (wajib — protokol §G.5; policy §9).** Keputusan ini diambil **setelah** hasil studi kasus **dan** setelah audit kesiapan publikasi artefak (read-only, 2026-10-15) terlihat. Fakta tersebut dicatat apa adanya; entri ini tidak menyimpulkan status kepatuhan pra-registrasi (lihat `A-2`).

**(8) Dampak dokumen.** **Tidak ada** dokumen beku yang diubah; kelima hash dokumen Phase 2 tetap berlaku. `docs/PUBLICATION_STATUS.md` **dibuat** pada tanggal ini (ringkasan status publikasi — bukan pengulangan audit). `README.md` diperbarui **hanya** pada bagian yang menyebut jumlah dan daftar dokumen agar tetap sesuai. SHA-256 sebelum/sesudah dilaporkan di sesi.

**Log keputusan pemilik — 2026-10-15 (UTC; jam tidak dicatat — tidak ditetapkan pada saat keputusan dicatat):** **A-4 — Lisensi dokumentasi asli proyek: CC BY 4.0.** ID `A-4` melanjutkan seri administratif yang dibuka `A-1`; ia **tidak memasok teks pengganti** protokol/policy dan **tidak mengubah** hitungan "Keputusan turunan (12)" di §5. Keputusan ini **tidak menulis ulang** `A-1`, `A-2`, maupun `A-3`.

**(1) Yang diputuskan.** Lisensi **CC BY 4.0** (`Creative Commons Attribution 4.0 International`) disetujui **untuk dokumentasi asli proyek** — berkas dokumentasi yang ditulis untuk proyek ini. Keputusan ini **bukan** izin publikasi, **bukan** izin `push`, **bukan** persetujuan atas paket publikasi tertentu, dan **bukan** lisensi atas materi apa pun selain dokumentasi.

**(2) Ruang lingkup & batas.** (a) Lisensi **tidak** mencakup dataset, artefak data, CIF, CSV, JSON, atau manifest mana pun — semuanya tetap dikecualikan (kebijakan `A-3` tidak berubah). (b) Lisensi **tidak** mencakup aset gambar/visual dalam repositori — sumber/lisensinya belum terdokumentasi; aset tersebut **tidak** disertakan pada paket rilis dan **tidak** dinyatakan bebas lisensi. (c) Lisensi **tidak** mencakup materi pihak ketiga (mis. CIF AlphaFold DB/RCSB PDB, pustaka) — lisensinya berlaku tersendiri dan wajib diverifikasi ulang ke sumber resmi sebelum publikasi eksternal (policy §4). (d) Penerapan otomatis ke gambar/dataset/materi pihak ketiga **dilarang** oleh keputusan ini.

**(3) Pelaksanaan.** Berkas `LICENSE` ditambahkan berisi **teks resmi verbatim** CC BY 4.0 dari creativecommons.org (sumber, tanggal ambil, dan SHA-256 dicatat di `RELEASE_MANIFEST.md`; berkas **tidak** diubah dari bentuk resminya). Atribusi & penjelasan ruang lingkup: `ATTRIBUTION.md`; `README.md` §Lisensi diselaraskan; `CITATION.cff` diberi `license: CC-BY-4.0`.

**(4) Metadata atribusi.** Nama yang dipakai: **Kelvin Chandra Djayapranata** (terverifikasi dari konfigurasi Git repositori ini; sama dengan `CITATION.cff`). Tahun: **2026** (tahun penyusunan dokumentasi, sesuai tanggal dokumen). Tidak ada pemegang hak cipta, tahun, atau identitas lain yang dikarang; surel tidak dicantumkan (tidak terverifikasi untuk keperluan publikasi).

**(5) K-7.** Keputusan ini menetapkan **butir lisensi dokumentasi** pada `A-3` §4 butir (i). Cakupan `K-7` yang lebih luas — kebijakan atribusi output publikasi riset dan lisensi tool yang masih `UNVERIFIED` (mis. US-align/TM-align) — **tetap terbuka** dan tidak diselesaikan di sini.

**(6) Yang tidak berubah.** **T0 tetap HOLD**; `A-1`/`A-2`/`A-3` tetap berlaku; **tidak ada otorisasi retroaktif**; tidak ada eksperimen, unduhan data penelitian, instalasi tool, atau penghitungan ulang RMSD; **tidak ada operasi Git tulis** (tanpa `git add`, commit, push, atau perubahan visibilitas). Entri ini tidak menyentuh endpoint/preregistrasi mana pun (protokol §G.5; policy §9).

**(7) Dampak dokumen.** **Tidak ada** dokumen beku yang diubah (kelima hash dokumen Phase 2 diverifikasi sebelum/sesudah — dilaporkan di sesi). Berkas baru: `LICENSE`. Berkas yang diperbarui: `README.md`, `ATTRIBUTION.md`, `CHANGELOG.md`, `CITATION.cff`, `docs/PUBLICATION_STATUS.md` (dan entri ini). Paket rilis lokal `D:\project\alphafold-release-v0.1.0` dibangun ulang: `image/hero-banner.png` **dikeluarkan** dari paket karena sumber/lisensinya belum terdokumentasi; berkas sumbernya di repositori **tidak** dihapus dan **tidak** dipindah.

---

## 3. Konsistensi antar-dokumen — konflik & status

*Diperiksa pada fase 2B (baca ulang penuh kelima dokumen + review gate).*

| # | Konflik/inkonsistensi | Lokasi | Status penyelesaian |
|---|---|---|---|
| 1 | A3.1 ("Multi-chain/complex" dikecualikan) vs B.1.6 (mencatat oligomeric state → mengandaikan entri multi-chain bisa masuk) | Protokol §A.3 vs §B.1 | Klarifikasi di C-4(3): larangan = endpoint kompleks, bukan keberadaan multi-chain + aturan pemilihan chain. **Menunggu persetujuan.** |
| 2 | Cutoff "TBD 2018-04-30" vs bukti terverifikasi | Protokol §0.5, §B.1.5; policy §5.3; decision U1 | C-6 (WAJIB-T0) + ratifikasi K-2. **Menunggu.** |
| 3 | A3.6 (>70% IDR dikecualikan) berpotensi mengosongkan bin mean-pLDDT rendah (A.4 menargetkan bin terisi) | Protokol §A.3 vs §A.4 | Mekanisme yield T0 + fallback bin level-residu: C-1 A9.4 (D-4) + C-8(2). **Menunggu.** |
| 4 | Power T1 dihitung N=protein; CI memakai klaster → N-efektif bisa jauh lebih kecil | Protokol §F.2 vs §D.2 | C-2(2)(4): laporan jumlah klaster + gate minimum (**D-6b = X 30 — diputuskan pemilik 2026-10-11; cakupan gate: T1**); spesifikasi formula operasional (WORKING DRAFT / UNRESOLVED) & V1–V3 menyusul. **Sebagian menunggu.** |
| 5 | Uji berpasangan RQ1 & aturan G1.1 tanpa aturan bin kosong | Protokol §D.4, §G.1 | C-3(1)(2)(3) (D-7). **Menunggu.** |
| 6 | Falsifikasi H2 ("tanpa heterogenitas") & H4 ("<1%") tidak operasional | Protokol §G.2 | C-3(4)(5) (D-8). **Menunggu.** |
| 7 | Tabel K di protokol §H.2 berbeda bunyi dengan `PHASE2_DECISION.md` §6 (mis. K-7: "Sitasi & lisensi output | CC BY 4.0…" vs "Atribusi & lisensi output publikasi | kebijakan user") | Protokol §H.2 vs Decision §6 | **Diselesaikan di file ini:** kanonik = `PHASE2_DECISION.md` §6 (per instruksi pemilik); §H.2 tetap sebagai rujukan historis — tidak ada perubahan file beku. |
| 8 | Policy §4: "tidak dicek ulang ke server live" vs verifikasi live 2026-10-11 | Policy §4 | C-7. **Menunggu (opsional).** |
| 9 | Sitasi [L39]/[L40] (menjadi rujukan historis) vs sitasi resmi baru Fleming 2025 | Policy §4; LITREVIEW | C-7(2) + C-10(1). **Menunggu (opsional).** |
| 10 | C.4 GDT_TS "pelengkap" tanpa keputusan pin | Protokol §C | D-10. **Menunggu.** |
| 11 | 7 protein lokal (spesimen) tanpa label; risiko masuk set analisis | Protokol §A.1/§A.4; baseline | C-8(1). **Menunggu (opsional).** |
| 12 | §B.1.6 "state biologisnya paling netral" tanpa definisi eksekusi | Protokol §B.1.6 | C-4(5) (D-9). **Menunggu.** |
| 13 | B13 sumbu (ii) "kemiripan sekuens ke PDB pra-cutoff" tanpa definisi operasional | Policy §6 B13 | C-11 (dari NB-4). **Menunggu (opsional).** |
| 14 | U7 (lisensi & sitasi final) | Decision §4 | Sebagian terjawab (Fleming 2025 terverifikasi); format akhir = K-7. **Terbuka sebagian.** |

**Tidak ditemukan:** kontradiksi angka antar dokumen (40/40 sitasi, tier 30/100/300, hash 5/5, register B1–B14 konsisten) — sejalan lampiran `PHASE2_DECISION.md`.

---

## 4. Dependensi & urutan penyelesaian sebelum T0

> Urutan di bawah adalah **usulan urutan**; tidak ada langkah yang boleh dimulai tanpa persetujuan eksplisit pemilik (stop condition).

1. **Pemilik — keputusan & persetujuan (gerbang masuk):**
   - a. Setujui kelima dokumen Phase 2 (atau dengan amendemen).
   - b. Ratifikasi K-2 (cutoff 2018-04-30).
   - c. Setujui K-3 (daftar `TOOLS_LOCK.md` → boleh lanjut instalasi & uji).
   - d. Setujui K-5 + D-12 (tier & angka anggaran unduhan).
   - e. Setujui K-6 (venv) dan K-1 (git) — K-1 sebelum artefak T0 pertama.
   - f. Setujui amendemen `[WAJIB-T0]` C-1..C-6 beserta nilai yang tersisa (D-1, D-7..D-9 — **D-2..D-6 + D-6b + ambang identitas operasional telah diputuskan pemilik 2026-10-11**; dan putuskan D-10; D-11 setelah U-4).
2. **Implementasi awal (setelah 1c/1e):** buat venv + `requirements.txt` (versi+hash) → instalasi toolchain per `TOOLS_LOCK.md` → verifikasi integritas (§5 TOOLS_LOCK) → selesaikan register `UNVERIFIED` U-1..U-8 (termasuk keputusan D-11) → jalankan TD-1..TD-8 → isi bagian LOCKED → hash file.
3. **Sebelum unduhan pertama (setelah 1b/1d/1f + langkah 2 selesai):** enumerasi frame (metadata saja) → terapkan Lampiran A.9 → sampel T0 (30) → freeze manifest → **baru** unduhan terkontrol (anggaran D-12).
4. **T0:** mapping → metrik → laporan T0 (termasuk yield U3 + cakupan U6) → **gate review pemilik** sebelum T1 (K-5).
5. **Menyusul (tidak memblokir T0):** K-4 (sebelum analisis domain/RQ3; kandidat fallback sebelum laporan T0), C-7..C-11, D-10 (sebelum laporan T0), K-7 (sebelum publikasi).

**Ketergantungan kritis yang tidak boleh dilompati:** amendemen `[WAJIB-T0]` → `TOOLS_LOCK` LOCKED → unduhan → mapping → metrik → T0. Melompati salah satu = pelanggaran pra-registrasi (protokol §G.5).

---

## 5. Checklist ringkas untuk pemilik

**Keputusan utama (7):**

| # | Pertanyaan singkat | Rekomendasi audit (bukan keputusan) | Sebelum T0? |
|---|---|---|---|
| K-1 | `git init` di root audit? | init tanpa remote | Ya (sebelum artefak T0 pertama) |
| K-2 | Ratifikasi cutoff 2018-04-30 (bukti review gate §4.3)? | ratifikasi ya | Ya |
| K-3 | Setujui toolchain `TOOLS_LOCK.md`? | ya (dengan `UNVERIFIED` diselesaikan saat instalasi) | Ya |
| K-4 | Domain/disorder: prioritas + fallback? | tunda sampai U6, kandidat fallback sebelum laporan T0 | Sebelum analisis domain |
| K-5 | T0 dulu + angka anggaran unduhan? | T0 dulu; usulan ≤150 MB (D-12) | Ya |
| K-6 | venv di root audit? | ya | Ya |
| K-7 | Atribusi publikasi? | draf saat laporan T1 (pin sitasi sudah terbarui) | Tidak (sebelum publikasi) |

**Keputusan turunan (12):** D-1..D-9, D-11, D-12 = blokir T0 (nilai usulan di §2; **D-2, D-3, D-4 telah diputuskan 2026-10-11**; **D-5 = 0,80 + persetujuan prinsip bersyarat atas elemen C-2(1) tercatat** — adopsi formal C-2 tetap menunggu §7 butir 2; **D-6/D-6b dan ambang identitas operasional diputuskan pemilik 2026-10-11** (pra-registrasi; ambang identitas berlaku setelah adopsi formal entri C-2; formula operasional sisi graf: WORKING DRAFT / UNRESOLVED — belum dikunci; V1–V3 belum dijalankan)); D-10 sebelum laporan T0.

**Yang harus pemilik setujui agar T0 dapat dimulai (ringkas):** butir 1a–1f pada §4. Tanpa itu: **status tetap HOLD; nol eksekusi.**

---

## 6. Pernyataan batas

- File ini tidak memilih atas nama pemilik; seluruh "usulan" adalah rekomendasi berbasis bukti.
- Tidak ada implementasi T0, instalasi, unduhan, `git init`, atau perubahan dokumen beku yang terjadi bersamaan dengan penyusunan file ini.
- STOP: menunggu keputusan eksplisit pemilik atas K-1..K-7 (dan D-1..D-12) sebelum langkah apa pun di §4 dijalankan.
