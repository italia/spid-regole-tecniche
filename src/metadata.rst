Metadata
========

Ciascuna entità presente nella federazione SPID è descritta da un file di metadati, che ne riporta il certificato X509, gli endpoint e le altre informazioni necessarie alla comunicazione con le altre entità. 

.. Note::
    La distribuzione dei metadati a tutti i soggetti è operata dall'Agenzia per l'Italia Digitale attraverso il `Registro <https://registry.spid.gov.it/>`_.


Identity Provider
-----------------

Le caratteristiche dell'Identity provider sono definite attraverso metadata conformi allo standard SAML v2.0 (SAML-Metadata) e rispettano le condizioni di seguito indicate:

* nell'elemento ``<EntityDescriptor>`` deve essere presente il seguente attributo:

    * ``entityID`` indicante l'identificativo (URI) dell'entità univoco in ambito SPID

* l'elemento ``<IDPSSODescriptor>`` specifico che contraddistingue l'entità di tipo Identity Provider deve riportare i seguenti attributi:

    * ``protocolSupportEnumeration``: che enumera gli URI indicanti i protocolli supportati dall'entità (poiché si tratta di un'entità SAML 2.0, deve indicare almeno il valore del relativo protocollo: ``urn:oasis:names:tc:SAML:2.0:protocol``)
    * ``WantAuthnRequestSigned``: attributo con valore booleano che impone ai Service Provider che fanno uso di questo Identity provider l'obbligo della firma delle richieste di autenticazione;

    all'interno di ``<IDPSSODescriptor>`` devono essere presenti:

        * l'elemento ``<KeyDescriptor>`` che contiene l'elenco dei certificati e delle corrispondenti chiavi pubbliche dell'entità, utili per la verifica della firma dei messaggi prodotti da tale entità nelle sue interazioni con le altre (SAMLMetadata, par. 2.4.1.1)
        * l'elemento ``<KeyDescriptor>`` che contiene il certificato della corrispondente chiave pubblica dell'entità, utile per la verifica della firma dei messaggi prodotti da tale entità nelle sue interazioni con le altre (SAML-Metadata, par. 2.4.1.1)
        * l'elemento ``<NameIDFormat>`` riportante l'attributo:
        
            * ``format``, indicante il formato ``urn:oasis:names:tc:SAML:2.0:nameidformat:transient`` come quello supportato per l'elemento di ``<NameID>`` utilizzato nelle richieste e risposte SAML per identificare il *subject* cui si riferisce un'asserzione

        * uno o più elementi ``<SingleSignOnService>`` che specificano l'indirizzo del Single Sign-On Service riportanti i seguenti attributi:
        
            * ``Location`` URL endpoint del servizio per la ricezione delle richieste
            * ``Binding`` che può assumere uno dei valori
            
                * ``urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect``
                * ``urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST``

        * uno o più elementi ``<SingleLogoutService>`` che specificano l'indirizzo del Single Logout Service riportanti i seguenti attributi:
        
            * ``Location`` URL endpoint del servizio per la ricezione delle richieste di Single Logout;
            * ``Binding`` che può assumere uno dei valori
            
                * ``urn:oasis:names:tc:SAML:2.0:bindings:SOAP``
                * ``urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect``
                * ``urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST``

                .. Note::
                    Ad oggi, nessun Identity Provider espone un SingleLogoutService basato su SOAP.
                
            * ``ResponseLocation`` (opzionale): URL endpoint del servizio per la ricezione delle risposte alle richieste di Single Logout.

    opzionalmente possono essere presenti:
    
        * uno o più elementi ``<Attribute>`` ad indicare nome e formato degli attributi SPID certificabili dell'Identity Provider (cfr. Tabella attributi SPID), riportanti gli attributi:

            * ``Name``: nome dell'attributo SPID (colonna *identificatore* della Tabella attributi SPID)
            * ``xsi:type``: tipo dell'attributo (colonna *tipo* della Tabella attributi SPID)

* deve essere presente l'elemento ``<Signature>`` riportante la firma sui metadata. La firma deve essere prodotta secondo il profilo specificato per SAML (SAML-Metadata, cap. 3) utilizzando chiavi RSA almeno a 1024 bit e algoritmo di digest SHA-256 o superiore;

* è consigliata la presenza di un elemento ``<Organization>`` a indicare l'organizzazione a cui afferisce l'entità specificata, riportante gli elementi:

    * ``<OrganizationName>`` indicante un identificatore language-qualified dell'organizzazione a cui l'entità afferisce
    * ``<OrganizationURL>`` riportante in modalità language-qualified la URL istituzionale dell'organizzazione.

Esempio: Metadata IdP
^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: code-samples/idp-metadata.xml
   :language: xml
   :linenos:


Disponibilità dei metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^

I metadata Identity Provider saranno disponibili per tutte le entità SPID federate attraverso la URL *https://<dominioGestoreIdentita>/metadata*, ove non diversamente specificato nel **Registro SPID**, e saranno firmate in modalita detached dall'Agenzia per l'Italia Digitale.
L'accesso deve essere effettuato utilizzando il protocollo TLS nella versione più recente disponibile.


Service Provider
----------------

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
