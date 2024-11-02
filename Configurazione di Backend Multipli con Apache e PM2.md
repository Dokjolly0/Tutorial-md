
# README - Configurazione di Backend Multipli con Apache e PM2

## Obiettivo

L'obiettivo di questa configurazione è ospitare più backend Node.js su una singola istanza di server con **Apache** come proxy inverso per gestire il traffico HTTP. Ogni backend è eseguito su una porta diversa (ad esempio `3000`, `3001`), e Apache instrada il traffico a seconda della porta richiesta.

## Prerequisiti

- Server con Ubuntu
- Apache 2.4 installato
- Node.js e NPM installati
- PM2 per gestire le applicazioni Node.js
- Accesso SSH al server

## Passaggi completati

### 1. Installazione e Configurazione di Node.js e PM2

Abbiamo configurato **Node.js** e **PM2** per gestire i backend. Abbiamo usato **PM2** per garantire che i nostri server Node.js rimangano attivi in background anche dopo che una sessione SSH si chiude.

Ecco come abbiamo configurato i backend:

1. Installazione di **PM2**:
   ```bash
   sudo npm install -g pm2
   ```

2. Esecuzione dei backend su diverse porte (ad esempio `3000` e `3001`):
   ```bash
   pm2 start first-backend.js --name "backend1" --watch --port 3000
   pm2 start second-backend.js --name "backend2" --watch --port 3001
   ```

3. Verifica che i backend siano in esecuzione:
   ```bash
   pm2 list
   ```

### 2. Installazione e Configurazione di Apache

Abbiamo installato **Apache 2.4** per utilizzare il server come proxy inverso, che gestisce le richieste HTTP e le indirizza ai rispettivi backend in base alla porta richiesta.

1. Installazione di Apache:
   ```bash
   sudo apt update
   sudo apt install apache2
   ```

2. Abilitazione dei moduli proxy necessari:
   ```bash
   sudo a2enmod proxy
   sudo a2enmod proxy_http
   sudo systemctl restart apache2
   ```

#### Problemi riscontrati:
- **Errore "a2enmod: command not found"**: Durante l'abilitazione dei moduli, abbiamo riscontrato che il comando `a2enmod` non funzionava. Questo è stato risolto installando Apache correttamente e poi usando `a2enmod` per abilitare i moduli proxy necessari.

### 3. Configurazione di Apache per gestire più backend

Abbiamo configurato Apache in modo che potesse instradare le richieste HTTP verso i backend Node.js in esecuzione su diverse porte (`3000` e `3001`).

#### Passaggi:

1. Modifica del file di configurazione delle porte di Apache (`/etc/apache2/ports.conf`) per far sì che Apache ascolti anche su porte `3000` e `3001`:
   ```apache
   Listen 80
   Listen 3000
   Listen 3001
   ```

2. Configurazione di due **VirtualHost** per gestire il traffico su porte diverse:
   
   Modifica del file di configurazione del VirtualHost predefinito (`/etc/apache2/sites-available/000-default.conf`):

   ```apache
   # VirtualHost per la porta 3000
   <VirtualHost *:3000>
       ServerName www.alexviolatto.com

       # Inoltra tutto il traffico sulla porta 3000 al backend Node.js su localhost:3000
       ProxyPass / http://localhost:3000/
       ProxyPassReverse / http://localhost:3000/

       ErrorLog ${APACHE_LOG_DIR}/error_3000.log
       CustomLog ${APACHE_LOG_DIR}/access_3000.log combined
   </VirtualHost>

   # VirtualHost per la porta 3001
   <VirtualHost *:3001>
       ServerName www.alexviolatto.com

       # Inoltra tutto il traffico sulla porta 3001 al backend Node.js su localhost:3001
       ProxyPass / http://localhost:3001/
       ProxyPassReverse / http://localhost:3001/

       ErrorLog ${APACHE_LOG_DIR}/error_3001.log
       CustomLog ${APACHE_LOG_DIR}/access_3001.log combined
   </VirtualHost>
   ```

3. **Riavvio di Apache**:
   Dopo aver modificato i file di configurazione, abbiamo riavviato Apache:
   ```bash
   sudo systemctl restart apache2
   ```

4. Test delle configurazioni:
   - Per il backend sulla porta `3000`, abbiamo verificato che le richieste vengano inoltrate correttamente tramite:
     ```bash
     http://www.alexviolatto.com:3000/api/todos?showCompleted=true
     ```

   - Per il backend sulla porta `3001`, la richiesta corrispondente è stata:
     ```bash
     http://www.alexviolatto.com:3001/api/todos?showCompleted=true
     ```

#### Problemi riscontrati:
- **404 Not Found**: Inizialmente, ricevevamo un errore 404 quando tentavamo di accedere a uno dei backend. Questo era causato da una configurazione errata del **ProxyPass** e del percorso URL. Una volta corretta la configurazione dei percorsi nel file `000-default.conf`, il problema è stato risolto.

- **Risoluzione del DNS**: Abbiamo avuto problemi a risolvere il dominio `alexviolatto.com`, il che ci ha portato a verificare la configurazione DNS e il firewall per assicurare che il traffico fosse correttamente instradato al server.

### 4. Gestione del Firewall

Abbiamo dovuto aprire le porte `3000` e `3001` nel firewall per consentire il traffico esterno su queste porte. Questo è stato fatto con il seguente comando:

```bash
sudo ufw allow 3000
sudo ufw allow 3001
```

### 5. Verifica del Funzionamento

Una volta configurati i VirtualHost, i moduli proxy e PM2 per gestire i backend, abbiamo verificato che ogni backend rispondesse correttamente sulle rispettive porte.

- **Backend 1** (`localhost:3000`):
  ```bash
  http://www.alexviolatto.com:3000/api/todos?showCompleted=true
  ```

- **Backend 2** (`localhost:3001`):
  ```bash
  http://www.alexviolatto.com:3001/api/todos?showCompleted=true
  ```

### 6. Considerazioni Aggiuntive

- **Certificati SSL**: Non abbiamo ancora configurato certificati SSL per le porte `3000` e `3001`. Per l'uso in produzione, sarebbe ideale configurare HTTPS su tutte le porte tramite Apache con **Let's Encrypt** o un certificato SSL personalizzato.

- **Scalabilità**: Se hai bisogno di aggiungere altri backend, puoi semplicemente modificare il file di configurazione del VirtualHost e aggiungere altre porte.

## Conclusione

Seguendo questi passaggi, siamo riusciti a configurare correttamente più backend Node.js su porte diverse (`3000`, `3001`, ecc.) utilizzando Apache come proxy inverso. Apache è configurato per instradare il traffico HTTP verso il backend giusto in base alla porta richiesta, e PM2 gestisce l'esecuzione e la persistenza dei processi Node.js in background.

Se hai ulteriori backend da aggiungere, puoi seguire lo stesso processo, semplicemente configurando nuovi **VirtualHost** per le porte aggiuntive.
