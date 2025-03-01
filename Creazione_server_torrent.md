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
