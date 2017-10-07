Introduzione
============

SPID è basato sul framework SAML (Security Assertion Markup Language), sviluppato e manutenuto dal `Security Services Technical Committee di OASIS <https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=security>`_, che permette la realizzazione di un sistema sicuro di Single Sign On (SSO) federato, ovvero, che permette ad un utente di accedere ad una moltitudine di servizi, anche su domini differenti, effettuando un solo accesso.

Il sistema è composto da 3 entità:

* **Gestore delle identità (Identity Provider o IdP)** che gestisce gli utenti e la procedura di autenticazione;
* **Fornitore di servizi (Service Provider o SP)** che, dopo aver richiesto l'autenticazione dell'utente all'Identity Provider, gestisce l'autorizzazione, sulla base degli attributi restituiti dall'IdP, erogando il servizio richiesto;
* **Gestore di attributi qualificati (Attribute Authority o AA)** che fornisce attributi certificati sulla base dell'utente autenticato.

Le modalità di funzionamento del Sistema Pubblico di Identità Digitale sono quelle previste da SAML v2 per il profilo "Web
Browser SSO" - `SAML V2.0 Technical Overview - Oasis par4.3 <http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html>`_.

Devono essere previste le due versioni "**SP-Initiated**":

* *Redirect/POST binding*
* *POST/POST binding*

Il meccanismo di autenticazione è innescato dalla richiesta inoltrata dall'utente tramite browser ad un Fornitore di Servizi (Service Provider o SP) che si rivolge al Gestore delle Identità (Identity provider o IdP) selezionato dall'utente in modalità "pull".

La richiesta di autenticazione SAML, basata sul costrutto <AuthnRequest>, può essere inoltrata da un Service Provider all'Identity Provider usando il binding:

* *HTTP Redirect*
* *HTTP POST*

La relativa risposta SAML, basata sul costrutto <Response>, può essere, invece, inviata dall'Identity Provider al Service Provider solo tramite il binding HTTP POST.

Interfacce logiche
------------------

Identity Provider
^^^^^^^^^^^^^^^^^
Le interfacce logiche dell'**Identity Provider** coinvolte sono:

- *IIDPUserInterface*: permette agli utenti l'interazione via web con il componente tramite
- *User Agent (browser)*: in fase di challenge di autenticazione;
- *IAuthnRequest (singleSignOnService)*: ricezione di richieste di autenticazione SAML;
- *IMetadataRetrieve*: permette il reperimento dei SAML metadata dell'Identity Provider.

Service Provider
^^^^^^^^^^^^^^^^
Le Interfacce logiche del **Service Provider** coinvolte sono:

- *IAuthnResponse (Assertion Consumer Service)*: ricezione delle risposte di autenticazione SAML.
- *IMetadataRetrieve*: permette il reperimento dei SAML metadata del Service Provider
- *IDSResponse*: ricezione delle risposte da parte del Discovery Service.

Scenario di Interazione in modalità Single Sign On (SSO)
--------------------------------------------------------

**L'immagine successiva mostra il funzionamento di un'autenticazione SPID (SAML v2.0)**

.. figure:: _images/spid-saml2.png
   :alt: funzionamento di un'autenticazione SPID (SAML v2.0)

   Funzionamento di un'autenticazione SPID (SAML v2.0)

+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+------------------+
| ## |Descrizione                                                                                                                                                                                                                                                             |Interfaccia   |Saml        |Binding           |
+====+========================================================================================================================================================================================================================================================================+==============+============+==================+
| A  |L'utente richiede l'accesso ad un servizio                                                                                                                                                                                                                              |              |            |                  |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+------------------+
| B1 |Il Service Provider (SP) invia allo User Agent (UA) una richiesta di autenticazione da far pervenire all'Identity Provider (IdP)                                                                                                                                        |IAuthnRequest |AuthnRequest|HTTP POST/REDIRECT|
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+------------------+
| B2 |Lo User Agent inoltra la richiesta di autenticazione contattando L'Identity Provider                                                                                                                                                                                    |              |AuthnRequest|HTTP POST/REDIRECT|
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+------------------+
| C1 |L'Identity Provider esamina la richiesta ricevuta e, se necessario, esegue una challenge di autenticazione con l'utente                                                                                                                                                 |              |            |HTTP              |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+------------------+
| C2 |L'Identity Provider, portata a buon fine l'autenticazione, effettua lo user login e prepara l'asserzione contenente lo statement di autenticazione dell'utente destinato al Service Provider (più eventuali statement di attributo emessi dall'Identity Provider stesso)|              |            |                  |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+------------------+
| D  |L'Identity Provider restituisce allo User Agent la <Response> SAML contenente l'asserzione preparata al punto precedente                                                                                                                                                |              |Response    |HTTP POST         |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+------------------+
| E  |Lo User Agent inoltra al Service Provider (SP) la <Response> SAML emessa dall'Identity Provider                                                                                                                                                                         |IAuthnResponse|Response    |HTTP POST         |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+------------------+
