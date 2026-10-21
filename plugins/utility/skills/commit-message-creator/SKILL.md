---
name: commit-message-creator
description: "Buat pesan commit dari perubahan kode di source control (git). Gunakan ketika kamu selesai mengerjakan perubahan dan ingin commit, atau ketika diminta 'buatkan commit message', 'tulis pesan commit', atau 'commitkan perubahan ini'. Skill ini membaca semua perubahan (staged maupun unstaged) lalu merangkumnya menjadi pesan commit bergaya software engineer (Conventional Commits) — tanpa menjalankan git commit kecuali diminta."
argument-hint: "[opsional: fokus atau catatan perubahan]"
tools: Read, Bash
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.0.0"
---

# Commit Message Creator

Membaca perubahan di source control dan menyusun pesan commit yang rapi dan
informatif, seperti yang ditulis software engineer berpengalaman.

## Langkah kerja

1. **Baca state git**: `git status`, lalu `git diff --staged` (yang akan
   di-commit) dan/atau `git diff` (belum staged). Bila keduanya kosong, beri
   tahu pengguna belum ada perubahan.
2. **Pahami perubahan**: kelompokkan per file/fitur — apa yang ditambah, diubah,
   dihapus, atau di-refactor; apakah ada breaking change.
3. **Susun pesan** dengan format Conventional Commits:

```
<type>(<scope>): <ringkasan imperative>

<body — jelaskan APA yang berubah dan MENGAPA, bukan sekadar apa>

[BREAKING CHANGE: ...]   ← hanya bila ada
```

4. **Jangan jalankan `git commit`** kecuali pengguna minta. Cukup tampilkan
   pesannya, siap disalin.

## Aturan penulisan

- **Type**: `feat` (fitur baru), `fix` (perbaikan bug), `refactor`, `perf`,
  `docs`, `style`, `test`, `build`, `ci`, `chore`.
- **Ringkasan**: imperative mood ("add", "fix", "refactor"), bukan past tense,
  ≤ 72 karakter, tanpa titik di akhir.
- **Scope** opsional: nama modul/fitur yang terdampak.
- **Body**: kalimat pendek, jelaskan konteks dan alasan keputusan teknis.
- **Pisahkan perubahan besar** menjadi beberapa commit bila logikanya berbeda.
- Gunakan bahasa yang konsisten dengan repo (biasanya Inggris).

## Contoh

```
feat(auth): add refresh token rotation

Putar refresh token tiap refresh untuk cegah replay. Tambah kolom
token_version, invalidate sesi lama saat rotasi.

BREAKING CHANGE: endpoint /auth/refresh kini wajib kirim token lama.
```

Untuk commit yang kubuat sendiri lewat Claude Code, tambahkan baris trailer
`Co-Authored-By: Claude <noreply@anthropic.com>`.
