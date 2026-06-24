# SR/MA Ideation — Claude Skill

Sebuah [Claude Skill](https://docs.claude.com) untuk **menggali ide topik *systematic review* (SR) dan *meta-analysis* (MA)** yang novel dan dapat dipertahankan, lalu merekomendasikan **satu judul + PICO + vonis kelayakan**. Klaim *novelty* ditriangulasi lewat **PubMed**, **PROSPERO**, dan **Epistemonikos/Cochrane**.

> Skill ini **hanya untuk ideasi**. Ia tidak menulis manuskrip SR/MA, tidak membuat protokol/PRISMA, tidak memilih instrumen *risk-of-bias*, dan tidak menjalankan analisis statistik — itu tahap *conduct* setelah ideasi.

Diadaptasi dari prompt **"SR MA by DFA"** karya dr. Fadhiil Ansyarullah Murtadho ([@Fadhiilansyarullahmtdho](https://github.com/Fadhiilansyarullahmtdho)).

---

## Apa yang dilakukan skill ini

Mengikuti alur dua fase yang ringkas:

| Fase | Langkah | Keluaran |
| --- | --- | --- |
| **1 — Gali ide** | Preliminary mapping → evidence landscape & gaps → 3–5 kandidat pertanyaan | Peta bukti + daftar kandidat |
| **2 — Saring & vonis** | Triangulasi novelty → PICO → vonis kelayakan → decision matrix → rekomendasi | Satu judul + PICO + feasibility |

Prinsip inti yang ditegakkan:

- **Tidak pernah mensimulasikan pencarian.** Setiap klaim faktual (jumlah studi, PMID, DOI) berasal dari hasil tool PubMed; bila tidak terverifikasi, dinyatakan eksplisit.
- **Novelty bukan sekadar tak ada judul identik.** Topik dinilai *tidak* novel hanya bila sudah ada SR/MA mutakhir berPICO sama, atau ada protokol terdaftar yang menjawab pertanyaan yang sama.
- **Klaim "kuat & dapat dipertahankan", bukan "mutlak".** Triangulasi tiga sumber (PubMed + PROSPERO + Epistemonikos/Cochrane) menggantikan satu pencarian.
- **Scoping berbasis PubMed.** Vonis kelayakan bersifat awal; SR final tetap butuh pencarian multi-basis-data.

## Prasyarat

- **Konektor PubMed** aktif di Claude (`search_articles`, `get_article_metadata`, `convert_article_ids`, `find_related_articles`, `lookup_article_by_citation`).
- **web_search** untuk pengecekan PROSPERO, Epistemonikos, dan Cochrane Library.

## Instalasi

### Opsi A — pasang file `.skill` (paling cepat)

Unduh [`sr-ma-ideation.skill`](./sr-ma-ideation.skill) dari repo ini, lalu unggah/pasang melalui pengaturan Skills di Claude.

### Opsi B — dari sumber

Folder [`skill/`](./skill) berisi `SKILL.md`. Kemas ulang dengan skill-creator (`python -m scripts.package_skill skill/`) atau pasang langsung sesuai metode instalasi Skills yang berlaku.

## Cara pakai

Cukup minta dalam bahasa natural, mis.:

> "Carikan ide topik systematic review/meta-analysis di bidang **genetika leukemia anak**."

Claude akan menjalankan pemetaan PubMed, triangulasi novelty, lalu mengembalikan judul + PICO + vonis kelayakan beserta tabel ringkasan.

## Contoh hasil

Lihat [`examples/genetika-leukemia-anak.md`](./examples/genetika-leukemia-anak.md) untuk satu run penuh. Ringkasnya: skill menolak mengecap "novel" pertanyaan *NUDT15 → toksisitas 6-MP pada ALL* (sudah ada MA Du dkk. 2024 + registrasi PROSPERO), lalu mengarahkan ke celah *clinical utility* yang masih terbuka sambil jujur menyatakan keterbatasan feasibility.

## Struktur repo

```
.
├── README.md
├── LICENSE
├── CHANGELOG.md
├── sr-ma-ideation.skill        # paket siap pasang
├── skill/
│   └── SKILL.md                # sumber skill
└── examples/
    └── genetika-leukemia-anak.md
```

## Batasan

Hanya PubMed untuk studi primer (tanpa Embase/CENTRAL/grey literature → feasibility bisa *underestimate*); pengecekan novelty bergantung pada `web_search` yang berfungsi; hitungan PubMed via konektor dapat berbeda dari UI karena *automatic term mapping*.

## Atribusi & lisensi

Diadaptasi dari prompt "SR MA by DFA" oleh dr. Fadhiil Ansyarullah Murtadho. Lihat [LICENSE](./LICENSE). Pencarian literatur menggunakan data dari **PubMed**; setiap artikel yang dirujuk wajib disertai tautan DOI.

## Kontribusi

Issue dan pull request dipersilakan — terutama penambahan kasus contoh di `examples/` dan penyempurnaan string pencarian.
