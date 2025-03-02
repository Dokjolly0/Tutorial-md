# Setup e Configurazione di Transmission su Raspberry Pi

## Introduzione
Questa guida documenta il processo di installazione e configurazione di Transmission-daemon su un Raspberry Pi, incluso il superamento di problemi di connessione e autenticazione.

## 1. Installazione di Transmission-daemon
### Aggiornamento del sistema:
```bash
sudo apt update && sudo apt upgrade
```

### Installazione di Transmission:
```bash
sudo apt install transmission-daemon
```

### Arresto del demone per la configurazione:
```bash
sudo systemctl stop transmission-daemon
```

## 2. Configurazione di Transmission
Modifica del file di configurazione:
```bash
sudo nano /etc/transmission-daemon/settings.json
```

### Modifiche effettuate:
```json
"rpc-enabled": true,
"rpc-username": "dokjolly",
"rpc-password": "tua_password",
"rpc-whitelist-enabled": false,
"rpc-bind-address": "0.0.0.0",
"rpc-port": 9091,
"rpc-host-whitelist-enabled": false
```

> La password viene criptata al primo avvio dopo la modifica.

### Riavvio del servizio:
```bash
sudo systemctl start transmission-daemon
```

## 3. Apertura della Porta 9091 nel Firewall (UFW)
```bash
sudo ufw allow 9091/tcp
```

Verifica delle regole attive:
```bash
sudo ufw status
```

## 4. Verifica dell’Accesso
Dal Raspberry Pi:
```bash
curl http://localhost:9091/
```
Se restituisce `401: Unauthorized`, il servizio è attivo.

Dal PC in rete:
```bash
curl http://192.168.1.30:9091/
```
Se la connessione non va, verificare UFW e la whitelist RPC.

Accesso via browser:
```
http://192.168.1.30:9091/
```

## Problemi e Soluzioni
### 1. **401 Unauthorized**
- Il server richiede autenticazione. Inserire nome utente e password nel browser.

### 2. **Connessione rifiutata dal PC**
- Firewall UFW non permetteva il traffico sulla porta 9091. Risolto con:
  ```bash
  sudo ufw allow 9091/tcp
  ```

### 3. **Whitelist degli host bloccava connessioni esterne**
- Modificato:
  ```json
  "rpc-host-whitelist-enabled": false
  ```

## Conclusione
Ora Transmission è configurato e accessibile da remoto tramite interfaccia web. È possibile aggiungere torrent e monitorare i download anche fuori casa.

## 4. Cosa fare se Transmission Crasha
Se Transmission si arresta improvvisamente, segui questi passi per risolvere il problema.

### 4.1 Modifica delle impostazioni per ridurre il carico di sistema
Modifica il file di configurazione:
```bash
sudo nano /etc/transmission-daemon/settings.json
```
E aggiorna i seguenti parametri:
```json
"download-limit": 100,
"download-limit-enabled": true,
"upload-limit": 50,
"upload-limit-enabled": true,
"max-peers-global": 100,
"peer-limit-global": 150,
"peer-limit-per-torrent": 30,
"cache-size-mb": 16
```

### 4.2 Controllo dello spazio libero e della memoria
Verifica se lo spazio su disco è sufficiente:
```bash
df -h
```
Se la RAM è bassa, aumenta lo swap a 2GB:
```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
Per renderlo permanente, aggiungi questa riga a `/etc/fstab`:
```bash
/swapfile none swap sw 0 0
```

### 4.3 Riavvio del servizio
```bash
sudo systemctl restart transmission-daemon
```
Se il problema persiste, verifica i log:
```bash
journalctl -u transmission-daemon -n 50 --no-pager
```
Se Transmission viene terminato per mancanza di memoria, prova ad aggiungere un file di swap più grande o a ridurre il numero massimo di peer.

## Problemi e Soluzioni
### 1. **401 Unauthorized**
- Il server richiede autenticazione. Inserire nome utente e password nel browser.

### 2. **Connessione rifiutata dal PC**
- Firewall UFW non permetteva il traffico sulla porta 9091. Risolto con:
  ```bash
  sudo ufw allow 9091/tcp
  ```

### 3. **Whitelist degli host bloccava connessioni esterne**
- Modificato:
  ```json
  "rpc-host-whitelist-enabled": false
  ```

## Conclusione
Ora Transmission è configurato e accessibile da remoto tramite interfaccia web. È possibile aggiungere torrent e monitorare i download anche fuori casa.

## Allego il mio attuale file di configurazione `/etc/transmission-daemon/settings.json`:
```
{
    "alt-speed-down": 50,
    "alt-speed-enabled": false,
    "alt-speed-time-begin": 540,
    "alt-speed-time-day": 127,
    "alt-speed-time-enabled": false,
    "alt-speed-time-end": 1020,
    "alt-speed-up": 50,
    "bind-address-ipv4": "0.0.0.0",
    "bind-address-ipv6": "::",
    "blocklist-enabled": false,
    "blocklist-url": "http://www.example.com/blocklist",
    "cache-size-mb": 16,
    "dht-enabled": true,
    "download-dir": "/home/torrent-complete",
    "download-limit": 100,
    "download-limit-enabled": true,
    "download-queue-enabled": true,
    "download-queue-size": 5,
    "encryption": 1,
    "idle-seeding-limit": 30,
    "idle-seeding-limit-enabled": false,
    "incomplete-dir": "/home/torrent-incomplete",
    "incomplete-dir-enabled": true,
    "lpd-enabled": false,
    "max-peers-global": 100,
    "message-level": 1,
    "peer-congestion-algorithm": "",
    "peer-id-ttl-hours": 6,
    "peer-limit-global": 150,
    "peer-limit-per-torrent": 30,
    "peer-port": 51413,
    "peer-port-random-high": 65535,
    "peer-port-random-low": 49152,
    "peer-port-random-on-start": false,
    "peer-socket-tos": "default",
    "pex-enabled": true,
    "port-forwarding-enabled": true,
    "preallocation": 1,
    "prefetch-enabled": true,
    "queue-stalled-enabled": true,
    "queue-stalled-minutes": 30,
    "ratio-limit": 2,
    "ratio-limit-enabled": false,
    "rename-partial-files": true,
    "rpc-authentication-required": true,
    "rpc-bind-address": "0.0.0.0",
    "rpc-enabled": true,
    "rpc-host-whitelist": "",
    "rpc-host-whitelist-enabled": true,
    "rpc-password": "{27b1418aa9be47d107e0b8532c7f81ea22a7edb59VTsfryJ",
    "rpc-port": 9091,
    "rpc-url": "/transmission/",
    "rpc-username": "dokjolly",
    "rpc-whitelist": "127.0.0.1",
    "rpc-whitelist-enabled": false,
    "scrape-paused-torrents-enabled": true,
    "script-torrent-done-enabled": false,
    "script-torrent-done-filename": "",
    "seed-queue-enabled": false,
    "seed-queue-size": 10,
    "speed-limit-down": 100,
    "speed-limit-down-enabled": false,
    "speed-limit-up": 100,
    "speed-limit-up-enabled": false,
    "start-added-torrents": true,
    "trash-original-torrent-files": false,
    "umask": 2,
    "upload-limit": 50,
    "upload-limit-enabled": true,
    "upload-slots-per-torrent": 14,
    "utp-enabled": true,
    "watch-dir": "/home/torrent-watch",
    "watch-dir-enabled": true
}
```
