# TAXONOMY — Kategori Skill

Ini adalah **sumber kebenaran** untuk mengelompokkan skill. Helper `skill-forge`
(`/meta:skill-forge`) dan siapa pun yang menambah skill WAJIB membaca file ini
dulu sebelum memutuskan kategori. Jangan menebak — kalau ragu, tanya pengguna.

## Cara memakai

Setiap skill baru harus masuk ke **satu** plugin kategori di bawah. Pilih
berdasarkan *output* dan *domain* skill, bukan nama penulisnya.

## Daftar kategori

### ui-ux
Desain visual, styling, dan pengalaman pengguna.
- Ciri: output-nya tampilan, desain, warna, tipografi, layout, branding, aset visual.
- Contoh: design, design-system, ui-styling, ui-ux-pro-max, brand, banner-design,
  mobile-app-ui-design, slides, avoid-ai-design.

### slicing
Mengubah desain/gambar menjadi kode komponen.
- Ciri: input berupa screenshot/gambar desain, output berupa kode UI.
- Contoh: sliceui, sliceui-handoff.

### frontend
Engineering sisi klien (bukan desain visual, tapi struktur kode).
- Ciri: audit/refactor komponen, pola **pemakaian API dari sisi klien**
  (service, HTTP client), framework (Angular/React/Vue).
- Contoh: ng-component-audit-doc, service-pattern-handoff, bill-api-usage.

### backend
Skill tentang sisi server dari sebuah API.
- Ciri: membuat/mendefinisikan endpoint, controller, autentikasi server,
  database, logic di sisi server.
- Pembeda penting: pola **memakai** API dari kode klien (service, HTTP client)
  itu frontend — yang backend adalah API-nya sendiri.
- Status: kosong untuk sekarang, menunggu skill pertama.

### analysis
Analisis kode, QA, pengambilan keputusan, dan penulisan teks.
- Ciri: output berupa penilaian, laporan, keputusan, atau teks yang diperbaiki.
- Contoh: sonarqube, llm-council, humanizer.

### utility
Tooling umum dan konversi/transformasi file.
- Ciri: tugas teknis lintas-domain yang tidak spesifik ke satu stack.
- Contoh: markitdown.

### meta
Skill yang mengelola skill lain.
- Ciri: tugasnya tentang membuat/mengatur/menggolongkan skill.
- Contoh: skill-forge.

## Konvensi penamaan skill

- **kebab-case**, huruf kecil, dipisah tanda hubung: `angular-unit-test-gen`.
- Nama harus menggambarkan *aksi* atau *hasil*, bukan nama orang.
- Field `name:` di frontmatter SKILL.md HARUS sama dengan nama folder.
- Deskripsi harus berisi kata kunci pemicu yang natural (lihat template skill-forge).

## Frontmatter minimal SKILL.md

```markdown
---
name: <nama-skill>
description: "<satu-dua kalimat, berisi kata kunci pemicu natural>"
argument-hint: "[argumen opsional]"
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.0.0"
---
```
