<div align="center">

# AlphaFold Reliability Audit

**Audit reliabilitas metodologi riset berbasis AlphaFold — dokumentasi tata kelola dan pra-registrasi.**

Kalibrasi pLDDT/PAE model AlphaFold DB terhadap error struktural terukur (prediksi vs struktur eksperimen PDB).

![status: T0 HOLD](https://img.shields.io/badge/status-T0_HOLD-red?style=flat-square)
![C-2: belum diadopsi](https://img.shields.io/badge/C--2-belum_diadopsi-orange?style=flat-square)
![fase: audit dokumentasi](https://img.shields.io/badge/fase-audit_dokumentasi-blue?style=flat-square)
![visibilitas: publik](https://img.shields.io/badge/visibilitas-publik-brightgreen?style=flat-square)
![dokumen: 11 governance](https://img.shields.io/badge/dokumen-11_governance-blueviolet?style=flat-square)

</div>

---

> [!IMPORTANT]
> **T0 = HOLD — protokol utama belum menjalankan eksekusi apa pun.** Repositori ini murni **dokumentasi tahap persiapan/pra-registrasi**: **bukan** hasil eksperimen, **bukan** data, dan **bukan** sekuens protein. Satu-satunya pekerjaan yang sudah dieksekusi adalah **studi kasus eksploratif myoglobin** yang berada **di luar** protokol (lihat [Studi Kasus Eksploratif Myoglobin](#studi-kasus-eksploratif-myoglobin-di-luar-t0)); hasilnya berlaku **hanya** untuk protein itu dan **bukan** hasil protokol utama.

## Daftar Isi

- [Ringkasan](#ringkasan)
- [Status Riset (2026-10-11)](#status-riset-2026-10-11)
- [Status Bukti & Tingkat Klaim](#status-bukti--tingkat-klaim)
- [Mengapa Proyek Ini Penting](#mengapa-proyek-ini-penting)
- [Pertanyaan Riset](#pertanyaan-riset)
- [Prinsip dan Alur Kerja Audit](#prinsip-dan-alur-kerja-audit)
- [Struktur Repositori](#struktur-repositori)
- [Verifikasi dan Reproduksibilitas](#verifikasi-dan-reproduksibilitas)
- [Studi Kasus Eksploratif Myoglobin](#studi-kasus-eksploratif-myoglobin-di-luar-t0)
- [Sitasi](#sitasi)
- [Lisensi](#lisensi)
- [Keterbatasan](#keterbatasan)

---

## Ringkasan

Repositori dokumentasi tata kelola untuk **audit reliabilitas metodologi riset berbasis AlphaFold** — kalibrasi indikator keandalan pLDDT/PAE model AlphaFold DB terhadap error struktural terukur, dibandingkan dengan struktur eksperimen PDB.

Isi repositori ini murni **dokumentasi tahap persiapan/pra-registrasi** — **bukan** hasil eksperimen, **bukan** data, dan **bukan** sekuens protein. Seluruh temuan audit, koreksi, dan keputusan tercatat lengkap di `docs/`; README ini tidak merangkum ulang isinya.

Satu pengecualian terhadap pernyataan di atas: artefak **studi kasus eksploratif myoglobin** ada di lingkungan kerja (di luar Git) dan **bukan** bagian dari protokol praregistrasi — lihat [Studi Kasus Eksploratif Myoglobin](#studi-kasus-eksploratif-myoglobin-di-luar-t0).

---

## Status Riset (2026-10-11)

| Aspek | Status |
|---|---|
| Tahap | Dokumentasi/pra-registrasi — **protokol utama** belum menjalankan unduhan data, instalasi tool, atau eksekusi eksperimen |
| Gerbang eksekusi **T0** | **HOLD** — tanpa persetujuan pemilik tidak ada unduhan/instalasi/eksekusi |
| Studi kasus eksploratif **myoglobin** | **Di luar protokol T0** — artefak eksekusi satu protein ada di lingkungan kerja (di luar Git); otorisasi **tidak ditemukan**, dan eksekusinya **ditetapkan tidak sesuai** `DATA_AND_BIAS_POLICY.md` §8.9 (keputusan `A-2`, 2026-10-15); provenance **belum diverifikasi**; **tidak mengubah status T0** |
| Uji validasi pra-eksekusi **V1–V3** | Belum dijalankan dan belum diotorisasi |
| Spesifikasi operasional **C-2** | **USULAN — BELUM BERLAKU** (belum diadopsi); `WORKING DRAFT / UNRESOLVED`; keputusan pemilik **O1–O6** masih tertunda |
| **Formula** | Belum final — berstatus **WORKING DRAFT / UNRESOLVED** kecuali yang secara eksplisit tercatat diputuskan |
| Ratifikasi **K-2** (cutoff data latih) | Tertunda |
| Prasyarat minimum **CONDITIONAL GO** sebelum T0 | Belum terpenuhi |
| Visibilitas repositori | **Publik** (repositori dokumentasi ini) — repositori audit utama tetap privat; publikasi hasil penelitian tetap menunggu keputusan pemilik |

> [!NOTE]
> Angka/ambang yang tercatat di `docs/OWNER_DECISIONS.md` dan `docs/PROTOCOL_AMENDMENTS.md` adalah **konvensi pra-registrasi**, bukan klaim kebenaran biologis. Dokumen berstatus beku hanya berubah melalui amendemen tercatat.

---

## Status Bukti & Tingkat Klaim

Repositori ini membedakan empat tingkat bukti. Tidak ada tingkat yang boleh dibaca sebagai tingkat di atasnya.

| Tingkat | Artinya | Isi repositori ini |
|---|---|---|
| **Sudah dijalankan** | Eksekusi nyata dengan artefak tercatat | **Hanya** studi kasus eksploratif myoglobin — di luar protokol, dan ditetapkan **tidak sesuai** `DATA_AND_BIAS_POLICY.md` §8.9 (keputusan `A-2`, 2026-10-15) |
| **Dilaporkan, belum diverifikasi** | Angka yang ditulis oleh skrip atau laporan sebelumnya | RMSD myoglobin ≈ 0,595–0,613 Å atas 153 pasangan Cα; **tidak** dihitung ulang pada audit mana pun |
| **Diverifikasi independen** | Verifikasi oleh pihak/lingkungan terpisah | **Nol.** Tidak ada klaim verifikasi independen yang dibuat di repositori ini |
| **Belum dikerjakan** | Rencana, hipotesis, atau prasyarat | Seluruh protokol utama (T0 HOLD): unduhan data, mapping, metrik, analisis RQ1–RQ4; uji V1–V3; adopsi C-2 |

Yang **tidak** boleh disimpulkan dari repositori ini:

- **pLDDT/PAE bukan pengukuran akurasi eksperimental** — keduanya skor keyakinan prediksi.
- Ketiga entri struktur pembanding (1BZ6, 1BZP, 1BZR) berasal dari **satu publikasi** — **bukan** tiga sumber validasi independen.
- Provenance artefak **belum terverifikasi**; versi tool dan parameter **tidak tercatat**.
- Performa satu protein **bukan** estimasi akurasi AlphaFold DB secara umum.

---

## Mengapa Proyek Ini Penting

Model AlphaFold DB banyak digunakan untuk triase dan analisis struktural, dan **pLDDT/PAE adalah indikator keandalan de-facto** yang umum dibaca sebagai "kualitas model". Pertanyaan audit ini: **seberapa andal indikator tersebut ketika diukur terhadap struktur eksperimen yang sebenarnya — dan di mana mereka gagal.**

Titik tolaknya adalah temuan **Phase 1 Baseline Audit** pada proyek rujukan `ai-biotech` (read-only): prediksi AlphaFold **belum pernah** dibandingkan dengan ground truth eksperimen — tidak ada struktur PDB, tidak ada RMSD/TM-score/lDDT, dan tidak ada tool pembanding struktur yang terpasang (lihat `docs/PHASE1_BASELINE_AUDIT.md`). Audit ini menjadi fokus penutup celah tersebut, dengan desain yang dikunci **sebelum** data hasil dilihat.

Audit ini juga secara eksplisit mengukur **kegagalan high-confidence** — residu dengan pLDDT ≥ 90 yang menyimpang > 2,0 Å secara lokal — alih-alih mengasumsikan pLDDT tinggi berarti benar. Komitmen pelaporan: **hasil apa pun, termasuk null/negatif, dilaporkan apa adanya.**

---

## Pertanyaan Riset

> **RQ utama** (kutipan dari `docs/RESEARCH_PROTOCOL_V1.md` §0.1):
>
> *Seberapa andal pLDDT dan PAE sebagai indikator akurasi struktural model AlphaFold, diukur terhadap struktur eksperimen, dan seberapa besar gap antara akurasi prediksi dan struktur eksperimen?*

| # | Sub-pertanyaan | Level analisis |
|---|---|---|
| **RQ1** | Apakah pLDDT memprediksi error lokal terukur? Berapa distribusi error lokal di tiap bin pLDDT? | Per-residu |
| **RQ2** | Apakah pLDDT tinggi menjamin kesesuaian global (TM-score/RMSD) — dan di mana ia gagal walau tinggi? | Per-protein |
| **RQ3** | Apakah PAE cross-domain memprediksi error orientasi domain relatif? | Per-protein multidomain |
| **RQ4** | Berapa frekuensi dan bentuk kegagalan high-confidence (pLDDT ≥ 90 dengan error lokal > 2,0 Å)? | Per-residu terkondisi |

Arah hipotesis kerja (H1–H4) bersumber dari literatur dan **bukan hasil**; jika hasil bertentangan dengan arah tersebut, hasil dilaporkan apa adanya.

---

## Prinsip dan Alur Kerja Audit

1. **Pra-registrasi** — semua keputusan metodologis yang memengaruhi hasil ditetapkan **sebelum** data hasil dilihat; perubahan setelahnya hanya melalui **amendemen tercatat** (`docs/PROTOCOL_AMENDMENTS.md`).
2. **Read-only lebih dulu** — pemeriksaan dijalankan tanpa mengubah state; temuan dilaporkan, bukan diperbaiki otomatis.
3. **Gerbang otorisasi** — setiap penulisan dokumen memerlukan otorisasi tertulis dari pemilik.
4. **Verifikasi byte-level** — setelah setiap penulisan, SHA-256, line-ending, dan encoding berkas diverifikasi sebelum dicatat.
5. **Tanpa penyaringan senyap** — eksklusi sampel dicatat satu per satu dengan alasan.
6. **Pelaporan jujur** — hasil null/negatif tetap dilaporkan.

---

## Struktur Repositori

Berkas dokumentasi inti (aset gambar lokal dan materi data **tidak** termasuk paket rilis v0.1.0 — lihat `docs/PUBLICATION_STATUS.md`):

```text
alphafold-reliability-audit/
├── README.md                  ← dokumen ini
├── ATTRIBUTION.md             ← ruang lingkup lisensi & atribusi
├── CHANGELOG.md
├── CITATION.cff
├── LICENSE                    ← teks resmi CC BY 4.0 (dokumentasi asli)
├── .gitattributes             ← kebijakan line-ending (LF)
├── .gitignore
└── docs/
    ├── RESEARCH_PROTOCOL_V1.md
    ├── PROTOCOL_AMENDMENTS.md
    ├── OWNER_DECISIONS.md
    ├── PHASE2_REVIEW_GATE.md
    ├── PHASE2D_DECISION_PACKAGE.md
    ├── TOOLS_LOCK.md
    ├── DATA_AND_BIAS_POLICY.md
    ├── LITERATURE_REVIEW.md
    ├── PHASE1_BASELINE_AUDIT.md
    ├── PHASE2_DECISION.md
    └── PUBLICATION_STATUS.md
```

Dokumen dalam `docs/`:

| Dokumen | Isi |
|---|---|
| `RESEARCH_PROTOCOL_V1.md` | Protokol riset (dokumen beku; perubahan hanya via amendemen) |
| `PROTOCOL_AMENDMENTS.md` | Riwayat amendemen — semua perubahan dokumen beku mengalir lewat berkas ini |
| `OWNER_DECISIONS.md` | Kanonik keputusan pemilik + entri log |
| `PHASE2_REVIEW_GATE.md` | Laporan gate review Tahap 2 (blocker KB/NB; koreksi C-1..C-11) |
| `PHASE2D_DECISION_PACKAGE.md` | Paket keputusan pra-registrasi (rekaman D-2..D-6b) |
| `TOOLS_LOCK.md` | Rencana penguncian tool & versi (penguncian belum disetujui) |
| `DATA_AND_BIAS_POLICY.md` | Kebijakan data, versi, dan bias |
| `LITERATURE_REVIEW.md` | Sumber tunggal sitasi/DOI (40 sumber, [L1]–[L40]) |
| `PHASE1_BASELINE_AUDIT.md` | Artefak fase sebelumnya — audit baseline |
| `PHASE2_DECISION.md` | Artefak fase sebelumnya — keputusan Tahap 2 |
| `PUBLICATION_STATUS.md` | Status publikasi artefak — daftar keputusan rilis: yang disertakan, yang dikecualikan, dan syarat yang tersisa |

Berkas akar (bukan di `docs/`):

| Dokumen | Isi |
|---|---|
| `LICENSE` | Teks resmi **CC BY 4.0** (verbatim) — lisensi dokumentasi asli proyek |
| `ATTRIBUTION.md` | Ruang lingkup lisensi & atribusi — termasuk materi yang **tidak** tercakup |
| `CHANGELOG.md` | Riwayat rilis paket ini |
| `CITATION.cff` | Metadata sitasi (CFF 1.2.0) — hanya informasi yang diketahui |

> [!WARNING]
> Draft spesifikasi operasional C-2 ada di lingkungan kerja tetapi **belum diterbitkan** pada repositori (menunggu keputusan pemilik O1–O6) — karena itu **tidak** tercantum pada struktur di atas.
>
> Folder `artifacts/colab-2026-10-15/` (studi kasus eksploratif myoglobin) juga ada di lingkungan kerja dan **belum diterbitkan** — tidak dilacak Git, dan tidak dianggap terbit. **Paket rilis v0.1.0 tidak menyertakan satu pun berkas artefak data** — lihat `docs/PUBLICATION_STATUS.md`. Lihat [Studi Kasus Eksploratif Myoglobin](#studi-kasus-eksploratif-myoglobin-di-luar-t0).

### Urutan Membaca yang Disarankan

1. `docs/RESEARCH_PROTOCOL_V1.md`
2. `docs/PROTOCOL_AMENDMENTS.md`
3. `docs/OWNER_DECISIONS.md`
4. `docs/PHASE2_REVIEW_GATE.md`
5. `docs/PHASE2D_DECISION_PACKAGE.md`
6. `docs/TOOLS_LOCK.md`
7. `docs/DATA_AND_BIAS_POLICY.md`
8. `docs/LITERATURE_REVIEW.md`
9. `docs/PHASE1_BASELINE_AUDIT.md`, `docs/PHASE2_DECISION.md` — artefak fase sebelumnya (hash pembekuan di lampiran `PHASE2_DECISION.md`)
10. `docs/PUBLICATION_STATUS.md` — daftar keputusan rilis: isi paket, pengecualian, syarat tersisa
11. `LICENSE`, `ATTRIBUTION.md`, `CHANGELOG.md`, `CITATION.cff` — teks lisensi resmi, ruang lingkup atribusi, riwayat rilis, metadata sitasi

---

## Verifikasi dan Reproduksibilitas

Tahap ini tidak memiliki hasil untuk direproduksi — **reproduksibilitas di sini berarti jejak audit**: setiap perubahan dapat diverifikasi ulang dari status repositori.

- Setiap berkas diverifikasi **byte-level** (SHA-256 + line-ending + encoding) sebelum dicatat; operasi tulis juga memverifikasi bahwa berkas yang **tidak** tersentuh tetap identik.
- Kebijakan line-ending: **LF** untuk `*.md`, `LICENSE`, `.gitignore`, dan `.gitattributes` (lihat `.gitattributes`).
- Nilai hash pembekuan dokumen direkam **di dalam repositori** (mis. lampiran `docs/PHASE2_DECISION.md`); nilai hash lengkap setiap penulisan dilaporkan pada sesi kerja dan diverifikasi ulang secara manual — **tidak ada** verifikasi otomatis pada setiap commit.

Contoh pemeriksaan (Git Bash / Unix shell):

```bash
sha256sum README.md docs/RESEARCH_PROTOCOL_V1.md
git check-attr text eol -- README.md
git log --oneline --stat -1
```

---

## Studi Kasus Eksploratif Myoglobin (di luar T0)

Di luar protokol praregistrasi terdapat pekerjaan eksploratif **satu protein**: **satu** model AlphaFold DB (`AF-P02185-F1`, myoglobin, 154 residu) dibandingkan dengan **tiga** entri struktur eksperimen RCSB yang terkait (1BZ6, 1BZP, 1BZR; masing-masing 153 residu). Artefaknya ada di lingkungan kerja pada `artifacts/colab-2026-10-15/` dan **tidak dilacak Git**.

| Aspek | Status |
|---|---|
| Hubungan dengan protokol utama | **Terpisah.** Keberadaan studi kasus ini **tidak mengubah** status T0 — **T0 tetap HOLD** — dan bukan bagian dari praregistrasi |
| Cakupan | **Satu** protein. Hasilnya **bukan** estimasi akurasi AlphaFold DB secara umum |
| Susunan pembandingan | Satu model vs tiga entri eksperimen dari protein yang sama, memakai satu pemetaan residu identik |
| Asal ketiga entri | **Satu publikasi** — Kachalova, Popov & Bartunik, *Science* **284**, 473–476 (1999), DOI `10.1126/science.284.5413.473`, PMID 10205052; ketiganya **bukan** tiga sumber eksperimen independen |
| Hasil yang dilaporkan | RMSD Cα setelah superposisi **≈ 0,595–0,613 Å** atas **153 pasangan Cα** terpetakan — **nilai yang dilaporkan di dalam artefak; belum diverifikasi ulang secara independen** (tidak dihitung ulang pada audit mana pun) |
| Independensi | **Ketiga nilai RMSD bukan validasi independen** — ketiganya berasal dari satu model yang sama, dan ketiga struktur pembandingnya berasal dari satu publikasi |
| Versi tool & parameter | **Tidak tercatat** — versi Python/Biopython, parameter alignment (mis. `alignment_score`), dan algoritme superposisi **tidak** terdokumentasi pada artefak maupun `docs/` |
| N-terminal metionin | **UNRESOLVED / UNVERIFIED** — perbedaan sekuens terbukti, tetapi **pemrosesan konstruk eksperimen tidak terbukti**; alignment sekuens saja tidak membuktikannya |
| Catatan otorisasi | **Tidak ada catatan otorisasi** untuk eksekusi ini di `docs/`, dan eksekusinya **ditetapkan tidak sesuai** ketentuan fase dokumentasi `DATA_AND_BIAS_POLICY.md` §8.9 — keputusan pemilik **`A-2`** (2026-10-15, `docs/OWNER_DECISIONS.md`). Penetapan itu **tidak menyimpulkan niat pelaksana** dan **tidak meniadakan** kemungkinan bukti di luar `docs/`; **tidak ada otorisasi retroaktif**, dan T0 tetap HOLD |
| Provenance | **Belum diverifikasi** — kecocokan hash lokal terhadap manifest lokal **bukan** bukti asal-usul dari sumber eksekusi |
| Kelayakan publikasi | **Belum ditetapkan** — artefak **tidak** dinyatakan siap dipublikasikan dan **belum** lolos audit provenance |

> [!NOTE]
> **Tidak ada artefak data dari studi kasus ini yang disertakan dalam paket rilis.** Berkas CIF, CSV, JSON, dan manifest tetap berada di lingkungan kerja lokal (di luar Git) dan dikecualikan dari rilis — lihat `docs/PUBLICATION_STATUS.md`. Angka di bawah hanya dirujuk agar keterbatasannya terdokumentasi; **tidak ada klaim hasil** yang dibuat dari angka tersebut.

> Angka RMSD di atas adalah **hasil studi kasus satu protein**, bukan ukuran kinerja AlphaFold DB secara umum, dan bukan keluaran protokol utama.
>
> **pLDDT pada artefak ini ada dua tingkat dan angkanya berbeda.** Skor **global** model = **97,48** (`_ma_qa_metric_global.metric_value` pada `AF-P02185-F1.cif`); **97,50** adalah nilai **per-residu (lokal)** pada `_ma_qa_metric_local` — hanya muncul pada dua residu (HIS 82 dan ILE 143) dan **bukan** skor global model. Keduanya adalah **skor keyakinan prediksi**, **bukan** pengukuran akurasi eksperimental dan bukan pengganti perbandingan terhadap struktur eksperimen. Tidak ada besaran bernama `globalMetricValue` pada dokumentasi maupun artefak repositori ini.

---

## Sitasi

Audit ini **belum dipublikasikan sebagai hasil penelitian** (belum ada preprint/publikasi ilmiah; hasil eksperimen tidak diterbitkan). Dokumentasi tata kelola tersedia publik di repositori ini. Bila perlu merujuk:

```bibtex
@misc{alphafold_reliability_audit_2026,
  title  = {AlphaFold Reliability Audit},
  author = {Kelvin Chandra Djayapranata},
  year   = {2026},
  note   = {Repositori dokumentasi publik — tata kelola/pra-registrasi berjalan; bukan publikasi ilmiah}
}
```

---

## Lisensi

**Lisensi dokumentasi asli proyek: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — *Creative Commons Attribution 4.0 International*** (keputusan pemilik 2026-10-15; teks resmi verbatim ada di [`LICENSE`](LICENSE)).

Atribusi yang diminta: **“AlphaFold Reliability Audit” oleh Kelvin Chandra Djayapranata (2026), dilisensikan CC BY 4.0.**

Ruang lingkup — lisensi ini **tidak otomatis** mencakup:

| Materi | Status | Disertakan dalam paket rilis? |
|---|---|---|
| Dokumentasi asli proyek (`README.md`, `docs/*.md`, berkas akar) | **CC BY 4.0** | Ya |
| Aset gambar/visual dalam repositori | Sumber/lisensi **belum terdokumentasi** — **tidak** tercakup lisensi ini | **Tidak** |
| Materi pihak ketiga (AlphaFold DB, RCSB PDB, pustaka) | Lisensi pihak ketiga berlaku tersendiri (CC BY 4.0 / CC0 1.0 — tercatat di `docs/OWNER_DECISIONS.md` `K-7`) | **Tidak** |

Rincian ruang lingkup, atribusi, dan kewajiban pembaca ada di [`ATTRIBUTION.md`](ATTRIBUTION.md).

---

## Keterbatasan

- **Protokol utama** belum menghasilkan data/eksekusi apa pun; prasyarat minimum sebelum T0 (**CONDITIONAL GO**) belum terpenuhi.
- Studi kasus eksploratif myoglobin mencakup **satu** protein dengan **tiga** entri terkait yang berasal dari **satu publikasi**; ketiga RMSD-nya **bukan** validasi independen, angkanya **dilaporkan dan belum diverifikasi ulang**, dan hasilnya **tidak** mewakili akurasi AlphaFold DB secara umum. Eksekusinya **ditetapkan tidak sesuai** §8.9 (keputusan `A-2`) **tanpa otorisasi retroaktif**; provenance-nya **belum diverifikasi**, dan versi tool serta parameter alignment **tidak tercatat**.
- Seluruh formula identity/coverage masih draft; C-2 belum diadopsi (keputusan O1–O6 tertunda).
- Repositori hanya berisi dokumentasi — tidak ada kode analisis yang dapat dijalankan pada tahap ini.
- Lisensi **CC BY 4.0** berlaku untuk dokumentasi asli saja; aset gambar dan materi pihak ketiga **tidak** tercakup dan **tidak** disertakan — lihat `ATTRIBUTION.md`.

---

<div align="center">
<sub>Repositori audit — setiap perubahan tercatat pada riwayat git.</sub>
</div>
