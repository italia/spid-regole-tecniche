Regole tecniche Attribute Authority
===================================

Un Gestore di attributi qualificati, nel seguito indicato anche con il termine tecnico Attribute
Authority, deve essere in grado di certificare un determinato set di attributi relativi ad un
soggetto titolare di una identità digitale. A fronte di una richiesta di uno o più attributi l'Attribute
Authority deve essere in grado di:
1. ricevere ed interpretare la richiesta di attributo pervenuta da una Service Provider;
2. elaborare la richiesta;
3. costruire la risposta inerente la richiesta pervenuta ed inoltrarla alla Service Provider.
Il componente Attribute Authority deve esporre le seguenti interfacce:
· IAttributeQuery: interfaccia applicativa che supporta le operazioni di richiesta di
attributo SAML;
· IMetadataRetrive: permette il reperimento dei SAML metadata da parte delle Service
Provider.

2.1. SCENARIO DI INTERAZIONE
Descrizione Interfaccia SAML Binding
1
La Service Provider invia all'Attribute
Authority una richiesta di attributi.
Ciò avviene utilizzando il costrutto
<AttributeQuery> della specifica
SAML e interagendo mediante
"SAML SOAP binding".
IAttributeQuery <AttributeQuery>
SOAP
Over
HTTP
2 L'Authority Registry elabora la
richiesta ricevuta.
- - -
3
La Attribute Authority risponde alla
richiesta di attributi del Service
Provider con una <Response> SAML
contenente l'asserzione, interagendo
mediante "SAML SOAP binding".
IAttributeQuery <Response>
SOAP
Over
HTTP
Tabella 2 - AttributeRequest

2.2. SPECIFICHE DELLE INTERFACCE
Di seguito vengono esposte le specifiche delle interfacce dell'Attribute Authority riportanti:
- le caratteristiche delle asserzioni prodotte;
- le caratteristiche delle AttributeQuery e della Response;
- le caratteristiche del binding;
- i metadati.
2.2.1. CARATTERISTICHE DELLE ASSERZIONI
Le asserzioni prodotte dall' Attribute Authority devono essere conformi allo standard SAML v2.0
(cfr. [SAML-Core]) e rispettare le condizioni di seguito indicate.
L'Asserzione deve avere le seguenti caratteristiche:
· nell'elemento <Assertion> devono essere presenti i seguenti attributi:
- l'attributo ID univoco, per esempio basato su un Universally Unique Identifier (UUID)
o su una combinazione origine + timestamp (quest'ultimo generato con una
precisione di almeno un millesimo di secondo per garantire l'univocità);
- l'attributo Version, che deve valere sempre "2.0", coerentemente con la versione
della specifica SAML adottata;
- l'attributo IssueInstant a indicare l'istante di emissione della richiesta, in formato
UTC (esempio: "2008-03-13T18:04:15.531Z");
· deve essere presente l'elemento <Subject> a indicare il soggetto a cui si riferiscono gli
attributi in cui deve comparire:
- l'elemento <NameID> atto a qualificare il soggetto dell'asserzione, in cui sono
presenti i seguenti attributi:
 Format che deve assumere il valore "urn:oasis:names:tc:SAML:1.1:nameidformat:
unspecified" (cfr. SAMLCore, sez. 8.3);
 NameQualifier che qualifica il dominio a cui afferisce tale valore (URI
riconducibile all'Attribute Authority);
· l'elemento <Issuer> a indicare l'entityID dell'Attribute Authority emittente ( attualizzato
come l'attributo entityID presente nei corrispondenti AAA metadata.) con l'attributo Format
riportante il valore "urn:oasis:names:tc:SAML:2.0:nameid-format:entity";
· deve essere presente l'elemento <Conditions> in cui devono essere presenti gli attributi:
- NotBefore,
- NotOnOrAfter;

e l'elemento:
- <AudienceRestriction> riportante a sua volta l'elemento <Audience>
attualizzato con l'entityID del ServiceProvider per il quale l'asserzione è emessa;
· deve essere presente l'elemento <AttributeStatement> riportante gli attributi
certificati dall'Attribute Authority. Tale elemento dovrà comprendere uno o più elementi di
tipo <Attribute>;
· un elemento di tipo <Attribute> relativo ad un attributo certificato dovrà
comprendere:
- l'attributo Name attualizzato con identificativi di attributo definiti nella tabella
attributi SPID (cfr. SPID - Tabella attributi);
- uno o più elementi <AttributeValue> ciascuno riportante l'attributo Type (cfr.
SPID - Tabella attributi) e attualizzato con il valore assunto dall'attributo;
· l'elemento <Assertion> può eventualmente presentare l'elemento <Advice>,
contenente altri elementi <Assertion> di cui è necessario fornire evidenza in forma
originale in sede di risposta alla richiesta di attributo;
· l'elemento <Signature> riportante la firma sull'asserzione apposta dall'Identity Provider
emittente. La firma deve essere prodotta secondo il profilo specificato per SAML (cfr
[SAML-Core] cap5) utilizzando chiavi RSA almeno a 1024 bit e algoritmo di digest SHA-256
o superiore.

.. literalinclude:: code-samples/attribute-assertion.xml
   :language: xml
   :linenos:

2.2.2. CARATTERISTICHE DELLE ATTRIBUTEQUERY E DELLA RESPONSE
Il protocollo attributeQuery previsto per l'Attribute Authority deve essere conforme allo standard
SAML v2.0 (cfr. [SAML-Core]) e rispettare le condizioni di seguito indicate.
2.2.2.1. ATTRIBUTEQUERY
L' attributeQuery deve avere le seguenti caratteristiche:
· nell' elemento <AttributeQuery> devono essere presenti i seguenti attributi:
- l'attributo ID univoco, per esempio basato su un Universally Unique Identifier
(UUID) o su una combinazione origine + timestamp;
- l'attributo Version, che deve valere sempre "2.0", coerentemente con la versione
della specifica SAML adottata;
- l'attributo IssueInstant a indicare l'istante di emissione della richiesta, in formato
UTC;
- l'attributo Destination, a indicare l'indirizzo (URI reference) a cui è inviata la
richiesta, cioè l'AttributeService della Attribute Authority;
· deve essere presente l'elemento <Issuer> a indicare l'identificatore univoco del Service
Provider emittente attualizzato come l'attributo entityID riportato nel corrispondente SP
metadata.. L'elemento deve riportare l'attributo Format attualizzato con il valore
"urn:oasis:names:tc:SAML:2.0:nameid-format:entity";
· deve essere presente l'elemento <Subject> a referenziare il soggetto a cui si riferisce la
richiesta di attributo, in cui deve comparire:
- l'elemento <NameID> attualizzato con il codice fiscale del soggetto (cfr. Tabella
attributi SPID), in cui dee essere presente l'attributo:
 Format che deve assumere il "urn:oasis:names:tc:SAML:1.1:nameidformat:
unspecified" (cfr. SAMLCore, sez. 8.3);
 NameQualifier che qualifica il dominio a cui afferisce tale valore (URI
riconducibile all'Attribute Authority);
· deve essere presente uno o più elementi <Attribute>, il cui attributo Name indica lo
specifico attributo di cui si vuole conoscere il valore (cfr. SPID - Tabella attributi);
· in ciascun elemento <Attribute> possono essere presenti uno o più elementi
<AttributeValue> per richiedere la verifica che l'attributo abbia i valori specificati;
· deve essere presente l'elemento <Signature> riportante la firma sull'asserzione apposta
dall'Identity Provider emittente. La firma deve essere prodotta secondo il profilo specificato
per SAML (cfr [SAML-Core] cap5) utilizzando chiavi RSA almeno a 1024 bit e algoritmo di
digest SHA-256 o superiore.

.. literalinclude:: code-samples/attribute-query.xml
   :language: xml
   :linenos:

2.2.2.2. RESPONSE
Le caratteristiche che deve avere la risposta inviata dall' Attribute Authority al Service Provider a
seguito di una richiesta di attributi sono le seguenti:
· nell' elemento <Response> devono essere presenti i seguenti attributi:
- deve essere presente l'attributo ID univoco, per esempio basato su un Universally
Unique Identifier (UUID) (cfr. UUID) o su una combinazione origine + timestamp;
- deve essere presente l'attributo Version, che deve valere sempre "2.0",
coerentemente con la versione della specifica SAML adottata;
- deve essere presente l'attributo IssueInstant a indicare l'istante di emissione della
risposta, in formato UTC;
- deve essere presente l'attributo InResponseTo, il cui valore deve fare riferimento
all'ID della richiesta a cui si risponde;
- deve essere presente l'attributo Destination, a indicare l'indirizzo (URI reference) a
cui è inviata la richiesta, cioè l'AttributeService del Service Provider;
· deve essere presente l'elemento <Issuer> a indicare l'identificatore univoco dall'
Attribute Authority emittente attualizzato come l'attributo entityID riportato nel
corrispondente AA metadata.,. L'elemento deve riportare l'attributo Format attualizzato
con il valore "urn:oasis:names:tc:SAML:2.0:nameid-format:entity";
· deve essere presente l'elemento <Status> a indicare l'esito della attributeQuery secondo
quanto definito nelle specifiche SAML (cfr. [SAML-Core] par. 3.2.2.1 e ss.) comprendente
il sotto-elemento <StatusCode> ed opzionalmente i sotto-elementi
<StatusMessage> <StatusDetail> (cfr [SPID-TabErr]);
· deve essere presente l'elemento <Assertion> come specificato al paragrafo 2.3.1,
contenenti elementi <AttributeStatement> relativi agli attributi richiesti;
· può presente l'elemento <Signature> riportante la firma sull'asserzione apposta
dall'Identity Provider emittente. La firma deve essere prodotta secondo il profilo specificato
per SAML (cfr [SAML-Core] cap5) utilizzando chiavi RSA almeno a 1024 bit e algoritmo di
digest SHA-256 o superiore.

.. literalinclude:: code-samples/attribute-response.xml
   :language: xml
   :linenos:

2.2.3. CARATTERISTICHE DEL BINDING
Il binding previsto per il trasporto di messaggi è il SAML SOAPbinding su http(cfr. [SAMLBin]
par. 3.2.).
2.2.4. ATTRIBUTE AUTHORITY METADATA
Le caratteristiche dell'Attribute Authority devono essere definite attraverso metadata conformi allo
standard SAMLv2.0.( cfr. [SAML-Metadata]), e rispettare specificatamente le condizioni di
seguito indicate:
· nell'elemento <EntityDescriptor> devono essere presenti i seguenti attributi:
- entityID: indicante l'identificativo univoco (un URI) dell'entità;
· l'elemento <AttributeAuthorityDescriptor> specifico che contraddistingue
l'entità di tipo Attribute Authority; deve riportare il seguente attributo:
- protocolSupportEnumeration: che enumera gli URI indicanti i protocolli
supportati dall'entità (poiché si tratta di un'entità SAML 2.0, deve indicare almeno
il valore del relativo protocollo: "urn:oasis:names:tc:SAML:2.0:protocol");
inoltre al suo interno devono essere presenti:
- l'elemento <KeyDescriptor> che contiene l'elenco dei certificati e delle
corrispondenti chiavi pubbliche dell'entità, utili per la verifica della firma dei
messaggi prodotti da tale entità nelle sue interazioni con le altre (cfr.[SAMLMetadata],
sez. 2.4.1.1);
- uno o più elementi <AttributeService> indicante il servizio a cui contattare
l'Attribute Authority riportante i seguenti attributi:
 Binding posto al valore "urn:oasis:names:tc:SAML:2.0:bindings:SOAP";
 Location url endpoint del servizio per la ricezione delle richieste;
- l'elemento <NameIDFormat> riportante l'attributo:
 format , indicante il formato "urn:oasis:names:tc:SAML:1.1:nameidformat:
unspecified" come quello supportato per l'elemento di <NameID>
utilizzato nelle richieste e risposte SAML per identificare il subject cui si
riferisce un'asserzione;
- <AttributeProfile>: enumerazione dei profili di rappresentazione di
attributi supportati dall'entità (cfr.[SAML-Profile], sez. 8); nel caso specifico solo
"basic" (cfr. [SAML-Profile], sez. 8.1);
- uno o più elementi <Attribute> riportanti gli attributi:
 Name riportante l'identificativo dell'attributo;
 NameFormat riportante il format dell'attributo;
· deve essere l'elemento <Signature> riportante la firma sui metadata . La firma deve
essere prodotta secondo il profilo specificato per SAML (cfr. [SAML-Metadata] cap3)
utilizzando chiavi RSA almeno a 1024 bit e algoritmo di digest SHA-256 o superiore;
· è consigliata la presenza di un elemento <Organization> a indicare l'organizzazione a
cui afferisce l'entità specificata, riportante gli elementi:
- <OrganizationName> indicante un identificatore language-qualified
dell'organizzazione a cui l'entità afferisce;
- <OrganizationURL> riportante in modalità language-qualified la url
istituzionale dell'organizzazione.
I metadata Attribute Authority saranno disponibili per tutte le entità SPID federate attraverso
l'interfaccia IMetadataRetrive alla URL <dominioAttributiQualificati>/metadata, ove non
diversamente specificato nel Registro SPID, e saranno firmate dell'Agenzia per l'Italia Digitale.
L'accesso deve essere effettuato utilizzando il protocollo TLS nella versione più recente
disponibile.

.. literalinclude:: code-samples/aa-metadata.xml
   :language: xml
   :linenos:
