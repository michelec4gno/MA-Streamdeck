# GrandMA3 3.21 fix2 — versione interna 3.21.0.2

Per scaricare un file aprilo e premi **Download raw file**.

1. Scarica e installa **GrandMA3_v3_21_fix2.streamDeckPlugin**. Chiudi e riapri completamente Stream Deck. Non serve reimportare il profilo3.21.
2. Per la modalità rapida scarica **MA_StreamDeck_Bridge.zip** e segui [AVVIO_BRIDGE.md](AVVIO_BRIDGE.md): importa XML+Lua in uno slot Plugins vuoto su MA e avvialo una volta.
3. Il tasto Status sul Deck deve mostrare **Bridge attivo**. Senza bridge resta un fallback OSC limitato.

**GrandMA3_v3_21_fix2_bundle.zip** include tutto: plugin, profilo, bridge, sorgenti e istruzioni.

La fix2 elimina i caricamenti Lua ripetuti, conserva le assegnazioni durante brevi ritardi e accorpa i ridisegni. Il bridge richiede campionamento20Hz dei valori e10Hz della selezione, inviando i cambiamenti. Prova simulata dopo l'avvio:15messaggi Deck→MA in30secondi, zero polling/upload ripetuti. MA continua a usare SendOSC per il feedback, quindi non si promette cronologia completamente vuota.

65test JavaScript e39test Lua passati. Frequenza/risposta effettiva, importazione e funzionamento sul tuo MA2.5.1.1 richiedono la prova reale: non si promette equivalenza con una superficie MA nativa. Restano i limiti sulle pagine encoder personalizzate, readout per finestra e lettura del programmer attivo.

I file fix1 restano come riferimento precedente; per questo aggiornamento usa **fix2**.
