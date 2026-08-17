# GestioneOrdini
Gestione ordini per una generica attività.
Flusso operativo
 - Acquisizione richiesta ordine da file
 - Creazione dell'ordine
 - Creazione della spedizione
 - Prelievo prodotti
 - Spedizione pronta
 - Creazione viaggio
 - Partenza viaggio + chiusura spedizione

## Domini
- Ordine
  Risponde a cosa è stato richiesto? Lista di prodotti richiesti.
- Spedizione
  Risponde a cosa sto preparando? Lista di prodotti necessari per essere spediti.
- Prodotto
  Entità a se, per adesso gestita anagraficamente a magazzino
- Viaggio
  Rappresenta il trasporto di una o più spedizioni.
  La partenza del viaggio determina la partenza delle spedizioni associate.

### Entità
  Ordine
  Contiene la lista dei prodotti e le relative quantità che sono state richieste.
  id (int) NOT NULL,dataCreazioneOrdine (Datetime) NOT NULL, stato (int) NOT NULL, Spedito (Datetime) NULL,

  Stati 
  id (int) NOT NULL,
  descrizione (varchar) NOT NULL
  
  OrdineProdotti
  IdOrdine (int) NOT NULL, IdProdotto (int) NOT NULL, quantita (int) NOT NULL

  Spedizione
  Contiene la lista equivalente all'ordine, rappresenta però la merce preparata ed in uscita.
  Id (int) NOT NULL, idOrdine (int) NOT NULL, inizioLavorazione (Datetime) NOTNULL, fineLavorazione (Datetime) NULL, idViaggio (int) NULL, stato (int) NOT NULL

  StatiSpedizione
  id (int) NOT NULL, descrizione (string) NOT NULL.

  SpedizioneProdotti
  IdSpedizione (int) NOT NULL, IdProdotto (int) NOT NULL, quantitaOrdinata (int) NOT NULL, quantitaPreparata (int) NOT NULL

  Prodotto
  In questa versione semplice anagrafica (c'è o non c'è)
  IdProdotto (int) NOT NULL
  Descrizione (string) NOT NULL

  Viaggio
  Contiene la lista di spedizioni associate e il trasporto in uscita. La partenza del viaggio indica la chiusura della spedizione associata.
  id (int) NOT NULL, dataPartenza (datetime) NOT NULL , stato (int) NOT NULL

  StatiViaggio 
  id (int) NOT NULL, descrizione (varchat) NOT NULL

  
 ## Responsabilità e transizioni
 Ordine -> Un ordine alla creazione è nello stato creato, in questo stato è possibile modificare l'ordine e collegarci spedizioni. Alla prima associazione di
 una spedizione, l'ordine passa in stato in lavorazione, questo stato permette di associare altre spedizioni ma blocca ogni modifica sull'ordine. Infine viene settato 
 esplicitamente lo stato a chiuso.
 Spedizione -> La spedizione alla creazione è nello stato creato, alla prima assegnazione di una quantità di un prodotto passa allo stato in lavorazione. Nello stato chiusa
 non si possono più assegnare nuove quantità di prodotti alla suddetta spedizione, in questo stato può essere associata ad un viaggio. Nello stato in viaggio significa che
 è fisicamente in un viaggio che è partito.
 Viaggio -> Viaggio alla creazione è nello stato di creato, in questo stato non vi è alcun collegamento con le spedizioni, al primo collegamento con una spedizione cambia
 stato in in carico, una volta completato il carico esplicitamente si passa il viaggio in stato Pronto, una volta partito lo stato passa in viaggio, in questi ultimi 2 stati non
 è possibile associare altre spedizioni.
 Vincoli 
 Ordine in stato lavorazione solo se ha una spedizione associata.
 Spedizione in stato lavorazione passa solo dopo che un prodotto è stato caricato, se vengono rimossi tutti i prodotti, allora torna in stato creato. Lo stato in viaggio è possibile
 solo se viene collegata ad un viaggio che è effettivamente partito.
 Viaggio in carico è presente solo se una spedizione è associata, se vengono tolte tutte le spedizioni, allora torna in stato creato. Pronto e In viaggio sono 2 stati che vengono
 settati manualmente.
 
