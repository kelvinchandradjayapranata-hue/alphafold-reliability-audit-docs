# STATUS PUBLIKASI — KEPUTUSAN RILIS v0.1.0

| | |
|---|---|
| **Root audit** | `D:\project\alphafold-reliability-audit` |
| **Tanggal** | 2026-10-15 |
| **Status** | **FINAL untuk rilis v0.1.0** — dokumen ini adalah **daftar keputusan rilis**, bukan jurnal audit |
| **Produk publikasi** | **Pilihan A — repositori protokol/metodologi** (tanpa hasil empiris, tanpa artefak data) |
| **Lisensi dokumentasi** | **CC BY 4.0** (keputusan `A-4`, 2026-10-15) — teks resmi verbatim di `LICENSE`; aset gambar & materi pihak ketiga **tidak** tercakup |
| **Paket rilis lokal** | `D:\project\alphafold-release-v0.1.0` — snapshot untuk ditinjau; **bukan** commit, **bukan** push |
| **Dasar** | Keputusan `A-1`, `A-2`, `A-3` (`docs/OWNER_DECISIONS.md`); audit kesiapan publikasi artefak (read-only, 2026-10-15) |

---

## 1. Keputusan produk

**Dipilih: Pilihan A — Research protocol / methodology repository.** Bukan B, bukan C.

Alasan berbasis bukti:

1. Satu-satunya eksekusi nyata (studi kasus myoglobin) **ditetapkan tidak sesuai** fase dokumentasi `DATA_AND_BIAS_POLICY.md` §8.9 — keputusan `A-2` (2026-10-15), **tanpa otorisasi retroaktif**. Eksekusi itu tidak boleh ditampilkan sebagai eksperimen yang mematuhi protokol.
2. **Provenance artefak UNVERIFIED**; **versi tool & parameter tidak tercatat** → angkanya tidak dapat dinyatakan reproducible.
3. Ketiga entri struktur pembanding berasal dari **satu publikasi** → **bukan** validasi independen.
4. Sisa repositori — protokol praregistrasi, kebijakan data & bias, register keputusan, tinjauan 40 sumber, gate review — **kokoh tanpa** hasil empiris. Itulah kontribusi rilis ini.

**Konsekuensi yang diterima:** rilis ini **tidak** mengklaim hasil penelitian apa pun. Studi kasus myoglobin muncul **hanya** sebagai dokumentasi keterbatasan: baris status pada `README.md`, tabel keterbatasan, dan catatan keputusan `A-1`/`A-2`. Angka RMSD disebut **semata** untuk menandainya `dilaporkan, belum diverifikasi secara independen`. **Nol** berkas artefak data disertakan.

**Hubungan dengan `A-3`:** `A-3` menetapkan **arah** (derived-only, bersyarat — bukan izin terbit). Rilis v0.1.0 mengambil posisi **lebih konservatif**: tidak ada kandidat turunan yang melewati seluruh syarat `A-3` (provenance, atribusi, versi tool, label), sehingga **tidak ada artefak** yang disertakan. Ini tidak membatalkan `A-3` dan tidak menghapus apa pun secara lokal.

**Lisensi dokumentasi:** **CC BY 4.0** untuk dokumentasi asli proyek (keputusan `A-4`, 2026-10-15) — **tidak** mencakup aset gambar, materi pihak ketiga, atau data. Rincian: `ATTRIBUTION.md`.

---

## 2. Disertakan dalam paket rilis v0.1.0

18 berkas berikut (7 berkas akar + 11 dokumen `docs/`), ditambah `RELEASE_MANIFEST.md` (memuat ukuran + SHA-256 setiap berkas) → **19 berkas** pada direktori rilis:

| # | Berkas | Peran |
|---|---|---|
| 1 | `README.md` | Dokumen utama: pertanyaan riset, ruang lingkup, status bukti, tingkat klaim, keterbatasan |
| 2 | `LICENSE` | Teks resmi **CC BY 4.0** (verbatim) — lisensi dokumentasi asli proyek |
| 3 | `ATTRIBUTION.md` | Ruang lingkup lisensi & atribusi — termasuk materi yang **tidak** tercakup |
| 4 | `CHANGELOG.md` | Riwayat rilis |
| 5 | `CITATION.cff` | Metadata sitasi (CFF 1.2.0) — hanya informasi yang diketahui |
| 6 | `.gitignore` | Termasuk aturan pencegahan publikasi tak sengaja (§5) |
| 7 | `.gitattributes` | Kebijakan line-ending (LF) |
| 8 | `docs/RESEARCH_PROTOCOL_V1.md` | Protokol riset (dokumen beku) |
| 9 | `docs/DATA_AND_BIAS_POLICY.md` | Kebijakan data, versi, dan bias (dokumen beku) |
| 10 | `docs/LITERATURE_REVIEW.md` | 40 sumber terverifikasi `[L1]`–`[L40]` (dokumen beku) |
| 11 | `docs/PHASE1_BASELINE_AUDIT.md` | Audit baseline fase sebelumnya (dokumen beku) |
| 12 | `docs/PHASE2_REVIEW_GATE.md` | Laporan gate review Tahap 2 (dokumen beku) |
| 13 | `docs/PHASE2_DECISION.md` | Keputusan Tahap 2 + lampiran hash pembekuan |
| 14 | `docs/PHASE2D_DECISION_PACKAGE.md` | Paket keputusan praregistrasi (D-2..D-6b) |
| 15 | `docs/TOOLS_LOCK.md` | Rencana penguncian tool & versi |
| 16 | `docs/PROTOCOL_AMENDMENTS.md` | Riwayat amendemen dokumen beku |
| 17 | `docs/OWNER_DECISIONS.md` | Register keputusan pemilik (K, D, dan seri administratif A-1..A-4) |
| 18 | `docs/PUBLICATION_STATUS.md` | Dokumen ini |

---

## 3. Dikecualikan dari paket rilis

| Item | Ukuran | Alasan |
|---|---|---|
| `artifacts/colab-2026-10-15/**` (34 berkas) | 2.997.815 B | Provenance **UNVERIFIED**; berasal dari eksekusi yang **ditetapkan tidak sesuai §8.9** (`A-2`); versi tool & parameter tidak tercatat; berisi CIF pihak ketiga tanpa blok lisensi di dalam berkas, manifest yang menunjuk payload, dan label `verified`/`independent`/`recomputed` tanpa bukti. **Tidak ada** berkas yang lulus seluruh syarat `A-3`. Tetap utuh di disk — tidak dihapus, tidak dipindah. |
| `image/hero-banner.png` | 2.048.346 B | Sumber/lisensi **tidak terdokumentasi** → **dikeluarkan** dari paket rilis. Berkas sumber tetap di repositori — **tidak** dihapus, **tidak** dipindah, **tidak** dinyatakan bebas lisensi. |
| `Downloads/**` (3.744 berkas) | ≈ 25,01 GiB | Data lokal pemilik, **bukan** bagian proyek. Tidak disentuh. |
| `docs/C2_OPERATIONAL_SPEC_DRAFT.md` | 15.414 B | Draft `WORKING DRAFT / UNRESOLVED`; publikasi menunggu keputusan pemilik O1–O6. Tidak dihapus. |
| `image/Protein Structure Analysis Dashboard.png` | 1.682.297 B | Sumber & lisensi **tidak terdokumentasi** di repositori → tidak disertakan (aturan “lisensi tidak jelas → tidak disertakan”). Tidak dihapus. |
| Berkas mentah pihak ketiga (CIF/CIF AFDB) | — | Tidak ada redistribusi data pihak ketiga pada paket ini |

**Catatan:** pengecualian dilakukan lewat **aturan `.gitignore`** dan **tidak menyertakan** berkas ke direktori rilis — **bukan** lewat penghapusan. Tidak ada satu berkas pun yang dihapus dari disk.

---

## 4. Syarat yang masih harus diputuskan pemilik sebelum publikasi eksternal

| # | Syarat | Mengapa memblokir | Tindakan |
|---|---|---|---|
| 1 | **Aset gambar (`image/`) — konfirmasi asal** | Gambar telah **dikeluarkan** dari paket; hanya relevan bila pemilik kelak ingin menyertakannya kembali | Pemilik mengonfirmasi asal/lisensi lebih dulu bila ingin menyertakan |
| 2 | **Verifikasi ulang lisensi pihak ketiga ke sumber resmi** | `DATA_AND_BIAS_POLICY.md` §4 mewajibkannya sebelum publikasi eksternal; belum dijalankan ulang | Wajib **hanya bila** kelak ada materi pihak ketiga yang disertakan — rilis ini tidak memuatnya |
| 3 | **Keputusan menerbitkan repo GitHub yang ada (kini privat) atau membuat rilis baru** | Di luar wewenang pekerjaan ini (dilarang push/ubah visibilitas) | Pemilik memutuskan setelah meninjau paket |
| 4 | **Sikap resmi bila kelak ingin menerbitkan hasil studi kasus** | Hasil berasal dari eksekusi non-§8.9; `A-3` memberi arah, belum memberi izin | Keputusan pemilik tersendiri + penyelesaian syarat `A-3` |
| 5 | **Cakupan `K-7` lainnya** (atribusi output riset; lisensi tool `UNVERIFIED` mis. US-align/TM-align) | Tidak diselesaikan oleh `A-4` — `A-4` hanya menetapkan lisensi dokumentasi | Keputusan pemilik saat menyusun laporan/publikasi |

**Yang tidak berubah:** **T0 tetap HOLD**; prasyarat CONDITIONAL GO belum terpenuhi; `A-1`/`A-2` tetap berlaku; **tidak ada otorisasi retroaktif**.

---

## 5. Perubahan `.gitignore` (2026-10-15)

Empat aturan ditambahkan agar materi lokal tidak ikut ter-commit tanpa keputusan eksplisit:

| Aturan | Alasan |
|---|---|
| `Downloads/` | 3.669 berkas (23,30 GiB) sebelumnya **tidak** tertutup pola mana pun → risiko `git add -A` |
| `artifacts/` | Artefak kerja studi kasus (publikasi menuntut keputusan terpisah, `A-3`) |
| `docs/C2_OPERATIONAL_SPEC_DRAFT.md` | Draft; publikasi menunggu O1–O6 |
| `image/Protein Structure Analysis Dashboard.png` | Sumber/lisensi tidak terdokumentasi |

**Yang tidak diabaikan (sengaja):** seluruh berkas paket rilis (§2). `docs/*.md` tetap tidak diabaikan.

---

## 6. Batas klaim rilis ini

- Rilis ini **tidak** memuat hasil penelitian, **tidak** mengklaim verifikasi independen, **tidak** mengklaim reproducibility, dan **tidak** mengklaim lisensi yang belum dipilih.
- Tidak ada eksperimen, unduhan, instalasi, penghitungan ulang RMSD, `git add`, commit, push, penghapusan, atau pemindahan berkas yang dilakukan bersamaan dengan penyusunan rilis ini.
- Lima dokumen beku tetap **byte-identik**; hash-nya dilaporkan pada sesi kerja dan pada lampiran `docs/PHASE2_DECISION.md`.
- Lisensi dokumentasi: **CC BY 4.0** (keputusan `A-4`); teks resmi verbatim di `LICENSE`; aset gambar dan materi pihak ketiga **tidak** tercakup lisensi ini.

---

## 7. Riwayat dokumen

| Tanggal | Perubahan |
|---|---|
| 2026-10-15 | Dibuat sebagai ringkasan status publikasi artefak (audit kesiapan publikasi) |
| 2026-10-15 | Difinalkan sebagai **daftar keputusan rilis v0.1.0**: produk dipilih (Pilihan A), isi paket ditetapkan, pengecualian dicatat, syarat pemilik dirinci |
| 2026-10-15 | Paket dibangun ulang setelah keputusan `A-4`: `LICENSE` (CC BY 4.0) ditambahkan; aset gambar dikeluarkan dari paket; `README.md`/`ATTRIBUTION.md` diselaraskan |
