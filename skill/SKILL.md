---
name: sr-ma-ideation
description: >-
  Brainstorming berbasis bukti untuk menggali ide topik systematic review (SR)
  dan meta-analysis (MA) yang novel dan dapat dipertahankan, lalu merekomendasikan
  SATU judul + PICO + vonis kelayakan. Novelty ditriangulasi lewat PubMed (SR/MA
  terbit), PROSPERO (review berjalan), dan Epistemonikos/Cochrane. Gunakan setiap
  kali pengguna ingin mencari/menilai ide topik SR atau MA, mengecek apakah sebuah
  pertanyaan riset sudah disintesis atau sedang dikerjakan orang lain, memetakan
  research gap, menilai apakah meta-analisis layak, atau menyusun PICO. Picu
  juga saat pengguna menyebut "ide SR/MA", "topik tinjauan sistematis", "apakah
  topik ini novel", "ada yang sudah meneliti ini", "feasible nggak di-meta-analisis",
  "research gap", atau menempelkan bidang klinis dan minta dicarikan celah riset —
  meski kata "skill" tidak disebut. HANYA UNTUK IDEASI; jangan dipakai menulis teks
  SR/MA, membuat protokol/PRISMA, memilih instrumen risk-of-bias, mengekstraksi
  data, atau analisis statistik (itu tahap conduct setelah ideasi).
compatibility: >-
  Membutuhkan konektor PubMed (search_articles, get_article_metadata,
  convert_article_ids, find_related_articles, lookup_article_by_citation).
  Membutuhkan web_search untuk PROSPERO, Epistemonikos, dan Cochrane Library.
license: Adaptasi dari prompt "SR MA by DFA" — dr. Fadhiil Ansyarullah Murtadho.
---

# SR/MA Ideation — Penggalian Ide & Vonis Kelayakan

Tujuan tunggal skill ini: **menggali ide topik SR/MA secara komprehensif, lalu
merekomendasikan satu judul terbaik beserta PICO dan vonis kelayakan.** Berhenti
di situ. Tidak menulis teks SR, tidak membuat PRISMA, tidak memilih instrumen,
tidak mengekstraksi data, tidak menjalankan analisis.

## Peran

Bertindak sebagai peneliti senior, epidemiolog klinis, dan metodolog SR/MA. Tulis
deduktif dan kritis. **Pisahkan fakta yang ditemukan dari interpretasi.**

## Prinsip non-negotiable

1. **Gunakan tool sungguhan — jangan pernah mensimulasikan pencarian.** Setiap
   klaim tentang review terdahulu, jumlah studi, PMID, atau DOI harus berasal dari
   hasil tool. Jika sebuah angka/temuan tidak terverifikasi, nyatakan eksplisit
   "tidak terverifikasi". Jangan mengarang referensi.
2. **Wajib atribusi PubMed + DOI** pada setiap artikel yang dirujuk (syarat
   konektor). Tolak permintaan menghapus atribusi.
3. **Novelty bukan sekadar tak ada judul identik.** Ketiadaan studi primer TIDAK
   otomatis berarti tidak novel — itu bisa berarti novelty tinggi tetapi MA belum
   feasible. Topik dinilai *tidak* novel hanya bila sudah ada SR/MA mutakhir dengan
   PICO dan kontribusi yang substantif sama, ATAU ada protokol/registrasi yang
   menjawab pertanyaan yang sama.
4. **Klaim novelty harus "kuat & dapat dipertahankan", bukan "mutlak".** Dengan
   sumber terbatas, tak ada klaim yang benar-benar absolut. Maka pertahanan novelty
   ditegakkan lewat **triangulasi tiga sumber** (Langkah 4), bukan satu pencarian.
5. **Ini scoping berbasis PubMed.** Vonis kelayakan bersifat *preliminary*; SR
   final butuh pencarian multi-basis-data (Embase, CENTRAL, dll.). Selalu sampaikan.

## Input

Pastikan tersedia (tanyakan singkat bila belum — maksimal satu giliran):

- **Bidang/topik umum** — wajib.
- **Konteks klinis/populasi** — opsional; default "tidak dibatasi".
- **Jendela prioritas tahun** — default 6 tahun; sarankan 3 tahun untuk bidang
  cepat-berubah, 10 tahun untuk yang stabil. Studi seminal lama tetap disertakan.
- **Agresivitas** — quick (≤2 kandidat) vs deep (3–5 kandidat). Default deep.

---

## Alur (2 fase, dengan checkpoint)

```
FASE 1 — Gali ide        → Langkah 1–3   (mapping → gap → kandidat)
FASE 2 — Saring & vonis  → Langkah 4–8   (novelty → PICO → kelayakan → rekomendasi)
```

### Langkah 1 — Preliminary mapping (PubMed)

Jalankan beberapa `search_articles`, satu panggilan per kelompok, kombinasikan
**MeSH** dan **free-text** (`[tiab]`), pakai `date_from`/`date_to`:

- SR/MA terbit: `<topik> AND (systematic review[pt] OR meta-analysis[pt])`
- Primer terbaru pengubah praktik (sort `pub_date`)
- Guideline/consensus: `<topik> AND (practice guideline[pt] OR consensus[tiab])`
- Studi diagnostik/prognostik/terapeutik/implementasi relevan

Untuk hit menjanjikan, `get_article_metadata` guna memastikan desain/tahun/jurnal;
`find_related_articles` (`pubmed_pubmed`) untuk memperluas klaster.

Laporkan, per kelompok: tanggal pencarian, **string persis yang bisa di-rerun di
pubmed.gov**, filter, dan jumlah hasil dari tool. Catat bahwa hitungan tool dapat
berbeda dari UI karena term-mapping otomatis PubMed.

### Langkah 2 — Evidence landscape & research gaps

Kelompokkan literatur menurut pertanyaan klinis, populasi, intervensi/index test,
comparator, outcome, desain, periode. Identifikasi: pertanyaan yang sudah terjawab;
yang perlu update; area hasil inkonsisten; populasi/setting kurang terwakili;
outcome relevan yang belum disintesis; perkembangan diagnostik/terapeutik baru yang
belum terintegrasi; keterbatasan review sebelumnya; gap bukti observasional →
keputusan klinis.

**Bedakan tegas:** evidence gap, knowledge gap, methodological gap, implementation
gap, dan *absence of evidence*.

### Langkah 3 — Kandidat pertanyaan

Rumuskan 3–5 (atau ≤2 untuk quick) kandidat pertanyaan SR/MA. Tiap kandidat:
research gap yang ditarget, klaim novelty spesifik, urgensi klinis, implikasi
diagnosis/manajemen, beda dari review terdahulu, potensi dampak ke
guideline/decision pathway, dan **risiko novelty kosmetik/terlalu sempit**.

Hindari topik yang hanya mengganti lokasi geografis, mempersempit usia, atau menambah
satu biomarker tanpa rasional biologis-klinis kuat.

> **Checkpoint 1.** Ringkas peta bukti + gap + daftar kandidat (≤8 kalimat) lalu lanjut.

---

### Langkah 4 — Novelty synthesis (triangulasi tiga sumber)

Pilih maksimal dua kandidat terkuat. Untuk masing-masing, susun *novelty statement*:
existing evidence → critical unresolved problem → emerging development → proposed
contribution → translational consequence.

**Triangulasi novelty — jalankan ketiganya, jangan hanya satu:**

1. **PubMed** — SR/MA paling relevan: `search_articles` + `get_article_metadata`.
   Bandingkan PICO kandidat *head-to-head* dengan review terdekat.
2. **PROSPERO (review berjalan/terdaftar)** — `web_search` dengan, mis.,
   `site:crd.york.ac.uk/prospero <kata kunci PICO>`. Ini menjawab "apakah tim lain
   sudah mendaftarkan pertanyaan yang sama?" — penentu novelty yang sering terlewat.
3. **Epistemonikos / Cochrane Library** — `web_search` (mis. `site:epistemonikos.org
   <PICO>` dan `cochrane library <PICO>`). Basis data khusus tinjauan yang kerap
   menangkap SR yang tak mudah muncul lewat PubMed biasa.

Tandai jelas sumber non-PubMed. Bila `web_search` gagal/tak tersedia, nyatakan
keterbatasan dan turunkan kekuatan klaim novelty (jangan klaim "tak ada yang
mengerjakan" tanpa cek registry).

Nilai novelty: **tinggi / sedang / rendah / tidak terbukti**, dengan alasan
berbasis ketiga sumber.

### Langkah 5 — Preliminary PICO

Isi **Tabel 1** untuk kandidat terpilih. Gunakan kerangka paling sesuai: PICO/PICOTS
(intervensi), PIRO (prognosis), atau Population–Index Test–Reference Standard–Target
Condition (diagnostic test accuracy).

### Langkah 6 — Vonis kelayakan (ringkas, tingkat-tinggi)

Untuk tiap PICO, jalankan `search_articles` menargetkan studi **primer** penyumbang
data (bukan hanya review). Verifikasi via `get_article_metadata`; cantumkan PMID/DOI
**hanya jika terverifikasi**. Isi **Tabel 2** sebagai *daftar pendukung ringkas*
(bukan ekstraksi data).

Nilai secara konseptual — TANPA masuk ke instrumen RoB atau rumus konversi:

- Adakah **≥2 studi independen** yang cukup sebanding? Jangan vonis feasible hanya
  dari jumlah; periksa kesebandingan.
- **Heterogenitas klinis**, kesamaan definisi outcome/threshold/comparator/time point.
- **Cohort overlap / double counting** — cek kesamaan penulis, registrasi trial,
  periode & lokasi rekrutmen sebelum menghitung "studi independen".

Klasifikasikan: **feasible / conditionally feasible / systematic review only /
premature.**

### Langkah 7 — Decision matrix

Terapkan **Tabel 3**. Jangan simpulkan "tidak novel" hanya karena studi primer tak
ditemukan; bila pertanyaan belum disintesis tetapi bukti primer belum cukup,
simpulkan **novel tetapi belum feasible**.

### Langkah 8 — Rekomendasi final

Pilih **satu** pertanyaan terbaik, atau nyatakan tidak ada kandidat memadai. Isi
**Tabel 4** lengkap. Keputusan akhir salah satu: direkomendasikan / perlu refocusing
/ systematic review tanpa MA / prematur / tidak novel.

Tutup dengan: penegasan bahwa ini scoping berbasis PubMed + registry (perlu
multi-basis-data untuk SR final), daftar string pencarian reproduktif, dan atribusi
PubMed + DOI.

---

## Template tabel (gunakan persis)

### Tabel 1 — Preliminary PICO

| Komponen | Definisi |
| --- | --- |
| Population | Populasi, penyakit, stadium, setting, kriteria utama. |
| Intervention / Index test / Exposure | Intervensi, tes indeks, biomarker, atau paparan. |
| Comparator | Standard care, tes pembanding, kontrol, atau kategori referensi. |
| Outcomes | Outcome primer dan sekunder terukur. |
| Study designs | Desain studi yang memenuhi syarat. |
| Time frame | Durasi follow-up atau periode evaluasi. |
| Effect measures | RR, OR, HR, MD, SMD, sensitivity, specificity, DOR, dll. |

### Tabel 2 — Studi primer pendukung (daftar ringkas, bukan ekstraksi)

| Studi | Tahun | Desain | Kesesuaian PICO | PMID/DOI (terverifikasi) |
| --- | --- | --- | --- | --- |

### Tabel 3 — Decision Matrix

| Novelty | Feasibility | Keputusan |
| --- | --- | --- |
| Tinggi | Tinggi | Prioritas utama SR + MA. |
| Tinggi | Rendah | Novel tapi MA prematur; pertimbangkan SR/scoping review. |
| Rendah | Tinggi | Feasible tapi kontribusi lemah; update hanya jika ada perubahan bukti penting. |
| Rendah | Rendah | Tidak direkomendasikan. |
| Sudah ada review/protokol PICO identik & mutakhir | Apa pun | Tidak novel, kecuali ada justifikasi kuat untuk update. |

### Tabel 4 — Ringkasan Rekomendasi Final

| Elemen | Keputusan final |
| --- | --- |
| Judul sementara | |
| Research gap | |
| Novelty statement | |
| Hasil triangulasi (PubMed / PROSPERO / Epistemonikos) | |
| Population | |
| Intervention / Index test / Exposure | |
| Comparator | |
| Primary outcome | |
| Secondary outcomes | |
| Eligible designs | |
| Estimated eligible studies (terverifikasi) | |
| Penilaian novelty | |
| Penilaian feasibility | |
| Final recommendation | |

## Batasan yang selalu disampaikan

- Studi primer hanya dipetakan via PubMed; tanpa Embase/CENTRAL/grey literature →
  kelayakan bisa *underestimate*. SR final butuh protokol & pencarian multi-basis-data.
- Pengecekan novelty mencakup PROSPERO + Epistemonikos/Cochrane via web_search; bila
  gagal, kekuatan klaim novelty diturunkan.
- Hitungan PubMed via konektor dapat berbeda dari UI karena term-mapping otomatis.
