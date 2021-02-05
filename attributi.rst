Tabella attributi
=================

L'identificatore sotto indicato è il valore dell’attributo ``Name`` dell’elemento ``<saml:Attribute>``. Il valore dell’attributo ``NameFormat`` dello stesso elemento è, come da specifica SAML-core, ``urn:oasis:names:tc:SAML:2.0:attrname-format:basic``.

Il tipo sotto indicato è il valore dell’attributo ``xsi:type`` dell’elemento ``<saml:AttributeValue>``.

.. list-table:: Tabella attributi identificativi
    :widths: auto
    :header-rows: 1
    
    * - Attributo
      - Identificatore
      - Tipo
      - Note
    * - Codice identificativo
      - ``spidCode``
      - xs:string
      - Il codice identificativo è assegnato dal gestore dell’identità digitale, deve essere univoco in ambito SPID. Il formato è il seguente:
        
        ``<cod_IdP><nr. univoco>``
        
        dove:
        
        * ``<cod_IdP>`` è un codice composto da 4 lettere univocamente assegnato al gestore delle identità;
        * ``<nr.univoco>`` è una stringa alfanumerica composta da 10 caratteri che il gestore delle identità genera in maniera univoca nell’ambito del proprio dominio.
        
        (Es. ``ABCD123456789A``)
    * - Nome
      - ``name``
      - xs:string
      - Stringa composta da una sequenza di una o più sottostringhe non vuote con carattere iniziale in maiuscolo intervallate da uno (solo) spazio
        
        (Es. ``Francesca`` , ``Giovanni Mario``)
    * - Cognome
      - ``familyName``
      - xs:string
      - Stringa composta da una sequenza di una o più sottostringhe non vuote con carattere iniziale in maiuscolo intervallate da uno (solo) spazio
        
        (Es. ``Rossi``, ``Bianchi Verdi``)
    * - Luogo di nascita
      - ``placeOfBirth``
      - xs:string
      - Stringa corrispondente al codice catastale (Codice Belfiore) del Comune o della nazione estera di nascita.
        
        (Es. ``F205`` per la città di Milano)
    * - Provincia di nascita
      - ``countyOfBirth``
      - xs:string
      - Stringa corrispondente alla sigla della provincia di nascita.
        
        (Es. ``MI`` per provincia di Milano)
    * - Data di nascita
      - ``dateOfBirth``
      - xs:date
      - Secondo specifica xs:date nel formato *YYYY-MM-DD* dove:
        
        * YYYY indica l’anno utilizzando 4 cifre;
        * MM indica il mese in (due) cifre;
        * DD indica il giorno in (due) cifre;
        
        (Es. ``2002-09-24``)
    * - Sesso
      - ``gender``
      - xs:string
      - Valori ammessi:
        
        * F per sesso femminile;
        * M per sesso maschile.
    * - Ragione o denominazione sociale
      - ``companyName``
      - xs:string
      - Stringa composta da una sequenza di sottostringhe non vuote intervallate da uno (solo) spazio. In maiuscolo le sottostringhe corrispondenti a nomi.
        
        (Es. ``Agenzia per l’ Italia Digitale``)
    * - Sede legale
      - ``registeredOffice``
      - xs:string
      - Stringa composta da una sequenza di sottostringhe non vuote intervallate da uno (solo) spazio rappresentanti:
        
        * Tipologia(via, viale, piazza...);
        * Indirizzo;
        * Nr. civico;
        * CAP;
        * Luogo;
        * Provincia;
        
        (Es. ``via Listz 21 00144 Roma``)
    * - Codice fiscale
      - ``fiscalNumber``
      - xs:string
      - Per il formato si faccia riferimento alla codifica dell’attributo CF per i certificati, proposta nell’ambito del Draft ETSI EN 319 412-1, che nel caso specifico prevede la seguente composizione:
        
        ``TINIT-<CodiceFiscale>``
    * - Partita IVA
      - ``ivaCode``
      - xs:string
      - Per il formato si faccia riferimento alla codifica dell’attributo Partita IVA per i certificati, proposta nell’ambito del Draft ETSI EN 319 412-1, che nel caso specifico prevede la seguente composizione:
        
        ``VATIT-<PartitaIVA>``
    * - Documento d'identità
      - ``idCard``
      - xs:string
      - Stringa composta dalla sequenza di sottostringhe (non vuote) concatenate nell’ordine sotto riportato e intervallate da uno (solo) spazio:
        * <tipo di documento> xs:string; valori ammessi: *cartaIdentita, passaporto, patenteGuida; patenteNautica; librettoPensione, patentinoImpTermici, portoArmi, tesseraRiconoscimento*;
        * *<numero di documento>* xs:string; Numero del documento;
        * *<ente emettitore>* xs:string; stringa ottenuta dalla concatenazione dei termini costituenti la denominazione dell’ente a meno di congiunzioni, articoli e preposizioni.
        
          Es. ``regioneLazio`` (Regione Lazio); ``provinciaCatania`` (Provincia di Catania); ``prefetturaRoma`` (Prefettura di Roma); ``MinisteroEconomiaFinanze`` (Ministero dell’Economia e delle Finanze);
        
        * *<data emissione>* xs:date; data di rilascio del documento;
        * *<data scadenza>* xs:date; data di scadenza del documento;
        
        (Es. ``CartaIdentità AS09452389 ComuneRoma 2013- 01-02 2013-01-31``)


.. list-table:: Tabella attributi secondari
    :widths: auto
    :header-rows: 1
    
    * - Attributo
      - Identificatore
      - Tipo
      - Note
    * - Numero di telefono mobile
      - ``mobilePhone``
      - xs:string
      - Stringa numerica senza spazi intermedi 
        
        (Es. ``34912345678``)
    * - Indirizzo di posta elettronica
      - ``email``
      - xs:string
      - Formato standard indirizzo di posta elettronica
    * - Domicilio 
      - ``domicileStreetAddress``
      - xs:string
      - via, viale, piazza
    * - Codice Postale
      - ``domicilePostalCode``
      - xs:string
      - CAP
    * - Comune
      - ``domicileMunicipality``
      - xs:string
      - Comune
    * - Provincia
      - ``domicileProvince``
      - xs:string
      - 
    * - Domicilio fisico
      - ``address``
      - xs:string
      - Stringa composta da una sequenza di sottostringhe non vuote intervallate da uno (solo) spazio rappresentanti:
        
        * Tipologia (via, viale, piazza...);
        * Indirizzo;
        * Nr. civico;
        * CAP;
        * Luogo;
        * Provincia.
    * - Nazione
      - ``domicileNation``
      - xs_string
      -
    * - Data di scadenza identità
      - ``expirationDate``
      - xs:date
      - Secondo specifica xs:date
    * - Domicilio digitale
      - ``digitalAddress``
      - xs:string
      - Indirizzo casella PEC


.. warning::
    L'attributo `address` è stato sostituito dall `Avviso AgID n25 <https://www.agid.gov.it/sites/default/files/repository_files/spid-avviso-n25-nuova-codifica-domicilio_fisico.pdf>`_
