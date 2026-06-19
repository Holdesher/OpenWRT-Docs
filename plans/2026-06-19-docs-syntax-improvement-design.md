# Documentation Syntax & Structure Improvement — Design

**Date**: 2026-06-19
**Status**: Approved (Approach B)
**Author**: @holdesher (project owner)

## 1. Purpose

Улучшить восприятие и читабельность Markdown-документации в `docs/` и `fixed/` без изменения контекста, смысла и формулировок инструкций.

## 2. Scope

### In scope (7 файлов)
- `docs/amnezia.md`
- `docs/podkop.md`
- `docs/tailscale.md`
- `docs/vlan.md`
- `docs/zeroblock.md`
- `fixed/amnezia-rx.md`
- `fixed/package.md`

### Out of scope
- `README.md` — оставить как есть.
- `domains/inside.lst`, `domains/outside.lst` — данные, не разметка.
- `.github/*` — стандартные шаблоны Contributor Covenant.
- `.gitignore`, структура каталогов, имена файлов.

## 3. Approach (B)

Сбалансированный вариант: применять три уровня изменений — STRUCTURAL, FORMATTING, STYLISTIC — но **не** добавлять новые секции (FAQ/Materials), даже если они отсутствуют. Цель: синхронизировать структуру между файлами и нормализовать форматирование, не расширяя scope.

## 4. Changes

### 4.1 STRUCTURAL (4 изменения)

| ID | File:Line | Before | After | Reason |
|----|----------|--------|-------|--------|
| S1 | `docs/zeroblock.md:15,69` | `## AmneziaWG` (line 15) before `## VLESS` (line 69) | `## VLESS` before `## AmneziaWG` | Синхронизация порядка с `docs/podkop.md` |
| S2 | `docs/zeroblock.md:100` | `## Checker` | `## Diagnostics` | Соответствует convention: H2 в английском стиле; русское название UI упоминается в теле (`Службы → ZeroBlock → Диагностика`) |
| S3 | `docs/zeroblock.md:109` | `## Zapret` | `## Zapret2` | Соответствует имени пакета и фильтру в UI |
| S4 | `docs/tailscale.md:7` | `## Auth` | `## Auth Key` | Уточнение: речь о ключе аутентификации |

### 4.2 FORMATTING (8 изменений)

| ID | File:Line | Before | After |
|----|----------|--------|-------|
| F1 | `docs/tailscale.md:38` | `"Сохранить и применить"` | `"Сохранить" и "Применить"` |
| F2 | `docs/tailscale.md:32` | `Перейдите в "Система" -> "Пакеты" и нажмите "Обновить список".` | Два отдельных шага |
| F3 | `docs/vlan.md:22-29` | Список `- Ethernet (IPoE/DHCP/статический IP): 1500` | Markdown-таблица |
| F4 | 6 callout-блоков | Разный формат (с/без пустой строки между `> [!TYPE]` и текстом) | Унифицированный: `> [!TYPE]` + пустая строка + `> Text` |
| F5 | `docs/podkop.md:13` | Код-блок сразу после `:` без пустой строки | Пустая строка + код-блок |
| F6 | `docs/zeroblock.md:118` | `Перейдите в "Службы" -> "ZeroBlock" -> "Авто-конфигурация":` (без подсписка) | Терминатор `.` |
| F7 | UI-пути — аудит | Микс `.` и `:` | `.` для одиночных шагов, `:` для шагов с подсписком |
| F8 | Все callout-блоки | Без пустой строки-разделителя | Пустая строка-разделитель |

### 4.3 STYLISTIC (5 изменений)

| ID | File:Line | Before | After |
|----|----------|--------|-------|
| Y1 | `docs/zeroblock.md:115` | `"Установить"/"Обновить"` | Подпункт с пояснением |
| Y2 | `docs/zeroblock.md:107` | `Запустить диагностику` | `"Запустить диагностику"` (UI-кнопка в кавычках) |
| Y3 | `docs/zeroblock.md:20` | `Добавить новый интерфейс` | `"Добавить новый интерфейс"` (UI-кнопка) |
| Y4 | `fixed/amnezia-rx.md:25` | `Применить` | `"Применить"` (UI-кнопка) |
| Y5 | `docs/tailscale.md:38` | `Нажмите "Сохранить и применить" и обновите страницу.` | Нормализация пробела и кавычек |

**Total: 17 изменений в 7 файлах.**

## 5. Guarantees (что НЕ меняется)

- Текст инструкций (глаголы, существительные, порядок шагов внутри подсписков).
- Содержание callout-блоков (только их разметка).
- Списки доменов (`domains/inside.lst`, `domains/outside.lst`).
- Файл `README.md`.
- Шаблоны `.github/*`.
- Имена файлов и структура каталогов.

## 6. Procedure (порядок исполнения)

1. Создать backup каждого файла как `*.md.conform.bak`.
2. Применять изменения в порядке: STRUCT → FORMAT → STYLIST.
3. Перед применением показать diff (per-file) — docs-conform hybrid mode.
4. После — `git diff` и итоговый отчёт.
5. Использовать `docs-conform` skill для исполнения.

## 7. Convention Profile (зафиксированный)

| Aspect | Convention | Strength |
|--------|-----------|----------|
| List style | `-` (dash) | Strong (8/8) |
| Code block language | `bash` | Strong (100%) |
| UI path delimiter | `" -> "` (с пробелами) | Strong (8/8) |
| UI path terminator | `.` для одиночных, `:` для шагов с подсписком | Moderate |
| Callout types | `NOTE`, `WARNING`, `TIP` | Moderate |
| Callout format | `> [!TYPE]` + пустая строка + `> Text` | Moderate |
| Button action phrase | `Нажмите "Сохранить" и "Применить".` | Strong (4/5) |
| Section order | `VLESS → AmneziaWG` | Strong (после S1) |
| H1 per file | Один, название сервиса | Strong |

## 8. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Изменение смысла при структурной перестановке | Low | Medium | Один блок `## AmneziaWG` ↔ `## VLESS` перемещается как есть, без правки содержимого |
| Потеря данных при записи | Low | High | Backup `.conform.bak` для всех 7 файлов |
| Несоответствие callout-формата в одном из 6 мест | Medium | Low | Прохождение по всем callout явно (F4, F8) |
| Разные UI-терминаторы | Low | Low | Глобальный аудит (F7) |

## 9. Success Criteria

1. Все 17 изменений применены корректно.
2. Смысл и контекст инструкций — сохранены (проверка: `git diff` показывает только переименования, перемещения блоков, нормализацию кавычек/пробелов/таблиц).
3. `git status` содержит только 7 модифицированных `.md` файлов + 7 backup-файлов.
4. Все ссылки (URL, file references) рабочие.
5. Все code-блоки остались с тегом `bash`.

## 10. Out of Scope (явно)

- Изменение текста инструкций.
- Добавление новых секций (FAQ, Materials).
- Добавление TOC.
- Изменение `README.md`, `domains/*`, `.github/*`.
- Любые изменения в формате/структуре callout-содержимого (только разметка).
