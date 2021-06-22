Messaggi di errore
==================

Autenticazione corretta
-----------------------

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 1 (Autenticazione corretta)
    * - Binding:
      - HTTP-POST, HTTP-Redirect
    * - HTTP status code:
      - 200
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Success``
    * - Destinatario notifica:
      - Fornitore del servizio (SP)

Anomalie del sistema
--------------------

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 2 (Indisponibilità sistema)
    * - Binding:
      - HTTP-POST
    * - Destinatario notifica:
      - Utente
    * - Schermata IdP:
      - Messaggio di errore generico
    * - Troubleshooting utente:
      - Ripetere l'accesso al servizio più tardi

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 3 (Errore di sistema)
    * - Binding:
      - HTTP-Redirect
    * - HTTP status code:
      - 500
    * - Destinatario notifica:
      - Utente
    * - Schermata IdP:
      - Pagina di cortesia con messaggio “Sistema di autenticazione non disponibile - Riprovare più tardi”
    * - Troubleshooting utente:
      - Ripetere l'accesso al servizio più tardi
    * - Note:
      - Tutti i casi di errore di sistema in cui è possibile mostrare un messaggio informativo all’utente

Anomalie delle richieste
------------------------

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 4 (Formato binding non corretto)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - HTTP status code:
      - 403
    * - Destinatario notifica:
      - Utente
    * - Schermata IdP:
      - Pagina di cortesia con messaggio *“Formato richiesta non corretto - Contatare il gestore del servzio”*
    * - Troubleshooting utente:
      - Contattare il gestore del servizio
    * - Troubleshooting SP:
      - Verificare la conformità con le regole tecniche SPID del formato del messaggio di richiesta
    * - Parametri obbligatori:
      - * ``SAMLRequest``
        * ``SigAlg`` (solo per HTTP-Redirect)
        * ``Signature`` (solo per HTTP-Redirect)
    * - Parametri non obbligatori:
      - * ``RelayState``

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 5 (Verifica della firma fallita)
    * - Binding:
      - HTTP-Redirect
    * - HTTP status code:
      - 403
    * - Destinatario notifica:
      - Utente
    * - Schermata IdP:
      - Pagina di cortesia con messaggio *“Impossibile stabilire l’autenticità della richiesta di autenticazione - Contattare il gestore del servizio”*
    * - Troubleshooting utente:
      - Contattare il gestore del servizio
    * - Troubleshooting SP:
      - Verificare certificato o modalità di apposizione firma
    * - Note:
      - Firma sulla richiesta non presente, corrotta, non conforme in uno dei parametri, con certificato scaduto o con certificato non associato al corretto EntityID nei metadati registrati

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 6 (Binding su metodo HTTP errato)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - HTTP status code:
      - 403
    * - Destinatario notifica:
      - Utente
    * - Schermata IdP:
      - Pagina di cortesia con messaggio *“Formato richiesta non ricevibile - Contattare il gestore del servizio”*
    * - Troubleshooting utente:
      - Contattare il gestore del servizio
    * - Troubleshooting SP:
      - Verificare metadata Gestore dell'identita (IdP)
    * - Note:
      - Invio richiesta in HTTP-Redirect su entrypoint HTTP-POST dell'identity, oppure invio richiesta in HTTP-POST su entrypoint HTTP-Redirect dell'identity

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 7 (Errore sulla verifica della firma della richiesta)
    * - Binding:
      - HTTP-POST
    * - HTTP status code:
      - 403
    * - Destinatario notifica:
      - Utente
    * - Schermata IdP:
      - Pagina di cortesia con messaggio *“Formato richiesta non corretto - Contattare il gestore del servizio”*
    * - Troubleshooting utente:
      - Contattare il gestore del servizio
    * - Troubleshooting SP:
      - Verificare certificato o modalità di apposizione firma
    * - Note:
      - Firma sulla richiesta non presente, corrotta, non conforme in uno dei parametri, con certificato scaduto o con certificato non associato al corretto EntityID nei metadati registrati

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 8 (Formato della richiesta non conforme alle specifiche SAML)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML StatusMessage:
      - ErrorCode nr08
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Formulare la richiesta secondo le regole tecniche SPID - Fornire pagina di cortesia all'utente
    * - Note:
      - Non conforme alle specifiche SAML - Il controllo deve essere operato successivamente alla verifica positiva della firma

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 9 (Parametro version non presente, malformato o diverso da ``2.0``)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:VersionMismatch``
    * - SAML StatusMessage:
      - ErrorCode nr09
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Formulare la richiesta secondo le regole tecniche SPID - Fornire pagina di cortesia all'utente

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 10 (Issuer non presente, malformato o non corrispondete all'entità che sottoscrive la richiesta)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - HTTP status code:
      - 403
    * - Destinatario notifica:
      - Utente
    * - Schermata IdP:
      - Pagina di cortesia con messaggio *“Formato richiesta non corretto - Contattare il gestore del servizio”*
    * - Troubleshooting utente:
      - Contattare il gestore del servizio
    * - Troubleshooting SP:
      - Verificare formato delle richieste prodotte

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 11 (ID non presente, malformato o non conforme)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML StatusMessage:
      - ErrorCode nr11
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Formulare correttamente la richiesta - Fornire pagina di cortesia all'utente
    * - Note:
      - Identificatore necessario per la correlazione con la risposta. L’eventuale presenza dell’anomalia va verificata e segnalata solo a seguito di una positiva verifica della firma.

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 12 (``RequestAuthnContext`` non presente, malformato o non previsto da SPID)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:NoAuthnContext``
    * - SAML StatusMessage:
      - ErrorCode nr12
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Schermata IdP:
      - Pagina temporanea con messaggio di errore: *“Autenticazione SPID non conforme o non specificata”*
    * - Troubleshooting SP:
      - Informare l'utente
    * - Note:
      - Identificatore necessario per la correlazione con la risposta. L’eventuale presenza dell’anomalia va verificata e segnalata solo a seguito di una positiva verifica della firma.

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 13 (``IssueInstant`` non presente, malformato o non coerente con l'orario di arrivo della richiesta)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:RequestDenied``
    * - SAML StatusMessage:
      - ErrorCode nr13
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Formulare correttamente la richiesta - Fornire pagina di cortesia all'utente

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 14 (``Destination`` non presente, malformata o non coincidente con il Gestore delle identità ricevente la richiesta)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:RequestUnsupported``
    * - SAML StatusMessage:
      - ErrorCode nr14
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Formulare correttamente la richiesta - Fornire pagina di cortesia all'utente

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 15 (Attributo ``IsPassive`` presente e attualizzato al valore true)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:NoPassive``
    * - SAML StatusMessage:
      - ErrorCode nr15
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Formulare correttamente la richiesta - Fornire pagina di cortesia all'utente

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 16 (``AssertionConsumerService`` non correttamente valorizzato)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:RequestUnsupported``
    * - SAML StatusMessage:
      - ErrorCode nr16
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Formulare correttamente la richiesta - Fornire pagina di cortesia all'utente
    * - Note:
      - * ``AssertionConsumerServiceIndex`` presente e attualizzato con valore non riportato nei metadata
        * ``AssertionConsumerServiceIndex`` riportato in presenza di uno od entrambi gli attributi ``AssertionConsumerServiceURL`` e ``ProtocolBinding``
        * ``AssertionConsumerServiceIndex`` non presente in assenza di almeno uno attributi ``AssertionConsumerServiceURL`` e ``ProtocolBinding``
        * La response deve essere inoltrata presso ``AssertionConsumerService`` di default riportato nei metadata

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 17 (Attributo ``Format`` dell'elemento ``NameIDPolicy`` assente o non valorizzato secondo specifica)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:RequestUnsupported``
    * - SAML StatusMessage:
      - ErrorCode nr17
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Formulare correttamente la richiesta - Fornire pagina di cortesia all'utente
    * - Note:
      - Nel caso di valori diversi dalla specifica del parametro opzionale ``AllowCreate`` si procede con l'autenticazione senza riportare errori

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 18 (``AttributeConsumerServiceIndex`` malformato o che riferisce a un valore non registrato nei metadati di SP)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Requester``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:RequestUnsupported``
    * - SAML StatusMessage:
      - ErrorCode nr18
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Riformulare la richiesta con un valore dell'indice presente nei metadati

Anomalie derivanti dall'utente
------------------------------

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 19 (Autenticazione fallita per ripetuta sottomissione di credenziali errate - superato numero tentativi secondo le policy adottate)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Responder``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:AuthnFailed``
    * - SAML StatusMessage:
      - ErrorCode nr19
    * - Destinatario notifica:
      - HTTP POST/HTTP Redirect
    * - Schermata IdP:
      - Messaggio di errore specifico ad ogni interazione prevista
    * - Troubleshooting utente:
      - Inserire credenziali corrette
    * - Troubleshooting SP:
      - Fornire una pagina di cortesia notificando all'utente le ragioni che hanno determinato il mancato accesso al servizio richiesto
    * - Note:
      - Si danno indicazioni specifiche e puntuali all’utente per risolvere l’anomalia, rimanendo nelle pagine dello IdP. Solo al verificarsi di determinate condizioni legate alle policy di sicurezza aziendali, ad esempio dopo 3 tentativi falliti, si risponde al SP.

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 20 (Utente privo di credenziali compatibili con il livello HTTP richiesto dal fornitore del servizio)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Responder``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:AuthnFailed``
    * - SAML StatusMessage:
      - ErrorCode nr20
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting utente:
      - Acquisire credenziali di livello idoneo all'accesso al servizio richiesto
    * - Troubleshooting SP:
      - Fornire una pagina di cortesia notificando all'utente le ragioni che hanno determinato il mancato accesso al servizio richiesto

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 21 (Timeout durante l’autenticazione utente)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Responder``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:AuthnFailed``
    * - SAML StatusMessage:
      - ErrorCode nr21
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting utente:
      - Si ricorda che l’operazione di autenticazione deve essere completata entro un determinato periodo di tempo
    * - Troubleshooting SP:
      - Fornire una pagina di cortesia notificando all'utente le ragioni che hanno determinato il mancato accesso al servizio richiesto

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 22 (Utente nega il consenso all’invio di dati al SP in caso di sessione vigente)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Responder``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:AuthnFailed``
    * - SAML StatusMessage:
      - ErrorCode nr22
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting utente:
      - Dare consenso
    * - Troubleshooting SP:
      - Fornire una pagina di cortesia notificando all'utente le ragioni che hanno determinato il mancato accesso al servizio richiesto
    * - Note:
      - Sia per autenticazione da fare, sia per sessione attiva di classe SpidL1.

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 23 (Utente con identità sospesa/revocata o con credenziali bloccate)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Responder``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:AuthnFailed``
    * - SAML StatusMessage:
      - ErrorCode nr23
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Schermata IdP:
      - Pagina temporanea con messaggio di errore: “Credenziali sospese o revocate”
    * - Troubleshooting SP:
      - Fornire una pagina di cortesia notificando all'utente le ragioni che hanno determinato il mancato accesso al servizio richiesto

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 24 (Riservato)
    * -
      -

.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 25 (Processo di autenticazione annullato dall’utente)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Responder``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:AuthnFailed``
    * - SAML StatusMessage:
      - ErrorCode nr25
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Fornire una pagina di cortesia notificando all'utente le ragioni che hanno determinato il mancato accesso al servizio richiesto


.. list-table::
    :widths: 20 80
    :header-rows: 1

    * - Error code:
      - 30 (tentativo dell’utente di utilizzare una tipologia di identità digitale diversa da quanto richiesto dal SP)
    * - Binding:
      - HTTP-Redirect, HTTP-POST
    * - SAML StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:status:Responder``
    * - SAML sub-StatusCode:
      - ``urn:oasis:names:tc:SAML:2.0:statuss:AuthnFailed``
    * - SAML StatusMessage:
      - ErrorCode nr30
    * - Destinatario notifica:
      - Fornitore del servizio (SP)
    * - Troubleshooting SP:
      - Fornire una pagina di cortesia notificando all'utente le ragioni che hanno determinato il mancato accesso al servizio richiesto. Per maggiori dettagli consultare `Avviso 18 <https://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n18_v.2-_autenticazione_persona_giuridica_o_uso_professionale_per_la_persona_giuridica.pdf>_`
