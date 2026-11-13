# Shattrath Trade — idle delle professioni di TBC 2.4.3

Il gioco è tutto in due file:

- `index.html` contiene il gioco
- `config.js` contiene la configurazione (qui incolli i dati di Firebase per giocare in due)

Se apri `index.html` senza configurare niente, il gioco parte in **modalità solo**: i progressi restano salvati nel browser di quel dispositivo.

Per giocare con il tuo amico, e passare dal PC al telefono con lo stesso personaggio, servono due cose: un database gratuito (Firebase) e un posto dove mettere i file (Netlify o GitHub Pages). Ci vogliono circa 10 minuti, una volta sola.

---

## 1. Crea il database (Firebase, gratis)

1. Vai su https://console.firebase.google.com e accedi con un account Google.
2. **Create a project** → dagli un nome (es. `shattrath-trade`) → Google Analytics puoi disattivarlo → crea.
3. Nel menu a sinistra: **Build → Realtime Database → Create Database**.
   - Location: **Belgium (europe-west1)**
   - Modalità: **Start in locked mode**
4. Apri la scheda **Rules**, cancella tutto e incolla questo, poi **Publish**:

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "guilds": {
      "$guild": { ".read": true, ".write": true }
    }
  }
}
```

   Il database non si può elencare dall'esterno: chi non conosce il codice gilda non vede niente. Il codice gilda funziona quindi come una password condivisa, sceglilo non banale.

5. Torna su **Project Overview** (icona ingranaggio → **Project settings**), in basso clicca sull'icona web `</>`, registra l'app con un nome qualsiasi (niente Firebase Hosting). Ti mostra un blocco `firebaseConfig = { ... }`.
6. Apri `config.js` e sostituisci `firebase: null,` con i tuoi valori, così:

```js
window.SHATTRATH_CONFIG = {
  firebase: {
    apiKey: "AIza...",
    authDomain: "shattrath-trade.firebaseapp.com",
    databaseURL: "https://shattrath-trade-default-rtdb.europe-west1.firebasedatabase.app",
    projectId: "shattrath-trade",
    appId: "1:...:web:..."
  },
};
```

   Controlla che ci sia `databaseURL`: se manca, copialo dalla pagina Realtime Database (è l'indirizzo in alto, quello che finisce con `firebasedatabase.app`).

Il piano gratuito (Spark) basta e avanza: 1 GB di dati e 10 GB di traffico al mese, due giocatori ne usano una frazione minima.

## 2. Metti il gioco online

**Opzione A, la più veloce: Netlify Drop**

1. Vai su https://app.netlify.com/drop (serve un account gratuito).
2. Trascina la cartella con `index.html` e `config.js`.
3. Ti dà un indirizzo tipo `https://nome-a-caso.netlify.app`. Puoi rinominarlo in *Site settings*.

Per aggiornare il gioco in futuro, trascini di nuovo la cartella nella sezione *Deploys* del sito.

**Opzione B: GitHub Pages**

1. Crea un repository (anche pubblico), carica `index.html` e `config.js`.
2. *Settings → Pages → Branch: main / root → Save*.
3. Dopo un minuto il gioco è su `https://<tuo-utente>.github.io/<repo>/`.

Nota: la `apiKey` di Firebase non è un segreto, è fatta per stare nelle pagine web. La protezione vera sono le regole del database del punto 1.

## 3. Giocare

- Tu e il tuo amico aprite lo stesso indirizzo e inserite **lo stesso Guild Code**.
- Ognuno sceglie nome personaggio e PIN. Con nome + PIN riprendi il personaggio da qualsiasi dispositivo.
- Un personaggio può essere aperto su un dispositivo alla volta: se entri dal telefono, la scheda del PC si scollega (basta ricaricarla per riprenderlo da lì).
- Sul telefono: apri il sito e usa *Aggiungi a schermata Home*, così si apre a schermo intero come un'app.

## Come funziona

- **Va avanti anche quando non giochi.** Imposti un'attività (raccolta o combattimento) e una coda di crafting: al rientro il gioco calcola tutto quello che è successo, fino a 24 ore di assenza.
- **Professioni**: 2 primarie a scelta tra 10, più Fishing gratis. Skill 1–375 con i colori originali delle ricette e dei nodi (arancio, giallo, verde, grigio), ranghi da Apprentice a Master da allenare dal trainer.
- **Cose condivise con il tuo amico**: banca di gilda, chat di gilda, mercato dell'Auction House (se uno dei due svende un materiale, il prezzo scende per entrambi e poi risale col tempo), e il progetto comune dell'**Offensiva del Sole Infranto** sull'isola di Quel'Danas (patch 2.4), in 6 fasi che danno bonus a tutta la gilda.
- **Trade chat**: giocatori finti chiedono materiali e pagano più dell'asta. Le richieste arrivano anche mentre sei offline.
- **Easter egg**: ce ne sono diversi. Qualche indizio: prova i comandi in chat, il Konami code sulla tastiera, il logo, i link in Trade chat, i messaggi sussurrati dei gold seller, e pesca in Terokkar.

## Problemi comuni

- **"Could not connect"**: `databaseURL` sbagliato o mancante in `config.js`, oppure le regole non sono state pubblicate.
- **Le icone non si vedono**: sono caricate da Wowhead (`wow.zamimg.com`). Se una non carica, al suo posto compaiono le iniziali dell'oggetto.
- **Voglio ricominciare**: cambia nome del personaggio, oppure in Firebase cancella il nodo `guilds/<codice>/saves/<nome>`.

Progetto amatoriale e non commerciale. World of Warcraft, nomi e icone degli oggetti © Blizzard Entertainment.
