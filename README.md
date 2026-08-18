# zapret-discord-youtube 1.10.1 (custom build)

English | [Русский](#русская-версия)

Windows build based on [flowseal/zapret-discord-youtube](https://github.com/flowseal/zapret-discord-youtube) `1.10.1` and [bol-van/zapret](https://github.com/bol-van/zapret).

This copy keeps the original Discord / YouTube strategies and adds:

- Mistfall Hunter support (AWS AS16509, TCP `12001`, UDP `20001–21000`, TTL helper)
- extra strategies `zapret#awow.bat` and `zapret#bf.bat`
- user hostlists under `lists\` (`list-general-user.txt` and related files)

If you do not trust the bundled binaries, compare checksums of `WinDivert64.sys`, `WinDivert.dll`, and `winws.exe` with the official zapret release:

https://github.com/bol-van/zapret/releases

## What it is for

zapret bypasses DPI interference on the local machine. Typical uses of this build:

- Discord and YouTube
- Mistfall Hunter on Steam (game servers on AWS, custom binary protocol over TCP)
- other games / sites listed in user lists

## How to run

1. For **Mistfall Hunter**, run `utils\calc_ttl.bat` once. It traces a route to an AWS (AS16509) address and writes a starting `--dpi-desync-ttl` value to `utils\ttl.txt`. This is only an estimate: game IPs can change.
2. Start a strategy `.bat` from this folder, or use `service.bat` to install it as a Windows service.
3. If the game drops the connection after that, lower the number in `utils\ttl.txt` and restart the strategy.

`service.bat` can install any strategy from this folder except files that start with `service`.

## Strategies

| File | Notes |
| --- | --- |
| `general.bat` and `general (ALT*).bat` | stock 1.10.1 strategies, with Mistfall filters added |
| `zapret#awow.bat` | extra TCP game ports |
| `zapret#bf.bat` | Battlefield-oriented WinDivert filter (`lists\wf-bf.txt`) |

Mistfall-specific pieces used by all of the above:

- WinDivert payload filter: `windivert.filter\windivert_part.customgame.txt` (first TCP packet 79 bytes, signature `00 00 04 4D`)
- AWS IP set: `lists\ipset-all-as16509-amazon.txt`

## User lists

Edit these files; they are not replaced by the built-in update check:

- `lists\list-general-user.txt` — extra hostnames
- `lists\list-exclude-user.txt` — hostnames to skip
- `lists\ipset-exclude-user.txt` — IPs to skip

Do not leave `list-general-user.txt` empty.

## Mistfall Hunter notes

Mistfall Hunter dedicated servers run on AWS (AS16509). In some Russian networks AWS traffic can hit a ~16 KB DPI block. The game uses a non-standard binary protocol over TCP; the custom WinDivert filter matches that handshake.

Background:

- https://github.com/net4people/bbs/issues/490
- Mistfall fork this build was adapted from: local `Fork-zapret-for-MistfallHunter`

## Credits

- [bol-van/zapret](https://github.com/bol-van/zapret)
- [flowseal/zapret-discord-youtube](https://github.com/flowseal/zapret-discord-youtube) v1.10.1
- Mistfall Hunter zapret fork (AWS/TTL/WinDivert filter)

---

# Русская версия

Сборка для Windows на базе [flowseal/zapret-discord-youtube](https://github.com/flowseal/zapret-discord-youtube) `1.10.1` и [bol-van/zapret](https://github.com/bol-van/zapret).

Сохранены штатные стратегии Discord / YouTube, дополнительно:

- поддержка Mistfall Hunter (AWS AS16509, TCP `12001`, UDP `20001–21000`, подбор TTL)
- стратегии `zapret#awow.bat` и `zapret#bf.bat`
- пользовательские списки в `lists\` (`list-general-user.txt` и связанные файлы)

Если не доверяете бинарникам из папки, сверьте контрольные суммы `WinDivert64.sys`, `WinDivert.dll` и `winws.exe` с официальным релизом zapret:

https://github.com/bol-van/zapret/releases

## Назначение

zapret обходит DPI на этом компьютере. В этой сборке в первую очередь:

- Discord и YouTube
- Mistfall Hunter в Steam (серверы на AWS, свой бинарный протокол поверх TCP)
- другие игры и сайты из пользовательских списков

## Запуск

1. Для **Mistfall Hunter** один раз запустите `utils\calc_ttl.bat`. Скрипт трассирует маршрут до адреса AWS (AS16509) и записывает стартовое значение `--dpi-desync-ttl` в `utils\ttl.txt`. Это грубая оценка: IP серверов могут меняться.
2. Запустите нужный `.bat` стратегии из этой папки или поставьте его службой через `service.bat`.
3. Если после этого рвётся соединение или выкидывает с сервера — уменьшите число в `utils\ttl.txt` и перезапустите стратегию.

`service.bat` ставит в службу любой `.bat` из этой папки, кроме файлов, имя которых начинается с `service`.

## Стратегии

| Файл | Назначение |
| --- | --- |
| `general.bat` и `general (ALT*).bat` | штатные стратегии 1.10.1 с фильтрами Mistfall |
| `zapret#awow.bat` | дополнительные TCP-порты игр |
| `zapret#bf.bat` | фильтр WinDivert под Battlefield (`lists\wf-bf.txt`) |

Во всех перечисленных стратегиях используются:

- фильтр полезной нагрузки: `windivert.filter\windivert_part.customgame.txt` (первый TCP-пакет 79 байт, сигнатура `00 00 04 4D`)
- список IP AWS: `lists\ipset-all-as16509-amazon.txt`

## Пользовательские списки

Их можно править вручную, встроенная проверка обновлений их не затирает:

- `lists\list-general-user.txt` — дополнительные домены
- `lists\list-exclude-user.txt` — домены-исключения
- `lists\ipset-exclude-user.txt` — IP-исключения

Файл `list-general-user.txt` нельзя оставлять пустым.

## Заметки по Mistfall Hunter

Игровые серверы Mistfall Hunter работают на AWS (AS16509). В ряде регионов РФ трафик до AWS может попадать под блок ~16 КБ. Игра использует нестандартный бинарный протокол поверх TCP; отдельный фильтр WinDivert ловит этот handshake.

Контекст:

- https://github.com/net4people/bbs/issues/490
- форк, с которого перенесены правки: локальный `Fork-zapret-for-MistfallHunter`

## Авторы исходников

- [bol-van/zapret](https://github.com/bol-van/zapret)
- [flowseal/zapret-discord-youtube](https://github.com/flowseal/zapret-discord-youtube) v1.10.1
- форк zapret для Mistfall Hunter (AWS / TTL / фильтр WinDivert)
