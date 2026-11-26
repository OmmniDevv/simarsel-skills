---
name: simarsel-workflow
description: Standar kerja, prinsip greenfield (bangun dari 0, zero CreaBoard leakage), alur Git fork dev-to-dev, aturan Jira & Definition of Done, disiplin PR, arsitektur Laravel 13, dan verifikasi live test untuk proyek SiMarsel V2. Aktifkan skill ini setiap kali mengerjakan tiket SiMarsel V2 (SAV-*), menganalisis ERD/database, membuat fitur backend atau frontend, menjalankan pengujian sebelum PR, menyiapkan deskripsi PR, atau memperbarui status dan DoD di Jira, meskipun pengguna tidak menyebut nama skill secara eksplisit.
---

# SiMarsel V2 Team Workflow

Ikuti panduan ini di setiap tugas pada repositori `SiMarsel_v2` agar seluruh anggota tim memakai alur Git, standar kode, disiplin pengujian, dan pelaporan PR/Jira yang seragam.

## 1. Analisis Sebelum Menulis Kode

Sebelum menulis kode apa pun pada sebuah tiket:

1. Baca parent User Story, sub-task, acceptance criteria, technical notes, dan daftar dependency tiket secara utuh.
2. Baca `AGENTS.md` di root repositori serta telusuri kode eksisting yang terdampak (route, Form Request, controller, service, repository, model, migration, view, dan JS) agar tidak menduplikasi pola yang sudah ada.
3. Periksa schema database / ERD yang relevan sebelum mengasumsikan nama tabel, kolom, atau relasi.
4. Jika ada konflik antara tiket/desain dengan implementasi atau data aktual, jangan mengubah aturan bisnis secara diam-diam. Catat gap yang ditemukan, sampaikan dampaknya, dan koordinasikan ke pemilik tiket dependency atau maintainer.
5. Kerjakan hanya tiket yang di-assign ke developer bersangkutan. Jangan menyentuh modul, Epic, atau fitur eksisting di luar scope tiket.

## 2. Prinsip Greenfield & Zero CreaBoard Leakage (Wajib)

Modul CreativeDesk (Create Konten & Dashboard) di `SiMarsel_v2` dibangun **100% dari 0 (greenfield)**, bukan migrasi data legacy:

1. **Single Source of Truth:** Semua desain schema, validasi, query, dokumentasi, dan kontrak API wajib mengacu hanya pada repo `SiMarsel_v2`, **ERD To-Be final (`SAV-48`)**, dan spesifikasi tiket Jira (`SAV`). Semua referensi pengguna langsung mengarah ke `accounts.id`.
2. **Dilarang mengambil dari `CreaBoard`:** Jangan mengambil struktur database, nama kolom (`creative_user_id` As-Is), data runtime (`pglite`, `server/db.js`), bukti query, maupun asumsi mapping/migrasi akun legacy (`users.id`) dari `CreaBoard`.
3. **Zero Leakage di Deliverable:** Dilarang mencantumkan nama atau referensi `CreaBoard`, `pglite`, audit `As-Is`, maupun migrasi data legacy di dalam kode, komentar kode, dokumen `docs/`, commit message, deskripsi PR, maupun komentar Jira pada `SiMarsel_v2`.
4. **Peran `CreaBoard` hanya referensi pasif:** `CreaBoard` hanya boleh dilihat secara internal sebagai referensi konsep atau alur bisnis apabila dibutuhkan, tidak pernah dijadikan sumber data/schema ataupun bahan deliverable.
5. **Pre-PR Grep Gate (Wajib):** Sebelum commit dan membuka PR di `SiMarsel_v2`, wajib audit semua file perubahan untuk memastikan **0 kemunculan** `creaboard`, `pglite`, `as-is`, dan mapping `users.id` legacy.

## 3. Alur Git & Fork Pull Request

Alur standar tim memakai fork per developer tanpa membuat branch fitur baru di fork (menimpa poin branch fitur pada `AGENTS.md` khusus alur fork):

1. Fork repositori upstream `marselcerebrum-jpg/SiMarsel_v2` ke akun GitHub masing-masing.
2. Konfigurasikan dua remote lokal: `origin` mengarah ke upstream (`marselcerebrum-jpg/SiMarsel_v2`) dan `fork` mengarah ke fork pribadi.
3. Selalu sinkronkan branch `dev` lokal dengan `origin/dev` terbaru sebelum memulai pekerjaan baru dan sebelum push:
   ```bash
   git checkout dev
   git fetch origin dev
   git merge --ff-only origin/dev
   ```
4. Kerjakan, uji, dan commit perubahan langsung pada branch `dev`, lalu push ke branch `dev` di fork:
   ```bash
   git push --no-thin fork dev
   ```
5. Buka Pull Request dari `<akun-fork>:dev` menuju `marselcerebrum-jpg/SiMarsel_v2` branch `dev`.
6. Selama PR pada `<akun-fork>:dev` masih berstatus `OPEN`, jangan push commit untuk tiket baru ke branch `dev` fork agar dua tiket tidak tercampur dalam satu PR. Tunggu PR sebelumnya di-merge terlebih dahulu.
7. Jaga agar setiap commit dan PR tetap sekecil mungkin sesuai kebutuhan tiket. Jangan menyelipkan refactor atau pembersihan kode yang tidak diminta.

## 4. Standar Commit & Deskripsi Pull Request

- **Tanpa atribusi AI:** Dilarang menyertakan AI, LLM, atau bot sebagai kontributor maupun co-author di commit atau PR (jangan pernah menambahkan baris `Co-authored-by:` atas nama AI). Pekerjaan diatribusikan penuh atas nama developer.
- **Format Judul PR (Wajib Parent / Child):** Jangan hanya menulis ID child/sub-task di judul PR. Wajib cantumkan Parent User Story dan Sub-task di judul PR, misalnya `feat(creative-desk): read-only dashboard data source validator (SAV-3 / SAV-15)` atau `SAV-3 / SAV-15`.
- **Struktur deskripsi PR:**
  - **Ringkasan:** Perilaku apa yang ditambah atau diubah.
  - **Konteks tiket:** Cantumkan ID Parent User Story dan Sub-task Jira (contoh: `SAV-3` dan `SAV-15`), tujuan parent, acceptance criteria yang dikerjakan, batas scope, serta dependency tiket lain.
  - **Dampak lintas modul:** Sebutkan modul lain yang ikut terpengaruh beserta buktinya, atau nyatakan secara eksplisit jika tidak ada dampak lintas modul.
  - **Bukti pengujian:** Cantumkan perintah tes yang benar-benar dijalankan pada commit PR, hasil aktualnya, serta bagian yang belum teruji (misal constraint khusus MySQL bila uji lokal memakai SQLite).
- **Khusus Frontend (FE):** Wajib menyertakan screenshot atau dokumentasi visual hasil antarmuka pada deskripsi PR.

## 5. Disiplin Jira & Definition of Done (DoD)

- Perbarui status tiket secara berkala di board Jira proyek (baik Story maupun Sub-task) saat mulai dikerjakan dan saat siap direview.
- Jangan memindahkan tiket ke `Done` sebelum PR di-merge dan syarat tiket terpenuhi, kecuali ada persetujuan eksplisit dari maintainer/atasan.
- Centang checklist **Definition of Done (DoD)** secara jujur berdasarkan bukti nyata:
  - Hanya tandai selesai untuk item yang benar-benar sudah terverifikasi.
  - Biarkan item yang bergantung pada merge PR atau tiket lain tetap belum dicentang, lalu tulis alasannya secara ringkas di komentar tiket.

## 6. Standar Teknis & Arsitektur (`AGENTS.md`)

Pertahankan arsitektur monolith Laravel 13 yang sudah berjalan di `SiMarsel_v2`:

- **Stack:** PHP `^8.3.36`, Laravel 13, Eloquent, MySQL (koneksi aplikasi default), Blade, Alpine.js, Tailwind CSS v4, Chart.js, Vite, dan native `fetch()`. Jangan menambah framework frontend baru atau memecah ke arsitektur service terpisah.
- **Backend:**
  - Controller menangani input/output HTTP, Form Request memvalidasi input, Service memegang business rule dan workflow, Model mendefinisikan relasi/cast/scope, dan Repository dipakai hanya untuk query kompleks atau lintas fitur.
  - Utamakan Eloquent atau Query Builder. Gunakan SQL eksplisit hanya jika agregasi atau performa memerlukannya, wajib memakai binding parameter dan disertai pengujian.
  - Perubahan schema harus melalui migration yang sinkron dengan model, factory, dan test. Jangan menjalankan perintah reset database yang destruktif pada database non-disposable.
  - Tegakkan otorisasi di sisi server, bukan hanya menyembunyikan tombol di UI. Jangan mencatat credential atau data pribadi user di log maupun respons error.
- **Frontend:**
  - Gunakan Blade, Alpine.js, Tailwind, dan Vite eksisting. Tempatkan logika Alpine non-trivial di `resources/js/` dan jaga directive `@vite` selaras dengan `vite.config.js`.
  - Validasi server tetap otoritatif. Pertahankan perlindungan CSRF untuk request non-GET dan tampilkan error validasi/API dengan jelas.
  - Pastikan desain antarmuka disetujui terlebih dahulu sebelum mulai menulis kode FE.

## 7. Pengujian Wajib Sebelum PR

Jangan membuka PR hanya bermodal pembacaan kode. Jalankan verifikasi yang relevan dengan perubahan:

```bash
./vendor/bin/pint --test
php artisan test --filter=NamaTestTerkait
npm run test:frontend   # jika file JS/FE berubah
npm run build           # jika aset frontend berubah
git diff --name-only origin/dev | xargs -r grep -inE "creaboard|pglite|as-is|as_is|users\.id|creative_user_id"
```

1. Jalankan suite tes yang lebih luas (misalnya `tests/Feature/Database` atau modul terkait) jika perubahan berpotensi memengaruhi fitur bersama.
2. Lakukan **live test** nyata (Artisan CLI, endpoint HTTP lokal, atau browser untuk FE) mencakup skenario utama dan minimal satu skenario gagal/edge case, lalu laporkan hasil observasinya.
3. Ingat bahwa PHPUnit memakai SQLite in-memory; jika logika bergantung pada fitur khusus MySQL (seperti `information_schema`, lock, atau constraint tertentu), catat secara jujur bahwa bagian tersebut belum teruji di MySQL live.
4. Pastikan **Pre-PR Grep Gate** pada file perubahan menghasilkan **0 kecocokan** terhadap referensi `CreaBoard`, `pglite`, `As-Is`, dan mapping `users.id` legacy.

## 8. Standar Kualitas Output & Plugin `antislop`

Selalu install dan terapkan plugin [`anti-slop`](https://github.com/miqdadbadjuber/anti-slop) pada setiap sesi pengerjaan kode, dokumentasi, deskripsi PR, UI, maupun laporan:

- Install di Antigravity CLI: `agy plugin install https://github.com/miqdadbadjuber/anti-slop` (atau pakai panduan install untuk Claude Code / Codex dari repositori `miqdadbadjuber/anti-slop`).
- Patuhi aturan inti `antislop` (`antislop-code`, `antislop-ui`, `antislop-copywriting`):
  - Jangan memakai em dash dalam teks, kode, komentar, dokumen, atau PR.
  - Jangan pernah mengarang klaim, angka, hasil tes, atau status yang tidak benar-benar diverifikasi.
  - Hindari komentar deklaratif yang hanya mengulang kode, decorative banner, wrapper spekulatif, dan teks filler AI.

## 9. Prinsip Ownership

AI dipakai untuk mempercepat analisis dan implementasi, tetapi developer tetap memegang kendali penuh: pahami alur bisnis, verifikasi semua klaim dan keluaran tes secara langsung, dan pastikan setiap perubahan yang diajukan memang benar-benar dibutuhkan.
