Log
===

**Il fornitore di servizi (Service Provider o SP)** *eroga i servizi agli utenti, richiede l'autenticazione agli Identity Provider e gestisce la procedura di autorizzazione al servizio*

4.1. TRACCIATURE IDENTITY PROVIDER
Ai fini della tracciatura l'Identity Provider dovrà mantenere un Registro delle transazioni contenente i
tracciati delle richieste di autenticazione servite negli ultimi 24 mesi. L'unità di
memorizzazione di tale registro dovrà rendere persistente per ogni transazione la tripla
composta dell'identificativo dell'identità digitale (spidCode) interessata dalla transazione, dalla
<AuthnRequest> e della relativa < Response>. Al fine di consentire una facile ricerca e
consultazione dei dati di tracciature potrebbe essere opportuno memorizzare in ogni record
informazioni direttamente estratte dai suddetti messaggi in formato SAML. A titolo
esemplificativo e non esaustivo le informazioni presenti in un record del registro potrebbero
essere le seguenti:
SpidCode
<AuthnRequest>
<Response>
AuthnReq_ID
AuthnReq_IssueInstant
AuthnReq_Issuer
Resp_ID
Resp_IssueInstant
Resp_Issuer
Assertion_ID
Assertion_subject
Assertion_subject_NameQualifier

4.2. TRACCIATURE SERVICE PROVIDER
Il comma 2 dell'articolo 13 del DPCM obbliga i fornitori di servizi ( service provider ) alla
conservazione per ventiquattro mesi delle informazioni necessarie a imputare alle singole
identità digitali le operazioni effettuate sui propri sistemi. A tal fine un service provider dovrà
mantenere un Registro delle transazioni contenente i tracciati delle richieste di autenticazione
servite negli ultimi 24 mesi. L'unità di memorizzazione di tale registro dovrà rendere
persistente per ogni transazione la coppia dalla <AuthnRequest> e della relativa <
Response>. Al fine di consentire una facile ricerca e consultazione dei dati di tracciature
potrebbe essere opportuno memorizzare in ogni record informazioni direttamente estratte dai
suddetti messaggi in formato SAML. A titolo esemplificativo e non esaustivo le informazioni
presenti in un record del registro potrebbero essere le seguenti:
<AuthnRequest>
<Response>
AuthnReq_ID
AuthnReq_IssueInstant
Resp_ID
Resp_IssueInstant
Resp_Issuer
Assertion_ID
Assertion_subject
Assertion_subject_NameQualifier

4.3. MANTENIMENTO TRACCIATURE
Le tracciature devono essere mantenute nel rispetto del codice della privacy sotto la
responsabilità titolare del trattamento dell'Identity Provider. e l'accesso ai dati di tracciatura
deve essere riservato a personale incaricato.
Al fine di garantire la confidenzialità potrebbero essere adottai meccanismi di cifratura dei dati o
impiegati sistemi di basi di dati (DBMS) che realizzano la persistenza cifrata delle informazioni.
Per il mantenimento devono essere messi in atto meccanismi che garantiscono l'integrità e il
non ripudio.
