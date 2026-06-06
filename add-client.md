# Adding a new WireGuard client

## 1. Generate client keys

Generate private and public keys:

wg genkey | tee client_private.key | wg pubkey > client_public.key

Check generated keys:

cat client_private.key
cat client_public.key

---

## 2. Add client to server

Edit WireGuard configuration:

nano /etc/wireguard/wg0.conf

Add a new peer:

[Peer]
PublicKey = CLIENT_PUBLIC_KEY
AllowedIPs = 10.0.0.4/32

Apply changes:

wg syncconf wg0 <(wg-quick strip wg0)

Verify:

wg show

---

## 3. Create client configuration

Create file `client.conf`:

[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.0.0.4/24
DNS = 8.8.8.8

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = SERVER_IP:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25

---

## 4. Import configuration on client

Import `client.conf` into the WireGuard application and enable the tunnel.

---

## 5. Verify connection

On the server:

wg show
