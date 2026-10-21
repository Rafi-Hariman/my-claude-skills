---
name: typescript-pro
description: "Standar dan panduan TypeScript tingkat lanjut agar kode aman tipe (type-safe). Gunakan ketika menulis, me-review, atau memperbaiki kode TypeScript di project frontend — menegakkan strict typing, menghindari any, memakai generics, utility types, discriminated unions, dan type narrowing. Cocok untuk menetapkan standar typing ketika mengerjakan project Angular/React/Vue yang memakai TS."
argument-hint: "[file TS / kode yang mau di-review / tipe yang mau diperbaiki]"
tools: Read, Grep, Glob
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.0.0"
---

# TypeScript Pro

Standar typing untuk kode TypeScript yang aman tipe dan mudah dipelihara. Ikuti
aturan ini sebagai "linting mental" setiap kali menulis atau me-review TS.

## Prinsip inti

1. **Aktifkan strict mode** (`strict: true`) dan jangan dilonggarkan. Target
   `strictNullChecks`, `noImplicitAny`, `noUncheckedIndexedAccess` menyala.
2. **`any` adalah kegagalan.** Cari penggantinya: `unknown` + narrowing, generics,
   union type, atau interface yang tepat. `any` hanya dibolehkan jika ada
   justifikasi eksplisit dan ditandai komentar.
3. **Lebih baik `unknown` daripada `any`** untuk input dari luar (API, event,
   parse JSON) — paksa validasi sebelum dipakai.

## Aturan menulis tipe

- **Gunakan `interface`** untuk bentuk objek publik, `type` untuk union/alias.
- **Discriminated union** untuk state/action: beri properti pembeda (`kind`,
  `status`, `type`) lalu narrowing via `switch`/`if`.
- **Type narrowing**: pakai `typeof`, `instanceof`, `in`, custom type guard
  (`x is T`) — jangan asal pakai `as`.
- **Hindari type assertion (`as`)** kecuali benar-benar perlu; prioritaskan
  `satisfies` untuk validasi literal tanpa mengubah tipe.
- **Generics** untuk fungsi/util yang generic — jangan duplikasi overload manual.
- **Utility types** (`Partial`, `Pick`, `Omit`, `Record`, `Readonly`,
  `ReturnType`, `Awaited`) sebelum bikin tipe manual.
- **`readonly`** untuk properti yang tak boleh berubah; `as const` untuk literal
  yang harus sempit.
- **Jangan `enum`** bila `const` union atau `as const` sudah cukup (hindari
  kode runtime ekstra).
- **Hindari `any` di parameter/return** fungsi publik; eksplisitkan tipe return.

## Siklus review

1. Cari `any`, `as any`, `@ts-ignore`, `// @ts-expect-error` yang tak perlu.
2. Cek apakah tipe bisa disempitkan (union besar → discriminated union).
3. Cek duplikasi tipe yang bisa dipakai ulang via utility type / generic.
4. Pastikan `unknown` yang datang dari luar divalidasi dulu, bukan di-cast.
5. Usulkan perbaikan dengan **sebelum/sesudah** kode, bukan sekadar kritik.

## Output

- Beri contoh kode **sebelum → sesudah** yang konkret.
- Sebutkan baris/file yang dilanggar (jika review).
- Jelaskan *kenapa* aturannya penting, bukan hanya "jangan".
