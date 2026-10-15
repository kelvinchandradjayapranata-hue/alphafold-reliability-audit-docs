# Changelog

Semua perubahan penting pada paket rilis ini dicatat di sini. Format terinspirasi [Keep a Changelog](https://keepachangelog.com/); versi memakai penomoran `MAJOR.MINOR.PATCH`.

> **Catatan status:** `v0.1.0` diterbitkan 2026-10-15 sebagai **repositori dokumentasi publik** (salinan dari paket rilis lokal v0.1.0; hanya teks status repositori yang disesuaikan). Repositori audit utama tetap privat. Lihat `docs/PUBLICATION_STATUS.md`.

## [0.1.0] — 2026-10-15

Rilis pertama yang disiapkan. Produk: **repositori protokol/metodologi** — tanpa hasil empiris, tanpa artefak data.

### Ditambahkan

- `LICENSE` — teks resmi **CC BY 4.0** verbatim dari creativecommons.org (lisensi dokumentasi asli proyek; keputusan `A-4`). Sumber, tanggal ambil, ukuran, dan SHA-256 dicatat di `RELEASE_MANIFEST.md`.
- `ATTRIBUTION.md` — ruang lingkup lisensi CC BY 4.0, atribusi yang diminta, dan materi yang **tidak** tercakup (aset gambar, materi pihak ketiga, data).
- `CHANGELOG.md` — berkas ini.
- `CITATION.cff` — metadata sitasi (CFF 1.2.0); hanya memuat informasi yang diketahui — tanpa lisensi, tanpa DOI, tanpa tanggal rilis.
- `README.md` §Status Bukti & Tingkat Klaim — tabel empat tingkat bukti: **sudah dijalankan** / **dilaporkan, belum diverifikasi** / **diverifikasi independen (nol)** / **belum dikerjakan**, beserta daftar hal yang tidak boleh disimpulkan.
- `docs/PUBLICATION_STATUS.md` difinalkan sebagai **daftar keputusan rilis** (sebelumnya jurnal audit): produk dipilih (Pilihan A), isi paket, pengecualian, dan syarat tersisa.
- `.gitignore` — empat aturan pencegahan publikasi tak sengaja: `Downloads/`, `artifacts/`, `docs/C2_OPERATIONAL_SPEC_DRAFT.md`, `image/Protein Structure Analysis Dashboard.png`.
- Bagian struktur repositori pada `README.md` — daftar berkas akar + urutan membaca diperbarui (butir 10–11).

### Diubah

- `README.md` §Lisensi — ditetapkan **CC BY 4.0** untuk dokumentasi asli proyek (atribusi + tabel ruang lingkup); materi pihak ketiga dan aset gambar **tidak** tercakup.
- `README.md` — gambar hero **dikeluarkan dari paket**: blok gambar header dan keterangannya dihapus; struktur repositori tidak lagi mencantumkan `image/`; `LICENSE` ditambahkan ke daftar berkas akar dan urutan membaca.
- `CITATION.cff` — field `license: CC-BY-4.0` ditambahkan.
- `.gitattributes` — aturan `LICENSE text eol=lf` (teks lisensi tetap LF di semua platform).
- `README.md` §Studi Kasus Eksploratif Myoglobin — ditambahkan catatan bahwa **tidak ada** artefak data yang disertakan pada paket rilis; angka tetap ditandai `dilaporkan, belum diverifikasi`.
- `README.md` — perbaikan penanda alert ganda pada blok catatan studi kasus; baris kosong akhir ditambahkan.

### Tidak diubah (sengaja)

- **Lima dokumen beku byte-identik**: `RESEARCH_PROTOCOL_V1.md`, `DATA_AND_BIAS_POLICY.md`, `LITERATURE_REVIEW.md`, `PHASE1_BASELINE_AUDIT.md`, `PHASE2_REVIEW_GATE.md` — hash pembekuan tetap berlaku.
- **Status T0 tetap HOLD**; keputusan `A-1`, `A-2`, `A-3` tetap berlaku; tidak ada otorisasi retroaktif.
- **Tidak ada penghapusan atau pemindahan berkas** — termasuk `Downloads/` dan `artifacts/` (tetap utuh di disk, kini tertutup `.gitignore`).
- **Tidak ada operasi Git tulis** — tanpa `git add`, commit, atau push; tanpa perubahan visibilitas repositori.
- **Tidak ada eksperimen, unduhan, instalasi, atau penghitungan ulang RMSD.**

### Dikecualikan dari paket rilis

- Seluruh `artifacts/colab-2026-10-15/**` (34 berkas, 2.997.815 B) — provenance belum terverifikasi; eksekusi ditetapkan tidak sesuai §8.9 (`A-2`); versi tool/parameter tidak tercatat.
- `Downloads/**` — data lokal pemilik, bukan bagian proyek.
- `docs/C2_OPERATIONAL_SPEC_DRAFT.md` — draft; menunggu keputusan O1–O6.
- `image/Protein Structure Analysis Dashboard.png` — sumber/lisensi tidak terdokumentasi.
- `image/hero-banner.png` — sumber/lisensi belum terdokumentasi → **dikeluarkan** dari paket rilis (berkas sumber tetap di repositori; **tidak** dihapus/dipindah; **tidak** dinyatakan bebas lisensi).

### Diketahui belum selesai

- Lisensi dokumentasi ditetapkan **CC BY 4.0** (`A-4`); cakupan `K-7` lainnya (atribusi output riset, lisensi tool `UNVERIFIED`) **tetap terbuka**.
- Aset `image/` (termasuk hero) dikeluarkan dari paket; konfirmasi asal hanya diperlukan bila pemilik kelak ingin menyertakannya kembali.
- Verifikasi ulang lisensi pihak ketiga ke sumber resmi belum dijalankan (belum diperlukan selama tidak ada materi pihak ketiga yang disertakan).
- ~~Keputusan menerbitkan repositori~~ — **selesai 2026-10-15**: dibuat repositori dokumentasi publik `alphafold-reliability-audit-docs`; repositori audit utama tetap privat.
