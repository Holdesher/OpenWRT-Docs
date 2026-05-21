<h1 align="center">OpenWRT-Docs</h1>

## Docs

- [Amnezia](docs/amnezia.md)
- [Podkop](docs/podkop.md)
- [ZeroBlock](docs/zeroblock.md)
- [TailScale](docs/tailscale.md)

## FAQ

- Если появились ошибки при установке или обновлении пакетов системы: повторите попытку снова, воспользуйтесь терминал или переустановите пакет.

- Если есть неявная ошибка при установке пакетов через UI:
	1. Перейдите "Службы" -> "Терминал" или подключитесь по SSH `ssh root@192.168.1.1`.
	2. Выполните `opkg update` — обновление списка пакетов.
	3. `opkg install PACKAGE` — установка пакета.
	4. `opkg upgrade PACKAGE` — обновление пакета.
	5. `opkg remove PACKAGE` — удаление пакета.

- Если есть проблемы с работой интерфесом, а именно отсуствие пакетов (Rx = 0), то импорт и передача конфигурации `*.conf`, нужно выполнять в браузере с "Инкогнито" и без кэша.

- Если после изменений или настроек возникают проблемы с сетью или работой сервисов, подождите, выполните переподключение к сети или перезапустите роутер.

## Materials

- [OpenWRT-Monitoring](https://github.com/Holdesher/OpenWRT-Monitoring)
- [OpenWRT-Specification](https://openwrt.org)
