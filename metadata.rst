Metadata
========

Ciascuna entità presente nella federazione SPID è descritta da un file di metadati, che ne riporta il certificato X509, gli endpoint e le altre informazioni necessarie alla comunicazione con le altre entità. 

.. Note::
    La distribuzione dei metadati a tutti i soggetti è operata dall'Agenzia per l'Italia Digitale attraverso il `Registro <https://registry.spid.gov.it/>`_.

.. Warning::
    I fornitori di servizi pubblici - come identificati nell’`Avviso AgID numero 28/2020 <https://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n28-le_pa_nella_federazione_spid.pdf>`_ - possono continuare ad utilizzare certificati self-signed.
    Per la richiesta di emissione dei certificati digitali ai fini del Sistema Pubblico delle Identità Digitali (SPID) si rimanda ai seguenti Avvisi:
    - `Avviso AgID n23 <https://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n23-certificati-agid-per-soggetti-spid_v.2_0.pdf>`_ 
    - `Avviso AgID n23 - modulo di richiesta <https://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n23-mod.-richiesta-registrazione.pdf>`_


Disponibilità dei metadata
--------------------------

I metadata Identity Provider saranno disponibili per tutte le entità SPID federate attraverso 
la URL *https://<dominioGestoreIdentita>/metadata*, ove non diversamente specificato 
nel **Registro SPID**, e saranno firmate in modalita detached dall'Agenzia per l'Italia Digitale. 
L'accesso deve essere effettuato utilizzando il protocollo TLS nella versione più recente disponibile.

I metadata dei Service Provider saranno disponibili per tutte le entità SPID federate attraverso la URL **https://<dominioServiceProvider>/metadata** e saranno firmate dall'Agenzia per l'Italia Digitale.
L'accesso deve essere effettuato utilizzando il protocollo TLS nella versione più recente disponibile.

.. Note::
    Nonostante sia richiesta la pubblicazione dei metadata nel dominio del Service Provider, la distribuzione dei metadati agli Identity Provider è operata centralmente dall'Agenzia per l'Italia Digitale. Gli Identity Provider di conseguenza non ottengono i metadati direttamente dai Service Provider.



Identity Provider
-----------------

Le caratteristiche dell'Identity provider sono definite attraverso metadata conformi allo standard SAML v2.0 (SAML-Metadata) e rispettano le condizioni di seguito indicate:

.. admonition:: SI DEVE

    * Nell'elemento ``<EntityDescriptor>`` deve essere presente il seguente attributo:

        * ``entityID`` indicante l'identificativo (URI) dell'entità univoco in ambito SPID

    * L'elemento ``<IDPSSODescriptor>`` contraddistingue l'entità di tipo Identity Provider e deve riportare i seguenti attributi:

        * ``protocolSupportEnumeration``: che enumera gli URI indicanti i protocolli supportati dall'entità (poiché si tratta di un'entità SAML 2.0, deve indicare almeno il valore del relativo protocollo: ``urn:oasis:names:tc:SAML:2.0:protocol``)
        * ``WantAuthnRequestSigned``: attributo con valore booleano che impone ai Service Provider che fanno uso di questo Identity provider l'obbligo della firma delle richieste di autenticazione;

        all'interno di ``<IDPSSODescriptor>`` devono essere presenti:

            * l'elemento ``<KeyDescriptor>`` che contiene l'elenco dei certificati e delle corrispondenti chiavi pubbliche dell'entità, utili per la verifica della firma dei messaggi prodotti da tale entità nelle sue interazioni con le altre (SAMLMetadata, par. 2.4.1.1)

            * l'elemento ``<NameIDFormat>`` riportante l'attributo:
        
                * ``format``, indicante il formato ``urn:oasis:names:tc:SAML:2.0:nameidformat:transient`` come quello supportato per l'elemento di ``<NameID>`` utilizzato nelle richieste e risposte SAML per identificare l'identificativo del soggetto (*subject id*) cui si riferisce un'asserzione

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


Service Provider
----------------

Le caratteristiche del Service Provider devono essere definite attraverso 
metadata conformi allo standard SAML v2.0 (SAML-Metadata) e rispettare 
le condizioni di seguito indicate:

* Nell'elemento ``<EntityDescriptor>`` deve essere presente il seguente attributo:

    * entityID (1 occorrenza) - Attributo valorizzato con l’Entity id,
      così come riportato nell’estensione commonName del certificato
      elettronico del SP. In caso il SP svolga più attività - come ad
      esempio quella di SP pubblico e di SP privato - si dota di metadata
      saml differenti, ciascuno con un diverso Entity id.

* Deve essere presente l'elemento ``<KeyDescriptor>`` contenenete il certificato della corrispondente chiave pubblica dell'entità, utile per la verifica della firma dei messaggi prodotti da tale entità nelle sue interazioni con le altre (SAML-Metadata, par. 2.4.1.1);
* Deve essere presente l'elemento ``<Signature>`` riportante la firma sui metadata. La firma deve essere prodotta secondo il profilo specificato per SAML (SAML-Metadata, cap. 3) utilizzando chiavi RSA almeno a 2048 bit e algoritmo di digest SHA-256 o superiore;
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


*  Organization (1 occorrenza) — Contiene vari tag, ciascuno dei quali
   ripetuto almeno una volta valorizzato in lingua italiana, più
   occorrenze facoltative localizzanti il medesimo nome in ulteriori
   lingue (identificate mediante l’attributo xml:lang, obbligatoriamente
   presente in tutti i tag figli):

   -  OrganizationName (1 o più occorrenze) — Denominazione – *completa
      e per esteso* e con il corretto uso di minuscole, maiuscole,
      lettere accentate e altri segni diacritici – del SP, così come
      riportata nell’estensione organizationName del certificato
      elettronico del SP (esempio: “Agenzia per l'Italia Digitale”).

   -  OrganizationDisplayName (1 o più occorrenze) — Denominazione del
      SP, eventualmente in forma abbreviata (senza
      esplicitare gli eventuali acronimi) con il corretto utilizzo
      delle minuscole e maiuscole (esempio: “AgID”). Durante la fase di
      autenticazione, gli IdP avvisano l’utente dell’invio degli
      attributi al SP, visualizzando il valore di questo tag per
      indicare il soggetto richiedente.

   -  OrganizationURL (1 o più occorrenze) — Contiene l’ url di una
      pagina del sito web del SP relativa al servizio di autenticazione
      o ai servizi accessibili tramite essa, i cui contenuti sono
      localizzati nella lingua specificata dal proprio attributo
      xml:lang.

*  ContactPerson (1 o 2 occorrenze) — Tag utilizzato per veicolare le
   informazioni per contattare il soggetto cui il metadata afferisce.
   Ogni occorrenza è dotata dei seguenti attributi:

   -  contactType — L’occorrenza *obbligatoria* di ContactPerson è
      valorizzata con other; l’ulteriore occorrenza, obbligatoria per i
      soli SP privati, è valorizzata con billing.

..

   L’occorrenza di ContactPerson con l’attributo contactType valorizzato
   come other contiene i seguenti tag (*namespace* md):

-  Extensions (1 occorrenza *obbligatoria*) — Contenente almeno uno dei
   seguenti tag (tutti con *namespace* spid):

   1. IPACode — Presente *solo* per il SP *pubblico*, è valorizzato con
      il codice ipa dell’Ente.

   2. VATNumber — Obbligatorio per il SP *privato* dotato di partita iva
      (altrimenti facoltativo), è valorizzato comprensivo del codice ISO
      3166-1 α-2 del Paese (senza spazi).

   3. FiscalCode — Obbligatorio per il SP *privato* non dotato di
      partita iva (altrimenti facoltativo), è valorizzato con il codice
      fiscale del SP.

   4. Public — Tag vuoto, *obbligatorio* per il SP pubblico o,

..

   *in alternativa*,

* Private — Tag vuoto, *obbligatorio* per il SP privato.

-  Company (0 o 1 occorrenze) — Se presente, è valorizzato come il tag
   OrganizationName contenuto nel tag Organization.

-  EmailAddress (1 occorrenza, *obbligatorio*) — Contiene l’indirizzo di
   posta elettronica per contattare il SP. Non deve trattarsi di un
   indirizzo riferibile direttamente ad una persona fisica.

-  TelephoneNumber (0 o 1 occorrenze) — Contiene il numero di telefono,
   per contattare il SP; *senza spazi* e comprensivo del prefisso
   internazionale (esempio: “+39” per l’Italia).


Certificati 
^^^^^^^^^^^


I certificati di sigillo elettronico utilizzati dai SP
pubblici e privati sono conformi a `RFC-5280 <https://tools.ietf.org/html/rfc5280>`_
e devono contenere le seguenti estensioni, valorizzate con il corretto uso di minuscole, maiuscole, lettere
accentate e altri segni diacritici:

1. Nel campo **SubjectDN**:

   a. **commonName** (oid `2.5.4.3 <http://oid-info.com/get/2.5.4.3>`__) —
      Entity id del SP, così come riportato nell’attributo entityID del
      tag xml <EntityDescriptor> del metadata del SP.

   b. **organizationName** (oid `2.5.4.10 <http://oid-info.com/get/2.5.4.10>`__) — Denominazione
      *completa e per esteso* del SP, così indicata nei pubblici
      registri e come riportata nel tag xml <OrganizatioName> del
      metadata del SP (esempio: “Comune di Forlì” e *non* “COMUNE DI FORLI'”);

   c. **organizationIdentifier** (oid `2.5.4.97 <http://oid-info.com/get/2.5.4.97>`__) 
         Codice identificativo unico del SP all’interno della federazione SPID,
         conforme alla sintassi prevista dalla norma ETSI `en 319-412-1 <http://www.etsi.org/deliver/etsi_en/319400_319499/31941201/01.01.01_60/en_31941201v010101p.pdf>`__,
         §5.1.4:

      **SP pubblici**
      valorizzato con il prefisso ‘PA:IT-’ seguito dal
      codice ipa dell’Ente — esempio, per il Comune di Roma
      (codice ipa ‘c_h501’) tale estensione è valorizzata come
      “PA:IT-c_h501”;

      **SP privati** — la seguente alternativa di codici
      utilizzando, in ordine di preferenza:

            il numero di partita iva, preceduto dal prefisso ‘VAT,’ seguito dal
            codice ISO 3166-1 α-2 del Paese, seguito dal carattere ‘-’
            (esempio, “VAT IT-12345678901”);

            per i soggetti *non* provvisti di partita iva, il codice
            fiscale, preceduto dal prefisso ‘CF:IT-’ (esempio: “CF: IT-XYZABCAAMGGJ000W”);

      Altro codice alternativo fornito da AgID in casi particolari.

   d. **countryName** (oid `2.5.4.6 <http://oid-info.com/get/2.5.4.6>`__) —
      il codice ISO 3166-1  del Paese ove è situata la sede legale
      del SP (esempio: “IT”);

   e. **localityName** (oid `2.5.4.7 <http://oid-info.com/get/2.5.4.7>`__) —
      il nome completo della città ove è situata la sede legale del SP
      (esempio: *Forlì* e **non** *Forli'*).

2. Nel campo **CertificatePolicies**:

   **policyIdentifier** — contenente almeno uno dei seguenti
   identificatori:

    - **SP pubblici** — spid-publicsector-SP (oid
      `1.3.76.16 <http://oid-info.com/get/1.3.76.16>`__.\ **4.2.1**);

    - **SP privati** — spid-privatesector-SP (oid
      `1.3.76.16 <http://oid-info.com/get/1.3.76.16>`__.\ **4.3.1**).

Trattandosi di certificati di *sigillo elettronico* e non di certificati
di firma elettronica, gli attributi name (oid `2.5.4.41 <http://oid-info.com/get/2.5.4.41>`__), 
surname (oid `2.5.4.4 <http://oid-info.com/get/2.5.4.4>`__), 
givenName (oid `2.5.4.42 <http://oid-info.com/get/2.5.4.42>`__), 
initials (oid `2.5.4.43 <http://oid-info.com/get/2.5.4.43>`__) e 
pseudonym (oid `2.5.4.65 <http://oid-info.com/get/2.5.4.65>`__) non devono essere
utilizzati. 
Altre estensioni, come ad esempio emailAddress (oid `1.2.840.113549.1.9.1 <http://oid-info.com/get/1.2.840.113549.1.9.1>`__),
se presenti, non sono valorizzate con dati personali afferenti a persone fisiche.

Gli SP pubblici possono creare autonomamente i certificati elettronici
necessari. I certificati possono anche essere di tipo *self-signed*.
Qualora il SP pubblico utilizzi un certificato dedicato all’apposizione
del sigillo elettronico sul proprio metadata e un altro certificato
dedicato all’apposizione di sigilli elettronici sulle proprie *request*,
il presente Avviso si applica ad entrambi.

A seguito dell’accreditamento presso AgID, i SP privati ricevono un
**certificato di federazione** emesso dall’infrastruttura a
chiave pubblica (**pki**) che AgID ha istituito appositamente per la
gestione dell’intera federazione SPID. Al fine di ottenere detto
certificato si deve far riferimento all’Avviso SPID n23/2016 e s.m.i. e
compilare il previsto
`modulo <http://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n23-allegato-mod-richiesta-registrazione.pdf>`__
di richiesta. La chiave privata cui tale certificato afferisce è
utilizzata dal SP privato per apporre sigilli elettronici avanzati sia
sul proprio metadata che sulle proprie *request*.

Ulteriori estensioni stabilite dagli standard e dalle normative sono
liberamente utilizzabili.


Algoritmi crittografici, di *hash* e tipologia delle chiavi
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Per la generazione delle chiavi crittografiche i SP utilizzano l’algoritmo **rsa** (Rivest-Shamir-Adleman) con
lunghezza delle chiavi non inferiore a 2048 bit. L’algoritmo impiegato
per le impronte crittografiche è il *dedicated hash-function 4* definito
nella norma ISO/IEC 10118-3, corrispondente alla funzione **sha-256**. È
consentito l’uso della funzione **sha-512**.


Informazioni obbligatorie per la fatturazione
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    
       L’occorrenza di ContactPerson con l’attributo contactType valorizzato
       come billing è obbligatoria in caso sia presente l’estensione Private
       nel tag Extensions (dell’occorrenza di ContactPerson con l’attributo
       contactType valorizzato come other). Contiene le informazioni fiscali
       *minime* per l’individuazione del soggetto che sarà il destinatario
       di fatturazione elettronica, in qualità di **committente**, da parte
       degli IdP. Al suo interno sono presenti i seguenti tag:
    
    -  Extensions (1 occorrenza *obbligatorio*) — Tramite estensione con
       opportuno *namespace* https://spid.gov.it/invoicing-extensions,
       ispirato dallo standard **FatturaPA** dell’Agenzia delle
       Entrate, contiene i tag minimi necessari alla suddetta individuazione
       fiscale. Sono dunque presenti il tag figlio CessionarioCommittente e,
       qualora necessario, il tag figlio
       TerzoIntermediarioSoggettoEmittente, valorizzati come previsto dallo
       standard:
    
       -  **CessionarioCommittente** (1 occorrenza) — con figli:
    
          -  DatiAnagrafici (1 occorrenza) — con figli: IdFiscaleIVA (figli:
                IdPaese e IdCodice) e/o CodiceFiscale; Anagrafica (figli:
                Denominazione, *ovvero* Nome e Cognome; opzionalmente
                Titolo; opzionalmente CodiceEORI);
    
          -  Sede (1 occorrenza) — con figli: Indirizzo, NumeroCivivo
             (opzionale), CAP, Comune, Provincia (opzionale), Nazione.
    
       -  **TerzoIntermediarioSoggettoEmittente** (0 o 1 occorrenze) —
          valorizzato, se necessario e *solo relativamente al committente*.
    
    -  Company (0 o 1 occorrenze) — Obbligatoriamente presente qualora il
       soggetto per l’emissione delle fatture sia distinto dal SP stesso (e
       in ogni caso riportante il nome completo e per esteso di una persona
       giuridica, con il corretto uso di minuscole, maiuscole e segni
       diacritici).
    
    -  EmailAddress (1 occorrenza, *obbligatorio*) — Contiene l’indirizzo di
       posta elettronica, *aziendale o istituzionale*, per contattare il
       soggetto per questioni di fatturazione elettronica. Può trattarsi di
       un indirizzo di posta elettronica certificata (pec) aziendale, ma non
       deve trattarsi di una casella e-mail personale.


Esempio: Contatti metadata SP per Fatturazione
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: code-samples/sp-metadata-fatturazione.xml
   :language: xml
   :linenos:


Esempio: metadata SP
^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: code-samples/sp-metadata.xml
   :language: xml
   :linenos:
