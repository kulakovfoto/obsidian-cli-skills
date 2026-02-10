---
name: obsidian-cli
description: Управление Obsidian через Obsidian CLI (`obsidian`) из терминала/скриптов: поиск по vault, чтение/создание заметок, вставка/добавление контента, работа с шаблонами, tasks, tags и properties, анализ ссылок (backlinks/outgoing/unresolved), diff/history и прочие команды. Используй, когда нужно сформировать корректные команды Obsidian CLI или автоматизировать работу с заметками без ручного открытия UI.
---

# Obsidian CLI

## Быстрый старт

- Проверить, что CLI доступен: `obsidian version`, `obsidian help`.
- Учитывать, что CLI подключается к запущенному Obsidian (если Obsidian не запущен, первый вызов обычно стартует приложение).
- Для актуального списка команд всегда сверяться с `obsidian help` (CLI в раннем доступе, синтаксис может меняться).

## Базовые правила (чтобы команды были воспроизводимыми)

- Предпочитать `path=...` вместо `file=...` в автоматизации (избегать неоднозначностей, когда в vault есть одноимённые файлы).
- Значения с пробелами всегда заключать в кавычки: `path="03. IT технологии/..."`.
- Для многострочного `content` использовать `\\n` и `\\t`.
- Если нужно работать не из корня vault — явно задавать `vault="<имя>"`.
- Если нужно быстро передать вывод в буфер обмена — добавлять `--copy` в конец команды.

## Частые операции (шпаргалка)

- Прочитать файл: `obsidian read path="folder/note.md"`
- Открыть файл: `obsidian open path="folder/note.md" newtab`
- Создать файл: `obsidian create path="folder/note.md" content="# Заголовок\\n\\nТекст" silent`
  - Перезаписать существующий: добавить флаг `overwrite`.
  - Создать из шаблона: `obsidian create path="folder/note.md" template="TemplateName" silent`
- Добавить в конец: `obsidian append path="folder/note.md" content="\\n- пункт"`
- Добавить в начало (после frontmatter): `obsidian prepend path="folder/note.md" content="\\n> примечание"`
- Поиск: `obsidian search query="DDD" path="03. IT технологии" matches limit=20`
  - Для машинной обработки: `format=json`.
- Properties:
  - Прочитать: `obsidian property:read path="folder/note.md" name=last_updated`
  - Установить: `obsidian property:set path="folder/note.md" name=last_updated value="2026-02-10" type=date`
- Теги:
  - Все теги + счётчики: `obsidian tags all counts sort=count`
  - Инфо по тегу: `obsidian tag name="#active" total verbose`
- Задачи:
  - Все задачи: `obsidian tasks all`
  - Задачи из daily note: `obsidian tasks daily`
  - Переключить задачу: `obsidian task ref="folder/note.md:42" toggle`

## Проверки после правок/рефакторинга

- Неразрешённые ссылки: `obsidian unresolved counts total`
- Сироты (без входящих): `obsidian orphans total`
- Тупики (без исходящих): `obsidian deadends total`
- Ссылки конкретного файла:
  - Входящие: `obsidian backlinks path="folder/note.md" counts`
  - Исходящие: `obsidian links path="folder/note.md" total`

## История и восстановление

- Посмотреть версии: `obsidian diff path="folder/note.md"`
- Сравнить версии: `obsidian diff path="folder/note.md" from=2 to=1`
- Открыть UI восстановления: `obsidian history:open path="folder/note.md"`

## Важно про переименование/перемещение заметок

- Для перемещения/переименования заметок использовать отдельный навык `$obsidian-safe-move` (с проверками ссылок).

## Справочник

- Полный снимок документации Obsidian CLI: `references/obsidian_cli.md`
