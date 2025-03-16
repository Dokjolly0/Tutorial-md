# Telegram Anime Bot - Guida Completa

Questa guida dettagliata ti aiuterà a creare, configurare, avviare e hostare un bot Telegram per cercare anime, utilizzando la libreria **Telethon**.

## 📌 Requisiti
- **Account Telegram**
- **API ID & API Hash** da [my.telegram.org](https://my.telegram.org/)
- **Python 3.8+** installato
- **Librerie necessarie** (Telethon, aiogram, etc.)

---

## 1️⃣ **Creare l'API ID & API Hash**
Per poter interagire con Telegram, devi ottenere le tue credenziali API:
1. Vai su [my.telegram.org](https://my.telegram.org/)
2. Accedi con il tuo numero di telefono
3. Clicca su **"API Development Tools"**
4. Compila i campi richiesti:
   - **Nome dell'app** → Un nome a tua scelta
   - **Short Name** → Nome breve
   - **Platform** → Scegli **Android** o **Desktop**
   - **URL** → Se richiesto, inserisci `https://alexviolatto.com/`
5. Dopo la registrazione, otterrai:
   - **API ID** → Numero univoco
   - **API Hash** → Chiave segreta

---

## 2️⃣ **Creare e configurare il bot Telegram**
Per ottenere il token del bot:
1. Apri Telegram e cerca `@BotFather`
2. Scrivi il comando `/newbot`
3. Segui le istruzioni e ottieni il tuo **TOKEN API**

---

## 3️⃣ **Configurare il progetto**
1. **Clona il repository o crea una cartella**:
   ```bash
   mkdir tgAnimeBot && cd tgAnimeBot
   ```
2. **Crea un ambiente virtuale** (opzionale, ma consigliato):
   ```bash
   python -m venv venv
   source venv/bin/activate  # Su Linux/macOS
   venv\Scripts\activate  # Su Windows
   ```
3. **Installa le dipendenze**:
   ```bash
   pip install telethon aiogram
   ```
4. **Crea un file di configurazione `.env`** e aggiungi:
   ```ini
   API_ID=TUO_API_ID
   API_HASH=TUO_API_HASH
   BOT_TOKEN=IL_TUO_TOKEN
   ```

---

## 4️⃣ **Avviare il bot**
Esegui il bot con:
```bash
python bot.py
```
Se tutto è corretto, dovresti vedere:
```plaintext
✅ Bot avviato! Aspettando messaggi...
```
Per testarlo, apri Telegram e scrivi `/start` al bot.

---

## 5️⃣ **Hostare il bot per tenerlo sempre online**
Puoi usare diverse piattaforme:

### **Opzione 1: Heroku** (gratuito)
1. Installa la CLI di Heroku:
   ```bash
   curl https://cli-assets.heroku.com/install.sh | sh  # Linux/macOS
   choco install heroku-cli  # Windows
   ```
2. Accedi:
   ```bash
   heroku login
   ```
3. Crea un'app su Heroku:
   ```bash
   heroku create tuo-bot-name
   ```
4. Aggiungi i file e fai il deploy:
   ```bash
   git init
   git add .
   git commit -m "Deploy bot"
   git push heroku main
   ```

### **Opzione 2: VPS con Systemd**
Se hai un server Linux:
1. Crea un servizio Systemd:
   ```bash
   sudo nano /etc/systemd/system/tgAnimeBot.service
   ```
2. Aggiungi il contenuto:
   ```ini
   [Unit]
   Description=Bot Telegram Anime
   After=network.target
   
   [Service]
   User=root
   WorkingDirectory=/home/utente/tgAnimeBot
   ExecStart=/usr/bin/python3 bot.py
   Restart=always
   
   [Install]
   WantedBy=multi-user.target
   ```
3. Abilita e avvia il servizio:
   ```bash
   sudo systemctl enable tgAnimeBot
   sudo systemctl start tgAnimeBot
   ```

Ora il bot rimarrà attivo anche dopo un riavvio.

---

## 6️⃣ **Come il bot risponde ovunque?**
Il bot non richiede un dominio o un IP pubblico perché:
✅ Si connette ai server Telegram.
✅ Telegram gestisce la comunicazione tra bot e utenti.
✅ Finché il server è acceso, il bot riceverà i messaggi.

---

## 📌 **Debug e Log**
Se il bot non risponde:
```bash
ps aux | grep bot.py  # Controlla se il processo è attivo
journalctl -u tgAnimeBot.service -f  # Log di Systemd
heroku logs --tail  # Log di Heroku
```

---

## 🚀 **Conclusione**
Ora il tuo bot Telegram è attivo e pronto per essere usato! 🎉
Se hai domande, miglioramenti o vuoi aggiungere nuove funzionalità, sentiti libero di contribuire al codice!

---

### **📬 Contatti**
Se hai bisogno di supporto, puoi contattarmi su Telegram o GitHub. 😊
