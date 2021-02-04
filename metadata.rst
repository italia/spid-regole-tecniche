Metadata
========

Ciascuna entità presente nella federazione SPID è descritta da un file di metadati, che ne riporta il certificato X509, gli endpoint e le altre informazioni necessarie alla comunicazione con le altre entità. 

.. Note::
    La distribuzione dei metadati a tutti i soggetti è operata dall'Agenzia per l'Italia Digitale attraverso il `Registro <https://registry.spid.gov.it/>`_.

.. Warning::
    I fornitori di servizi pubblici - come identificati nell’Avviso AgID numero 28/2020 - possono continuare ad utilizzare certificati self-signed.
    Per la richiesta di emissione dei certificati digitali ai fini del Sistema Pubblico delle Identità Digitali (SPID) si rimanda ai seguenti Avvisi:
    - `Avviso AgID n23 <https://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n23-certificati-agid-per-soggetti-spid_v.2_0.pdf>`_ 
    - `Avviso AgID n23 - modulo di richiesta <https://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n23-mod.-richiesta-registrazione.pdf>`_


Identity Provider
-----------------

Le caratteristiche dell'Identity provider sono definite attraverso metadata conformi allo standard SAML v2.0 (SAML-Metadata) e rispettano le condizioni di seguito indicate:

.. admonition:: SI DEVE

    * Nell'elemento ``<EntityDescriptor>`` deve essere presente il seguente attributo:

        * ``entityID`` indicante l'identificativo (URI) dell'entità univoco in ambito SPID

    * L'elemento ``<IDPSSODescriptor>`` specifico che contraddistingue l'entità di tipo Identity Provider deve riportare i seguenti attributi:

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

    * Deve essere presente l'elemento ``<Signature>`` riportante la firma sui metadata. La firma deve essere prodotta secondo il profilo specificato per SAML (SAML-Metadata, cap. 3) 
      utilizzando chiavi RSA almeno a 2048 bit e algoritmo di digest SHA-256 o superiore. Gli SP pubblici possono creare autonomamente i certificati elettronici necessari e questi possono essere anche
      di tipo self-signed. I Fornitori Privati dovranno fare invece richiesta presso AgID. 
    
    
.. admonition:: SI PUÒ

    * È consigliata la presenza di un elemento ``<Organization>`` a indicare l'organizzazione a cui afferisce l'entità specificata, riportante gli elementi:

        * ``<OrganizationName>`` indicante un identificatore language-qualified dell'organizzazione a cui l'entità afferisce
        * ``<OrganizationURL>`` riportante in modalità language-qualified la URL istituzionale dell'organizzazione.

Esempio: metadata IdP
^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: code-samples/idp-metadata.xml
   :language: xml
   :linenos:


Disponibilità dei metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^

I metadata Identity Provider saranno disponibili per tutte le entità SPID federate attraverso 
la URL *https://<dominioGestoreIdentita>/metadata*, ove non diversamente specificato 
nel **Registro SPID**, e saranno firmate in modalita detached dall'Agenzia per l'Italia Digitale. 
L'accesso deve essere effettuato utilizzando il protocollo TLS nella versione più recente disponibile.


Service Provider
----------------

.. warning::
    Per la struttura dei certificati elettronici e dei Metadata dei Service Provider si rimanda alle specifiche contenute in 
    `Avviso AgID n29 v3 <https://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n29v3-specifiche_sp_pubblici_e_privati_0.pdf>`_.

Le caratteristiche del Service Provider devono essere definite attraverso metadata conformi allo standard SAML v2.0 (SAML-Metadata) e rispettare le condizioni di seguito indicate:

.. admonition:: SI DEVE

    * Nell'elemento ``<EntityDescriptor>`` deve essere presente il seguente attributo:

        * ``entityID``: indicante l'identificativo univoco (un URI) dell'entità;

    * Deve essere presente l'elemento ``<KeyDescriptor>`` contenenete il certificato della corrispondente chiave pubblica dell'entità, utile per la verifica della firma dei messaggi prodotti da tale entità nelle sue interazioni con le altre (SAML-Metadata, par. 2.4.1.1);
    * Deve essere presente l'elemento ``<Signature>`` riportante la firma sui metadata. La firma deve essere prodotta secondo il profilo specificato per SAML (SAML-Metadata, cap. 3) utilizzando chiavi RSA almeno a 1024 bit e algoritmo di digest SHA-256 o superiore;
    * Deve essere presente l’elemento ``<SPSSODescriptor>`` riportante i seguenti attributi:

        * ``protocolSupportEnumeration``: che enumera, separati da uno spazio, gli URI associati ai protocolli supportati dall’entità (poiché si tratta di un’entità SAML 2.0, deve indicare almeno il valore del relativo protocollo: ``urn:oasis:names:tc:SAML:2.0:protocol``);
        * ``AuthnRequestSigned``: valorizzato ``true`` attributo con valore booleano che esprime il requisito che le richieste di autenticazione inviate dal Service Provider siano firmate;

    * Deve essere presente almeno un elemento ``<AssertionConsumerService>`` indicante il servizio (in termini di URL e relativo binding HTTP-POST) a cui contattare il Service Provider per l’invio di risposte SAML, riportante i seguenti attributi:

        * ``index`` che può assumere valori unsigned;
        * ``Binding`` posto al valore ``urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST``;
        * ``Location`` URL endpoint del servizio per la ricezione delle risposte;

        In particolare il primo di questi elementi (o l’unico elemento riportato) deve obbligatoriamente riportare:

        * l’attributo ``index`` posto al valore ``0``;
        * l’attributo ``isDefault`` posto al valore ``true``;

    * Deve essere presente almeno un elemento ``<SingleLogoutService>`` indicante l'indirizzo del SingleLogoutService e riportante i seguenti attributi:

        * ``Location`` URL endpoint del servizio per la ricezione delle richieste di Single Logout;
        * ``Binding`` che può assumere uno dei valori
    
            * ``urn:oasis:names:tc:SAML:2.0:bindings:SOAP``
            * ``urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect``
            * ``urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST``
        
        ed opzionalmente l'attributo:
        
            * ``ResponseLocation``, URL endpoint del servizio per la ricezione delle risposte alle richieste di Single Logout.
    
    * Deve essere presente uno o più elementi ``<AttributeConsumingService>`` a descrizione dei set di attributi richiesti dal Service Provider, riportante:

        * l’attributo ``index``, indice posizionale dell’elemento relativo all'i-esimo servizio richiamato dalla AuthnRequest mediante l’attributo ``AttributeConsumingServiceIndex``;
        * l’elemento ``<ServiceName>``, riportante l’identificatore dell'i-esimo set minimo di attributi necessari per l’autorizzazione all’accesso (per la massima tutela della privacy dell'utente il Service Provider deve rendere minima la visibilità dei servizi effettivamente invocati; in questa logica occorre rendere ove possibile indifferenziate le richieste relative a servizi che condividono lo stesso set minimo di attributi necessari per l'autorizzazione);

.. admonition:: SI PUÒ

    * È consigliata la presenza di un elemento ``<Organization>`` a indicare l’organizzazione a cui afferisce l’entità specificata, riportante gli elementi:

        * ``<OrganizationName>`` indicante un identificatore language-qualified dell’organizzazione a cui l’entità afferisce;
        * ``<OrganizationURL>`` riportante in modalità language-qualified la URL istituzionale dell’organizzazione.

Esempio: metadata SP
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
