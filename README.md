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
  
