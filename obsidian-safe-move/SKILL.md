---
name: obsidian-safe-move
description: Безопасное переименование и перемещение заметок/файлов в Obsidian через Obsidian CLI (`obsidian move`) с контрольными проверками ссылок (backlinks/outgoing/unresolved) и планом отката. Используй, когда нужно rename/move заметку и важно не сломать wikilinks и структуру vault.
---

# Obsidian Safe Move

## Цель

Выполнять `move/rename` так, чтобы:
- Obsidian корректно обновил внутренние ссылки (если включено авто-обновление).
- После операции не появилось новых битых ссылок.
- Был понятный путь отката через историю/перемещение обратно.

## Процедура безопасного перемещения/переименования

1) Выбрать точный источник

- Предпочитать точный путь: `path="..."`.
- Если известен только “wikilink-нейм”, сначала выяснить путь: `obsidian file file="ИмяБезРасширения"`.

2) Снять “снимок” перед изменением

- Зафиксировать входящие: `obsidian backlinks path="SOURCE.md" counts total`
- Зафиксировать исходящие: `obsidian links path="SOURCE.md" total`
- Зафиксировать текущее число битых ссылок в vault: `obsidian unresolved total`

3) Выполнить перенос/переименование

- В папку: `obsidian move path="SOURCE.md" to="folder/"`
- С переименованием: `obsidian move path="SOURCE.md" to="folder/NEW-NAME.md"`

4) Проверить результат

- Битые ссылки (должно быть не хуже, чем было): `obsidian unresolved counts total`
- Поиск возможных “хвостов” по старому имени: `obsidian search query="OldName" matches limit=50`
- Проверить, что бэклинки видят новый путь: `obsidian backlinks path="folder/NEW-NAME.md" total`

5) Обновить инфраструктуру vault (если применимо)

- Если в репозитории есть автогенерация индекса — обновить индекс (например, `make index`).

## Примечания

- `obsidian move` использует поведение Obsidian по обновлению ссылок. Убедиться, что в Obsidian включено авто-обновление внутренних ссылок (Settings → Files & Links → Automatically update internal links).
- Делать массовые переносы небольшими батчами, с проверками после каждого батча.

## Откат

- Если что-то пошло не так: сравнить/восстановить через `obsidian diff` / `obsidian history:open`, либо выполнить обратный `obsidian move` обратно.
