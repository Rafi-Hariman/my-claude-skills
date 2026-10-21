---
name: skill-forge
description: "Buat, kategorikan, dan daftarkan skill baru ke dalam koleksi skill pribadi (my-claude-skills). Gunakan ketika kamu ingin menambah skill baru — entah sudah punya ide/prompt sendiri lalu mau menyimpannya ke kategori yang tepat, atau mau brainstorming kebutuhan skill (termasuk mencari skill open-source di GitHub/internet untuk di-vendor). Helper ini menentukan kategori/plugin yang tepat, memberi nama, dan menulis SKILL.md sesuai konvensi koleksi."
argument-hint: "[ide skill / paste prompt skill kamu / kebutuhan skill]"
tools: Read, Write, Glob, Grep, Bash, WebSearch, WebFetch
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.0.0"
---

# Skill Forge

Helper untuk membuat dan mengelola skill dalam koleksi pribadi `my-claude-skills`.
Tujuannya: setiap skill baru masuk ke **kategori yang tepat**, dengan **nama yang
benar**, dan **frontmatter yang konsisten** — tanpa harus mengingat aturannya.

## Prasyarat: tentukan path repo sumber

Skill baru harus ditulis ke **repo sumber** (clone git lokal), BUKAN ke plugin
cache yang ter-install. Lakukan urutan ini untuk menemukan path-nya:

1. Cek `~/Development/freelance/my-claude-skills` (default).
2. Kalau tidak ada, cari dengan `Glob`/`find` di `~/Development` untuk folder
   bernama `my-claude-skills` yang berisi `.claude-plugin/marketplace.json`.
3. Kalau tetap tidak ketemu, **tanya pengguna** path repo-nya dan simpan untuk
   sesi ini.

Simpan path itu sebagai variabel `REPO` untuk dipakai sepanjang tugas.

## Langkah 1 — Baca TAXONOMY.md

Selalu baca `$REPO/TAXONOMY.md` dulu. Ini sumber kebenaran kategori, aturan
penamaan, dan template frontmatter. Jangan menebak kategori.

## Langkah 2 — Kenali mode

Ada dua mode, ditentukan dari input pengguna:

### Mode A — Tambah manual

Pengguna sudah punya ide/prompt skill dan mau menyimpannya.

1. **Pahami** apa yang skill itu lakukan, dari ide/prompt yang dipaste.
2. **Tentukan kategori** dari `TAXONOMY.md` (output/domain skill).
   - Kalau jelas → langsung lanjut.
   - Kalau ambigu → tanya pengguna maksimal 1–2 pertanyaan singkat dengan
     opsi kategori yang mungkin.
3. **Beri nama** (kebab-case, huruf kecil, menggambarkan aksi/hasil).
   - Konfirmasi nama ke pengguna kalau tidak yakin.
4. **Tulis SKILL.md** ke `$REPO/plugins/<kategori>/skills/<nama>/SKILL.md`
   menggunakan template di bawah. Deskripsi harus berisi **kata kunci pemicu
   natural** yang biasa dipakai pengguna.
5. **Laporkan** hasilnya: kategori, nama, path file, dan perintah lanjutan
   (`git add/commit/push` + `claude plugin update <kategori>@my-claude-skills`).

### Mode B — Brainstorm / riset

Pengguna belum punya skill, tapi punya kebutuhan ("aku butuh skill buat X").

1. **Cari** apakah sudah ada skill bagus di GitHub/internet (WebSearch/WebFetch).
2. **Rekomendasikan** salah satu:
   - **Vendor** — copy skill orang lain. WAJIB: simpan file lisensi asli di
     samping `SKILL.md`, cantumkan author asli di frontmatter, jangan hapus
     copyright notice. Lalu lanjutkan Langkah Mode A (kategori + nama + tulis).
   - **Build sendiri** — kalau tidak ada yang pas, tulis dari nol (Mode A).
3. Kalau ragu antara vendor vs build, tanya pengguna dengan ringkasan trade-off.

## Template SKILL.md

```markdown
---
name: <nama-skill>
description: "<satu-dua kalimat. Berisi kata kunci pemicu natural yang akan dipakai pengguna untuk memanggil skill ini. Contoh: 'audit komponen', 'de-slop UI', 'buat desain sistem'."
argument-hint: "[argumen opsional]"
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.0.0"
---

# <Nama Skill>

## Kapan digunakan
<kondisi pemicu>

## Cara kerja
<langkah-langkah utama>
```

Aturan penting:
- `name:` harus **sama persis** dengan nama folder.
- `description:` adalah penentu auto-trigger — tulis kata kunci yang natural,
  bukan jargon internal.
- Isi body ringkas tapi operasional; jangan salin prompt mentah pengguna apa
  adanya — rapikan jadi langkah yang bisa dijalankan.

## Setelah selesai

Selalu akhiri dengan ringkasan:
- Kategori & nama skill yang dibuat.
- Path file.
- Perintah lanjut: commit, push, dan update plugin di device.
