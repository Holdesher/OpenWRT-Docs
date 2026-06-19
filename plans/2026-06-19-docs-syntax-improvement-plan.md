# Documentation Syntax & Structure Improvement — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Применить 17 согласованных изменений в 7 файлах документации (`docs/*.md`, `fixed/*.md`) для улучшения читабельности без изменения контекста инструкций.

**Architecture:** Sequential file-by-file modification с backup перед каждым файлом, пошаговым diff'ом и атомарными коммитами по категориям (STRUCT → FORMAT → STYLIST). Категории применяются в указанном порядке для минимизации конфликтов.

**Tech Stack:** Markdown (GitHub-flavored), shell (`bash`, `git`), `docs-conform` skill (hybrid mode).

## Global Constraints

- **Целевые файлы (7)**: `docs/amnezia.md`, `docs/podkop.md`, `docs/tailscale.md`, `docs/vlan.md`, `docs/zeroblock.md`, `fixed/amnezia-rx.md`, `fixed/package.md`.
- **Не трогать**: `README.md`, `domains/*.lst`, `.github/*`, `.gitignore`.
- **Содержание инструкций не меняется** — только структура, форматирование, нормализация кавычек/пробелов.
- **H2-секции — на английском** (по convention); русский UI-текст — только в теле.
- **UI-путь**: `"Раздел" -> "Подраздел"` (с пробелами); терминатор `.` для одиночных шагов, `:` для шагов с подсписком.
- **Code-блоки**: всегда с тегом `bash`.
- **Backup**: для каждого файла создаётся `*.md.conform.bak` перед записью.
- **Backup-файлы** НЕ коммитятся (`.gitignore` или удаление перед коммитом).
- **Конвенция коммитов**: Conventional Commits (`fix(docs):`, `refactor(docs):`).

---

## File Structure

**Modify (7 files)**:
- `docs/amnezia.md` — apply F4, F8 (callout-нормализация)
- `docs/podkop.md` — apply F5 (пустая строка перед код-блоком)
- `docs/tailscale.md` — apply S4, F1, F2, F8, Y5
- `docs/vlan.md` — apply F3 (MTU → таблица)
- `docs/zeroblock.md` — apply S1, S2, S3, F4, F6, F8, Y1, Y2, Y3
- `fixed/amnezia-rx.md` — apply F8, Y4
- `fixed/package.md` — apply F8

**Create (7 backup files, затем удаляются)**:
- `docs/amnezia.md.conform.bak`
- `docs/podkop.md.conform.bak`
- `docs/tailscale.md.conform.bak`
- `docs/vlan.md.conform.bak`
- `docs/zeroblock.md.conform.bak`
- `fixed/amnezia-rx.md.conform.bak`
- `fixed/package.md.conform.bak`

**Create (1 new file)**:
- Этот plan-файл (уже создан)

---

## Task 1: Create backups for all 7 files

**Files:**
- Create: `docs/*.md.conform.bak` (5 files)
- Create: `fixed/*.md.conform.bak` (2 files)

- [ ] **Step 1: Run backup command**

```bash
for f in docs/amnezia.md docs/podkop.md docs/tailscale.md docs/vlan.md docs/zeroblock.md fixed/amnezia-rx.md fixed/package.md; do
  cp "$f" "$f.conform.bak"
done
```

- [ ] **Step 2: Verify backups**

```bash
ls -la docs/*.conform.bak fixed/*.conform.bak
```

Expected: 7 files listed, same sizes as originals.

- [ ] **Step 3: No commit (backup files are .gitignore'd or removed before commit)**

---

## Task 2: Apply STRUCTURAL changes to `docs/zeroblock.md`

**Files:**
- Modify: `docs/zeroblock.md`

**Changes:**
- **S1**: Swap `## AmneziaWG` (L15-67) ↔ `## VLESS` (L69-90). New order: VLESS first.
- **S2**: Rename `## Checker` (L100) → `## Diagnostics`
- **S3**: Rename `## Zapret` (L109) → `## Zapret2`

- [ ] **Step 1: Reorder sections (S1)**

Move the entire `## VLESS` block (L69-90) to position L15, before the current `## AmneziaWG` block. Adjust internal line numbers.

- [ ] **Step 2: Rename headings (S2, S3)**

```diff
- ## Checker
+ ## Diagnostics
```

```diff
- ## Zapret
+ ## Zapret2
```

- [ ] **Step 3: Show diff to user for confirmation**

```bash
git diff docs/zeroblock.md
```

- [ ] **Step 4: Verify no content changed (only reorder + renames)**

```bash
diff <(grep -v "^## " docs/zeroblock.md.conform.bak | sed 's/^[ \t]*//') \
     <(grep -v "^## " docs/zeroblock.md | sed 's/^[ \t]*//')
```

Expected: empty output (content identical, only H2 order changed).

- [ ] **Step 5: Commit**

```bash
git add docs/zeroblock.md
git -c user.name=opencode -c user.email=opencode@local commit -m "refactor(docs): reorder VLESS/AmneziaWG and rename Checker/Zapret in zeroblock"
```

---

## Task 3: Apply STRUCTURAL change to `docs/tailscale.md`

**Files:**
- Modify: `docs/tailscale.md:7`

**Change:**
- **S4**: Rename `## Auth` → `## Auth Key`

- [ ] **Step 1: Edit heading**

```diff
- ## Auth
+ ## Auth Key
```

- [ ] **Step 2: Commit**

```bash
git add docs/tailscale.md
git -c user.name=opencode -c user.email=opencode@local commit -m "refactor(docs): rename Auth to Auth Key in tailscale"
```

---

## Task 4: Apply FORMATTING + STYLISTIC to `docs/zeroblock.md`

**Files:**
- Modify: `docs/zeroblock.md`

**Changes:**
- **F6**: `Перейдите в "Службы" -> "ZeroBlock" -> "Авто-конфигурация":` → терминатор `.` (L118)
- **Y1**: `"Установить"/"Обновить"` → подпункт с пояснением (L115)
- **Y2**: `Запустить диагностику` → `"Запустить диагностику"` (L107)
- **Y3**: `Добавить новый интерфейс` → `"Добавить новый интерфейс"` (L20)
- **F4, F8**: Нормализация callout-блоков (если есть в этом файле)

- [ ] **Step 1: Add quotes around UI buttons (Y2, Y3)**

In `## Setup` (around L20): `Добавить новый интерфейс` → `"Добавить новый интерфейс"`.

In `## Diagnostics` (around L107): `Запустить диагностику` → `"Запустить диагностику"`.

- [ ] **Step 2: Fix terminator (F6)**

L118: `Перейдите в "Службы" -> "ZeroBlock" -> "Авто-конфигурация":` → `Перейдите в "Службы" -> "ZeroBlock" -> "Авто-конфигурация".` (no sublist after, so use period)

- [ ] **Step 3: Improve install/upgrade instructions (Y1)**

L115: `"Установить"/"Обновить" напротив каждого пакета по порядку.`

Replace with:
```markdown
  - Нажмите "Установить" или "Обновить" напротив каждого пакета по порядку (в зависимости от того, установлен ли пакет). В появившемся окне нажмите "Установить" еще раз, после завершения закройте окно.
```

- [ ] **Step 4: Commit**

```bash
git add docs/zeroblock.md
git -c user.name=opencode -c user.email=opencode@local commit -m "style(docs): normalize UI button quotes and terminators in zeroblock"
```

---

## Task 5: Apply FORMATTING to `docs/podkop.md`

**Files:**
- Modify: `docs/podkop.md:10-14`

**Change:**
- **F5**: Добавить пустую строку между шагом `Выполните установку пакетов:` и код-блоком.

- [ ] **Step 1: Verify current state**

Lines 10-14 should be:
```
- Выполните установку пакетов:

```bash
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/podkop/refs/heads/main/install.sh)
```
```

- [ ] **Step 2: If blank line is missing, add it**

- [ ] **Step 3: Commit (if changed)**

```bash
git add docs/podkop.md
git -c user.name=opencode -c user.email=opencode@local commit -m "style(docs): add blank line before code block in podkop"
```

---

## Task 6: Apply FORMATTING + STYLISTIC to `docs/tailscale.md`

**Files:**
- Modify: `docs/tailscale.md`

**Changes:**
- **F1**: L38: `"Сохранить и применить"` → `"Сохранить" и "Применить"`
- **F2**: L32: Разделить "Перейдите в "Система" -> "Пакеты" и нажмите "Обновить список"." на два шага
- **F8**: Callout-нормализация (L3)
- **Y5**: L38: убрать лишнюю запятую + пробел

- [ ] **Step 1: Split combined action (F2)**

L32-33:
```diff
- - Перейдите в "Система" -> "Пакеты" и нажмите "Обновить список".
- - Введите в фильтр `tailscale`.
+ - Перейдите в "Система" -> "Пакеты".
+ - Нажмите "Обновить список".
+ - Введите в фильтр `tailscale`.
```

- [ ] **Step 2: Normalize save button (F1, Y5)**

L38:
```diff
-   - Нажмите "Сохранить и применить" и обновите страницу.
+   - Нажмите "Сохранить" и "Применить", затем обновите страницу.
```

- [ ] **Step 3: Normalize callout (F8)**

L3-5:
```diff
- > [!NOTE]
- >
- > Дополнительная информация о настройке и устранении проблем есть в документации [remote](https://docs.routerich.ru/ru/remote).
+ > [!NOTE]
+ >
+ > Дополнительная информация о настройке и устранении проблем есть в документации [remote](https://docs.routerich.ru/ru/remote).
```
(if not already in this format)

- [ ] **Step 4: Commit**

```bash
git add docs/tailscale.md
git -c user.name=opencode -c user.email=opencode@local commit -m "style(docs): normalize save button, split action, fix callout in tailscale"
```

---

## Task 7: Apply FORMATTING to `docs/vlan.md` (MTU → table)

**Files:**
- Modify: `docs/vlan.md:22-29`

**Change:**
- **F3**: Список MTU → markdown-таблица

- [ ] **Step 1: Replace list with table**

L22-29: from
```markdown
- Ethernet (IPoE/DHCP/статический IP): `1500`.
- PPPoE: `1492` или `1480`.
- PPTP: `1460`.
- L2TP: `1460` или `1400`.
- 4G/3G-модемы: `1420` или `1400`.
```

to
```markdown
| Тип подключения | MTU |
|-----------------|-----|
| Ethernet (IPoE/DHCP/статический IP) | `1500` |
| PPPoE | `1492` или `1480` |
| PPTP | `1460` |
| L2TP | `1460` или `1400` |
| 4G/3G-модемы | `1420` или `1400` |
```

- [ ] **Step 2: Commit**

```bash
git add docs/vlan.md
git -c user.name=opencode -c user.email=opencode@local commit -m "style(docs): convert MTU list to table in vlan"
```

---

## Task 8: Apply FORMATTING to `docs/amnezia.md` (callout normalization)

**Files:**
- Modify: `docs/amnezia.md`

**Changes:**
- **F4, F8**: Нормализация 2 callout-блоков (L5-7 NOTE, L46-48 WARNING)

- [ ] **Step 1: Normalize NOTE callout (L5-7)**

Ensure format is:
```markdown
> [!NOTE]
>
> Создать конфиг или токен подключения можно только на устройстве, на котором был создан `self-host`.
```

- [ ] **Step 2: Normalize WARNING callout (L46-48)**

Ensure format is:
```markdown
> [!WARNING]
>
> На данный момент получить ключ `vless://` из "Amnezia Premium" нельзя.
```

- [ ] **Step 3: Commit (if changed)**

```bash
git add docs/amnezia.md
git -c user.name=opencode -c user.email=opencode@local commit -m "style(docs): normalize callout blocks in amnezia"
```

---

## Task 9: Apply FORMATTING + STYLISTIC to `fixed/amnezia-rx.md`

**Files:**
- Modify: `fixed/amnezia-rx.md`

**Changes:**
- **F8**: Callout-нормализация (L17-19 TIP)
- **Y4**: `Применить` → `"Применить"` (L25)

- [ ] **Step 1: Quote "Применить" button (Y4)**

L25: `Нажмите "Применить".` (already has quotes? verify) — если нет, добавить.

- [ ] **Step 2: Normalize TIP callout (F8)**

L17-19: ensure format is:
```markdown
> [!TIP]
>
> Если после импорта `*.conf` пакеты интерфейса не передаются (`Rx = 0`), повторите импорт в режиме "Инкогнито" и с очищенным кэшем браузера.
```

- [ ] **Step 3: Commit**

```bash
git add fixed/amnezia-rx.md
git -c user.name=opencode -c user.email=opencode@local commit -m "style(docs): normalize callout and UI button in amnezia-rx"
```

---

## Task 10: Apply FORMATTING to `fixed/package.md` (callout check)

**Files:**
- Modify: `fixed/package.md`

**Change:**
- **F8**: Если есть callout-блоки, нормализовать. Если нет — пропустить.

- [ ] **Step 1: Check for callouts**

```bash
grep -n '\[!' fixed/package.md
```

- [ ] **Step 2: If no callouts, skip this file (no commit needed)**

- [ ] **Step 3: If callouts exist, normalize and commit**

---

## Task 11: Final verification

- [ ] **Step 1: Remove all backup files**

```bash
find docs fixed -name "*.conform.bak" -delete
```

- [ ] **Step 2: Verify git status**

```bash
git status
```

Expected: clean working tree (no backups, no untracked), recent commits show docs/ and fixed/ changes only.

- [ ] **Step 3: View final diff summary**

```bash
git log --oneline -10
git diff HEAD~10 --stat
```

Expected: 5-10 commits, all touching `docs/*.md` or `fixed/*.md`. No changes to `README.md`, `domains/*`, `.github/*`.

- [ ] **Step 4: Verify success criteria from spec**

- [ ] Все 17 изменений применены (см. spec)
- [ ] `git diff` показывает только структурные/форматные изменения, не текстовые
- [ ] Все ссылки рабочие (URLs не сломаны)
- [ ] Code-блоки остались с тегом `bash`

---

## Self-Review Notes

**Spec coverage check:**
- S1 (zeroblock reorder) → Task 2 ✓
- S2 (zeroblock Checker→Diagnostics) → Task 2 ✓
- S3 (zeroblock Zapret→Zapret2) → Task 2 ✓
- S4 (tailscale Auth→Auth Key) → Task 3 ✓
- F1 (tailscale save button) → Task 6 ✓
- F2 (tailscale split action) → Task 6 ✓
- F3 (vlan MTU table) → Task 7 ✓
- F4, F8 (callout normalization) → Tasks 4, 6, 8, 9 ✓
- F5 (podkop blank line) → Task 5 ✓
- F6 (zeroblock terminator) → Task 4 ✓
- F7 (UI terminator audit) → Task 4 (F6) + Task 6 ✓
- Y1 (zeroblock install/upgrade) → Task 4 ✓
- Y2 (zeroblock diagnostics button) → Task 4 ✓
- Y3 (zeroblock add interface button) → Task 4 ✓
- Y4 (amnezia-rx Применить) → Task 9 ✓
- Y5 (tailscale save normal) → Task 6 ✓

**Placeholder scan:** No TBD/TODO. Each step has concrete code/diff. ✓

**Type consistency:** All backup file names use `.conform.bak` suffix consistently. ✓
