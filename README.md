# Bilanciatore Squadre 5vs5 / 6vs6

Applicazione web a **file singolo** (HTML + CSS + JavaScript vanilla, nessun framework, nessun backend) per creare una lista di giocatori e generare automaticamente due squadre il più possibile equilibrate, in formato 5vs5 o 6vs6.

## Demo / Utilizzo

Basta aprire `index.html` in un browser — funziona anche offline (a parte il caricamento di Tailwind da CDN al primo avvio).

## Funzionalità

- Inserimento giocatori con statistiche (Attacco, Difesa, Resistenza, Velocità, Tecnica) su scala 1–10.
- Flag "Portiere fisso".
- Campo "Gruppo / Relazione" per tenere insieme giocatori che vogliono stare nella stessa squadra.
- Selettore di **formato partita**: 5vs5 (10 giocatori) o 6vs6 (12 giocatori), ricordato tra un refresh e l'altro.
- Tabella roster con **modifica inline dell'intera riga** (clic sulla sezione modifica: nome, ruolo, gruppo e tutte le statistiche diventano editabili direttamente in tabella) ed eliminazione giocatori.
- Salvataggio automatico in `localStorage` (i dati restano dopo un refresh, ma solo su quel browser/dispositivo).
- Algoritmo di bilanciamento che prova tutte le combinazioni possibili (o tutte le combinazioni di gruppi/blocchi) e sceglie quella più equilibrata, guardando sia il punteggio totale sia l'equilibrio statistica per statistica (vedi sotto).
- Se i gruppi impostati squilibrano troppo le squadre, l'app mostra **entrambe le composizioni**: quella che rispetta i gruppi e, sotto, la migliore possibile ignorandoli.
- Slide finale con nomi, ruolo, gruppo, punteggio totale di squadra e differenza — **senza mostrare i punteggi individuali**, per non creare confronti scomodi tra giocatori.

## Come funziona l'algoritmo

1. **Score del giocatore**: media pesata delle 5 statistiche. Attacco, Difesa e Tecnica pesano 1.0, Resistenza e Velocità pesano 0.8 (incidono un po' meno nel gioco a 5/6).
2. **Il problema della sola media**: un giocatore 10/10/10/1/1 e uno 6/6/6/6/6 hanno lo stesso Score medio, ma sono profili molto diversi (specialista sbilanciato vs. giocatore completo). Bilanciare guardando solo lo Score totale può quindi far finire per errore troppi "specialisti" nella stessa squadra.
3. **Metrica combinata**: per scegliere lo split migliore, l'algoritmo non guarda solo la differenza di punteggio totale tra le due squadre, ma somma anche la differenza, statistica per statistica (Attacco vs Attacco, Difesa vs Difesa, ecc.), pesata come nello Score. Questo penalizza gli split che concentrano troppe statistiche "estreme" in una sola squadra, anche quando il punteggio medio complessivo sembrerebbe già pari. La differenza mostrata nei risultati resta comunque quella sul punteggio totale, per semplicità di lettura.
4. **Ricerca esaustiva**: in base al formato scelto, il programma calcola tutti i modi possibili di dividere i giocatori selezionati in due squadre della dimensione richiesta (252 combinazioni per il 5vs5, 924 per il 6vs6) e sceglie quella con la metrica combinata più bassa.
5. **Gruppi/relazioni**: i giocatori con lo stesso "Gruppo" vengono trattati come blocchi indivisibili; l'algoritmo cerca la combinazione di blocchi più equilibrata con la stessa metrica.
6. **Squilibrio da gruppi**: se rispettare i gruppi peggiora troppo l'equilibrio rispetto alla soluzione libera, l'app mostra entrambe le composizioni, lasciando a te la scelta.

## Deploy su GitHub Pages

È un progetto **completamente statico** (un solo file `.html`, nessun server, nessuna variabile d'ambiente, nessun database): GitHub Pages è la soluzione più semplice e gratuita.
