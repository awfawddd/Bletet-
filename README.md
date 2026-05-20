# Bluetooth Music

App Android che scansiona i dispositivi Bluetooth (auto, casse, cuffie...),
ti porta ad accoppiarli, e fa partire la musica — dal telefono o da YouTube.

Progetto in Java, minSdk 31 (Android 12).

---

## Come compilarla

### Strada A — dal telefono, con GitHub Actions (consigliata)

Non serve installare niente sul telefono. Compilano i server di GitHub.

1. Vai su github.com e crea una **nuova repository** (puoi farlo dal browser
   del telefono). Chiamala per esempio `BluetoothMusic`.

2. Carica tutti i file di questo progetto nella repo. Dal telefono il modo
   piu' comodo e' l'app **GitHub** ufficiale, oppure dal browser con il
   pulsante "Add file" → "Upload files" (puoi trascinare la cartella).
   IMPORTANTE: carica anche la cartella nascosta `.github`, contiene la
   ricetta di compilazione.

3. Appena fai il caricamento, vai nella scheda **Actions** della repo.
   Vedrai partire da sola la voce "Compila APK". Aspetta il pallino verde
   (qualche minuto).

4. Apri il risultato → in fondo, sezione **Artifacts** → scarica
   `BluetoothMusic-apk`. Dentro c'e' il file `app-debug.apk`.

5. Apri l'APK sul telefono per installarlo. Android ti chiedera' di
   autorizzare le "app da origini sconosciute": e' normale, accetta.

### Strada B — su PC con Android Studio

1. Installa **Android Studio** (https://developer.android.com/studio).
2. Apri Android Studio → **Open** → seleziona la cartella `BluetoothMusic`.
3. Aspetta la sincronizzazione di Gradle (scarica le librerie).
4. Collega il telefono via USB con il *debug USB* attivo, premi **▶ Run**.

> Nota: il file `gradle/wrapper/gradle-wrapper.jar` non e' incluso.
> Lo generano da soli sia Android Studio (all'apertura) sia il workflow
> di GitHub Actions. Non devi fare nulla.

> Importante: NON usare l'emulatore per testare il Bluetooth.
> L'emulatore non ha un Bluetooth vero, quindi non vedrai né auto né casse.
> Per la parte Bluetooth serve un telefono fisico.

---

## Come è fatta (i file)

| File | Cosa fa |
|------|---------|
| `MainActivity.java`      | La schermata principale con i 3 pannelli |
| `PermissionHelper.java`  | Chiede i permessi Bluetooth/audio a runtime |
| `BluetoothScanner.java`  | Scansiona i dispositivi e ne capisce il tipo |
| `DispositivoBT.java`     | Modello dati di un dispositivo trovato |
| `MusicPlayerService.java`| Service che suona la musica in background |
| `LettoreMusica.java`     | Legge i brani dalla memoria del telefono |
| `Brano.java`             | Modello dati di una canzone |

---

## Cosa fa e cosa NON può fare (leggi qui!)

**Cosa fa:**
- Scansiona tutti i dispositivi Bluetooth vicini e quelli già accoppiati.
- Riconosce il tipo (auto, speaker, cuffie...) dall'icona.
- Suona la musica del telefono, anche in background, sull'output
  Bluetooth attivo (cassa o auto).
- Player YouTube ufficiale nella schermata YouTube.

**Cosa NON può fare (e perché):**
- **Non può accoppiare da sola un nuovo speaker/auto.** L'accoppiamento
  audio (profilo A2DP) lo gestisce Android a livello di sistema, per
  sicurezza. Per questo l'app apre le *Impostazioni Bluetooth*: lì
  accoppi il dispositivo una volta, poi la musica ci esce sopra da sola.
- **Non scarica l'audio da YouTube.** Sarebbe contro i termini di
  YouTube. La schermata YouTube usa il sito/player ufficiale: legale,
  ma l'audio suona solo con l'app aperta.

Per la musica "vera" in macchina, usa la sezione **Musica** con i tuoi
file: è la parte che funziona davvero bene in background.
