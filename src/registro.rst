Registro
========

Il Registro SPID è il repository di tutte le informazioni relative alla entità aderenti a SPID e costituisce l’evidenza del cosiddetto circle of trust in esso stabilito.
La relazione di fiducia su cui si basa la federazione stabilita in SPID si realizza per il tramite dell’intermediazione dell’Agenzia, terza parte garante, attraverso il processo di accreditamento dei gestori dell’identità digitale, dei gestori degli attributi qualificati e dei fornitori di servizi. L’adesione a SPID costituisce l’instaurazione di una relazione di fiducia con tutti i soggetti già aderenti, accreditati dall’Agenzia, sulla base della condivisione dei livelli standard di sicurezza dichiarati e garantiti da SPID.
L’adesione al patto di fiducia tra le entità aderenti (gestori dell’identità digitale, gestori degli attributi qualificati e fornitori di servizi) si evidenzia nella presenza di tali entità nel Registro SPID gestito dall’Agenzia.

Contenuti del Registro
----------------------

Il federation registry contiene la lista delle entità che hanno superato il processo di accreditamento e quindi facenti parte della federazione SPID. Le informazioni contenute nel registro per ciascuna delle suddette entità sono le seguenti:

* ``AuthorityInfo``: entry del Registro relativa ad una entità, a sua volta costituita da:

    * ``EntityId``: identificatore SAML dell’entità;
    * ``Soggetto``: denominazione del soggetto a cui afferisce l’entità della federazione;
    * ``EntityType``: tipo di entità (Identity Provider, Attribute Authority, Service Provider);
    * ``MetadataProviderURL``: l’URL del servizio di reperimento metadati;
    * ``AttributeList``: elenco di attributi qualificati certificabili da una entità di tipo Attribute Authority.

Il federation registry viene popolato dall’Agenzia per l’Italia Digitale a seguito del processo di stipula delle convenzioni e aggiornata dalla stessa Agenzia nel corso delle attività legate alla gestione delle convenzioni e della vigilanza sui soggetti del circuito SPID.
Il contenuto informativo della federation registry è in fruizione a tutte le entità appartenenti al circuito SPID ai fini della verifica della sussistenza di relazioni di trust nei confronti di entità terze (IdP, AA, SP) e del reperimento delle informazioni associate alla alle stesse. Il Discovery Service può anch’esso accedere al federation registry per utilizzarne i contenuti ai fini dell'attività di discovering.

.. Note::
    Non è al momento attivo un Discovery Service.

Accesso al Registro
-------------------

.. WARNING::
    Questa sezione non è stata ancora trascritta nel presente documento consolidato. Si rimanda al documento originale delle Regole Tecniche SPID.

.. Note::
    Il Registro è disponibile alla URL https://registry.spid.gov.it/

Accesso al Registro in modalità LDAP
------------------------------------

.. WARNING::
    Questa sezione non è stata ancora trascritta nel presente documento consolidato. Si rimanda al documento originale delle Regole Tecniche SPID.

.. Note::
    L'accesso via LDAP non è al momento attivo.


