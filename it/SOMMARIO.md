## OVERVIEW

Utilizzando le API di S24 gli sviluppatori possono integrare il nostro servizio di delivery nelle proprie applicazioni.  
Le API sono progettate per stimare i costi di delivery, consegnare degli ordini e tracciare l'andamento delle consegne.

Le API di S24 sono sviluppate secondo i principi **REST**.
Questo significa che: 
- le chiamate usano i verbi HTTP standard *GET*, *POST*, *PUT* e *DELETE* a seconda dell'azione da compiere su una risorsa
- vengono usati gli gli HTTP *status code* standard per descrivere gli errori
- l'autenticazione è fatta attraverso HTTP *Basic Authentication*
- viene ritornato un oggetto json per ogni chiamata, anche in caso di errore

### Autenticatione
L'autenticazione alle API di S24 viene fatta attraverso HTTP Basic Auth headers.  
La tua API key deve essere inserita in ogni richiesta come username, mentre la password è lasciata vuota.  
Tutte le richieste devono essere effettuate in HTTPS.

### Per iniziare
Per iniziare ad usare le API di S24 hai bisogno di:
- registrarti con un account developer sulla piattaforma s24
- generare un API key
- effettuare richieste a  
``` https://www.s24srl.com/api/ ```  
- in modalità **sviluppo** (o **test**), effettuare richieste a  
``` https://staging.s24srl.com/api/ ```   
