# Tailscale

> [!WARNING]
>
> Дополнительная информация о настройке и устранении проблем есть в документации [remote](https://docs.routerich.ru/ru/remote).

## Remote

- Создайте учетную запись [tailnet](https://remote.routerich.ru/create-tailnet).
- Система автоматически создаст уникальное имя сети и ключи (срок действия 1 год):
  - `Device Auth Key` для подключения устройств к сети.
  - `Management Key` для доступа к панели управления.

## Setup

- Перейдите в "Система" -> "Пакеты" и нажмите "Обновить список".
- Введите в фильтр `tailscale`.
- Обновите/установите пакеты: `luci-i18n-tailscale` и `luci-i18n-tailscale-ru`.
- Перейдите в "VPN" -> "Tailscale" -> "Основные настройки":
  - Включите параметр "Включить".
  - Укажите `https://rc.routerich.ru/` в поле "Адрес сервера".
  - Нажмите "Сохранить и применить" и обновите страницу.
  - Статус должен стать "Tailscale РАБОТАЕТ".
- Перейдите в "Службы" -> "Терминал" или подключитесь по SSH `ssh root@192.168.1.1`.
- Остановите процесс:

```bash
tailscale down
```

- Запустите процесс с параметрами и ключом доступа:

```bash
tailscale up --reset --force-reauth --accept-routes --advertise-exit-node --advertise-routes=192.168.1.0/24 --hostname=routerich --login-server=https://rc.routerich.ru/ --snat-subnet-routes=false --auth-key=DEVICE_AUTH_KEY
```

## Network

- Перейдите в [Панель управления](https://remote.routerich.ru/devices) и укажите `Device Auth Key` для авторизации.
- В разделе "Devices" есть все доступные устройства, подключенные по `Device Auth Key`.
- Включите функцию `Exit Node` (превращает выбранное устройство в домашней сети или на удаленном сервере в персональный VPN-шлюз) на устройстве, к которому вы подключаетесь.

## Materials

- [Remote](https://docs.routerich.ru/ru/remote)
- [Devices](https://remote.routerich.ru/devices)
