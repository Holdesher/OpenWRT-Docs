<h1 align="center">OpenWRT-Docs</h1>

## Docs

- [Amnezia](docs/amnezia.md)
- [Podkop](docs/podkop.md)
- [ZeroBlock](docs/zeroblock.md)
- [Tailscale](docs/tailscale.md)
- [VLAN](docs/vlan.md)

## FAQ

- Если при установке или обновлении пакетов возникает ошибка, повторите попытку, выполните установку через терминал или переустановите пакет.
- Если ошибка возникает при установке пакетов через web UI:
  1. Перейдите в "Службы" -> "Терминал" или подключитесь по SSH: `ssh root@192.168.1.1`.
  2. Выполните `opkg update` для обновления списка пакетов.
  3. Выполните `opkg install PACKAGE` для установки пакета.
  4. Выполните `opkg upgrade PACKAGE` для обновления пакета.
  5. Выполните `opkg remove PACKAGE` для удаления пакета.
- Если после импорта `*.conf` пакеты интерфейса не передаются (`Rx = 0`), повторите импорт в режиме "Инкогнито" и с очищенным кэшем браузера.
- Если после изменений появились проблемы с сетью или сервисами, выполните переподключение к сети или перезапустите роутер.

## Materials

- [OpenWRT-Monitoring](https://github.com/Holdesher/OpenWRT-Monitoring)
- [OpenWRT-Specification](https://openwrt.org)
