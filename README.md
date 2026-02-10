# obsidian-cli-skills

# Codex Skills: Obsidian CLI

A small set of Codex skills to operate Obsidian via the official Obsidian CLI (`obsidian`): read/create/edit notes, search your vault, manage tasks/tags/properties, run link health checks, and safely move/rename notes without breaking wikilinks.

## What’s included

- `obsidian-cli` — general “operator” skill for composing correct `obsidian ...` commands (best practices + common recipes).
- `obsidian-safe-move` — safe move/rename workflow using `obsidian move` with link checks and a rollback strategy.

## Requirements

1. Obsidian CLI must be installed and enabled in Obsidian:
   - Obsidian 1.12+ (early access) and Catalyst (per current CLI docs).
   - Enable **Settings → General → Command line interface** and complete CLI registration.
2. `obsidian` must be available in your `PATH`:
   - Check: `obsidian version` and `obsidian help`.
3. Typically Obsidian must be running (if it’s not, the first CLI command usually launches the app).

## Install

Copy the skill folders into `$CODEX_HOME/skills` (commonly `~/.codex/skills/`), then restart Codex if needed.

Example:
```bash
cp -R obsidian-cli obsidian-safe-move "$CODEX_HOME/skills/"
# or (if CODEX_HOME is not set)
cp -R obsidian-cli obsidian-safe-move ~/.codex/skills/

___

Как пользоваться в чате с Codex
Скиллы активируются, когда вы прямо упоминаете их в запросе.

1) obsidian-cli
Используйте, когда нужно:

составить корректные команды obsidian ...;
автоматизировать чтение/создание/правки заметок;
поиск по vault, задачи, теги, properties, проверки ссылок, diff/history.
Примеры запросов:

Используй $obsidian-cli. Составь команды, чтобы найти все упоминания "DDD" в папке "03. IT технологии" и вернуть контекст матчей.
Используй $obsidian-cli. Создай команду для добавления задачи в daily note без открытия файла.
Используй $obsidian-cli. Обнови property last_updated на 2026-02-10 в файле path="03. IT технологии/..." через Obsidian CLI.
Что вы получите в ответ:

готовые команды obsidian ... (с правильными path=..., кавычками, флагами типа silent, overwrite, --copy и т.д.);
при необходимости — последовательность команд (precheck → действие → postcheck).
Рекомендации (важно для автоматизации):

предпочитать note.md" вместо file=Name, чтобы избежать неоднозначностей;
многострочный content писать через \n и \t.
2) obsidian-safe-move
Используйте, когда нужно переименовать/переместить заметку и важно не сломать wikilinks.

Примеры запросов:

Используй $obsidian-safe-move. Перемести path="03. IT технологии/Old.md" в "03. IT технологии/NewFolder/".
Используй $obsidian-safe-move. Переименуй path="Notes/OldName.md" в "Notes/NewName.md" и проверь, что не появилось новых битых ссылок.
Что делает скилл (типовой сценарий):

Снимает “снимок” (backlinks/links/unresolved).
Делает obsidian move ... to=....
Проверяет obsidian unresolved ..., ищет хвосты по старому имени через obsidian search ....
Подсказывает, как откатиться (через diff/history или обратный move), если что-то пошло не так.
Важно:

obsidian-safe-move рассчитан на то, что Obsidian настроен обновлять внутренние ссылки (Settings → Files & Links → Automatically update internal links).
Troubleshooting
obsidian: command not found → проверьте регистрацию CLI и PATH (в документации Obsidian CLI есть инструкции по ОС).
Команды “не видят” нужный vault → запускайте из корня vault или явно задавайте vault="<имя>".
Некоторые команды зависят от возможностей Obsidian/плагинов; если команда недоступна, начните с obsidian help и версии Obsidian.
