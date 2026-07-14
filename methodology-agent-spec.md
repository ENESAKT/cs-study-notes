# Dersler Agent Guide

This repository is a BEU Engineering study tracking workspace.

## Language Contract

- Internal agent instructions, command specs, and rule files are written in English to reduce token cost.
- All user-facing chat must be Turkish.
- Course artifacts must stay Turkish: `ders.md`, `defter_notlari.md`, `calisma_plani.md`, `ogrenilenler.md`, quizzes, summaries, and explanations.
- Code identifiers and official technical terms may stay in their original form.

## Course Files

Each course folder must keep a GitHub-facing three-file structure:

- `ders.md`: structured course notes, required
- `defter_notlari.md`: short notebook notes, required
- `calisma_plani.md`: roadmap/material-based study plan, required

Local-only files may exist, but must not be pushed by default:

- `yol_haritasi.md`: checkbox roadmap/progress tracker, local-only unless user explicitly asks to publish it.
- `oturum_durumu.md`: current active study session state, local-only.
- `pdf/`: local-only source material folder for each course. The user will place all new PDF lesson notes here.
- `defter/`: local-only student notebook/source scans. These are the user's own class notes and must be scanned together with `pdf/`.
- PDFs, Word files, slides, Packet Tracer files, `hafta*.md`, practice files, extracted materials, and temporary files are local-only. Their useful content must be merged into `ders.md`.

Optional:

- `ogrenilenler.md`: learned concepts

## Courses

| Alias | Folder | Topic |
| --- | --- | --- |
| `sp`, `sistem` | `Sistem Programlama/` | OS / System Programming |
| `m`, `mikro` | `Mikroişlemciler/` | 8051 Microcontroller |
| `a`, `ag`, `ağ` | `Bilgisayar Ağları/` | Network Protocols |
| `i`, `internet`, `js` | `İnternet teknolojileri/` | JavaScript DOM / Events |
| `ym`, `yazilim` | `yazılım mühendisligi/` | Design Patterns |
| `db`, `veritabani` | `veritabanı/` | SQL and DB Design |
| `pm`, `proje` | `Proje yönetimi/` | ML Ransomware Detection |
| `bitirme` | `Bitirme_projesi/` | BEU Scheduling System |

## Roadmap Scope Rule

Only topics already present in `yol_haritasi.md` or an approved `calisma_plani.md` may be taught, planned, or marked complete.

- Do not invent new headings or subtopics.
- If a useful topic is missing, ask the user in Turkish whether it should be added.
- Do not add roadmap items without explicit user approval.

## Source Role and Ordering Rule

- Treat `pdf/` as the instructor/source-material folder.
- Treat `defter/` as the user's own notebook folder.
- When both folders contain material for the same course, merge useful, non-duplicate content into `ders.md` in the actual lesson/page order: earlier lessons first, later/final material later.
- Preserve the source order from filenames, dates, visible page numbers, and notebook evidence. Do not move an early prerequisite behind a later final topic.
- Cite source references in course notes as `Kaynak: pdf/[file], s. X-Y` or `Kaynak: defter/[file], s. X-Y`.
- If a PDF is a scanned image and text extraction is poor, inspect rendered pages when needed; if a page is unreadable, state the uncertainty instead of inventing content.
- For `Mikroişlemciler/`, `defter/` is the canonical source unless the user explicitly provides another source. If the same file exists in both `pdf/` and `defter/`, use the `defter/` copy for ordering and source references.

## Topic Resolution Rule

For every course request, always resolve progress from the real course files. Do not pin a course to one topic forever.

Required order:

1. Read the selected course `yol_haritasi.md`.
2. Read the selected course `defter_notlari.md`.
3. Infer the last studied roadmap topic from the latest relevant notebook entries.
4. Reconcile notebook evidence with roadmap checkboxes.
5. Use the latest user message only to clarify or override the inferred topic.
6. Use `oturum_durumu.md` only as a secondary hint, never as the primary source.
7. If evidence conflicts, explain the conflict briefly in Turkish and ask which point to continue from.

The first unchecked roadmap item is only a fallback when notebook notes do not show a later studied topic.

## Standard Study Flow

For `/ders-baslat` and `/ders-mentor`:

1. Scan the selected course folder.
   - Ensure the selected course has a local `pdf/` folder; create it if missing.
   - Ensure the selected course has a local `defter/` folder when notebook scans are expected; create it only when needed.
   - If new PDFs exist under `pdf/` or new notebook scans exist under `defter/`, first merge their useful Turkish content into `ders.md`, update `calisma_plani.md`, and update `yol_haritasi.md` with approved checkpoints before teaching.
2. Read `yol_haritasi.md`.
3. Read `defter_notlari.md` and infer where the course was left.
4. Resolve the active topic using the rule above.
5. Read the relevant section from `ders.md`.
6. Show a short Turkish lesson-note summary in chat.
7. Append a short Turkish notebook note to `defter_notlari.md` only when starting a new topic or when the existing note is missing/incomplete. Never overwrite old notes.
8. Teach in Turkish.
9. Ask one scenario-based comprehension question.
10. When a topic stage has been taught and the session moves to the next roadmap stage, update the exact roadmap checkbox to `[x]`. If the user does not know the answer and the assistant explains the answer, treat the stage as passed before moving on.
11. Mark a parent section `[x]` only when all child items are complete.
12. Update `oturum_durumu.md` as a derived summary after each topic step.

## Notebook Note Format

Notebook notes must be Turkish, short, and appended. Use the established Turkish notebook labels in generated course content.
Every new notebook note must include a source line immediately under the title. The source line must name the PDF or defter file and page number or page range used for that note, so the user can open the original source later. If a topic is synthesized from multiple sources, list all relevant files/pages. If no source file exists, write `**Kaynak:** ders.md`.

```markdown

---

## [DATE] - [Topic Title]

**Kaynak:** `pdf/[PDF file name]`, s. [page or page range]
**Short Definition:** translate this label to Turkish in generated notes
**Key Concepts:** translate this label to Turkish in generated notes
- Concept: short Turkish explanation
**Example / Command:** translate this label to Turkish in generated notes
**Remember:** translate this label to Turkish in generated notes
```

## Completion and Git

When a topic is actually completed:

- Change the exact roadmap checkbox from `[ ]` to `[x]`.
- A topic is considered completed when its defter note and explanation have been given and the session is moving to the next roadmap item, even if the assistant had to provide the comprehension answer.
- Update the course `oturum_durumu.md` as a derived summary.
- Use short Turkish commit messages when committing is appropriate.

Default GitHub publishing rule:

- Only stage root `.gitignore`, `README.md`, `AGENTS.md`, and for each published course only `ders.md`, `defter_notlari.md`, `calisma_plani.md`.
- Do not stage `yol_haritasi.md`, `oturum_durumu.md`, PDFs, Word files, slides, Packet Tracer files, `.DS_Store`, virtual environments, editor folders, extracted files, or `hafta*.md` unless the user explicitly asks.
- If a new PDF/material/notebook scan is added for a course, summarize and merge it into that course's `ders.md`; update or create `calisma_plani.md`; then push only the three course files.
- If old auxiliary files are already tracked, remove them from Git tracking with `git rm --cached` without deleting local files.
- Only stage files related to the current task. Never stage unrelated dirty files.

## Material Intake

When the user provides a PDF, notebook scan, file, or new course material:

1. Ask in Turkish which course it belongs to, using the aliases `(m/sp/a/i/db/ym/pm)`, unless the file is already inside a course's `pdf/` folder.
2. Ensure that course has a local `pdf/` folder for instructor materials and a local `defter/` folder for the user's own notebook scans when needed.
3. When a new PDF appears in `pdf/` or a new notebook scan appears in `defter/`, read it, add structured Turkish content to the course `ders.md`, and include source references such as `Kaynak: pdf/file.pdf, s. 3-5` or `Kaynak: defter/file.pdf, s. 3-5`.
4. Create or update the course `calisma_plani.md` with the new material's study order.
5. Ask before adding new roadmap checkpoints to local `yol_haritasi.md`, unless the user explicitly asked to update the roadmap from the new PDFs.
6. Add approved roadmap items as unchecked `[ ]`.
7. Do not create notebook notes until a study session reaches that topic.
8. When later creating notebook notes from PDF-backed or defter-backed content, include the source file and page number/range under the note title.
9. Do not push the original material file unless the user explicitly says it should be published.

## Slash Commands

- `/ders-takip`
- `/ders-takip [ders]`
- `/ders-takip soru`
- `/ders-baslat`
- `/ders-baslat [ders]`
- `/ders-mentor`
- `/ders-mentor [m/a/i/db/sp/ym/pm]`
- `/ders-defter`
- `/ders-defter [m/a/i/db/sp/ym/pm]`
- `/ders-plan`
- `/ders-plan [m/a/i/db/sp/ym/pm]`

Before executing a slash command, read its matching file in `.claude/commands/`.
