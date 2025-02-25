
# Random Anime Generator - Installazione e Configurazione

Questa guida descrive i passaggi per installare e configurare l'applicazione Random Anime Generator (sia backend che frontend) su un server Ubuntu.

## 1. Aggiornamento del Sistema
Aggiorna il sistema operativo per garantire che tutte le dipendenze siano aggiornate:

```bash
sudo apt update && sudo apt upgrade
```

---

## 2. Installazione di NVM (Node Version Manager)
NVM permette di gestire facilmente le versioni di Node.js:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
```

---

## 3. Installazione di Node.js
Installa l'ultima versione di Node.js e impostala come predefinita:

```bash
nvm install node
nvm use node
```

---

## 4. Installazione di TypeScript
TypeScript viene utilizzato per il backend dell'applicazione:

```bash
npm install -g typescript
```

---

## 5. Installazione di PM2 (Process Manager)
PM2 permette di eseguire e gestire l'applicazione in background:

```bash
npm install -g pm2
```

---

## 6. Installazione di Nginx
Nginx funge da server web per il frontend:

```bash
sudo apt install nginx
```

---

## 7. Installazione di Git
Per clonare i repository:

```bash
sudo apt install git
```

---

## 8. Clonazione e Configurazione del Backend
Clona il repository del backend e installa le dipendenze:

```bash
git clone <URL_DEL_REPOSITORY>
cd <NOME_DEL_REPOSITORY>
npm i
npm run build  # "build": "tsc"
```

---

## 9. Clonazione e Configurazione del Frontend
Clona il repository del frontend e installa le dipendenze:

```bash
git clone <URL_DEL_REPOSITORY>
cd <NOME_DEL_REPOSITORY>
npm i
npm run build  # "build": "ng build --configuration=production --base-href="/random-anime-generator/"
```

- Per `powershell`: 
  ```bash
  ng build --configuration=production --base-href="/random-anime-generator/"
  ```
- Per `bash`: 
  ```bash
  ng build --configuration=production --base-href=//random-anime-generator/
  ```
- Per `cmd`: 
  ```bash
  ng build --configuration=production --base-href="/random-anime-generator/"
  ```

---

## 10. Configurazione delle Variabili di Ambiente
Crea o modifica il file `.env` per includere le configurazioni necessarie come URL del database, chiavi API, ecc.

---

## 11. Configurazione del Firewall
Abilita le porte necessarie per SSH e il backend:

```bash
sudo ufw allow 22/tcp  # per SSH
sudo ufw allow 3000/tcp  # per il backend
sudo ufw enable  # abilita UFW
sudo ufw status verbose  # verifica le regole attive
```

---

## 12. Avvio del Backend con PM2
Naviga nella cartella del backend ed avvia l'app:

```bash
pm2 start <NOME_DEL_FILE_DELL_APP> --name todo-app-backend
pm2 save
pm2 startup
# Incolla il link di risposta nel terminale
pm2 save
```

---

## 13. Avvio del Frontend con Nginx
Configura Nginx per servire il frontend:

- Crea la cartella del sito:

```bash
sudo mkdir -p /var/www/alexviolatto.com/
```

- Crea un file di configurazione per Nginx:

```bash
sudo nano /etc/nginx/sites-available/alexviolatto.com
```

- Esempio di configurazione Nginx:

```nginx
server {
    listen 80;
    server_name alexviolatto.com www.alexviolatto.com;

    root /var/www/alexviolatto.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location /random-anime-generator/ {
        alias /var/www/alexviolatto.com/random-anime-generator/;
        index index.html;
        try_files $uri $uri/ /random-anime-generator/index.html;
    }

    location /random-anime-generator/api/ {
        proxy_pass http://localhost:3000/api/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    error_page 404 /404.html;

    location = /404.html {
        root /var/www/alexviolatto.com;
        internal;
    }

    access_log /var/log/nginx/alexviolatto.access.log;
    error_log /var/log/nginx/alexviolatto.error.log;
}
```

- Abilita la configurazione e controlla lo stato di Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/alexviolatto.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl status nginx
```

---

## 14. Risoluzione dei Problemi Comuni
- **Problemi di Connessione Esterna:** Verifica le impostazioni del firewall dal dashboard del VPS.
- **Errore di Accesso Non Autorizzato:** Controlla il token di autenticazione e le variabili di ambiente.
- **PM2 Non Mostra Log o Server Non Si Avvia:** Verifica la porta del backend e i log di PM2.
- **Firewall Locale (UFW) Non Funzionante:** Controlla le regole del firewall a livello di provider.

---

## Licenza
Questo progetto è rilasciato sotto la licenza MIT. Consulta il file `LICENSE` per ulteriori dettagli.
