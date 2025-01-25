# Guida alla Configurazione di un Sito Web su Raspberry Pi con Nginx e Cloudflare Tunnel

Questa guida descrive come configurare un server web Nginx sul tuo Raspberry Pi per ospitare un sito web, configurare un tunnel Cloudflare per l'accesso remoto e personalizzare una pagina 404.

---

## Prerequisiti

- Un Raspberry Pi con Raspbian o un altro sistema operativo Linux installato.
- Un account Cloudflare configurato.
- Un dominio (in questo esempio `alexviolatto.com`).
- Accesso SSH al Raspberry Pi.

---

## 1. Installazione di Nginx

Per prima cosa, dobbiamo installare **Nginx**, che è il server web che utilizzeremo per ospitare il nostro sito.

1. Aggiorna i pacchetti:
    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

2. Installa Nginx:
    ```bash
    sudo apt install nginx
    ```

3. Verifica che Nginx sia in esecuzione:
    ```bash
    sudo systemctl status nginx
    ```

Se tutto è corretto, dovresti vedere Nginx in esecuzione.

---

## 2. Creazione della Pagina Web

1. Crea la cartella per il tuo sito web:
    ```bash
    sudo mkdir -p /var/www/alexviolatto.com
    ```

2. Aggiungi un file `index.html` nella cartella del tuo sito:
    ```bash
    sudo nano /var/www/alexviolatto.com/index.html
    ```

3. Inserisci il seguente contenuto HTML come homepage:

    ```html
    <html>
    <head>
        <title>Benvenuto su alexviolatto.com</title>
    </head>
    <body>
        <h1>Benvenuto su alexviolatto.com!</h1>
    </body>
    </html>
    ```

4. Salva e chiudi il file.

---

## 3. Configurazione di Nginx

1. Crea un file di configurazione per il tuo dominio:
    ```bash
    sudo nano /etc/nginx/sites-available/alexviolatto.com
    ```

2. Inserisci la seguente configurazione per il tuo dominio:

    ```nginx
    server {
        listen 80;
        server_name alexviolatto.com www.alexviolatto.com;

        root /var/www/alexviolatto.com;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }

        error_page 404 /404.html;

        location = /404.html {
            root /var/www/alexviolatto.com;
            internal;
        }

        # Logging
        access_log /var/log/nginx/alexviolatto.access.log;
        error_log /var/log/nginx/alexviolatto.error.log;
    }
    ```

3. Attiva il sito creando un collegamento simbolico:
    ```bash
    sudo ln -s /etc/nginx/sites-available/alexviolatto.com /etc/nginx/sites-enabled/
    ```

4. Verifica la configurazione di Nginx:
    ```bash
    sudo nginx -t
    ```

5. Ricarica Nginx per applicare le modifiche:
    ```bash
    sudo systemctl reload nginx
    ```

---

## 4. Creazione di una Pagina 404 Personalizzata

1. Crea il file `404.html` nella directory del tuo sito:
    ```bash
    sudo nano /var/www/alexviolatto.com/404.html
    ```

2. Inserisci il seguente contenuto HTML per una pagina 404 in stile Google:

    ```html
    <!DOCTYPE html>
    <html lang="it">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Pagina non trovata - alexviolatto.com</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                text-align: center;
                padding: 50px;
                background-color: #f7f7f7;
                color: #333;
            }

            h1 {
                font-size: 100px;
                color: #4285f4;
                margin: 0;
            }

            p {
                font-size: 24px;
                color: #666;
            }

            .search-box {
                margin-top: 30px;
            }

            input[type="text"] {
                padding: 10px;
                width: 300px;
                font-size: 18px;
                border: 1px solid #ccc;
                border-radius: 5px;
            }

            input[type="submit"] {
                padding: 10px 20px;
                background-color: #4285f4;
                color: white;
                border: none;
                border-radius: 5px;
                cursor: pointer;
            }

            input[type="submit"]:hover {
                background-color: #357ae8;
            }
        </style>
    </head>
    <body>
        <h1>404</h1>
        <p>Oops! La pagina che stai cercando non è stata trovata.</p>
        <div class="search-box">
            <form action="https://www.google.com/search" method="get" target="_blank">
                <input type="text" name="q" placeholder="Cerca su Google" required>
                <input type="submit" value="Cerca">
            </form>
        </div>
    </body>
    </html>
    ```

3. Salva e chiudi il file.

---

## 5. Configurazione del Tunnel Cloudflare

1. Vai al [Cloudflare Tunnel Dashboard](https://dash.cloudflare.com).
2. Crea un nuovo tunnel per il tuo dominio (es. `alexviolatto.com`).
3. Configura il tunnel:
   - Aggiungi il servizio `HTTP` o `HTTPS` (a seconda della configurazione del server).
   - Collega il tunnel al tuo Raspberry Pi (assicurati che Nginx stia eseguendo sulla porta 80 o 443).
4. Aggiungi un record DNS di tipo `CNAME` per il tuo dominio:
   - Nome: `alexviolatto.com`
   - Target: il nome del tunnel (es. `tunnelname.cloudflare-tunnel.com`).

---

## 6. Conclusione

Adesso il tuo sito `alexviolatto.com` è configurato e accessibile sia tramite HTTP che HTTPS attraverso il tunnel Cloudflare. Se desideri aggiungere altre pagine o sottodomini, puoi farlo modificando i file di configurazione Nginx e creando nuove cartelle nel percorso `/var/www`.

---

### **Download del `README.md`**

Per scaricare il file `README.md`, puoi cliccare sul link sottostante:

[Download README.md](sandbox:/mnt/data/README.md)
