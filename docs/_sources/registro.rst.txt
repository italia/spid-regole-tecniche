Registro
========

Il federation registry contiene la lista delle entità che hanno superato il processo di accreditamento
e quindi facenti parte della federazione SPID. Le informazioni contenute nel registro per
ciascuna delle suddette entità sono le seguenti:
· AuthorityInfo entry del registro relativa ad una entità; a sua volta costituita da:
· EntityId : identificatore SAML dell'entità;
· Soggetto: denominazione del soggetto a cui afferisce l'entità della federazione;
· EntityType: tipo di entità ( Identity Provider, Attribute Authority, Service Provider);
· MetadataProviderURL: l'URL del servizio di reperimento metadati;
· AttributeList: elenco di attributi qualificati certificabili da una entità di tipo
Attribute Authority.
Il federation registry viene popolato dall'Agenzia per l'Italia Digitale a seguito del processo di
stipula delle convenzioni e aggiornata dalla stessa Agenzia nel corso delle attività legate alla
gestione delle convenzioni e della vigilanza sui soggetti del circuito SPID.
Il contenuto informativo della federation registry è in fruizione a tutte le entità appartenenti al
circuito SPID ai fini della verifica della sussistenza di relazioni di trust nei confronti di entità
terze (IdP, AA, SP) e del reperimento delle informazioni associate alla alle stesse. Il Discovery
Service può anch'esso accedere al federation registry per utilizzarne i contenuti ai fini de attività di
discovering.
3.1.1. ACCESSO AL REGISTRO
L'accesso ai contenuti del federation registry avviene in modalità REST attraverso l'interfaccia
(risorsa) IRegistry. In particolare:
- l'accesso in consultazione ai contenuti del directory avviene attraverso il metodo
http GET
