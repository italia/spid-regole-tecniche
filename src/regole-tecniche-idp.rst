Regole tecniche Identity Provider
=================================

**Il Gestore delle identità (Identity Provider o IdP)** *gestisce gli utenti e la procedura di autenticazione a seguito di una richiesta da parte di un Fornitore di Servizi (Service Provider o SP).*

Specifiche delle interfacce
---------------------------

L'asserzione prodotta dall'Identity Provider deve essere conforme allo standard SAML v2.0 `(SAML Core) <https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf>`_ e rispettare le seguenti condizioni:


<Assertion>
^^^^^^^^^^^
.. Important::
    nell'elemento **<Assertion>** devono essere presenti i seguenti attributi:

        * l'attributo **ID** univoco, per esempio basato su un Universally Unique Identifier (UUID) o su una combinazione origine + timestamp  (quest'ultimo generato con una precisione di almeno un millesimo di secondo per garantire l'univocità);
        * l'attributo **Version**, che deve valere sempre "2.0", coerentemente con la  versione della specifica SAML adottata;
        * l'attributo **IssueInstant** a indicare l'istante di emissione della richiesta, in formato UTC (esempio: "2017-03-01T15:05:10.531Z");

<Subject>
^^^^^^^^^
.. Important::
    deve essere presente l'elemento **<Subject>** a referenziare il soggetto che si è autenticato in cui devono comparire gli elementi:

        * **<NameID>** atto a qualificare il soggetto dell'asserzione, in cui sono presenti i seguenti attributi:

          * **Format** che deve assumere il valore "urn:oasis:names:tc:SAML:2.0:nameidformat:transient" `(SAML Core, par8.3) <https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf>`_
          * **NameQualifier** che qualifica il dominio a cui afferisce tale valore (URI riconducibile all'Identity Provider stesso)

        * **<SubjectConfirmation>** contenente l'attributo

            * *Method* riportante il valore "urn:oasis:names:tc:SAML:2.0:cm:bearer"

        * **<SubjectConfirmationData>** riportante gli attributi:

            * *Recipient* riportante l'AssertionConsumerServiceURL relativa al servizio per cui è stata emessa l'asserzione e l'attributo
            * *NotOnOrAfter* che limita la finestra di tempo durante la quale l'asserzione può essere propagata.
            * *InResponseTo*, il cui valore deve fare riferimento all'ID della richiesta.

<Issuer>
^^^^^^^^
.. Important::
    deve essere presente l'elemento **<Issuer>** a indicare l'entityID dell'Identity Provider emittente (attualizzato come l'attributo entityID presente nei corrispondenti IdP metadata) con l'attributo Format riportante il valore "urn:oasis:names:tc:SAML:2.0:nameidformat:entity";

        * deve essere presente l'elemento **<Conditions>** in cui devono essere presenti gli attributi:

            * **NotBefore**,
            * **NotOnOrAfter**;
            * l'elemento **<AudienceRestriction>** riportante a sua volta l'elemento **<Audience>** attualizzato con l'entityID del ServiceProvider per il quale l'asserzione è emessa;

<AuthStatement>
^^^^^^^^^^^^^^^
.. Important::
    deve essere presente l'elemento **<AuthStatement>** a sua volta contenente l'elemento:

        * **<AuthnContext>** riportante nel sotto elemento <AuthnContextClassRef> la classe relativa all'effettivo contesto di autenticazione (es. https://www.spid.gov.it/SpidL2);

<Signature>
^^^^^^^^^^^
.. Important::
    Deve essere presente l'elemento **<Signature>**
    riportante la firma sull'asserzione apposta dall'Identity Provider emittente. La firma deve essere prodotta secondo il profilo specificato per SAML (cfr [SAML-Core] cap5) utilizzando chiavi RSA almeno a 2048 bit e algoritmo di digest SHA-256 o superiore

<AttributeStatement>
^^^^^^^^^^^^^^^^^^^^
.. Note::
    può essere presente l'elemento **<AttributeStatement>** riportante gli attributi identificativi certificati dall'Identity provider. Tale elemento se presente dovrà comprendere:

        * uno o più elementi di tipo **<Attribute>** relativi ad attributi che l'Identity Provider può rilasciare (cfr. Tabella attributi SPID) su richiesta del Service Provider espressa attraverso l'attributo AttributeConsumingServiceIndex quando presente nella authnrequest;
        * per gli elementi <AttributeValue> si raccomanda l'uso dell'attributo xsi:type attualizzato come specificato nella Tabella attributi SPID;

<Advice>
^^^^^^^^
.. Note::
    può essere presente un elemento **<Advice>**

        *può contenere a sua volta altri elementi **<Assertion>**. La possibile presenza dell'elemento, prevista per futuri usi, consente, nei casi in cui gli statement emessi dall'Identity Provider si basino su altre asserzioni SAML ottenute da altre authority, di fornire evidenza delle stesse in forma originale unitamente alla risposta alla richiesta di autenticazione.

    *L'elemento* **<Advice>** *è previsto per futuri usi ed al momento non deve essere utilizzato.*

Esempio: asserzione di autenticazione
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. literalinclude:: code-samples/authentication-assertion.xml
   :language: xml
   :linenos:

AuthnRequest
------------
Il protocollo AuthnRequest previsto per l'Identity Provider deve essere conforme allo standard
SAML v2.0 (cfr. [SAML-Core]) e rispettare le condizioni di seguito indicate.

<AuthnRequest>
^^^^^^^^^^^^^^
.. Important::
    nell' elemento <AuthnRequest> devono essere presenti i seguenti attributi:

        * l'attributo **ID** univoco, per esempio basato su un Universally Unique Identifier (UUID) o su una combinazione origine + timestamp (quest'ultimo generato con una precisione di almeno un millesimo di secondo per garantire l'univocità)
        * l'attributo **Version**, che deve valere sempre "2.0", coerentemente con la versione della specifica SAML adottata;
        * l'attributo **IssueInstant** a indicare l'istante di emissione della richiesta, in formato UTC (esempio: "2017-03-05T18:03:10.531Z")
        * l'attributo **Destination**, a indicare l'indirizzo (URI reference) dell'Identity provider a cui è inviata la richiesta, come risultante nell'attributo entityID presente nel metadata IdP dell'Identity Provider a cui viene inviata la richiesta
        * l'attributo **ForceAuthn** nel caso in cui si richieda livelli di autenticazione superiori a SPIDL1 ( SPIDL2 o SPIDL3)
        * l'attributo **AssertionConsumerServiceIndex**, riportante un indice posizionale facente riferimento ad uno degli elementi <AssertionConsumerService> presenti nei metadata del Service Provider, atto ad indicare, mediante l'attributo Location, l'URL a cui inviare il messaggio di risposta alla richiesta di autenticazione, e mediante l'attributo Binding, il binding da utilizzare, quest'ultimo valorizzato obbligatoriamente con "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
            * in alternativa, scelta sconsigliata, possono essere presenti
                * l'attributo **AssertionConsumerServiceURL** ad indicare l'URL a cui inviare il messaggio di risposta alla richiesta di autenticazione (l'indirizzo deve coincidere con quello del servizio riportato dall'elemento <AssertionConsumingService> presente nei metadata del Service Provider);
                * l'attributo **ProtocolBinding**, identificante il binding da utilizzare per inoltrare il messaggio di risposta, valorizzato con "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST";

.. Important::
    nell' elemento **<AuthnRequest>** non deve essere presente l'attributo IsPassive (ad indicare "false" come valore di default)

.. Note::
    nell' elemento **<AuthnRequest>** si può utilizzare opzionalmente l'attributo:

        * **AttributeConsumingServiceIndex** riportante un indice posizionale in riferimento alla struttura **<AttributeConsumingService>** presente nei metadata del Service Provider, atta a specificare gli attributi che devono essere presenti nell'asserzione prodotta. Nel caso l'attributo fosse assente l'asserzione prodotta non riporterà alcuna attestazione di attributo


<Subject>
^^^^^^^^^

.. Note::
    può essere presente l'elemento **<Subject>** a indicare il soggetto per cui si chiede l'autenticazione in cui deve comparire:

        * l'elemento **<NameID>** atto a qualificare il soggetto in cui sono presenti i seguenti attributi
            * **Format** che deve assumere il valore "urn:oasis:names:tc:SAML:2.0:nameid-format:unspecified" (cfr. SAMLCore, sez. 8.3)
            * **NameQualifier** che qualifica il dominio a cui afferisce tale valore (URI)


<Issuer>
^^^^^^^^

.. Important::
    deve essere presente l'elemento **<Issuer>** attualizzato come l'attributo entityID riportato nel corrispondente SP metadata, a indicare l'identificatore univoco del Service Provider emittente. L'elemento deve riportare gli attributi:

        * **Format** fissato al valore "urn:oasis:names:tc:SAML:2.0:nameid-format:entity"
        * **NameQualifier** che qualifica il dominio a cui afferisce tale valore (URI riconducibile al Service Provider stesso)


<NameIDPolicy>
^^^^^^^^^^^^^^

.. Important::
    deve essere presente l'elemento **<NameIDPolicy>** avente gli attributi

        * **AllowCreate**, se presente, valorizzato a "true"
        * **Format** valorizzato come "urn:oasis:names:tc:SAML:2.0:nameid-format:transient"


<Conditions>
^^^^^^^^^^^^

.. Important::
    l'elemento **<Conditions>**, se presente, deve indicare i limiti di validità attesi dell'asserzione ricevuta in risposta, per esempio specificando gli attributi

        * **NotBefore** e **NotOnOrAfter** opportunamente valorizzati in formato UTC

    *N.B. L'Identity Provider non è obbligato a tener conto dell'indicazione nel caso che questa non sia confacente con i criteri di sicurezza da esso adottati.*


<RequestedAuthnContext>
^^^^^^^^^^^^^^^^^^^^^^^

.. Important::
    deve essere presente l'elemento **<RequestedAuthnContext>** (SAMLCore, sez. 3.3.2.2.1) ad indicare il contesto di autenticazione atteso, ossia la "robustezza" delle credenziali richieste. Allo scopo sono definite le seguenti "authentication context class" estese (SAMLAuthContext, sez. 3) in riferimento SPID:

        * **https://www.spid.gov.it/SpidL1**
        * **https://www.spid.gov.it/SpidL2**
        * **https://www.spid.gov.it/SpidL3**

    referenziate dagli elementi **<AuthnContextClassRef>**

    Ciascuna di queste classi, indica in ordine di preferenza il contesto di autenticazione (atteso o effettivo) secondo alcune dimensioni di riferimento, quali per esempio i meccanismi di autenticazione con cui l'Identity Provider può identificare l'utente. L'elemento **<RequestedAuthnContext>** prevede un attributo **Comparison** con il quale indicare il metodo per stabilire il rispetto del vincolo sul contesto di abilitazione: i valori ammessi per questo attributo sono:

        * **exact**
        * **minimum**
        * **better**
        * **maximum**

    Nel caso dell'elemento **<RequestedAuthnContext>**, questa informazione si riflette sulle tipologie di meccanismi utilizzabili dall'Identity Provider ai fini dell'autenticazione dell'utente. L'esempio seguente di **<RequestedAuthnContext>** fa riferimento a una "authentication context class" di tipo "SpidL2" o superiore.

    .. literalinclude:: code-samples/requested-authn-context.xml
       :language: xml
       :linenos:

    *N.B. L'Identity Provider ha facoltà di utilizzare per l'autenticazione un livello SPID più alto rispetto a quelli risultanti dall'indicazione del richiedente mediante l'attributo Comparison. Tale scelta non deve comportare un esito negativo della richiesta.*


<Signature>
^^^^^^^^^^^

.. Important::
    nel caso del binding **HTTP POST** deve essere presente l'elemento **<Signature>** contenente la firma sulla richiesta apposta dal Service Provider. La firma deve essere prodotta secondo il profilo specificato per SAML (SAML-Core, cap. 5) utilizzando chiavi RSA almeno a 2048 bit e algoritmo di digest SHA-256 o superiore.


<Scoping>
^^^^^^^^^

.. Important::
    se presente l'elemento **<Scoping>** il relativo attributo ProxyCount deve assumere valore *"0"* per indicare che l'Identity Provider invocato non può delegare il processo di autenticazione ad altra Asserting Party.


<RequesterID>
^^^^^^^^^^^^^

.. Note::
    eventuali elementi **<RequesterID>** contenuti devono indicare l'URL del servizio di reperimento metadati di ciascuna delle entità che hanno emesso originariamente la richiesta di autenticazione e di quelle che in seguito la hanno propagata, mantenendo l'ordine che indichi la sequenza di propagazione (il primo elemento *<RequesterID>* dell'elemento *<Scoping>* è relativo all'ultima entità che ha propagato la richiesta).

    Gli elementi *<Scoping> <RequesterID>* sono previsti per futuri usi ed al momento non devono essere utilizzati. Nel caso di presenza di tali parametri nella richiesta questi dovranno essere al momento ignorati all’atto dell’elaborazione della risposta da parte dell'Identity Provider.


Esempio di AuthnRequest
^^^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: code-samples/authnrequest.xml
   :language: xml
   :linenos:


Response
--------

Le caratteristiche che deve avere la risposta inviata dall'Identity Provider al Service Provider a seguito di una richiesta di autenticazione sono le seguenti:


<Response>
^^^^^^^^^^

.. Important::
    nell' elemento <Response> devono essere presenti i seguenti attributi:

    * l'attributo *ID* univoco basato, per esempio, su un Universally Unique Identifier (UUID) (cfr. UUID) o su una combinazione origine + timestamp (quest'ultimo generato con una precisione di almeno un millesimo di secondo per garantire l'univocità);
    * deve essere presente l'attributo **Version**, che deve valere sempre *"2.0"*, coerentemente con la versione della specifica SAML adottata;
    * deve essere presente l'attributo **IssueInstant** a indicare l'istante di emissione della risposta, in formato UTC;
    * deve essere presente l'attributo **InResponseTo**, il cui valore deve fare riferimento all'ID della richiesta a cui si risponde;
    * deve essere presente l'attributo Destination, a indicare l'indirizzo (URI reference) del Service provider a cui è inviata la risposta;


<Status>
^^^^^^^^

.. Important::
    deve essere presente l'elemento <Status> a indicare l'esito della AuthnRequest secondo quanto definito nelle specifiche SAML (SAML-Core, par. 3.2.2.1 e successivi) comprendente il sotto-elemento

        * **<StatusCode>**

    ed opzionalmente i sotto-elementi

        * **<StatusMessage>**
        * **<StatusDetail>**

    (Messaggi di errore SPID)

<Issuer>
^^^^^^^^

.. Important::
    deve essere presente l'elemento **<Issuer>** a indicare l'entityID dell'entità emittente, cioè l'Identity Provider stesso. L'elemento

        * **Format** omesso o fissato al valore "urn:oasis:names:tc:SAML:2.0:nameid-format:entity"


<Assertion>
^^^^^^^^^^^

.. Important::
    deve essere presente un elemento **<Assertion>** ad attestare l'avvenuta autenticazione, contenente almeno un elemento **<AuthnStatement>**.
    Nel caso l'Identity Provider abbia riscontrato un errore nella gestione della richiesta di autenticazione l'elemento
    **<Assertion>** non deve essere presente.


.. Note::
    può essere presente l'elemento **<Signature>** contenente la firma sulla risposta apposta dall'Identity Provider.
    La firma deve essere prodotta secondo il profilo specificato per SAML (SAML-Core, cap. 5) utilizzando chiavi RSA almeno a 2048 bit e algoritmo di digest SHA-256 o superiore.
    Per l'asserzione veicolata resta valido quanto già specificato nei dettagli delle `<Assertion> <regole-tecniche-idp.html#assertion>`_

.. literalinclude:: code-samples/response.xml
   :language: xml
   :linenos:


Caratteristiche del Binding
---------------------------

Binding HTTP Redirect
^^^^^^^^^^^^^^^^^^^^^

Nel caso del binding HTTP Redirect la richiesta viene veicolata con le seguenti modalità:

    * **come risposta alla richiesta di accesso** dell'end user ad un servizio o risorsa, il Service Provider invia allo User Agent un messaggio HTTP di redirezione, cioè avente uno status code con valore 302 ("Found") o 303 ("See Other")
    * **il Location Header del messaggio HTTP contiene** l'URI di destinazione del servizio di Single Sign-On esposto dall' Identity Provider. L'interfaccia è sempre la **IAuthnRequest**
    * **il messaggio HTTP trasporta i seguenti parametri** (tutti URL-encoded):
        1. **"SAMLRequest"**: un costrutto SAML <AuthnRequest> codificato in formato Base64 e compresso con algoritmo DEFLATE. Come da specifica, il messaggio SAML non contiene la firma in formato XML Digital Signature esteso (come avviene in generale nel caso di binding HTTP POST). Ciò a causa delle dimensioni eccessive che esso raggiungerebbe per essere veicolato in una query string. La specifica indica come modalità alternativa quella di specificare con parametri aggiuntivi l'algoritmo utilizzato per firmare e la stringa con la codifica Base64 URL-encoded dei byte del messaggio SAML
        2. **"RelayState"**: identifica la risorsa (servizio) originariamente richiesta dall'utente e a cui trasferire il controllo alla fine del processo di autenticazione. Il Service Provider a tutela della privacy dell'utente nell'utilizzare questo parametro deve mettere in atto accorgimenti tali da rendere minima l'evidenza possibile sulla natura o tipologia della risorsa (servizio) richiesta
        3. **"SigAlg"**: identifica l'algoritmo usato per la firma prodotta secondo il profilo specificato per SAML (SAML-Core, cap. 5) utilizzando chiavi RSA almeno a 2048 bit e algoritmo di digest SHA-256 o superiore; il valore esteso di questo parametro è contestualizzato da un namespace appartenente allo standard XML Digital Signature. Come indicato al punto 1, tuttavia, la firma prodotta non fa uso della struttura XML definita in tale standard
        4. **"Signature"**: contiene la firma digitale della query string, così come prodotta prima di aggiungere questo parametro, utilizzando l'algoritmo indicato al parametro precedente;
        5. **Il browser dell'utente elabora quindi tale messaggio HTTP Redirect** indirizzando una richiesta HTTP con metodo GET al servizio di Single Sign-On dell' Identity Provider (interfaccia IAuthnRequest) sotto forma di URL con tutti i sopraindicati parametri contenuti nella query string. Un esempio di tale URL è il seguente, nel quale sono evidenziati in grassetto i parametri citati (i valori di alcuni parametri sono stati ridotti per brevità, inoltre il valore del parametro "RelayState" è stato reso non immediatamente intellegibile, come suggerito dalla specifica, sostituendo la stringa in chiaro con l'Id della richiesta: il Service Provider tiene traccia della corrispondenza)

Esempio: HTTP redirect query string
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

   'SAMLRequest'=nVPLbtswELz3KwTeZb0M2SYsBa6NoAbSRrGUHnqjqFVDQCJVLuU4f19KlhEDb
   VygR5K7O7Mzw%2FXdqW2cI2gUSiYkmPnEAclVJeTPhDwX%2B6S3KWf1sjapqOb3rzIA%2FzqAY2zQQRtbNtWSe
   [...]
   ZwPAU88aUQvQ%2F8oe8S68piBDNabB5s3AyThb1XZMCxxEhhPj5qLZddW2sZIcoP4fBW%2BWccqH0fZ6iNir0tU
   QGeCWZaGZxE5pM4n8Nz7p%2Be2D3S6L51x1NljO%2BCO2qh8zO%2Bji%2FfnN098%3D&'RelayState'=s29f6c7d
   6bbf9e62968d27309e2e4beb6133663a2e&'SigAlg'=http%3A%2F%2Fwww.w3.org%2F2000%2F09%2Fxmldsig
   %23rsa-sha1&'Signature'=LtNj%2BbMc8j%2FhglWzHPMmo0ESQzBaWlmQbZxas%2B%2FIfNO4F%2F7WNoMKDZ4
   VVYeBtCEQKWp12pU7vPB5WVVMRMrGB8ZRAdHmmPp0hJ9opO3NdafRc04Z%2BbfnkSuQCN9NcGV%2BajT
   [...]
   ra169jhaGRReRQ9KkgSB3aTpQGaffAYUPVo2XZiWy6f9Z7zsmV%2FFoT8dg%3D%3D


Binding HTTP POST
^^^^^^^^^^^^^^^^^^^^^

Nel caso del binding HTTP POST, come risposta alla richiesta di accesso dell'utente ad un
servizio o risorsa, il SP invia allo User Agent (il browser dell'utente) un messaggio HTTP con
status code avente valore 200 ("OK"):

    * **il messaggio HTTP contiene una form HTML** all'interno della quale è trasportato un costrutto SAML <AuthnRequest> codificato come valore di un hidden form control di nome "SAMLRequest". Rispetto al binding HTTP Redirect, l'utilizzo di una form HTML permette di superare i limiti di dimensione della query string. Pertanto, l'intero messaggio SAML in formato XML può essere firmato in accordo alla specifica XML Digital Signature. Il risultato a valle della firma è quindi codificato in formato Base64
    * la form HTML contiene un secondo hidden form control di nome **"RelayState"** che contiene il corrispondente valore del Relay State, cioè della risorsa originariamente richiesta dall'utente e alla quale dovrà essere trasferito il controllo al termine della fase di autenticazione
    * **la form HTML è corredata da uno script che la rende auto-postante** all'indirizzo indicato nell'attributo "action"
    * **Il browser** dell'utente elabora quindi la risposta HTTP e invia una richiesta HTTP POST verso il componente Single Sign-On dell'Identity Provider (interfaccia IAuthnRequest).

Un esempio di form HTML per trasferire in HTTP POST la richiesta di autenticazione è descritto nell'esempio successivo. Osservando attentamente il codice riportato in figura si può notare il valore del parametro "SAMLRequest" (ridotto per brevità); il valore del parametro RelayState reso non immediatamente intellegibile (cfr. sez. precedente); l'elemento *<input type="submit" value="Invia"/>*, che ha lo scopo di visualizzare all'interno del web browser il pulsante di invio della form utilizzabile dall'utente, non strettamente necessario in quanto la form è resa autopostante.

.. literalinclude:: code-samples/richiesta-http-post-binding.html
   :language: html
   :linenos:

Conclusa la fase di autenticazione, l'Identity Provider costruisce una **<Response>** firmata diretta al Service Provider, e in particolare al relativo servizio **AssertionConsumerService**. La **<Response>** viene inserita in una form HTML come campo nascosto di nome **"SAMLResponse"**. L'Identity Provider invia la form HTML al browser dell'utente in una risposta HTTP.

Il browser dell'utente elabora quindi la risposta HTTP e invia una richiesta HTTP POST contenente la **<Response>** firmata verso il Service Provider. Un esempio di tale form è riportato di seguito, anche in questo caso, il valore del parametro "SAMLResponse" è stato ridotto per brevità

.. literalinclude:: code-samples/risposta-http-post-binding.html
   :language: html
   :linenos:


Gestione della sicurezza sul canale di trasmissione
---------------------------------------------------

.. Important::
    Il profilo SAML SSO raccomanda l'uso di **TLS 1.2** nei colloqui tra Asserting party (Identity Provider e Attribute Authority), le Relaying Party (Service Provider) e lo user agent.
    In ambito  SPID si rende obbligatorio l'impiego di TLS 1.2.
    In casi particolari e temporanei,  può  essere  adottata  la  versione  1.1,  fermo  restando  che  la  versione  1.2 deve  essere  adottata  lato server.
    Conseguentemente, le versioni più obsolete dei browser, che non supportano le versioni del protocollo indicate, non devono essere utilizzate e, i livelli minimi accettati per l’accesso ai servizi SPID, devono essere documentati nei confronti degli utenti.


Metadata
--------

Le caratteristiche dell'Identity provider devono essere definite attraverso metadata conformi allo standard SAML v2.0 (SAML-Metadata), e rispettare le condizioni di seguito indicate:


<EntityDescriptor>
^^^^^^^^^^^^^^^^^^

.. Important::
    nell'elemento **<EntityDescriptor>** devono essere presenti i seguenti attributi:

    * **entityID** indicante l'identificativo (URI) dell'entità univoco in ambito SPID
    * **ID** univoco, per esempio basato su un Universally Unique Identifier (UUID) o su una combinazione origine + timestamp  (quest'ultimo generato con una precisione di almeno un millesimo di secondo per garantire l'univocità)


<IDPSSODescriptor>
^^^^^^^^^^^^^^^^^^

.. Important::
    * l'elemento **<IDPSSODescriptor>** specifico che contraddistingue l'entità di tipo Identity provider deve riportare i seguenti attributi:
        * **protocolSupportEnumeration**: che enumera gli URI indicanti i protocolli supportati dall'entità (poiché si tratta di un'entità SAML 2.0, deve indicare almeno il valore del relativo protocollo: "urn:oasis:names:tc:SAML:2.0:protocol")
        * **WantAuthnRequestSigned**: attributo con valore booleano che impone ai Service Provider che fanno uso di questo Identity provider l'obbligo della firma delle richieste di autenticazione;


<KeyDescriptor>
^^^^^^^^^^^^^^^

.. Important::
    all'interno di IDPSSODescriptor devono essere presenti:

        * l'elemento **<KeyDescriptor>** che contiene l'elenco dei certificati e delle corrispondenti chiavi pubbliche dell'entità, utili per la verifica della firma dei messaggi prodotti da tale entità nelle sue interazioni con le altre (SAMLMetadata, par. 2.4.1.1)
        * l'elemento **<KeyDescriptor>** che contiene il certificato della corrispondente chiave pubblica dell'entità, utile per la verifica della firma dei messaggi prodotti da tale entità nelle sue interazioni con le altre (SAML-Metadata, par. 2.4.1.1)

<NameIDFormat>
^^^^^^^^^^^^^^^

.. Important::
    * l'elemento **<NameIDFormat>** riportante l'attributo:
        * **format**, indicante il formato "urn:oasis:names:tc:SAML:2.0:nameidformat:transient" come quello supportato per l'elemento di **<NameID>** utilizzato nelle richieste e risposte SAML per identificare il subject cui si riferisce un'asserzione


<SingleLogoutService>
^^^^^^^^^^^^^^^^^^^^^

.. Important::
    * uno o più elementi **<SingleLogoutService>** che specificano l'indirizzo del Single Sign-On Service riportanti i seguenti attributi:
        * **Location** url endpoint del servizio per la ricezione delle richieste Binding che può assumere uno dei valori
            * "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect"
            * "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
        * **ResponseLocation**


<SingleSignOnService>
^^^^^^^^^^^^^^^^^^^^^

.. Important::
    * uno o più elementi **<SingleSignOnService>** che specificano l'indirizzo del Single Sign-On Service riportanti i seguenti attributi:
        * **Location** url endpoint del servizio per la ricezione delle richieste Binding che può assumere uno dei valori
            * "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect"
            * "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"


<Attribute>
^^^^^^^^^^^

.. Note::
    opzionalmente possono essere presenti:
    * uno o più elementi **<Attribute>** ad indicare nome e formato degli attributi certificabili dell'Identity provider (Tabella attributi SPID),riportanti gli attributi:

        * **Name** nome dell'attributo (colonna identificatore della Tabella attributi SPID)
        * **xsi:type** tipo dell'attributo (colonna tipo della Tabella attributi SPID)


<Signature>
^^^^^^^^^^^

.. Important::
    deve essere l'elemento **<Signature>** riportante la firma sui metadata.
    La firma deve essere prodotta secondo il profilo specificato per SAML (SAML-Metadata, cap. 3) utilizzando chiavi RSA almeno a 2048 bit e algoritmo di digest SHA-256 o superiore;


<Organization>
^^^^^^^^^^^^^^

.. Note::
    è consigliata la presenza di un elemento **<Organization>** a indicare l'organizzazione a cui afferisce l'entità specificata, riportante gli elementi:
        * **<OrganizationName>** indicante un identificatore language-qualified dell'organizzazione a cui l'entità afferisce
        * **<OrganizationURL>** riportante in modalità language-qualified la url istituzionale dell'organizzazione


Esempio: Metadata IdP
^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: code-samples/idp-metadata.xml
   :language: xml
   :linenos:


Disponibilità dei Metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^

I metadata Identity Provider saranno disponibili per tutte le entità SPID federate attraverso l'interfaccia IMetadataRetrive alla URL **https://<dominioGestoreIdentita>/metadata**, ove non diversamente specificato nel **Registro SPID**, e saranno firmate in modalita detached dall'Agenzia per l'Italia Digitale.
L'accesso deve essere effettuato utilizzando il protocollo TLS nella versione più recente disponibile.
