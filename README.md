# simarsel-skills

Kumpulan Agent Skill untuk menyamakan alur kerja, standar kode, disiplin Jira/DoD, pengujian live, dan aturan Pull Request tim pada proyek `SiMarsel_v2`.

## Skill Tersedia

| Skill | Lokasi | Ringkasan |
|---|---|---|
| `simarsel-workflow` | `skills/simarsel-workflow/SKILL.md` | Prinsip greenfield (bangun dari 0, zero CreaBoard leakage), alur fork Git (`<fork>:dev -> dev`), larangan AI co-author, analisis sebelum coding, update Jira & DoD jujur, standar arsitektur Laravel 13 (`AGENTS.md`), dan pengujian wajib + live test sebelum PR. |

## Cara Install

### 1. Lewat Skills CLI (Claude Code, Antigravity, Codex, Cursor, dll.)

```bash
npx skills add OmmniDevv/simarsel-skills
```

### 2. Install Manual ke Direktori Agent

Clone repo lalu salin folder `skills/simarsel-workflow` ke direktori skill agent yang kamu pakai:

```bash
git clone https://github.com/OmmniDevv/simarsel-skills.git
```

**Claude Code (global):**
```bash
mkdir -p ~/.claude/skills
cp -r simarsel-skills/skills/simarsel-workflow ~/.claude/skills/
```

**Antigravity (global):**
```bash
mkdir -p ~/.gemini/skills
cp -r simarsel-skills/skills/simarsel-workflow ~/.gemini/skills/
```

**Codex (global):**
```bash
mkdir -p ~/.codex/skills
cp -r simarsel-skills/skills/simarsel-workflow ~/.codex/skills/
```

**Per-proyek (lokal saja, jangan ikut di-commit ke PR fitur):**
```bash
mkdir -p /path/ke/SiMarsel_v2/.agents/skills
cp -r simarsel-skills/skills/simarsel-workflow /path/ke/SiMarsel_v2/.agents/skills/
```

## Plugin Pendamping Wajib (`antislop`)

Agar kualitas kode, UI, dokumen, dan deskripsi PR tetap bersih tanpa pola slop AI, install juga plugin [`miqdadbadjuber/anti-slop`](https://github.com/miqdadbadjuber/anti-slop):

```bash
# Antigravity CLI
agy plugin install https://github.com/miqdadbadjuber/anti-slop
```

Untuk Claude Code / Codex / Cursor, ikuti instruksi instalasi di repositori [miqdadbadjuber/anti-slop](https://github.com/miqdadbadjuber/anti-slop).

## Ringkasan Aturan Utama

1. **Fork `dev -> dev`:** Jangan membuat branch baru di fork. Sinkronkan `dev` lokal dengan `origin/dev` terbaru, push ke `fork dev`, lalu buka PR ke `marselcerebrum-jpg/SiMarsel_v2:dev`.
2. **Tanpa AI Co-Author & Judul PR `(PARENT / CHILD)`:** Semua commit dan PR murni atas nama developer, serta judul PR wajib mencantumkan parent dan child (contoh: `(SAV-3 / SAV-15)`), bukan child saja.
3. **Analisis & Scope Ketat:** Baca parent Story, sub-task, dependency, ERD/DB, dan `AGENTS.md` sebelum coding. Jangan sentuh modul di luar tiket.
4. **Tes Otomatis + Live Test:** Wajib lulus Pint, tes terkait, dan live test sebelum membuka PR.
5. **DoD Jujur:** Centang checklist Definition of Done di Jira hanya untuk item yang benar-benar terverifikasi.
6. **Selalu Pakai Plugin `antislop`:** Install dan patuhi plugin `miqdadbadjuber/anti-slop` pada kode, UI, dokumentasi, dan PR.
7. **Greenfield & Zero CreaBoard Leakage:** Modul CreativeDesk dibangun 100% dari 0 mengacu pada ERD To-Be final (`SAV-48`) dan repo `SiMarsel_v2`. `CreaBoard` hanya referensi pasif internal; dilarang mengambil schema/data atau menyebut `CreaBoard`/`As-Is`/migrasi legacy di kode, `docs/`, PR, commit, maupun Jira (wajib lulus pre-PR grep gate).
