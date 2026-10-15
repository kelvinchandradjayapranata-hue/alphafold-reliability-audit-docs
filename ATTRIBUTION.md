# Atribusi & Lisensi

Dokumen ini menjelaskan **ruang lingkup lisensi**, atribusi yang diminta, dan materi yang **tidak** tercakup. Keputusan lisensi: `docs/OWNER_DECISIONS.md` `A-4` (2026-10-15). Rujukan kebijakan: `docs/DATA_AND_BIAS_POLICY.md` §4.

| Pertanyaan | Jawaban singkat |
|---|---|
| Lisensi dokumentasi asli proyek? | **CC BY 4.0** — teks resmi verbatim di `LICENSE` |
| Apa yang tercakup? | Dokumentasi yang ditulis untuk proyek ini (`README.md`, `docs/*.md`, berkas akar) |
| Aset gambar ikut terlisensi? | **Tidak** — sumber/lisensinya belum terdokumentasi; tidak disertakan pada paket rilis |
| Materi pihak ketiga ikut terlisensi? | **Tidak** — lisensi pihak ketiga berlaku tersendiri |
| Data/artefak ikut terlisensi? | **Tidak** — nol berkas data disertakan (kebijakan `A-3` tidak berubah) |

---

## 1. Lisensi dokumentasi asli — CC BY 4.0

**Berlaku untuk:** berkas dokumentasi yang ditulis untuk proyek ini — `README.md`, seluruh `docs/*.md`, serta `ATTRIBUTION.md`, `CHANGELOG.md`, `CITATION.cff`, dan berkas akar lain yang merupakan karya asli proyek.

**Atribusi yang diminta:**

> *AlphaFold Reliability Audit*, © 2026 **Kelvin Chandra Djayapranata** — dilisensikan di bawah [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Jika Anda mengubah materi ini, tandai perubahan Anda.

Nama dan tahun di atas hanya memakai informasi yang terverifikasi (konfigurasi Git repositori ini; tanggal dokumen). Tidak ada pemegang hak cipta atau tahun lain yang diklaim; surel tidak dicantumkan.

**Berkas `LICENSE`** memuat teks resmi CC BY 4.0 (legal code) **verbatim** dari `https://creativecommons.org/licenses/by/4.0/legalcode.txt`, diambil **2026-10-15**; teks **tidak** diubah. Ukuran dan SHA-256-nya tercatat di `RELEASE_MANIFEST.md`.

**Efek lisensi (ringkas):** CC BY 4.0 mengizinkan penggunaan, adaptasi, dan redistribusi — termasuk untuk tujuan komersial — asalkan atribusi dipenuhi. Materi diberikan "sebagaimana adanya", tanpa jaminan. Teks resmi pada `LICENSE` adalah satu-satunya acuan.

---

## 2. Materi pihak ketiga — tidak disertakan, lisensi berlaku terpisah

Paket rilis v0.1.0 **tidak memuat satu pun berkas data pihak ketiga**. Berkas CIF (termasuk CIF AlphaFold DB), CSV, JSON, dan manifest dari lingkungan kerja lokal **dikecualikan** dari rilis — daftar dan alasan per butir ada di `docs/PUBLICATION_STATUS.md` §3.

Untuk konteks di masa depan, dua lisensi berikut **tercatat di repositori**:

| Sumber data | Lisensi yang tercatat | Dasar pencatatan | Catatan |
|---|---|---|---|
| AlphaFold DB (mis. `AF-P02185-F1`) | **CC BY 4.0** | Blok lisensi di dalam berkas CIF itu sendiri + hak cipta DeepMind; tercatat pada `K-7` (`docs/OWNER_DECISIONS.md`) | Jika kelak diredistribusi, wajib atribusi |
| RCSB PDB (mis. 1BZ6/1BZP/1BZR) | **CC0 1.0** | Tercatat pada `K-7` (`docs/OWNER_DECISIONS.md`) | Berkas CIF RCSB di lingkungan kerja **tidak** memuat blok lisensi di dalam berkas |

> [!IMPORTANT]
> `docs/DATA_AND_BIAS_POLICY.md` §4 mewajibkan **verifikasi ulang ke sumber resmi langsung** sebelum publikasi eksternal. Verifikasi ulang itu **belum dijalankan** dan **baru relevan** bila kelak ada materi pihak ketiga yang benar-benar disertakan. Lisensi CC BY 4.0 untuk dokumentasi repositori **tidak** mengubah status lisensi pihak ketiga.

---

## 3. Aset visual — tidak tercakup, tidak disertakan

Aset gambar/visual apa pun dalam repositori ini **tidak** tercakup lisensi dokumentasi (CC BY 4.0) dan **tidak** disertakan pada paket rilis v0.1.0 — sumber/lisensinya belum terdokumentasi di repositori. Aset tersebut **tidak** dinyatakan bebas lisensi dan tidak diberi lisensi apa pun oleh dokumen ini. Daftar pengecualian paket ada di `docs/PUBLICATION_STATUS.md` §3. Berkas sumber di repositori **tidak** dihapus dan **tidak** dipindah.

---

## 4. Pustaka & perangkat lunak pihak ketiga

- `docs/LITERATURE_REVIEW.md` mencantumkan 40 sumber dengan pengenal (DOI/PMID) — **tidak ada** teks berhak cipta yang direproduksi di repositori ini.
- Nama perangkat lunak/pihak ketiga yang disebut (mis. AlphaFold, Biopython, RCSB PDB, Google Colab) adalah **penanda identitas** — tidak ada kode pihak ketiga yang disertakan, dan tidak ada perangkat lunak yang dipasang atau dijalankan selama penyusunan paket rilis.
- Satu-satunya teks pihak ketiga yang disertakan adalah **teks lisensi CC BY 4.0 itu sendiri** (`LICENSE`) — disalin verbatim dari sumber resmi Creative Commons (lihat §1).

---

## 5. Jika Anda menggunakan materi ini

Saat menyalin, mengubah, atau mendistribusikan ulang dokumentasi proyek ini: cantumkan atribusi seperti pada §1, sertakan tautan lisensi, dan tandai perubahan bila ada. Atribusi yang benar adalah syarat CC BY 4.0 §3(a).
