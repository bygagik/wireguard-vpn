# Добавление нового клиента WireGuard

## 1. Сгенерировать ключи клиента

```bash
wg genkey | tee client2_private.key | wg pubkey > client2_public.key
```

Проверить ключи:

```bash
cat client2_private.key
cat client2_public.key
```

## 2. Добавить клиента на сервер

Открыть конфигурацию сервера:

```bash
nano /etc/wireguard/wg0.conf
```

Добавить в конец файла:

```ini
[Peer]
PublicKey = CLIENT2_PUBLIC_KEY
AllowedIPs = 10.0.0.4/32
```

Сохранить изменения.

Применить конфигурацию:

```bash
wg syncconf wg0 <(wg-quick strip wg0)
```

Проверить:

```bash
wg show
```

---

## 3. Создать конфигурацию клиента

Создать файл `client2.conf`:

```ini
[Interface]
PrivateKey = CLIENT2_PRIVATE_KEY
Address = 10.0.0.4/24
DNS = 8.8.8.8

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = SERVER_IP:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

## 4. Передать конфигурацию клиенту

Импортировать файл `client2.conf` в приложение WireGuard:

## 5. Проверить подключение

На клиенте включить VPN.

На сервере выполнить:

```bash
wg show
```

