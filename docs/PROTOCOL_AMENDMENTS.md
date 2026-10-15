# PROTOCOL AMENDMENTS — AlphaFold Reliability Audit

## Usulan Amendemen atas Koreksi Review Gate (C-1..C-10) — Fase Persiapan 2B

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal** | 2026-10-11 |
| **Status** | **USULAN — BELUM BERLAKU** — dengan nilai-nilai turunan yang telah diputuskan pemilik: **D-2 = opsi (A)** (ditandai di A9.2), **D-3 = Opsi 1, angka target** (ditandai di A9.3), **D-4 = opsi (a)** (ditandai di A9.4) — semuanya 2026-10-11 — serta **D-5 = 0,80** (praregistrasi; bukan validasi empiris) dan **persetujuan prinsip bersyarat atas elemen C-2(1)** (2026-10-11; ditandai di entri C-2; **bukan** adopsi formal entri C-2), **ambang identitas operasional = ≥30% inklusif — tepat 30,000% termasuk** (diputuskan pemilik 2026-10-11; konvensi operasional praregistrasi — bukan klaim kebenaran biologis; rumusan berlaku setelah adopsi formal entri C-2 melalui mekanisme amendment), serta **D-6 = B 10.000 / seed tetap 20261011** dan **D-6b = X 30** (diputuskan pemilik 2026-10-11; cakupan gate D-6b: T1); formula operasional sisi graf: **WORKING DRAFT / UNRESOLVED** — belum dikunci (spesifikasi eksplisit & V1 menyusul). Menunggu persetujuan pemilik atas entri lainnya. Setelah disetujui, entri yang disetujui ditandai `DISETUJUI (tanggal)` dan file ini menjadi **Amendment Log resmi** (protokol §G.5; policy §9). |
| **Dasar** | `docs/PHASE2_REVIEW_GATE.md` §7 (blocker KB/NB) & §8 (koreksi C-1..C-10); pembacaan ulang `RESEARCH_PROTOCOL_V1.md`, `DATA_AND_BIAS_POLICY.md`, `PHASE2_DECISION.md`. |
| **Aturan** | Lima dokumen Phase 2 yang dibekukan **TIDAK diubah** oleh file ini. Amendemen diterapkan dengan cara: entri di file ini = teks pengganti yang mengikat setelah disetujui. |
| **Pra-registrasi** | Semua entri di bawah disusun **sebelum ada hasil apa pun** (belum ada data hasil di seluruh proyek). Memenuhi syarat pra-registrasi §G.5. |
| **Batas** | Tidak ada unduhan, instalasi, eksekusi, atau perubahan pada `ai-biotech` dalam penyusunan file ini. Tidak ada nilai/ambang yang dikarang: setiap ambang baru diberi penanda `[D-x — usulan, keputusan pemilik]` (lihat `OWNER_DECISIONS.md`). |

**Cara baca status:**

- `[WAJIB-T0]` — harus disetujui & terpasang sebelum T0 dimulai.
- `[OPSIONAL]` — disarankan; boleh menyusul sebelum titik pakainya.
- `[KEPUTUSAN-PEMILIK]` — memuat pilihan nilai/kebijakan ilmiah; hanya pemilik yang memutuskan (tidak ada default yang diberlakukan tanpa persetujuan).

---

## Peta cakupan: koreksi (C) ↔ blocker review gate (KB/NB)

| C-item | Menangani blocker | Status usul |
|---|---|---|
| C-1 (sampling) | KB-1, NB-3 (parsial) | WAJIB-T0 |
| C-2 (klaster/N-efektif/bootstrap) | KB-2 | WAJIB-T0 |
| C-3 (bin kosong/uji berpasangan/falsifikasi) | KB-3 | WAJIB-T0 |
| C-4 (aturan tepi mapping) | KB-4 | WAJIB-T0 |
| C-5 (TOOLS_LOCK) | KB-5, NB-8 (parsial) | WAJIB-T0 |
| C-6 (cutoff terverifikasi) | KB-6 (ratifikasi), NB-2 | WAJIB-T0 |
| C-7 (lisensi & sitasi) | NB-7 | OPSIONAL |
| C-8 (spesimen & yield) | NB-1, NB-3 | OPSIONAL |
| C-9 (pin metrik) | NB-5 | OPSIONAL |
| C-10 (catatan literatur) | NB-6, NB-7 | OPSIONAL |
| C-11 (operasionalisasi B13 sumbu-ii — dari NB-4) | NB-4 | OPSIONAL |

---

## C-1 `[WAJIB-T0]` — Prosedur seleksi sampel yang dapat direproduksi

**Target dokumen:** `RESEARCH_PROTOCOL_V1.md` §A.1, §A.4 (+ lampiran baru A.9). **Sumber:** review gate §5.8.1, §8 C-1, §7 KB-1.

**Masalah.** §A.4 menetapkan *target stratifikasi* tetapi tidak menetapkan prosedur enumerasi frame, algoritma pemilihan, kuota, dan aturan under-fill. Tanpa prosedur, sampel T0/T1 tidak dapat direproduksi dan rawat pemilihan oportunistik (KB-1).

**Teks pengganti yang diusulkan (lampiran baru protokol):**

> **LAMPIRAN A.9 (BARU) — PROSEDUR SELEKSI SAMPEL YANG DAPAT DIREPRODUKSI**
>
> **A9.1 Frame kandidat.** Frame = himpunan protein (unit: UniProt canonical accession) yang memenuhi: (a) memiliki entri AlphaFold DB single-chain (F1, `isComplex=false`); (b) memiliki ≥1 entri PDB eksperimen (status current) dengan pemetaan resmi UniProt↔PDB (SIFTS/PDBe); (c) lolos §A.2–A.3 pada kandidat PDB-nya.
> Arah enumerasi **[D-1 — usulan: PDB-initiated]**: daftar entri PDB (filter metode X-ray/cryo-EM/NMR) → pemetaan SIFTS → UniProt canonical → pengecekan entri AFDB. Query enumerasi disimpan verbatim (JSON + tanggal) sebagai artefak; tanpa unduhan struktur pada tahap enumerasi (metadata saja).
>
> **A9.2 Deduplikasi & klaster.** (1) Per UniProt canonical: satu "kandidat protein" (pemilihan struktur primer per §B.1 langkah 3; kandidat lain disimpan untuk S1–S4/S8). (2) Klaster homolog dihitung (§C-2) pada sekuens seluruh protein frame. (3) Aturan independensi penarikan **[D-2 — DIPUTUSKAN PEMILIK 2026-10-11: opsi (A)]**: (A) maksimum 1 protein per klaster masuk set analisis (N-efektif = jumlah protein = jumlah klaster); opsi (B) — boleh >1 protein per klaster, N-efektif = jumlah klaster dilaporkan & seluruh CI cluster-robust — **tidak dipilih** (dicatat sebagai riwayat). Keputusan D-2 **tidak** menetapkan definisi klaster, ambang identitas (tetap >30% dari dokumen asli §0.5/§A.4), maupun parameter teknis klaster (coverage [D-5], tool & versi — K-3) — hal-hal tersebut tetap mengikuti entri C-2 yang belum diputuskan.
>
> **A9.3 Penarikan sampel per tier.** (1) Protein frame diurutkan deterministik: `sha256(UniProt accession)` menaik (leksikografis hex) — mencegah seleksi manual; urutan disimpan. (2) Kuota strata **[D-3 — DIPUTUSKAN PEMILIK 2026-10-11: Opsi 1 (angka target); sel pre/post aktif hanya bila K-2 diratifikasi]** pada strata: bin mean-pLDDT (<50 / 50–70 / 70–90 / ≥90), metode (X-ray/cryo-EM/NMR), tanggal rilis (pre/post-cutoff), panjang (§S6), multidomain (ya/tidak). (3) Dalam tiap sel: ambil sesuai urutan A9.3.1 sampai kuota terisi. (4) Kuota = *target* (semangat §A.4); tidak boleh dipenuhi dengan melonggarkan §A.2–A.3.
>
> **A9.4 Under-filled stratum** **[D-4 — DIPUTUSKAN PEMILIK 2026-10-11: opsi (a)]**: bila sel tidak terisi: (a) laporkan jumlah riil di laporan T0/T1 sebelum implikasi apa pun; (b) analisis set tetap dijalankan; (c) klaim per-stratum dibatasi N riil (§G.4); (d) **fallback pre-registered**: pelaporan pelengkap pada level residu per bin (setiap protein menyumbang residu di berbagai bin) — pelengkap, bukan pengganti analisis protein; (e) pengisian ulang stratum hanya via amendemen **sebelum** hasil dilihat. Keputusan pemilik menetapkan batas: (d) dijalankan **hanya** sejauh mekanisme, kondisi pemicu, dan prosedur yang sudah tertulis di entri ini sebelum hasil diketahui — tidak boleh diperluas atau diubah berdasarkan hasil aktual.
>
> **A9.5 Artefak:** file kandidat lengkap + alasan eksklusi per protein; daftar klaster; daftar final per tier (di-hash, dibekukan sebelum metrik, §5 policy). Prosedur dijalankan identik untuk T0 dan T1 (T1 = prosedur sama dengan kuota lebih besar; protein T0 boleh muncul kembali di T1).

**Usulan angka kuota `[D-3 — usulan, keputusan pemilik]`:** T0 (30 protein): ≥5 per bin mean-pLDDT (bila tersedia), ≥5 multidomain, ≥5 pre-cutoff & ≥5 post-cutoff, ≥3 NMR (bila tersedia), sisanya bebas. T1 (100 protein): ≥15 per bin mean-pLDDT (bila tersedia), ≥30 multidomain (agar N≥20 syarat RQ3 §F.3 realistis), ≥15 pre / ≥15 post-cutoff, sisanya bebas.

**Keputusan pemilik 2026-10-11 (06:15 UTC) — D-3, Opsi 1 (bunyi mengikat; riwayat usulan di atas dipertahankan):** target pra-registrasi T0 = 30 protein dan T1 = 100 protein — **target, bukan jaminan ketercapaian**. T0: minimal 5 protein per bin mean-pLDDT (<50, 50–70, 70–90, ≥90); multidomain ≥5; NMR target 3 bila tersedia. T1: minimal 15 protein per bin mean-pLDDT; multidomain ≥30; NMR dilaporkan aktual tanpa target numerik. Pre/post-cutoff: minimal 5 (T0) dan 15 (T1) masing-masing — **hanya jika K-2 diratifikasi**; bila tidak, sel pre/post tidak diaktifkan dan aturan eksklusi S3 yang berlaku dijalankan (`PHASE2_DECISION.md` §7: S3 di-drop). **Kelayakan kuota tidak diklaim** — sampling frame & diversitas klaster belum diverifikasi; yield diukur di T0 (U3; C-8(2)).

**Jika tidak diterapkan:** sampel tidak replikasi; hasil T0/T1 tidak dapat dipertahankan secara metodologis (kondisi HOLD review gate §9).

---

## C-2 `[WAJIB-T0]` — Definisi klaster, N-efektif, parameter bootstrap

**Target dokumen:** `RESEARCH_PROTOCOL_V1.md` §0.5 (baris "Klaster homolog"), §A.4 (butir klaster), §D.2, §F.1–F.2, §G.4. **Sumber:** review gate §5.6.1–5.6.4, §8 C-2, §7 KB-2.

**Masalah.** "Identitas berpasangan >30%" (§0.5) belum dapat dieksekusi (algoritma, coverage, tool tidak ditetapkan); power T1 dihitung pada N=protein padahal N-efektif = klaster; parameter bootstrap (B, seed) dan konsekuensi jumlah klaster kecil tidak ditetapkan.

**Teks pengganti yang diusulkan:**

> **(1) Definisi klaster (menggantikan §0.5 baris "Klaster homolog"):** Klaster homolog = komponen terhubung dari graf protein dengan sisi bila: identitas sekuens berpasangan **≥30%** DAN **coverage alignment timbal-balik ≥ [D-5 — DIPUTUSKAN PEMILIK 2026-10-11: 0,80]** dari sekuens yang lebih pendek. **Implementasi kandidat: `MMseqs2 easy-cluster`** (versi & parameter dipin di `TOOLS_LOCK.md`) — kesetaraan keluaran tool dengan definisi komponen terhubung **tidak diasumsikan**; status kandidat berlaku sampai **V1–V3 lulus** (lihat blok keputusan di bawah); daftar klaster anggota disimpan sebagai artefak hasil. Threshold 30% dipertahankan dari dokumen asli (§0.5, §A.4; dasar B2/B13).
>
> **(2) N-efektif (menggantikan penggunaan N di §F.1–F.2 untuk CI/uji):** semua CI, uji lintas-protein, dan pernyataan daya memakai **N-efektif = jumlah klaster yang terwakili di set analisis** (bukan jumlah protein). Laporan T0/T1 wajib memuat: jumlah protein, jumlah klaster, distribusi ukuran klaster. Kalimat daya T1 (≈85→100 pada §F.2) tetap boleh dikutip sebagai perhitungan pada asumsi klaster terpisah habis, **wajib** disertai: "daya riil dihitung ulang pada laporan T1 dari jumlah klaster riil".
>
> **(3) Bootstrap (melengkapi §D.2):** cluster-robust bootstrap = resample **klaster** dengan penggantian; setiap klaster membawa seluruh protein anggotanya; **B=[D-6 — DIPUTUSKAN PEMILIK 2026-10-11: 10.000]**, seed=[D-6 — DIPUTUSKAN PEMILIK 2026-10-11: 20261011] dicatat di `TOOLS_LOCK.md`; parameter & hasil CI disimpan sebagai artefak.
>
> **(4) Gate klaster (melengkapi §G.4):** bila jumlah klaster riil di T1 < **[D-6b — DIPUTUSKAN PEMILIK 2026-10-11: 30]**, seluruh CI dilaporkan sebagai "aproksimatif (jumlah klaster kecil)" dan kalimat klaim §G.1 tidak boleh dipakai tanpa menyebut angka ini.
>
> **(5) Penegasan anti-pseudoreplikasi:** tidak ada nilai-p, CI, atau klaim signifikansi pada level residu dalam bentuk apa pun; ringkasan residu per bin (median/IQR) bersifat deskriptif murni (sejalan §D.1, B9).

**Jika tidak diterapkan:** CI dapat terlalu sempit / klaster tidak konsisten antar implementasi; hasil lintas-protein tidak dapat dipertahankan.

**Keputusan pemilik 2026-10-11 (07:54 UTC) — D-5 & elemen C-2(1) (bunyi mengikat; riwayat usulan di atas dipertahankan; dicatat sebelum ada hasil apa pun — §G.5):**

**(a) D-5 — coverage alignment timbal-balik = 0,80 (praregistrasi).** Ambang coverage inklusif (≥ D-5). Dasar: keputusan praregistrasi murni; tidak ada angka pembanding di dokumen beku — **bukan hasil validasi empiris dan tidak diklaim paling baik**. Alasan (analitis, bukan empiris): 1,00 berisiko terlalu ketat (klaster pecah; N-efektif naik semu), 0,70 berisiko terlalu permisif (klaster besar; sel tipis); 0,80 = titik tengah konservatif yang menjaga kedua risiko tetap terbatas. Denominator = coverage dari sekuens yang lebih pendek (sesuai teks usulan (1) di atas). Pemetaan parameter coverage ke MMseqs2 diverifikasi di V1.

**(b) Elemen C-2(1) — disetujui secara prinsip dengan syarat ketat; BUKAN adopsi formal entri C-2 (status entri tetap `USULAN — BELUM BERLAKU`).** Definisi ilmiah yang **ditargetkan**: klaster = komponen terhubung dari graf protein; sisi = (identitas berpasangan memenuhi ambang identitas yang diputuskan pemilik) DAN (coverage alignment timbal-balik ≥ D-5). **Selama keputusan (c) belum diberikan, definisi sisi graf ini belum final dan tidak mengikat.** Implementasi = **kandidat** `MMseqs2 easy-cluster`; kesetaraan keluaran tool dengan definisi komponen terhubung **tidak diklaim** — status **BELUM DIVERIFIKASI** (V1–V3 belum dijalankan). Pelaksanaan clustering hanya bila: (c) ditetapkan, adopsi formal entri C-2 tuntas, dan V1–V3 lulus: V1 = semantik persis dokumentasi MMseqs2 (batas `--min-seq-id` pada tepat 30%; mode coverage "sekuens lebih pendek"; perilaku prefilter); V2 = uji konsistensi terkendali vs definisi komponen terhubung (aproksimasi dilaporkan); V3 = TD-5 determinisme 1-vs-N thread. **Jika verifikasi gagal: berhenti dan ajukan pilihan kepada pemilik — tanpa penggantian algoritme otomatis.** Slot D-6/D-6b tetap `OWNER DECISION REQUIRED`.

**(c) Ambang identitas operasional (≥30% inklusif vs >30% strict) — MASIH TERBUKA (keputusan tersendiri).** Konflik dokumen beku dicatat tanpa mengubahnya: §0.5 ("berbagi klaster > 30%"), §A.4 ("(≤30% → berbagi klaster)"), §D.2 ("≥30% identity"). **">30%" dan "≥30%" tidak boleh disamakan pada nilai batas tepat 30%.** **Definisi sisi graf C-2(1) belum final** sebelum keputusan ini. Teks usulan (1) di atas memakai "≥30%" sebagai **rumusan kerja non-final**; frasa pada teks usulan (1) dan ekspektasi batas V1 disesuaikan sebelum adopsi. Pemilihan ambang harus konsisten dengan perilaku batas tool yang kelak diverifikasi (V1); bila tool tidak dapat merealisasikan formulasi terpilih secara eksak, proses berhenti dan pilihan diajukan ke pemilik — definisi tidak diubah agar cocok dengan tool.

**(d) Izin eksekusi terpisah.** Keputusan ini tidak memberi izin eksekusi. Kondisi (b) juga mengharuskan: K-3 & `TOOLS_LOCK` tuntas per urutan terdokumentasi (instalasi → U-1..U-8 → TD-1..TD-8, kriteria §6 terikat K-3 → `TOOLS_LOCK = LOCKED` + hash), termasuk TD-5 (+TD-1); **TD-6 tetap UNRESOLVED** (keberlakuan PAE).

**(e) T0 tetap HOLD** — `PHASE2_REVIEW_GATE.md` §9 (CONDITIONAL GO butir 1–6).

**Keputusan pemilik 2026-10-11 (09:12 UTC) — ambang identitas operasional, target definisi C-2, formula operasional (WORKING DRAFT / UNRESOLVED), D-6, D-6b (bunyi mengikat; riwayat usulan di atas dipertahankan; dicatat sebelum ada hasil apa pun — §G.5):**

**(f) Ambang identitas operasional = ≥30% (inklusif) — tepat 30,000% termasuk klaster.** Konvensi operasional praregistrasi — **bukan klaim kebenaran biologis**. Dokumen beku memuat frasa yang tidak konsisten satu sama lain pada batas ini (§0.5 "berpasangan > 30%"; §A.4 "(≤30% → berbagi klaster)"; §D.2 "≥30% identity") — **ketidakkonsistenan tekstual ini dicatat; belum dikoreksi; dokumen beku tidak diubah**. Rumusan operasional tunggal (≥30% inklusif) **berlaku setelah adopsi formal entri C-2 melalui mekanisme amendment**; penyesuaian frasa diselesaikan pada adopsi itu. Realisasi batas eksak (perilaku tool pada tepat 30,000%) diverifikasi di V1; bila tool tidak dapat merealisasikannya secara eksak → proses berhenti dan pilihan diajukan ke pemilik (per (c)).

**(g) Target definisi klaster — definisi baru untuk adopsi C-2 (bukan penafsiran ulang teks beku):** klaster = komponen terhubung pada graf **tak berarah**; sisi hanya ada bila kriteria identitas **dan** coverage timbal-balik terpenuhi **pada kedua arah**; **singleton dihitung sebagai satu komponen** (satu klaster); **himpunan simpul = set analisis yang sedang dievaluasi** (didefinisikan eksplisit per set analisis, bukan diwarisi dari luar). Catatan: §0.5 memuat frasa "identitas sekuens **berpasangan** > 30%"; dokumen beku tidak menetapkan definisi operasional graf — **target definisi komponen terhubung ini adalah definisi baru yang berlaku setelah adopsi formal entri C-2 melalui mekanisme amendment**, dan **bukan** klaim bahwa teks beku memang bermakna demikian.

**(h) Formula operasional — WORKING DRAFT / UNRESOLVED (tidak dikunci).** Seluruh rumusan yang belum disetujui — perhitungan identity, panjang alignment, perlakuan gap, denominator coverage, evaluasi kedua arah, dan semantik tool — **berlabel WORKING DRAFT / UNRESOLVED**; termasuk "denominator = coverage dari sekuens yang lebih pendek" (draf (1) dan blok (a)) dan seluruh frasa operasional draf (1). **Tidak ada penguncian denominator, formula identity, atau semantik tool** sebelum: (i) spesifikasi eksplisit & non-ambigu yang cukup untuk reference implementation selesai, dan (ii) V1 lulus. Spesifikasi formula wajib selesai **sebelum pencatatan formal entri C-2**; ketergantungan semantik MMseqs2 **menunggu V1**.

**(i) D-6 — disetujui: B = 10.000; seed tetap = 20261011 (pra-registrasi).** Definisi bootstrap & unit resampling dinyatakan jelas: unit = klaster homolog; bootstrap = resample **klaster** dengan penggantian; setiap klaster membawa seluruh protein anggotanya (teks C-2(3)) — **tidak bertentangan dengan dokumen beku**: §D.2 ("resampling pada level klaster homolog"); §D.7 (determinisme & seed tetap); dokumen beku tidak menetapkan B/seed (review gate §5.6.4). Parameter & hasil CI disimpan sebagai artefak; dicatat di `TOOLS_LOCK.md` saat LOCK (§10 file itu).

**(j) D-6b — disetujui: X = 30 (pra-registrasi).** Interpretasi & cakupan eksplisit: pemicu = jumlah klaster riil **di T1** < 30 (per teks C-2(4) & review gate §5.6.2). Konsekuensi gate T1: CI untuk klaim primer pada T1 dinyatakan "aproksimatif (jumlah klaster kecil)"/butuh T2, atau tambah protein; draf C-2(4) (belum diadopsi) juga mensyaratkan kalimat klaim §G.1 menyebut angka jumlah klaster. **Cakupan gate = T1** (klaim primer — protokol §G.4; T0 hanya pilot) — **tidak diperluas** ke CI/endpoint lain **tanpa keputusan pemilik tersendiri**; frasa "seluruh CI" pada draf C-2(4) tidak dibaca sebagai perluasan otomatis. Jumlah klaster dilaporkan di T0 & T1 (C-2(2)). **X=30 adalah konvensi praregistrasi — bukan hasil validasi empiris; tidak diklaim tervalidasi** (sejalan usulan review gate §5.6.2).

**(k) Batas:** keputusan ini tidak memberi izin eksekusi apa pun; V1–V3 belum dijalankan dan belum diotorisasi; adopsi formal entri C-2 tetap menunggu `PHASE2D_DECISION_PACKAGE.md` §7 butir 2; **T0 tetap HOLD.**

---

## C-3 `[WAJIB-T0]` — Bin kosong, uji berpasangan, kalimat falsifikasi

**Target dokumen:** `RESEARCH_PROTOCOL_V1.md` §D.3, §D.4 (baris RQ1), §G.1 (G1.1), §G.2 (H2, H4). **Sumber:** review gate §5.6.3, §5.6.5, §8 C-3, §7 KB-3.

**Masalah.** Belum dispesifikasi: protein mana yang masuk uji berpasangan bila salah satu bin kosong (§D.4 RQ1), basis protein untuk aturan monotonisitas G1.1, dan kalimat falsifikasi H2 ("tanpa heterogenitas") / H4 ("<1%") tidak operasional.

**Teks pengganti yang diusulkan:**

> **(1) Definisi "protein uji-RQ1" (baru pada §D.4 baris RQ1):** protein yang masuk uji Wilcoxon = protein dengan **≥ [D-7 — usulan 10] residu matched di bin [90,100] DAN ≥ [D-7] residu matched di bin [70,90)**. Protein yang tidak memenuhi tetap dilaporkan distribusinya (deskriptif); jumlah protein yang masuk & gugur dilaporkan; tidak ada imputasi/penggabungan bin.
>
> **(2) Aturan monotonisitas G1.1 (memperjelas):** "ordering median menurun monoton di 4 bin" diuji hanya pada protein dengan **≥1 residu matched di keempat bin**; proporsi yang memenuhi + jumlah protein yang dievaluasi dilaporkan. Status: **deskriptif** (tanpa nilai-p; di luar keluarga Holm).
>
> **(3) Bin kosong (menegaskan §D.3):** bin tanpa residu dilaporkan kosong apa adanya di semua tabel; sel bin-protein kosong tidak diisi; uji yang tidak dapat dijalankan dilaporkan sebagai "tidak dijalankan pada N ini".
>
> **(4) Falsifikasi H2 (§G.2) — kalimat operasional [D-8a/b — usulan, keputusan pemilik]:** "H2 batal bila Spearman ρ(pLDDT mean, TM-score) **> 0,8** DAN **batas bawah CI95% ≥ 0,7** — yaitu pLDDT memprediksi global nyaris sekuat prediksinya terhadap lokal, bertentangan dengan [L7]."
>
> **(5) Falsifikasi H4 (§G.2) — [D-8c — usulan, keputusan pemilik]:** klausa H4 pada §G.2 **diganti** menjadi: "H4 tidak memiliki ambang numerik. Komitmen H4 dijalankan sebagai analisis S5: error di bin pLDDT rendah dipisah menurut anotasi disorder eksperimen dan dilaporkan keduanya (angka apa adanya). Frekuensi RQ4 (C.9) dilaporkan sebagai proporsi + CI; frasa 'sangat jarang' hanya boleh dipakai bila batas atas CI < **[usulan 1%]**."

**Jika tidak diterapkan:** pilihan protein-uji dan perlakuan bin kosong berpotensi ditentukan setelah hasil diketahui (pelanggaran pra-registrasi).

---

## C-4 `[WAJIB-T0]` — Aturan tepi mapping prediksi↔PDB

**Target dokumen:** `RESEARCH_PROTOCOL_V1.md` §A.3 (A3.1), §B.1 (langkah 2, 4, 6), §B.2. **Sumber:** review gate §5.4, §8 C-4, §7 KB-4.

**Masalah.** Belum ada aturan untuk: residu termodifikasi (MSE dkk.), isoform, pemilihan chain pada entri multi-chain, model NMR, dan definisi operasional apo/holo; ada kontradiksi halus A3.1 vs B.1.6.

**Teks pengganti yang diusulkan:**

> **(1) Residu termodifikasi (§B.1 langkah 4):** pemetaan residu model↔PDB memakai pemetaan residu-level resmi **SIFTS (PDBe)** sebagai sumber primer. Residu termodifikasi dinormalisasi ke residu induk memakai pemetaan parent wwPDB (contoh: MSE→M, SEP→S, TPO→T, PTR→Y, CSO/CME→C, KCX/MLY→K, PCA→E, FME→M). Daftar parent yang dipakai disimpan sebagai artefak; residu tanpa parent yang disepakati dicatat & dikecualikan dari perhitungan identitas (tidak silent). **Konsekuensi A2.5:** identitas 100% diperiksa pada sekuens **ternormalisasi** — residu termodifikasi tidak otomatis menggugurkan kandidat.
>
> **(2) Isoform (§B.1 langkah 2):** hanya pemetaan ke UniProt **canonical** yang dipakai. Kandidat PDB yang dipetakan SIFTS ke isoform non-kanonik → gugur A2.5 & dicatat. Bila satu entri PDB dipetakan ke beberapa UniProt, hanya pemetaan kanonik yang dievaluasi.
>
> **(3) Klarifikasi A3.1 vs B.1.6 (menyelesaikan konflik dokumen):** A3.1 melarang **endpoint** kompleks/interface (DockQ, pasangan chain), **bukan** keberadaan >1 chain di entri PDB. Entri multi-chain boleh dipakai sebagai sumber struktur monomer satu chain. Aturan pemilihan chain (melengkapi §B.2, deterministik): (a) chain yang lolos A2.4–A2.5; bila >1: (b) residu missing paling sedikit; (c) ID chain leksikografis terkecil. Chain terpilih + alasan dicatat di manifest. Catatan oligomeric state (§B.1.6) tetap diisi dari metadata entri.
>
> **(4) Model NMR (§B.1):** untuk entri NMR (strata S1), **model 1** ensemble dipakai sebagai referensi primer; tidak memilih model "terbaik" per protein. Sensitivitas tambahan (opsional, eksploratori): RMSD/lDDT antar-model ensemble dilaporkan deskriptif (dukungan ensemble [L19]).
>
> **(5) Definisi operasional apo/holo (menggantikan frasa "state biologisnya paling netral" pada §B.1 langkah 6) [D-9 — usulan, keputusan pemilik]:** chain referensi berlabel `holo` bila terdapat HETATM (selain air, ion monoatomik buffer, dan krioprotektan/komponen dalam daftar pengecualian yang direkam) dalam jarak **≤ [usulan 5] Å** dari atom chain terpilih; selain itu `apo`. Struktur primer = `apo` bila tersedia & memenuhi syarat; bila hanya `holo` → dipakai dengan label keterbatasan per protein. Daftar HETATM pengecualian disimpan sebagai artefak.
>
> **(6) Missing residues (menegaskan A3.4/§B.2):** residu missing PDB tidak dimodelkan ulang; hanya residu matched yang dihitung (A2.4); missing internal >30 residu → gugur (A3.4); residu missing termini dilaporkan terpisah dari missing internal; flag indel §B.2 dipertahankan.

**Jika tidak diterapkan:** mapping T0 berjalan tanpa aturan tepi → hasil tidak konsisten & tidak replikasi (KB-4).

---

## C-5 `[WAJIB-T0]` — `TOOLS_LOCK.md` (dokumen baru)

**Target:** protokol §C ("aturan umum" penutup tabel metrik) + §H.1(1). **Sumber:** review gate §5.10, §8 C-5, §7 KB-5/NB-8.

**Masalah.** Belum ada wadah penguncian tool/versi/lisensi; tanpa itu metrik pertama tidak boleh dihitung (aturan §C).

**Teks pengganti yang diusulkan:**

> "File `docs/TOOLS_LOCK.md` menjadi wadah tunggal penguncian tool: nama, versi/commit, hash, cara instalasi, flags, lisensi, dan bukti uji determinisme. Aturan §C 'dikunci sebelum skor pertama dihitung' tetap berlaku. **Tidak ada metrik yang boleh dihitung sebelum status `TOOLS_LOCK.md` = LOCKED** (seluruh uji determinisme lulus dan bagian LOCKED terisi). Status saat ini: RENCANA (fase persiapan 2B; belum ada instalasi)."

**Jika tidak diterapkan:** metrik tanpa versi terkunci → melanggar policy §8.5.

---

## C-6 `[WAJIB-T0]` — Pencatatan cutoff data latih terverifikasi (2018-04-30)

**Target:** protokol §0.5 (baris cutoff, TBD→RESOLVED), §B.1 langkah 5; policy §5.3; `PHASE2_DECISION.md` U1. **Sumber:** review gate §4.3 (bukti primer), §8 C-6, §7 KB-6.

**Masalah.** Nilai cutoff masih berstatus "default sementara 2018-04-30, TBD". Review gate telah menyelesaikan verifikasi dengan bukti primer; hasilnya harus dicatat sebagai amendemen (dilarang mengubah dokumen beku secara langsung).

**Teks pengganti yang diusulkan:**

> **(1) Nilai cutoff (menggantikan baris §0.5 & policy §5.3, status TBD → TERVERIFIKASI, menunggu ratifikasi pemilik):** cutoff data latih **struktur** untuk entri AFDB monomer (pipeline monomer v2.0, `toolUsed: "AlphaFold Monomer v2.0 pipeline"`) = **2018-04-30**. Dasar (terverifikasi 2026-10-11): (a) full text [L1] (PMC8371605) verbatim: *"To train, we use structures from the PDB with a maximum release date of 30 April 2018"*; (b) metadata resmi AFDB pada 7/7 entri dataset lokal v6 (`toolUsed`, diakses read-only).
>
> **(2) Nuansa dua-cutoff (dicatat):** catatan teknis resmi AlphaFold v2.3.0 menyatakan cutoff **2021-09-30** berlaku untuk **bobot Multimer** yang di-fine-tune; versi-versi sebelumnya (termasuk monomer) memakai **2018-04-30**. Entri monomer AFDB = jalur monomer → cutoff 2018-04-30 yang berlaku untuk audit ini.
>
> **(3) Manifest unduhan (§B.1 langkah 1 ditambah):** setiap entri wajib mencatat dari API AFDB: `toolUsed`, `modelCreatedDate`, `latestVersion` — agar klasifikasi tanggal tetap sah bila AFDB meregenerasi entri di masa depan (U5).
>
> **(4) Bila pemilik tidak meratifikasi:** kondisi pra-komitmen `PHASE2_DECISION.md` §7 berlaku — strata S3 di-drop dan B2/B13 diperlakukan sebagai keterbatasan tak-termigasi (tertulis di laporan).

**Jika tidak diterapkan:** filter tanggal (§B.1 langkah 5) tetap berstatus TBD — tidak boleh dijalankan per aturannya sendiri.

---

## C-7 `[OPSIONAL]` — Update lisensi & sitasi (policy §4)

**Target:** `DATA_AND_BIAS_POLICY.md` §4 (via amendemen; file beku tidak diubah). **Sumber:** review gate §4.4, §8 C-7, §7 NB-7.

**Masalah.** Policy §4 memuat catatan "tidak dicek ulang ke server live pada fase ini"; review gate menyelesaikan verifikasi live + sitasi resmi AFDB berubah.

**Teks pengganti yang diusulkan:**

> **(1) AlphaFold DB:** CC BY 4.0 — kutipan resmi (halaman `alphafold.ebi.ac.uk/download`, 2026-10-11): *"Data is available for academic and commercial use, under a CC-BY-4.0 licence."* Atribusi: EMBL-EBI, Google DeepMind, UniProt.
> **(2) Pin sitasi dataset:** **Fleming J. et al., J Mol Biol (2025), DOI `10.1016/j.jmb.2025.168967` (PMID 40133787)** + [L1] Jumper 2021; [L39]/[L40] tetap sebagai rujukan historis database. DOI diverifikasi via pipa Europe PMC (review gate §4.4).
> **(3) PDB:** file data "under the CC0 1.0 Universal (CC0 1.0) Public Domain Dedication" (RCSB Usage Policies, dibaca 2026-10-11); atribusi "encouraged" — praktik audit: tetap menyebut RCSB PDB/wwPDB + PDB ID + depositor.
> **(4) Tool:** Foldseek GPL-3.0; OpenStructure LGPL-3.0; MMseqs2 MIT (catatan rilis Release 16); TM-align/US-align — **tidak ada pernyataan lisensi di halaman resmi → UNVERIFIED**, wajib diselesaikan sebelum distribusi/publikasi (lihat `TOOLS_LOCK.md` §7).
> **(5) Kalimat kejujuran lama "tidak dicek ulang ke server live"** digantikan status terverifikasi di atas (tanggal verifikasi: 2026-10-11).

**Jika ditunda:** tidak memblokir T0 (T0 tidak publikasi); wajib sebelum publikasi eksternal.

---

## C-8 `[OPSIONAL]` — Spesimen pembelajaran & mekanisme yield

**Target:** protokol §A.1/§A.4 (via amendemen). **Sumber:** review gate §5.8.3, §8 C-8, §7 NB-1/NB-3.

**Teks pengganti yang diusulkan:**

> **(1) Spesimen pembelajaran:** tujuh protein pada dataset lokal v6 (baseline) berstatus **"spesimen pembelajaran"** (dipakai merancang pipeline selama Phase 1–2). Protein ini **tidak boleh** masuk set analisis T0/T1/T2 tanpa penanda eksplisit di manifest & laporan; boleh dipakai untuk uji QC pipeline (`TOOLS_LOCK` TD-6) tanpa masuk statistik.
> **(2) Mekanisme yield (U3):** laporan T0 wajib memuat tabel yield per sel strata (jumlah kandidat → lolos §A.2/A.3 → terpilih) **sebelum** implikasi apa pun dibahas; penanganan kuota tak terpenuhi mengikuti A9.4 (C-1, D-4).

---

## C-9 `[OPSIONAL]` — Pin operasional metrik (C.4/C.5a/C.5b + QC PAE↔CIF)

**Target:** protokol §C (baris C.4, C.5) + QC baru. **Sumber:** review gate §5.5, §8 C-9, §7 NB-5.

**Teks pengganti yang diusulkan:**

> **(1) C.5b:** "kontribusi lDDT per-residu" := kolom **`local_lddt`** keluaran OpenStructure `ost compare-structures --local-lddt` (versi dipin di `TOOLS_LOCK.md`); definisi lDDT tetap [L19].
> **(2) C.5a (klarifikasi):** untuk protein satu-domain (§D.5: <2 domain), "superposisi domain" = superposisi global full-length; untuk multidomain, per C.8 (superposisi domain pemilik residu).
> **(3) C.4 (GDT_TS) [D-10 — usulan: dikeluarkan dari output V1]** (bukan endpoint; dipertahankan hanya bila pemilik memilih pin LGA dengan versi terkunci).
> **(4) QC indeks PAE↔CIF (baru, wajib sebelum C.6/C.7 dihitung):** PAE JSON AFDB = matriks 0-based urutan sekuens; CIF = penomoran residu. Sebelum metrik PAE protein apa pun dihitung, pemetaan indeks↔residu diuji pada ≥2 protein (jumlah baris = panjang sekuens; spot-check ≥3 residu); ketidakcocokan = blocker implementasi (bukan diasumsikan).

---

## C-10 `[OPSIONAL]` — Catatan literatur (tanpa mengubah file beku)

**Target:** `LITERATURE_REVIEW.md` (catatan via amendemen; file beku tidak diubah). **Sumber:** review gate §4.4/§4.5, §8 C-10, §7 NB-6/NB-7.

**Teks pengganti yang diusulkan (catatan amendemen):**

> (1) **U7 (pin sitasi AFDB):** lihat C-7. (2) **Kutipan [L7]:** kalimat penuh wajib diverifikasi full text sebelum dipakai verbatim di publikasi (abstrak server terpotong pada "local sc…"; sudah ditandai dokumen — status dipertahankan). (3) **Ambang bin pLDDT [L5]:** konvensi 90/70/50 kini didukung full text [L5] verbatim (review gate §4.2) — dapat dikutip dengan yakin. (4) Daftar 40 sumber tetap berlaku — **40/40 terverifikasi ulang 2026-10-11**; tidak ada perubahan substansi lain.

---

## C-11 `[OPSIONAL]` — Operasionalisasi sumbu (ii) B13 (keterpaparan data latih) — tambahan dari daftar NB review gate

**Target:** policy §6 baris B13 (via amendemen). **Sumber:** review gate §5.6.8, §7 NB-4.

**Masalah.** B13 sumbu (ii) "kemiripan sekuens ke PDB pra-cutoff (klaster)" belum operasional — hanya konsep.

**Teks pengganti yang diusulkan:**

> **Sumbu (ii) B13 — definisi operasional:** untuk setiap protein di set analisis, tandai `terpapar-pra-cutoff` bila dalam klasternya (§C-2, D-5) terdapat ≥1 anggota dengan struktur PDB eksperimen rilis **sebelum 2018-04-30**. Sumber: klaster dihitung terhadap subset PDB pra-cutoff (metadata tanggal saja) memakai tool §C-2. Pelaporan: dua sumbu terpisah — (i) tanggal rilis struktur referensi sendiri (S3), (ii) keterpaparan klaster ini — keduanya dilaporkan berdampingan. Pelaksanaan: sebelum pelaporan T1 (bukan sebelum T0); sumber klaster pra-komputasi (bila tersedia) diverifikasi ketersediaannya saat implementasi — **status ketersediaan: UNVERIFIED**.

---

## Ringkasan status seluruh koreksi

| # | Item | Status usul | Blokir T0? | Keputusan turunan terkait |
|---|---|---|---|---|
| C-1 | Prosedur seleksi sampel (Lampiran A.9) | `[WAJIB-T0]` | Ya | D-1, D-2 (**diputuskan 2026-10-11: opsi (A)**), D-3 (**diputuskan 2026-10-11: Opsi 1, angka target**), D-4 (**diputuskan 2026-10-11: opsi (a)**) |
| C-2 | Klaster, N-efektif, bootstrap | `[WAJIB-T0]` | Ya | D-5 (**diputuskan pemilik 2026-10-11: 0,80** — praregistrasi; bukan validasi empiris); elemen C-2(1): **persetujuan prinsip bersyarat** (pelaksanaan hanya bila V1–V3 lulus) — bukan adopsi formal entri C-2; ambang identitas operasional (**diputuskan pemilik 2026-10-11: ≥30% inklusif — tepat 30,000% termasuk; berlaku setelah adopsi formal entri C-2**); D-6 (**diputuskan pemilik 2026-10-11: B=10.000; seed tetap 20261011**), D-6b (**diputuskan pemilik 2026-10-11: X=30 — cakupan gate T1**); formula operasional sisi graf: **WORKING DRAFT / UNRESOLVED** — belum dikunci (spesifikasi eksplisit & V1 menyusul); adopsi formal entri menunggu §7 butir 2 |
| C-3 | Bin kosong, uji berpasangan, falsifikasi | `[WAJIB-T0]` | Ya | D-7, D-8 |
| C-4 | Aturan tepi mapping | `[WAJIB-T0]` | Ya | D-9 |
| C-5 | `TOOLS_LOCK.md` | `[WAJIB-T0]` | Ya | D-11 (platform) |
| C-6 | Cutoff terverifikasi | `[WAJIB-T0]` | Ya | ratifikasi (K-2) |
| C-7 | Lisensi & sitasi | `[OPSIONAL]` | Tidak (sebelum publikasi) | — |
| C-8 | Spesimen & yield | `[OPSIONAL]` | Tidak (sebelum laporan T0) | — |
| C-9 | Pin metrik + QC PAE↔CIF | `[OPSIONAL]` | Tidak (sebelum metrik pertama) | D-10 |
| C-10 | Catatan literatur | `[OPSIONAL]` | Tidak | — |
| C-11 | B13 sumbu-ii (dari NB-4) | `[OPSIONAL]` | Tidak (sebelum laporan T1) | — |

**Tidak ada item `[WAJIB-T0]` yang mengubah RQ, hipotesis, endpoint, tiers, atau aturan §G** — seluruhnya menambal pra-registrasi pada tingkat operasional/konfigurasi (konsisten dengan review gate §9).

---

## Catatan penutup

- File ini **belum mengikat**. Setelah pemilik menyetujui item tertentu (dan nilai D-x terkait), entri ditandai `DISETUJUI (tanggal)` + catatan siapa/yang menyetujui; sisa yang tidak disetujui tetap berstatus usulan dan konsekuensinya berlaku (mis. C-1 tidak disetujui → kondisi HOLD review gate §9).
- Dokumen beku **tidak akan diubah**; seluruh riwayat amendemen hidup di file ini (protokol §G.5; policy §9).
- Tidak ada langkah pada file ini yang boleh dieksekusi sebelum izin eksplisit pemilik.

---

## Sanitasi S1 — Redaksi Lampiran A `PHASE2_REVIEW_GATE.md` (privasi & portabilitas) — 2026-10-11

**Status persetujuan: DISETUJUI & DILAKSANAKAN (2026-10-11).** Otorisasi tertulis pemilik: "OTORISASI TERBATAS — SANITASI S1, P-A SAJA" (patch P-A). Entri ini = teks pengganti yang mengikat (Aturan file ini: "entri di file ini = teks pengganti yang mengikat setelah disetujui"). **Bukan** bagian koreksi review gate C-1..C-11; **tidak mengubah status apa pun** — T0 tetap HOLD; C-2 belum diadopsi; V1–V3 belum dijalankan; tidak ada perubahan metodologi.

**Target:** `docs/PHASE2_REVIEW_GATE.md`, baris 440 (Lampiran A — "Perintah Verifikasi yang Dapat Direproduksi (read-only)", blok baris 434–448).

**Alasan (privasi & portabilitas):** baris 440 semula memuat path lingkungan eksekusi lokal — (a) interpreter venv proyek induk pada drive lokal; (b) skrip sementara di direktori `%TEMP%` akun Windows pengguna (memuat nama akun Windows). Keduanya detail lingkungan, bukan bukti audit; skrip sumber bersifat fana (di luar repositori, tidak diarsipkan) sehingga path absolut tidak menambah nilai reproduksi, dan path lokal mengurangi portabilitas perintah contoh. Patch dilakukan **pra-commit** sehingga path tidak pernah masuk riwayat Git.

**Teks pengganti yang mengikat (baris 440; berlaku sejak patch P-A):**

    "$VENV_INDUK/Scripts/python.exe" "$TMPDIR/source_verify.py" doi 10.1038/s41586-021-03819-2 10.1002/prot.26192 10.1016/j.jmb.2025.168967

**Konvensi placeholder:** `$VENV_INDUK` = direktori venv proyek induk (read-only); `$TMPDIR` = direktori sementara lokal mesin pelaksana. **Kedua variabel lingkungan harus ditetapkan sebelum perintah dijalankan** (mis. via `export`); tanpa penetapan variabel, perintah **tidak dapat dieksekusi** — entri ini tidak mengklaim sebaliknya. DOI dan seluruh argumen tidak berubah.

**Referensi teks lama:** baris 440 pada baseline pra-patch — `PHASE2_REVIEW_GATE.md` sha256 `a4c3e5f161aabb4556eb2b85d238dd3f2b2d7810f227928c148aa0c25ccf0150` (52.695 B). Teks lama sengaja **tidak dikutip** di entri ini agar nama akun tidak dimasukkan kembali ke dokumen lain yang akan di-commit; kecocokan diverifikasi lewat nomor baris + hash baseline + diff saat patch.

**Hasil patch (diukur langsung dari file hasil — bukan nilai prediksi):** sha256 `a0cba1f8578d2ea81bf3b937e5a05138c421b5d9b591d8a653d0e35bc66a74f7` — ukuran 52.653 B (Δ −42 B); identik dengan ekspektasi pra-patch; diff aktual = tepat 1 hunk, 1 baris diganti (hanya baris 440); baris 436/447 tidak disentuh. — ukuran pra-patch 52.695 B.

**Prosedur pemulihan:** salinan pra-patch kedua file disimpan **di luar repositori** pada `%TEMP%\s1_patch_before\` (`PHASE2_REVIEW_GATE.md`, `PROTOCOL_AMENDMENTS.md`); hash salinan terverifikasi = baseline (`a4c3e5f1…`; `b044c2cf…`). Pemulihan: salin salinan pra-patch menimpa file target, lalu verifikasi sha256 kembali ke baseline. **Backup tidak dihapus sebelum commit pertama selesai dan diverifikasi.** Hash `PROTOCOL_AMENDMENTS.md` pasca-entri dilaporkan di sesi (file tidak dapat memuat hash dirinya — konvensi sama dengan `PHASE2_DECISION.md` dan laporan gate).

**Penegasan non-substansi:** perubahan murni redaksi path lingkungan pada satu baris; tidak mengubah metodologi, perintah (selain path), DOI, hasil audit, status keputusan, maupun kesimpulan; tidak ada klaim baru.

**Tidak termasuk (ditunda atas keputusan pemilik 2026-10-11):** baris 436 & 447 (P-B/P-C tidak dijalankan). Kemunculan path lokal lain pada dokumen mana pun (tanpa nama akun) tetap sebagaimana adanya — keputusan tersendiri sebelum publikasi.

---

## Disposisi Eksekusi Fase Dokumentasi & Retensi Hasil Studi Kasus Myoglobin — 2026-10-15

**Status persetujuan: DISETUJUI (2026-10-15) — pemilik proyek.** Bunyi otorisasi **tidak dikutip verbatim**: tidak ada rumusan otorisasi verbatim yang ditetapkan untuk pencatatan ini; substansi persetujuan direkam pada butir (1)–(4). **Entri ini tidak memuat teks pengganti apa pun** dan **tidak mengubah** protokol, policy, maupun dokumen beku mana pun (Aturan file ini).

**(1) Fakta yang dicatat.** Eksekusi Colab **2026-10-15** menjalankan unduhan struktur dan komputasi metrik **pada fase dokumentasi**. **Tidak ditemukan catatan otorisasi** untuk eksekusi tersebut pada `docs/`.

**(2) Aturan yang bersinggungan.** `docs/DATA_AND_BIAS_POLICY.md` §8 butir 9 — *"Menjalankan tool/unduhan pada fase ini (fase dokumen): Phase 2 = dokumen saja."* — dinyatakan mengikat **seluruh fase**. Butir §8.5 (*"Memakai metrik tool tanpa versi terkunci"*) juga bersinggungan: **tool dan versi** yang dipakai untuk komputasi RMSD **tidak tercatat**, dan `TOOLS_LOCK.md` belum berstatus LOCKED.

**(3) Disposisi.** Pemilik **mempertahankan** hasil sebagai **studi eksploratif non-praregistrasi**, terpisah dari T0 — keputusan `A-1` pada `docs/OWNER_DECISIONS.md`.

**(4) Batas disposisi ini.**
- **Tidak ada otorisasi retroaktif.** Entri ini **tidak** menyatakan eksekusi dimaksud pernah diotorisasi, dan **tidak** memberi otorisasi untuk eksekusi lanjutan.
- **Tidak mengubah status.** **T0 tetap HOLD**; C-2 tetap `USULAN — BELUM BERLAKU`; V1–V3 belum dijalankan/belum diotorisasi; K-2 belum diratifikasi; tidak ada endpoint, ambang, atau metodologi yang berubah.
- **Studi kasus tidak memenuhi ketentuan praregistrasi** dan tidak dapat dipakai sebagai dasar klaim protokol.

**(5) Penilaian kepatuhan — TIDAK DILAKUKAN.** Entri ini merekam fakta dan disposisi hasil. Entri ini **bukan** pernyataan bahwa eksekusi dimaksud patuh terhadap `DATA_AND_BIAS_POLICY.md` §8, dan **bukan** pernyataan bahwa eksekusi dimaksud tidak melanggar. Penetapan status kepatuhan **belum dibuat** dan menunggu keputusan pemilik tersendiri.

**(6) Status sebelum/sesudah melihat hasil (wajib — protokol §G.5).** Disposisi ini ditetapkan **setelah** hasil studi kasus terlihat. Fakta tersebut dicatat apa adanya, **tanpa** menyimpulkan status kepatuhan (lihat butir 5).

**(7) Dampak dokumen.** `docs/OWNER_DECISIONS.md` menerima entri `A-1` (bukan dokumen beku). **Kelima dokumen Phase 2 tetap tidak diubah**; verifikasi hash dilaporkan di sesi.

**Referensi teks lama:** tidak berlaku — entri ini tidak mengganti teks apa pun.

**Tidak termasuk (menunggu keputusan pemilik tersendiri):** penetapan kepatuhan §8 (butir 5), provenance, versi tool, kelayakan publikasi, serta status `.gitignore` dan kebijakan retensi artefak.

---

## Catatan silang — Penetapan Kepatuhan §8.9 (keputusan `A-2`) — 2026-10-15 (UTC)

Butir **(5)** entri *"Disposisi Eksekusi Fase Dokumentasi & Retensi Hasil Studi Kasus Myoglobin — 2026-10-15"* di atas menyatakan penetapan status kepatuhan **belum dibuat** dan menunggu keputusan pemilik tersendiri. Keputusan tersendiri itu **telah dibuat** pada **2026-10-15 (UTC)** dan dicatat sebagai **`A-2`** pada `docs/OWNER_DECISIONS.md`.

Entri di atas **tidak diubah**: butir (5) tetap berlaku sebagai pernyataan status **pada saat entri itu ditulis**. Catatan silang ini hanya menautkan status terkini; ia **tidak** memuat teks pengganti protokol/policy, **tidak** mengulang rumusan keputusan (dihindari duplikasi — rumusan lengkap ada di `docs/OWNER_DECISIONS.md`), **tidak** mengubah metodologi, dan **tidak** mengubah status T0/C-2/K-2/V1–V3.
