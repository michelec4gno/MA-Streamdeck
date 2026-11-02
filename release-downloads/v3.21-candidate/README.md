# GrandMA3 3.21 fix1 — versione interna 3.21.0.1

Scarica **GrandMA3_v3_21_fix1.streamDeckPlugin** con Download raw file. Installa l'aggiornamento e chiudi e riapri completamente Stream Deck. Non serve reimportare il profilo 3.21.

Corregge il comando Lua lungo che poteva essere troncato da MA con unfinished string near <eof>, bloccando le manopole su In attesa MA. Nuovo trasporto in frammenti inferiori a 900 byte, ricomposizione completa prima dell'esecuzione, cache e recupero automatico dopo perdita di frammenti o riavvio.

38 test JavaScript e 19 test Lua passati. Non eseguito su MA fisico; permangono i limiti sulla ricostruzione esatta di layout encoder personalizzati già documentati.
