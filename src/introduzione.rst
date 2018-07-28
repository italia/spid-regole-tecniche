Introduzione
============

.. Note::
    Questo paragrafo ha scopo informativo e non normativo.

SPID è basato sul framework SAML (Security Assertion Markup Language), sviluppato e manutenuto dal `Security Services Technical Committee di OASIS <https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=security>`_, che permette la realizzazione di un sistema sicuro di Single Sign-On (SSO) federato. Grazie a SAML, un utente può accedere ad una moltitudine di servizi appartenenti a domini differenti effettuando un solo accesso.

Il sistema è composto da 3 entità:

* **Gestore delle identità (Identity Provider o IdP)** che gestisce gli utenti e la procedura di autenticazione;
* **Fornitore di servizi (Service Provider o SP)** che, dopo aver richiesto l'autenticazione dell'utente all'Identity Provider, ne gestisce l'autorizzazione sulla base degli attributi restituiti dal Gestore dell'identità, ed eroga il servizio richiesto;
* **Gestore di attributi qualificati (Attribute Authority o AA)** che fornisce attributi qualificati sulla base dell'utente autenticato.

Le modalità di funzionamento di SPID sono quelle previste da SAML v2 per il profilo "Web
Browser SSO" - `SAML V2.0 Technical Overview - Oasis par4.3 <http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html>`_, e in particolare per le due versioni "**SP-Initiated**":

* *Redirect/POST binding*
* *POST/POST binding*

Single Sign-On
--------------

Il meccanismo di autenticazione è innescato dalla selezione, da parte dell'utente, del Gestore delle Identità con cui intende effettuare l'accesso; tale selezione avviene all'interno del sito del Fornitore di Servizi mediante un bottone ufficiale "Entra con SPID" da integrarsi nel servizio. Il Fornitore di Servizi prepara di conseguenza una ``<AuthnRequest>`` da inoltrarsi al Gestore delle Identità, dove l'utente viene reindirizzato per effettuare l'autenticazione. Eseguita l'autenticazione, l'utente torna presso il sito del Fornitore di Servizi con un'asserzione firmata dal Gestore delle Identity contenente gli attributi richiesti (ad es. nome, cognome, codice fiscale) che il Fornitore di Servizi può usare per autorizzare l'utente in base alle proprie policy ed erogare il servizio richiesto.

.. figure:: _images/spid-saml2.png
   :alt: Flusso di autenticazione con SPID/SAML2

+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
|    |Descrizione                                                                                                                                                                                                                                                             |SAML            |Binding           |
+====+========================================================================================================================================================================================================================================================================+================+==================+
| A  |L'utente richiede l'accesso ad un servizio                                                                                                                                                                                                                              |                |                  |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| B1 |Il Service Provider (SP) invia allo User Agent (UA) una richiesta di autenticazione da far pervenire all'Identity Provider (IdP)                                                                                                                                        |``AuthnRequest``|HTTP POST/REDIRECT|
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| B2 |Lo User Agent inoltra la richiesta di autenticazione contattando L'Identity Provider                                                                                                                                                                                    |``AuthnRequest``|HTTP POST/REDIRECT|
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| C1 |L'Identity Provider esamina la richiesta ricevuta e, se necessario, esegue una challenge di autenticazione con l'utente                                                                                                                                                 |                |                  |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| C2 |L'Identity Provider, portata a buon fine l'autenticazione, effettua lo user login e prepara l'asserzione contenente lo statement di autenticazione dell'utente destinato al Service Provider (più eventuali statement di attributo emessi dall'Identity Provider stesso)|                |                  |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| D  |L'Identity Provider restituisce allo User Agent la <Response> SAML contenente l'asserzione preparata al punto precedente                                                                                                                                                |``Response``    |HTTP POST         |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| E  |Lo User Agent inoltra al Service Provider (SP) la <Response> SAML emessa dall'Identity Provider                                                                                                                                                                         |``Response``    |HTTP POST         |
+----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
