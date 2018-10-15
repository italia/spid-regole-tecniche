Log
===

Identity Provider
-----------------

Ai fini della tracciatura l’Identity Provider dovrà mantenere un Registro delle transazioni contenente i tracciati delle richieste di autenticazione servite negli ultimi 24 mesi. L’unità di memorizzazione di tale registro dovrà rendere persistente per ogni transazione la tripla composta dell’identificativo dell’identità digitale (``spidCode``) interessata dalla transazione, dalla ``<AuthnRequest>`` e della relativa ``<Response>``. 

Al fine di consentire una facile ricerca e consultazione dei dati di tracciature potrebbe essere opportuno memorizzare in ogni record informazioni direttamente estratte dai suddetti messaggi in formato SAML.
A titolo esemplificativo e non esaustivo le informazioni presenti in un record del registro potrebbero essere le seguenti:

- ``SpidCode``
- ``<AuthnRequest>``
- ``<Response>``
- ``AuthnReq_ID``
- ``AuthnReq_ IssueInstant``
- ``AuthnReq_`` Issuer
- ``Resp_ID``
- ``Resp_IssueInstant``
- ``Resp_Issuer``
- ``Assertion_ID``
- ``Assertion_subject``
- ``Assertion_subject_NameQualifier``


Service Provider
----------------

Il comma 2 dell’articolo 13 del DPCM obbliga i fornitori di servizi (Service Provider) alla conservazione per ventiquattro mesi delle informazioni necessarie a imputare alle singole identità digitali le operazioni effettuate sui propri sistemi.

A tal fine **un Service provider dovrà mantenere un Registro delle transazioni contenente i tracciati delle richieste di autenticazione servite negli ultimi 24 mesi.** L’unità di memorizzazione di tale registro dovrà rendere persistente per ogni transazione la coppia dalla ``<AuthnRequest>`` e della relativa ``<Response>``.

Al fine di consentire una facile ricerca e consultazione dei dati di tracciature potrebbe essere opportuno memorizzare in ogni record informazioni direttamente estratte dai suddetti messaggi in formato SAML. A titolo esemplificativo e non esaustivo le informazioni presenti in un record del registro potrebbero essere le seguenti:

- ``<AuthnRequest>``
- ``<Response>``
- ``AuthnReq_ID``
- ``AuthnReq_IssueInstant``
- ``Resp_ID``
- ``Resp_IssueInstant``
- ``Resp_Issuer``
- ``Assertion_ID``
- ``Assertion_subject``
- ``Assertion_subject_NameQualifier``
