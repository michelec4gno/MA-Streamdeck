# Avvio del bridge su grandMA3

Il bridge è un piccolo plugin Lua residente in MA. Legge gli attributi a intervalli regolari e invia i cambiamenti al Deck: evita che Stream Deck debba richiedere ogni lettura tramite la command line MA. I comandi di controllo continuano a usare OSC.

## Installazione

1. Aggiorna il plugin Stream Deck alla versione **3.21 fix2 (3.21.0.2)** e chiudi e riapri completamente l'app Stream Deck.
2. Copia **entrambi** i file seguenti, con questi nomi, nella cartella risorse di grandMA3 `grandMA3/shared/resource/lib_plugins`:
   - `MA_StreamDeck_Bridge.xml`
   - `MA_StreamDeck_Bridge.lua`
   Per trovare la cartella effettiva della tua installazione, puoi eseguire nella command line MA: `Lua "Printf(GetPath(Enums.PathType.PluginLibrary))"`. Il percorso completo appare nella Command Line History; su macOS/Windows la cartella di base può variare secondo la versione.
3. In MA apri il pool **Plugins** e l'editor di uno **slot vuoto**. Usa **Import** e seleziona `MA_StreamDeck_Bridge.xml` dalla cartella dei plugin. Non sovrascrivere un plugin del tuo show.
4. Avvia una volta il plugin **MA StreamDeck Bridge** dal pool. Lascia Stream Deck aperto con il profilo e i controlli MA visibili.
5. Sul tasto di stato del Deck verifica la scritta **Bridge attivo**. La prima installazione del codice di lettura può richiedere qualche secondo. Se non compare, controlla le impostazioni OSC già usate: IP, porta comandi, porta feedback e numero della linea feedback.

Il file XML usa lo schema di importazione con `ComponentLua` e `FileName` già presente nei plugin MA di riferimento, con `DataVersion="2.0.2.0"`. Non modifica lo show al di fuori dello slot nel quale scegli di importarlo. XML e Lua devono restare insieme durante l'importazione.

## Uso e arresto

Dopo un riavvio di MA o un caricamento dello show, avvia nuovamente **MA StreamDeck Bridge**. Non serve avviarlo a ogni giro di manopola o cambio pagina. Un secondo avvio sostituisce il precedente ciclo e non deve creare due lettori contemporanei.

Il bridge resta inattivo se Stream Deck non fornisce una configurazione valida. Dopo sei secondi senza rinnovo della connessione, interrompe letture e invii; resta disponibile per una riconnessione.

Per arrestarlo completamente, esegui nella command line MA:

```text
Lua "_G.MA322Runner=nil"
```

Per ripartire, avvialo di nuovo dal pool Plugins.

## Cosa è stato verificato

I test automatici eseguono il Lua effettivo con API MA simulate: 20 letture attributi al secondo, 10 letture encoder al secondo, heartbeat una volta al secondo, arresto dopo scadenza della connessione, ripartenza e protezione da cicli duplicati. La trasmissione dei dati è filtrata per inviare cambiamenti e aggiornamenti di stato, non ogni lettura identica.

L'importazione e la risposta su macOS/MA 2.5.1.1, console e Windows richiedono ancora una prova reale. Il bridge usa le API pubbliche MA e OSC: non è il protocollo di una superficie MA nativa e non garantisce la stessa latenza. Rimane il limite già documentato sulla ricostruzione di pagine encoder con layout o scroll personalizzati.
