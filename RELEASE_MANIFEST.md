# RELEASE MANIFEST — v0.1.0

| | |
|---|---|
| **Paket** | `alphafold-release-v0.1.0` |
| **Tanggal penyusunan** | 2026-10-15 (dibangun ulang setelah keputusan `A-4`) |
| **Sumber** | Salinan publikasi dari paket rilis `D:\project\alphafold-release-v0.1.0`; `README.md`, `CHANGELOG.md`, dan `CITATION.cff` diperbarui 2026-10-15 (teks status publikasi) sehingga tidak lagi byte-identik dengan sumber audit |
| **Produk** | Repositori protokol/metodologi — **tanpa** hasil empiris, **tanpa** artefak data |
| **Lisensi** | **CC BY 4.0** untuk dokumentasi asli (keputusan `A-4`); aset gambar & materi pihak ketiga **tidak** tercakup |
| **Status** | **Diterbitkan 2026-10-15** sebagai repositori dokumentasi publik `alphafold-reliability-audit-docs` (visibilitas publik); repositori audit utama tetap privat dan tidak diubah |
| **Jumlah berkas** | 19 (18 berkas isi + manifest ini) |
| **Ukuran 18 berkas isi** | 357.671 byte |

> **Catatan self-hash:** manifest ini **tidak dapat** memuat SHA-256 miliknya sendiri. Hash manifest dihitung setelah penulisan dan dilaporkan di luar berkas ini. Untuk memverifikasi paket: hitung ulang hash 18 berkas di bawah, lalu bandingkan.

---

## 1. Isi paket (18 berkas, SHA-256 + ukuran)

| # | Berkas | Byte | SHA-256 |
|---|---|---|---|
| 1 | `README.md` | 19.396 | `7787fb5c8f18f54f82da00437c65f685c9470c30751f3e4464348d41159ec6ad` |
| 2 | `LICENSE` | 18.657 | `9ba9550ad48438d0836ddab3da480b3b69ffa0aac7b7878b5a0039e7ab429411` |
| 3 | `ATTRIBUTION.md` | 4.739 | `b944bc397131d78fb22c193f0076b3d5f87f987b21599021416362742ba97cd7` |
| 4 | `CHANGELOG.md` | 4.699 | `24ea26ea8f87d3b873a1f3305a78e30adf5ebf0224d4bdea17e1bab891208c4c` |
| 5 | `CITATION.cff` | 688 | `5c0b41b4be8a3dfb457ac6f6e32f968ef113fdccf5363d07d3a7fde1846981ce` |
| 6 | `.gitignore` | 1.356 | `a3e724e956e6a2a9defdf3c6751633f63b7b50c0ac0aa1629942de7bc643ed85` |
| 7 | `.gitattributes` | 161 | `abf18ae443571da124f45e4354119f3763e5fade495141dfae5319590b762ecb` |
| 8 | `docs/RESEARCH_PROTOCOL_V1.md` | 27.147 | `71e665e64697c269eef80437b243409ccff5f5d07b6db3df5ec8da0324ac1536` |
| 9 | `docs/DATA_AND_BIAS_POLICY.md` | 12.952 | `fa08e40c4817748770ad353b460e5356fe5ce90773dd16d6e74c864827320b6f` |
| 10 | `docs/LITERATURE_REVIEW.md` | 41.430 | `471b24cc3c2edf3482dc4a55a2484f12db42efb383aea55ffb7279a67e5fd069` |
| 11 | `docs/PHASE1_BASELINE_AUDIT.md` | 19.145 | `da5adfb9087be790f97b39115c63b0fc198b164529dbb5bac61467b576fa0430` |
| 12 | `docs/PHASE2_REVIEW_GATE.md` | 52.653 | `a0cba1f8578d2ea81bf3b937e5a05138c421b5d9b591d8a653d0e35bc66a74f7` |
| 13 | `docs/PHASE2_DECISION.md` | 13.581 | `bd0a4deac66637625f77877610275f6eb68841f8766ad185ca3888ea3cc4d9b2` |
| 14 | `docs/PHASE2D_DECISION_PACKAGE.md` | 37.462 | `54fd3369dcd685e6b7c62042bb55b6f94f44e267fafa23d3976bf1bc1e90ea3f` |
| 15 | `docs/TOOLS_LOCK.md` | 15.371 | `8bb3b6f2740ba9329fee765f1b76513eb45d4535fa8f653973de6ab86da4a428` |
| 16 | `docs/PROTOCOL_AMENDMENTS.md` | 41.842 | `e6a8db38d4136ac83d102fb0f0aa47eae87b7c5126fbf2433f294d4f85906a69` |
| 17 | `docs/OWNER_DECISIONS.md` | 37.224 | `f554246ed50e118aac0126e230c4ab72c7c073d3770e5b540221043acfee69f5` |
| 18 | `docs/PUBLICATION_STATUS.md` | 9.168 | `0362ff326939da6f2c525e4bc6375a6d03fd67c784d30fb4b5cba0fd8aa180ce` |

**Lima dokumen beku** (baris 8–12 — `RESEARCH_PROTOCOL_V1`, `DATA_AND_BIAS_POLICY`, `LITERATURE_REVIEW`, `PHASE1_BASELINE_AUDIT`, `PHASE2_REVIEW_GATE`): hash **identik** dengan nilai pembekuan yang tercatat — tidak tersentuh.

**Provenance `LICENSE`:** teks resmi *Creative Commons Attribution 4.0 International Public License* (legal code, bahasa Inggris), disalin **verbatim** dari `https://creativecommons.org/licenses/by/4.0/legalcode.txt`, diambil **2026-10-15**; tidak diubah (396 baris, LF, tanpa BOM). SHA-256 tercatat di tabel di atas.

## 2. Dikecualikan (ringkas)

| Item | Alasan ringkas |
|---|---|
| `artifacts/colab-2026-10-15/**` (34 berkas, 2.997.815 B) | Provenance belum terverifikasi; eksekusi ditetapkan tidak sesuai §8.9 (`A-2`); versi tool/parameter tidak tercatat; label `verified`/`independent` tanpa bukti |
| `image/hero-banner.png` (2.048.346 B) | Sumber/lisensi **tidak terdokumentasi** → dikeluarkan dari paket; berkas sumber di repositori **tidak** dihapus/dipindah dan **tidak** dinyatakan bebas lisensi |
| `image/Protein Structure Analysis Dashboard.png` (1.682.297 B) | Sumber/lisensi tidak terdokumentasi |
| `Downloads/**` (3.744 berkas) | Data lokal pemilik — bukan bagian proyek |
| `docs/C2_OPERATIONAL_SPEC_DRAFT.md` (15.414 B) | Draft; menunggu keputusan pemilik O1–O6 |

Daftar lengkap + alasan per butir: `docs/PUBLICATION_STATUS.md` §3. **Tidak ada berkas yang dihapus/dipindah**; **tidak ada** berkas data, CIF/CSV/JSON, `Downloads/`, `artifacts/`, atau gambar apa pun di paket ini.

## 3. Pemeriksaan yang dijalankan saat penyusunan

| Pemeriksaan | Hasil |
|---|---|
| SHA-256 setiap berkas rilis vs sumber (18 berkas) | **18/18 identik, 0 mismatch** |
| Line-ending berkas teks | **LF murni** (CR = 0 di semua berkas, termasuk `LICENSE`); byte terakhir `0a`; tanpa BOM |
| Ekstensi terlarang di pohon rilis (`.cif/.csv/.json/.tsv/.zip/.gz/.tar/.log/.pdb/.fasta/.png/.jpg/.webp`) | **0 ditemukan**; tidak ada direktori `image/` |
| Gambar hero pada paket | **Tidak ada** (dikeluarkan); referensi di `README.md` | **0 ditemukan** (`grep -i "hero\|banner\|image/\|\.png"`) |
| Tautan lokal markdown | 1 tautan berkas (`ATTRIBUTION.md`) — **ada**; tautan eksternal (badge, lisensi CC, Keep a Changelog) tidak difetch |
| Anchor TOC README | 15 anchor ↔ 15 heading — **0 tak cocok** |
| `CITATION.cff` | Validasi struktural (parser YAML-subset, stdlib): 9 kunci top-level; `cff-version: 1.2.0`; `type: misc` (enum CFF 1.2.0); `license: CC-BY-4.0` (SPDX); 1 author dengan `family-names`/`given-names`; tanpa TAB/BOM/CR; **0 masalah** |
| Daftar dokumen beku sebelum/sesudah | **identik** (hash di §1 baris 8–12) |
| `git status` repositori sumber | 5 `M` (`.gitattributes`, `.gitignore`, `README.md`, `docs/OWNER_DECISIONS.md`, `docs/PROTOCOL_AMENDMENTS.md`) + 5 `??` (`ATTRIBUTION.md`, `CHANGELOG.md`, `CITATION.cff`, `LICENSE`, `docs/PUBLICATION_STATUS.md`) — semua disengaja |
| Berkas ter-stage | **0** (`git diff --cached` kosong) |
| Aturan `.gitignore` (`git check-ignore -v`) | 4/4 berlaku (`Downloads/`, `artifacts/`, draft C-2, aset dashboard) |
| `artifacts/` utuh | 34 berkas / 2.997.815 byte — tidak berubah |
| `Downloads/` utuh | 3.744 berkas; ZIP `…T055342Z.zip` tetap `1636049ce1b8c8e0094320dfad623dd1adadb1aeeaeccc573d72998890f5bc0b` |

## 4. Blocker sebelum publikasi eksternal

1. Aset gambar (`image/`) — sudah dikeluarkan dari paket; konfirmasi asal hanya diperlukan bila kelak ingin disertakan kembali.
2. Verifikasi ulang lisensi pihak ketiga ke sumber resmi — wajib hanya bila kelak ada materi pihak ketiga yang disertakan (paket ini tidak memuatnya).
3. Keputusan pemilik untuk menerbitkan — **selesai 2026-10-15**: dibuat repositori dokumentasi publik `alphafold-reliability-audit-docs`; repositori audit utama tetap privat.
4. Sikap resmi bila kelak hasil studi kasus ingin diterbitkan (hambatan §8.9/`A-2`).
5. Cakupan `K-7` lainnya (atribusi output riset; lisensi tool `UNVERIFIED`).

Rincian: `docs/PUBLICATION_STATUS.md` §4.

## 5. Cara memverifikasi paket ini

```bash
# dari direktori rilis — hitung ulang dan bandingkan dengan tabel §1
sha256sum --text README.md LICENSE ATTRIBUTION.md CHANGELOG.md CITATION.cff .gitignore .gitattributes docs/*.md
# ekspektasi: 18 baris; semua hash cocok dengan §1
```
