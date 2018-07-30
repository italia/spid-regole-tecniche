Fornitore di servizi (Service Provider)
=======================================

**Il Fornitore di Servizi (Service Provider o SP)** eroga i servizi agli utenti, richiede l'autenticazione agli Identity Provider e gestisce la procedura di autorizzazione al servizio.

Metadata
--------

Le caratteristiche del Service Provider devono essere definite attraverso metadata conformi allo standard SAML v2.0 (SAML-Metadata) e rispettare le condizioni di seguito indicate:

* nell'elemento ``<EntityDescriptor>`` deve essere presente il seguente attributo:

    * ``entityID``: indicante l'identificativo univoco (un URI) dell'entità;

* deve essere presente l'elemento ``<KeyDescriptor>`` contenenete il certificato della corrispondente chiave pubblica dell'entità, utile per la verifica della firma dei messaggi prodotti da tale entità nelle sue interazioni con le altre (SAML-Metadata, par. 2.4.1.1);
* deve essere presente l'elemento ``<Signature>`` riportante la firma sui metadata. La firma deve essere prodotta secondo il profilo specificato per SAML (SAML-Metadata, cap. 3) utilizzando chiavi RSA almeno a 1024 bit e algoritmo di digest SHA-256 o superiore;
* deve essere presente l’elemento ``<SPSSODescriptor>`` riportante i seguenti attributi:

    * ``protocolSupportEnumeration``: che enumera, separati da uno spazio, gli URI associati ai protocolli supportati dall’entità (poiché si tratta di un’entità SAML 2.0, deve indicare almeno il valore del relativo protocollo: ``urn:oasis:names:tc:SAML:2.0:protocol``);
    * ``AuthnRequestSigned``: valorizzato ``true`` attributo con valore booleano che esprime il requisito che le richieste di autenticazione inviate dal Service Provider siano firmate;

* deve essere presente almeno un elemento ``<AssertionConsumerService>`` indicante il servizio (in termini di URL e relativo binding HTTP-POST) a cui contattare il Service Provider per l’invio di risposte SAML, riportante i seguenti attributi:

    * ``index`` che può assumere valori unsigned;
    * ``Binding`` posto al valore ``urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST``;
    * ``Location`` URL endpoint del servizio per la ricezione delle risposte;

    In particolare il primo di questi elementi (o l’unico elemento riportato) deve obbligatoriamente riportare:

    * l’attributo ``index`` posto al valore ``0``;
    * l’attributo ``isDefault`` posto al valore ``true``;

* deve essere presente uno o più elementi ``<AttributeConsumingService>`` a descrizione dei set di attributi richiesti dal Service Provider, riportante:

    * l’attributo ``index``, indice posizionale dell’elemento relativo all'i-esimo servizio richiamato dalla AuthnRequest mediante l’attributo ``AttributeConsumingServiceIndex``;
    * l’elemento ``<ServiceName>``, riportante l’identificatore dell'i-esimo set minimo di attributi necessari per l’autorizzazione all’accesso (per la massima tutela della privacy dell'utente il Service Provider deve rendere minima la visibilità dei servizi effettivamente invocati; in questa logica occorre rendere ove possibile indifferenziate le richieste relative a servizi che condividono lo stesso set minimo di attributi necessari per l'autorizzazione);

* è consigliata la presenza di un elemento ``<Organization>`` a indicare l’organizzazione a cui afferisce l’entità specificata, riportante gli elementi:

    * ``<OrganizationName>`` indicante un identificatore language-qualified dell’organizzazione a cui l’entità afferisce;
    * ``<OrganizationURL>`` riportante in modalità language-qualified la URL istituzionale dell’organizzazione.

Esempio: Metadata SP
^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: code-samples/sp-metadata.xml
   :language: xml
   :linenos:

Disponibilità dei metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^

I metadata dei Service Provider saranno disponibili per tutte le entità SPID federate attraverso la URL **https://<dominioServiceProvider>/metadata** e saranno firmate dall'Agenzia per l'Italia Digitale.
L'accesso deve essere effettuato utilizzando il protocollo TLS nella versione più recente disponibile.

.. Note::
    Nonostante sia richiesta la pubblicazione dei metadata nel dominio del Service Provider, la distribuzione dei metadati agli Identity Provider è operata centralmente dall'Agenzia per l'Italia Digitale. Gli Identity Provider di conseguenza non ottengono i metadati direttamente dai Service Provider.


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
