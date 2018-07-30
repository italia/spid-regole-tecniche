Introduzione
============

SPID è basato sul framework SAML (Security Assertion Markup Language), sviluppato e manutenuto dal `Security Services Technical Committee di OASIS <https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=security>`_, che permette la realizzazione di un sistema sicuro di Single Sign-On (SSO) federato. Grazie a SAML, un utente può accedere ad una moltitudine di servizi appartenenti a domini differenti effettuando un solo accesso.

Il sistema è composto da 3 entità:

* **Gestore delle identità (Identity Provider o IdP)** che gestisce gli utenti e la procedura di autenticazione;
* **Fornitore di servizi (Service Provider o SP)** che, dopo aver richiesto l'autenticazione dell'utente all'Identity Provider, ne gestisce l'autorizzazione sulla base degli attributi restituiti dal Gestore dell'identità, ed eroga il servizio richiesto;
* **Gestore di attributi qualificati (Attribute Authority o AA)** che fornisce attributi qualificati sulla base dell'utente autenticato.

Le modalità di funzionamento di SPID sono quelle previste da SAML v2 per il profilo "Web
Browser SSO" - `SAML V2.0 Technical Overview - Oasis par4.3 <http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html>`_.

