Fornitore di servizi (Service Provider)
=======================================

**Il Fornitore di Servizi (Service Provider o SP)** eroga i servizi agli utenti, richiede l'autenticazione agli Identity Provider e gestisce la procedura di autorizzazione al servizio.

Processamento della <Response>
------------------------------

Alla ricezione <Response> qualunque sia il binding utilizzato il Service Provider prima di utilizzare l'asserzione deve operare almeno le seguenti verifiche:

Verifiche della response
^^^^^^^^^^^^^^^^^^^^^^^^

.. Important::
    * controllo delle firme presenti nella **<Assertion>** e nella **<response>**;
    * nell'elemento **<SubjectConfirmationData>** verificare che:
        * l'attributo **Recipient** coincida con la assertion consuming service URL a cui la **<Response>** è pervenuta
        * l'attributo **NotOnOrAfter** non sia scaduto;
        * l'attributo **InResponseTo** riferisca correttamente all'ID della <AuthnRequest> di richiesta

Il fornitore di servizi deve garantire che le asserzioni non vengano ripresentate, mantenendo il set di identificatori di richiesta (**ID**) usati come per le **<AuthnRequest>** per tutta la durata di tempo per cui l'asserzione risulta essere valida in base dell'attributo **NotOnOrAfter** dell'elemento **<SubjectConfirmationData>** presente nell'asserzione stessa.
