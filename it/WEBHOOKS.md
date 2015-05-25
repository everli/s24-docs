## WEBHOOKS
I webhooks ti consentono di ricevere aggiornamenti in real time sull'andamento dei tuoi ordini.  
Per usare i webhooks, devi configurare un URL in cui la tua applicazione accetti richieste HTTP POST da S24.  
Queste richieste contengono nel payload le informazioni circa l'aggiornamento avvenuto nella piattaforma di S24.
```
{
  event: $event,
  data: {
    // modello della risorsa
  }
}
```
Ad ogni passaggio di stato dell'ordine (*CONFIRMED*->*ASSIGNED*, *ASSIGNED*->*PAID*, *PAID*->*DELIVERED* ecc..) e alla sottomissione di un feedback viene lanciato un webhook con il modello della risorsa e, nella chiave `event` una stringa identificativa dell'evento generato:
- `order.delivery_time_updated` - aggiornata la data di consegna
- `order.confirmed` - ordine confermato: partito il job di ricerca di fattorini disponibili
- `order.assigned`- ordine assegnato ad un fattorino
- `order.paid` - ordine pagato, caricato lo scontrino
- `order.delivered` - ordine consegnato al cliente
- `order.deleted` - ordine eliminato
- `feedback.submitted` - inserito un feedback per un ordine

Per permetterti di autenticare i webhooks di S24, la chiamata viene popolata con due header custom:  
- `User-Agent`: S24_webhook
- `X-S24-Signature`: hash `hmac` del payload utilizzando algoritmo **sha1** algoritmo e l'API key come chiave di cifratura
