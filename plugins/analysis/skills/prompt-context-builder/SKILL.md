---
name: prompt-context-builder
description: "Mencerna permintaan yang kurang jelas (curhatan/ide mentah) dan mengembalikannya sebagai prompt/konteks yang terstruktur dan akurat. Gunakan ketika kamu ingin memastikan maksudmu ke Claude tidak ambigu — misal sebelum mengerjakan tugas kompleks, atau ketika meminta beberapa hal (a, b, c) sekaligus. Skill ini mengklarifikasi tujuan, ruang lingkup, batasan, dan kriteria hasil, lalu menyusunnya kembali menjadi prompt siap pakai."
argument-hint: "[permintaan / curhatan yang ingin dirapikan]"
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.0.0"
---

# Prompt / Context Builder

Mengubah permintaan mentah menjadi prompt yang jelas, terstruktur, dan bebas
ambigu — sehingga eksekusi oleh Claude lebih tepat sasaran.

## Langkah kerja

1. **Baca** permintaan mentah pengguna secara utuh.
2. **Ekstrak** unsur-unsurnya:
   - **Tujuan** — apa hasil akhir yang diinginkan.
   - **Konteks** — latar belakang teknis/proyek yang relevan.
   - **Ruang lingkup** — apa yang termasuk dan *tidak* termasuk.
   - **Batasan** — teknologi, gaya, aturan, batas waktu.
   - **Kriteria selesai** — bagaimana tahu ini dianggap berhasil.
   - **Ambiguitas** — hal yang masih bisa ditafsir ganda.
3. **Klarifikasi bila perlu**: hanya bila ambiguitasnya kritis, tanya maksimal
   2–3 pertanyaan singkat (bisa pilihan ganda). Jangan berlebihan.
4. **Susun ulang** menjadi prompt terstruktur dan kembalikan ke pengguna.

## Format output

```markdown
## Tujuan
<kalimat jelas, satu tujuan utama>

## Konteks
<latar belakang yang relevan>

## Ruang lingkup
- Termasuk: ...
- Tidak termasuk: ...

## Batasan
- ...

## Output yang diharapkan
<konkret, terukur, bisa diverifikasi>

## Pertanyaan terbuka (jika ada)
1. ...
```

## Aturan

- **Jaga maksud asli** — jangan menambah/mengubah keinginan pengguna, hanya
  memperjelas dan menyusun.
- **Ringkas** — prompt yang dihasilkan harus lebih mudah dibaca, bukan lebih
  panjang dari curhatannya.
- Setelah menyusun, tawarkan untuk **langsung lanjut mengerjakan** berdasarkan
  prompt yang sudah dirapikan.
