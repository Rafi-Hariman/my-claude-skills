# my-claude-skills

Koleksi skill pribadi untuk Claude Code, diorganisir per kategori sebagai
**plugin**, dipasang lewat **satu marketplace** yang bisa di-install ulang di
device mana pun.

## Struktur

```
my-claude-skills/
├── .claude-plugin/marketplace.json   ← katalog semua plugin
├── TAXONOMY.md                       ← sumber kebenaran kategori skill
├── plugins/
│   ├── ui-ux/        (desain & UI/UX)
│   ├── slicing/      (desain → kode)
│   ├── frontend/     (Angular, service/API FE)
│   ├── backend/      (API/service server)
│   ├── analysis/     (QA, council, humanizer)
│   ├── utility/      (konversi file)
│   ├── meta/         (skill-forge: pengelola skill)
│   └── bundle/       (plugin "all" = install semua)
```

Nama skill ter-namespace otomatis: `/ui-ux:design`, `/backend:api-usage`, dst.

## Install di device baru

```bash
# sekali: daftarkan marketplace
claude plugin marketplace add <github-user>/my-claude-skills

# install per kategori
claude plugin install ui-ux@my-claude-skills --scope user

# atau install semua sekaligus (bundle)
claude plugin install all@my-claude-skills --scope user
```

> Ganti `<github-user>` dengan username GitHub kamu. Catatan: beberapa skill
> project-specific (`bill-api-usage`, `ng-component-audit-doc`, `sonarqube`)
> lebih pas di-install dengan `--scope project` di project yang relevan,
> bukan `user`, biar global tetap bersih.

## Menambah skill baru

Gunakan helper bawaan:

```
/meta:skill-forge "deskripsi ide / paste prompt skill kamu"
```

Helper akan menentukan kategori (dari `TAXONOMY.md`), memberi nama, dan menulis
`SKILL.md`-nya. Lihat `TAXONOMY.md` untuk aturan kategori & penamaan.

## Update setelah menambah/mengubah skill

```bash
# di repo ini
git add -A && git commit -m "add skill: <nama>"
git push

# di device (setelah push)
claude plugin update <kategori>@my-claude-skills
```

## Lisensi skill pihak ketiga

Skill yang di-vendor dari sumber lain WAJIB menyertakan file lisensi aslinya di
samping `SKILL.md`, dan menyebutkan author aslinya di frontmatter. Jangan hapus
notice copyright (khususnya untuk lisensi MIT).
