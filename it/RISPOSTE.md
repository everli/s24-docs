## RISPOSTE

Le API di S24 usano gli standard HTTP status code per indicare lo stato della richiesta:
- 200 - OK
- 400 - Bad Request: hai fatto qualcosa di sbagliato, un argomento mancante o non valido
- 401 - Unauthorized: autenticazione sbagliata
- 403 - Forbidden: privilegi insufficienti per accedere/modificare la risorsa
- 404 - Not Found: risorsa non trovata
- 500 - Internal Server Error: problema di S24 nel gestire la richiesta

### Risposta con errore
Nel caso la richiesta abbia generato un errore, oltre allo status code identificativo, viene tornato un oggetto json
con il codice e la descrizione dell'errore.
```
{
    "errorCode": "error.invalidApiKey",
    "errorDescription": "A valid API key is required"
}
```
### Risposta con successo
In caso di successo la risposta è sempre un oggetto json con chiavi `object` e `data`. Sono previsti due tipi di risposta:
#### LISTE
Nel caso di liste (es: elenco ordini) la risposta è fatta come segue:
```
{
  object: "list",
  metadata: {
    totalCount: 12,
    page: 0,
    itemsPerPage: 20
  },
  data: [
    // array di risorse
  ]
}
```
#### RISORSA
Nel caso di chiamate e risorse (es: richiesta di un ordine) la risposta è del tipo che segue:
```
{
  object: “order”,
  data: {
    // modello dell’ordine
  }
}
```
### Paginazione
La navigazione tra le liste viene fatta usando i parametri `page` e `itemsPerPage`.  
``` /v1/orders?page=1&itemsPerPage=2 ```  
Nel metadata della risposta ci sono i riferimenti per le pagine precedenti e successive, per il primo e ultimo link:
```
"metadata": {
  "totalCount": 10,
  "page": 1,
  "itemsPerPage": 2,
  "fullUrl": "https://www.s24srl.com/api/v1/orders?itemsPerPage=2&page=1",
  "first": "https://www.s24srl.com/api/v1/orders?page=0&itemsPerPage=2",
  "last": "https://www.s24srl.com/api/v1/orders?page=5&itemsPerPage=2",
  "next": "https://www.s24srl.com/api/v1/orders?page=2&itemsPerPage=2",
  "prev": "https://www.s24srl.com/api/v1/orders?page=0&itemsPerPage=2"
   },
  ..
}
```
