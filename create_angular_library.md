
# 📦 Guida Completa: Creare e Pubblicare una Libreria Angular su NPM

Questa guida spiega passo dopo passo come creare una libreria Angular standalone, compilarla e pubblicarla su [npmjs.com](https://www.npmjs.com).

---

## ✅ Requisiti

- Node.js ≥ 18
- Angular CLI ≥ 16
- Account su [npmjs.com](https://www.npmjs.com/signup)
- Login effettuato via `npm login`

---

## 🧱 1. Creazione del progetto e della libreria

```bash
npx @angular/cli@latest new my-workspace --create-application false
cd my-workspace
ng generate library auth-lib --standalone
```

---

## 🛠️ 2. Aggiunta di componenti alla libreria

```bash
ng generate component login --project=auth-lib --standalone
```

Modifica `projects/auth-lib/src/public-api.ts` per esportare il componente:

```ts
export * from './lib/login/login.component';
```

---

## 🏗️ 3. Build della libreria

```bash
ng build auth-lib
```

Il pacchetto sarà in `dist/auth-lib/`.

---

## 📝 4. Configurazione del package.json

Modifica `projects/auth-lib/package.json`:

```json
{
  "name": "nome-della-tua-libreria",
  "version": "1.0.0",
  "description": "Una libreria Angular personalizzata",
  "author": "Il Tuo Nome",
  "keywords": ["angular", "component", "auth", "login"],
  "license": "MIT"
}
```

---

## 🔑 5. Login su NPM

```bash
npm login
```

---

## 📦 6. Pubblicazione

```bash
cd dist/auth-lib
npm publish --access public
```

---

## ✅ 7. Uso della libreria in altri progetti

```bash
npm install nome-della-tua-libreria
```

Poi importa e usa i componenti nei tuoi file `.ts`.

---

## 🚨 Errori Comuni

| Errore | Soluzione |
|--------|-----------|
| `403 Forbidden` | Aggiungi `--access public` o cambia nome |
| `You must be logged in to publish` | Fai `npm login` |
| Il componente non viene trovato | Verifica gli export in `public-api.ts` |
| Mancano dipendenze | Usa `dependencies` e `peerDependencies` in `package.json` |

---

## 🧼 Consiglio: Ignora file inutili

Crea `.npmignore` nella root della libreria:

```
*.spec.ts
.storybook
*.stories.ts
```

---

## 🏁 Fine!

Hai pubblicato con successo la tua libreria Angular su npm 🎉
