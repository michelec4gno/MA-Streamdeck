# Verifica della 3.21 fix2 — 3.21.0.2

Ambiente cloud Linux, Node 24.19.0, Python 3.12.14, Lupa 2.6. Comando riproducibile:

```sh
MA3_TEST_PYTHON=/workspace/.venvs/ma3-tests/bin/python bash scripts/test.sh
```

**65 test JavaScript e 39 test Lua passati**, nessun fallimento o test saltato. Il JavaScript è quello distribuito nel plugin; i test Lua eseguono il codice effettivo generato e il bridge residente con API MA simulate. Test UDP reali su localhost verificano il codec e il trasporto dei comandi. Validazione pacchetti: 21 azioni, 225 collegamenti del profilo, tutti i moduli JS e corrispondenza XML→Lua del bridge.

## Misure di traffico

Con quattro attributi, la revisione precedente produceva circa 553 comandi Deck→MA in 30 secondi, incluse le reinstallazioni periodiche. Nella fix2:

| Scenario simulato | Risultato |
| --- | --- |
| Avvio e installazione iniziale | 53 comandi brevi, costo separato dall'uso continuo |
| Bridge attivo, 30 secondi dopo ACK | 15 messaggi lease; zero polling e zero upload |
| Fallback, 30 secondi | 120 richieste aggregate; 186 comandi includendo avvio e lease |
| Cambio pagina/selezione/layer dopo ACK | Una breve configurazione; zero nuovi upload |
| Nessuna pagina di attributi/bank visibile, 30 secondi | Un messaggio di disattivazione, poi nessun polling/upload |
| Quattro valori ricevuti nello stesso pacchetto | Un solo aggiornamento grafico del proprietario, nessuna animazione per token |

Queste misure contano i comandi inviati **dal Deck a MA**. I messaggi `SendOSC` di ritorno da MA sono ulteriori: cambiamenti di valore, conferme e heartbeat. Le fonti compilate non inviano ripetutamente lo stesso layout dopo conferma; valori invariati hanno un aggiornamento lento. I test combinano inoltre le fonti compilate reali con il bridge residente: a riposo resta il solo heartbeat; un valore modificato viene inviato al tick successivo.

Il campionamento richiesto nel bridge è 20 Hz per valori e 10 Hz per selezione encoder. Il tempo reale dipende dallo scheduling MA, dalla rete e dal computer: le prove simulate non misurano la latenza del tuo sistema.

## Regressioni coperte e limiti

Coperti ritardi di 400–1.200 ms senza richieste logiche sovrapposte, risposte lente/mancanti, perdita/riordino/duplicazione dei frammenti, cache MA mancante, ripartenza del bridge, protezione da lettori duplicati, valori locali che non arretrano per un campione subito precedente, nessuno svuotamento delle assegnazioni confermate dopo due secondi. Conservati i test dei 94 preset originali, Store/Please/Pool/Executor, range fisici e layer temporali.

**Non verificati su hardware:** importazione Stream Deck e bridge MA, comportamento sul tuo macOS/MA 2.5.1.1, console o Windows. Non si promette equivalenza con una superficie MA nativa. Restano i limiti di ricostruzione delle pagine encoder con layout/scroll arbitrari, readout forzato per finestra e lettura del solo programmer attivo. Store mantiene il buffer locale dell'originale.
