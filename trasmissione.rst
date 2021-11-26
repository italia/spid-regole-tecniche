Trasmissione dei messaggi (binding)
===================================

La trasmissione dei messaggi tra le entità della federazione SPID può avvenire secondo le due modalità previste da SAML:

* HTTP-Redirect
* HTTP-POST

Binding HTTP-Redirect
---------------------

Nel caso del binding HTTP-Redirect la richiesta viene veicolata con le seguenti modalità:

1. L'entità mittente invia allo User Agent un messaggio HTTP di redirezione, cioè avente uno status code con valore 302 ("Found") o 303 ("See Other").
2. Il Location Header del messaggio HTTP contiene l'URI di destinazione del servizio esposto dall'entità destinataria.
3. Il browser dell'utente elabora quindi tale messaggio HTTP-Redirect indirizzando una richiesta HTTP con metodo GET al servizio dell'entità destinataria sotto forma di URL con tutti i sopraindicati parametri contenuti nella query string.

Il messaggio HTTP trasporta i seguenti parametri (tutti URL-encoded):


``SAMLRequest``
  Un costrutto SAML codificato in formato Base64 e compresso con algoritmo DEFLATE. Come da specifica, il messaggio SAML **non contiene la firma** in formato XML Digital Signature esteso (come avviene in generale nel caso di binding HTTP-POST). Ciò a causa delle dimensioni eccessive che esso raggiungerebbe per essere veicolato in una query string. La specifica indica come modalità alternativa quella di specificare con parametri aggiuntivi l'algoritmo utilizzato per firmare (``SigAlg``) e la stringa con la codifica Base64 URL-encoded dei byte del messaggio SAML (``Signature``).

``RelayState``
  Identifica la risorsa (servizio) originariamente richiesta dall'utente e a cui trasferire il controllo alla fine del processo di autenticazione. Il Service Provider a tutela della privacy dell'utente nell'utilizzare questo parametro deve mettere in atto accorgimenti tali da rendere minima l'evidenza possibile sulla natura o tipologia della risorsa (servizio) richiesta

``SigAlg``
  Identifica l'algoritmo usato per la firma prodotta secondo il profilo specificato per SAML (SAML-Core, cap. 5) utilizzando chiavi RSA almeno a 2048 bit e algoritmo di digest SHA-256 o superiore; il valore esteso di questo parametro è contestualizzato da un namespace appartenente allo standard XML Digital Signature. Come indicato al punto 1, tuttavia, la firma prodotta non fa uso della struttura XML definita in tale standard

``Signature``
  Contiene la firma digitale della query string, così come prodotta prima di aggiungere questo parametro, utilizzando l'algoritmo indicato al parametro precedente;

Un esempio di tale URL è il seguente, nel quale sono evidenziati in grassetto i parametri citati (i valori di alcuni parametri sono stati ridotti per brevità, inoltre il valore del parametro ``RelayState`` è stato reso non immediatamente intellegibile, come suggerito dalla specifica, sostituendo la stringa in chiaro con l'ID della richiesta: l'entità mittente tiene traccia della corrispondenza)

.. code-block:: python

   SAMLRequest=nVPLbtswELz3KwTeZb0M2SYsBa6NoAbSRrGUHnqjqFVDQCJVLuU4f19KlhEDb
   VygR5K7O7Mzw%2FXdqW2cI2gUSiYkmPnEAclVJeTPhDwX%2B6S3KWf1sjapqOb3rzIA%2FzqAY2zQQRtbNtWSe
   [...]
   ZwPAU88aUQvQ%2F8oe8S68piBDNabB5s3AyThb1XZMCxxEhhPj5qLZddW2sZIcoP4fBW%2BWccqH0fZ6iNir0tU
   QGeCWZaGZxE5pM4n8Nz7p%2Be2D3S6L51x1NljO%2BCO2qh8zO%2Bji%2FfnN098%3D&RelayState=s29f6c7d
   6bbf9e62968d27309e2e4beb6133663a2e&SigAlg=http%3A%2F%2Fwww.w3.org%2F2000%2F09%2Fxmldsig
   %23rsa-sha1&Signature=LtNj%2BbMc8j%2FhglWzHPMmo0ESQzBaWlmQbZxas%2B%2FIfNO4F%2F7WNoMKDZ4
   VVYeBtCEQKWp12pU7vPB5WVVMRMrGB8ZRAdHmmPp0hJ9opO3NdafRc04Z%2BbfnkSuQCN9NcGV%2BajT
   [...]
   ra169jhaGRReRQ9KkgSB3aTpQGaffAYUPVo2XZiWy6f9Z7zsmV%2FFoT8dg%3D%3D


Binding HTTP-POST
-----------------

Nel caso del binding HTTP-POST, l'entità mittente invia allo User Agent (il browser dell'utente) un messaggio HTTP con status code avente valore 200 ("OK"):

1. Il messaggio HTTP contiene una form HTML all'interno della quale è trasportato un costrutto SAML codificato come valore di un hidden form control di nome ``SAMLRequest`` oppure ``SAMLResponse``. Rispetto al binding HTTP-Redirect, l'utilizzo di una form HTML permette di superare i limiti di dimensione della query string. Pertanto, l'intero messaggio SAML in formato XML può essere firmato in accordo alla specifica XML Digital Signature. Il risultato a valle della firma è quindi codificato in formato Base64

   * Il parametro deve essere denominato ``SAMLRequest`` nel trasporto dei messaggi ``<AuthnRequest>`` e ``LogoutRequest``, mentre deve essere denominato ``SAMLResponse`` nel trasporto dei messaggi ``<Response>`` e ``<LogoutResponse>``.

2. La form HTML contiene un secondo *hidden form control* di nome ``RelayState`` che contiene il corrispondente valore del Relay State, cioè della risorsa originariamente richiesta dall'utente e alla quale dovrà essere trasferito il controllo al termine della fase di autenticazione
3. La form HTML è corredata da uno script che la rende auto-postante all'indirizzo indicato nell'attributo ``action`` corrispondente alla *Location* del *SingleSignOnService*.
4. Il browser dell'utente elabora quindi la risposta HTTP e invia una richiesta HTTP POST verso il servizio dell'entità destinataria.

Un esempio di form HTML per trasferire in HTTP-POST la richiesta di autenticazione è descritto nell'esempio successivo. Osservando attentamente il codice riportato in figura si può notare il valore del parametro ``SAMLRequest`` (ridotto per brevità); il valore del parametro ``RelayState`` reso non immediatamente intellegibile (cfr. sez. precedente); l'elemento ``<input type="submit" value="Invia"/>``, che ha lo scopo di visualizzare all'interno del web browser il pulsante di invio della form utilizzabile dall'utente, non strettamente necessario in quanto la form è resa autopostante.

.. literalinclude:: code-samples/richiesta-http-post-binding.html
   :language: html
   :linenos:

Un esempio di form HTML per trasferire in HTTP-POST la risposta ad una richiesta di autenticazione è descritto nell'esempio successivo.

.. literalinclude:: code-samples/risposta-http-post-binding.html
   :language: html
   :linenos:

Sicurezza
---------

Il profilo SAML SSO raccomanda l'uso di *TLS 1.2* nei colloqui tra Asserting party (Identity Provider e Attribute Authority), le Relaying Party (Service Provider) e lo user agent.
In ambito  SPID si rende obbligatorio l'impiego di TLS 1.2.
In casi particolari e temporanei,  può  essere  adottata  la  versione  1.1,  fermo  restando  che  la  versione  1.2 deve  essere  adottata  lato server.
Conseguentemente, le versioni più obsolete dei browser, che non supportano le versioni del protocollo indicate, non devono essere utilizzate e, i livelli minimi accettati per l’accesso ai servizi SPID, devono essere documentati nei confronti degli utenti.
