## Lettera di Dimissione Ospedaliera (LDO)
La lettera di dimissione è un documento che viene rilasciato al paziente al termine di una fase di ricovero ospedaliero e contiene le indicazioni per gli eventuali controlli o terapie da effettuare. Questo documento contiene le principali informazioni inerenti al passaggio di cura dal contesto ospedaliero a quello territoriale. Le informazioni contenute nel documento sono destinate al medico che seguirà il paziente successivamente. Una copia della lettera viene inviata anche al medico di famiglia, al prescrittore del trattamento o a eventuali altri istituti di cura interessati nel percorso diagnostico-terapeutico. La responsabilità della corretta compilazione della lettera di dimissione compete al medico responsabile della dimissione e la LDO DEVE riportare l’identificazione di detto medico e la sua firma. 
### Header
- _inFulfillmentOf_: elemento OPZIONALE che identifica la prescrizione di ricovero.
  ```xml
  <inFulfillmentOf>
    <order classCode="ACT" moodCode="RQO">
      <id root="2.16.840.1.113883.2.9.4.3.8"
          extension="[NRE]"
          assigningAuthorityName="Ministero delle Finanze"/>
    </order>
  </inFulfillmentOf>
  ```
  Nel caso di ricette rosse cartacee ``extension="[CONCATENAZIONE BAR1 BAR2]"``.
- ***componentOf***: identifica il ricovero a cui si riferisce la dimissione, riferito da ``<componentOf>/<encompassingEncounter>``
- ***id***:  rappresenta l’identificativo del ricovero, cioè riporta il numero nosologico corrispondente al ricovero.
  ```xml
  </componentOf>
    <encompassingEncounter> 
      <id root="2.16.840.1.113883.2.9.2.[RAMO:AZIENDALE.NOSOLOGICI].4.6"
          extension="NUMERO _NOSOLOGICO"
          assigningAuthorityName="Azienda" displayable="true"/>
      …
    </encompassingEncounter>
  </componentOf>
  ```
- ***effective time***: identifica le date di inizio e fine ricovero.

### Body
Lo standard CDA prevede che il corpo di un documento possa essere formato in modo strutturato (``<structuredBody>``) o in modo destrutturato (``<nonXMLBody>``). 

  | Sezioni                                                            | Codici LOINC | Descrizioni LOINC ShortName |
  |--------------------------------------------------------------------|--------------|-----------------------------|
  | Motivo del ricovero                                                | 46241-6      | Hospital Admission Dx       |
  | Decorso Ospedaliero                                                | 8648-8       | Hospital Course             |
  | Condizioni del paziente alla dimissione + diagnosi alla dimissione | 11535-2      | Hospital Discharge Dx       |

- ***"Motivo del ricovero"***: descrive la causa principale che ha determinato il ricovero del paziente attraverso la diagnosi di accettazione.
  * ``<code>``: definisce nel dettaglio, sulla base di un particolare vocabolario predefinito, la tipologia di ``<section>`` che si sta compilando.

    | Attributo         | Tipo | Valore                   | Dettagli                                                                                               |
    |-------------------|------|--------------------------|--------------------------------------------------------------------------------------------------------|
    | code              | ST   | "46241-6"                | OID codifica.                                                                                          |
    | codeSystem        | OID  | "2.16.840.1.113883.6.1"  | OID del vocabolario utilizzato                                                                         |
    | cideSystemName    | ST   | "LOINC"                  | Nome del vocabolario utilizzato: LOINC.                                                                |
    | codeSystemVersion | ST   | [VERSIONE]               | Versione del vocabolario utilzzata (ad es. 2.19).                                                      |
    | displayName       | ST   | Diagnosi di Accettazione | Nome della section ovvero descrizione sintetica del contenuto informativo secondo il vocabolario usato |

    Esempio di utilizzo:
    ```xml
      <code code="46241-6"
            codeSystem="2.16.840.1.113883.6.1"
            codeSystemName="LOINC"
            codeSystemVersion="2.19"
            displayName=" Diagnosi di Accettazione "/>
    ```
  * ``<title>``: rappresenta il titolo della sezione (``<title>Motivo del ricovero</title>``).
  * ``<text>``: blocco narrativo, un esempio di utilizzo:

    ```xml
    <text>
      <list>
        <item>
          <content ID="DIAG-1">Disturbo di panico</content>
        </item>
        <item>
          <content ID="DIAG-2">Ipertiroidismo</content>
        </item>
      </list>
    </text>
    ```

- ***"Sezione Decorso Ospedaliero"***: descrivere l’andamento del ricovero, il percorso diagnostico, terapeutico, riabilitativo o assistenziale.
  * ``<code>``: il valore del campo ``code`` è ``"8648-8"``.
  * ``<title>``: titolo della sezione (``<title>Decorso ospedaliero</title>``).
  * ``<text>``: esempio d utilizzo:
    ```xml
    <text>
      <paragraph>
        Il paziente giungeva alla nostra attenzione sintomatico per
        scompenso cardiaco acuto. Durante il ricovero è stato ottenuto un
        ripristino dello stato di compenso emodinamico mediante trattamento
        farmacologico intensivo.
      </paragraph>
    </text>
    ```
- ***"Condizioni del paziente e diagnosi alla dimissione"***: descrive l'elenco delle diagnosi di dimissione, in ordine di rilevanza.
  * ``<code>``: il valore del campo ``code`` è ``"11535-2"``.
  * ``<title>``: titolo della sezione (``<title>Condizioni del paziente e diagnosi alla dimissione</title>``).
  * ``<text>``: esempio di utilizzo:
    ```xml
    <text>
      Paziente in cattivo compenso emodinamico per insufficenza della Valvola
      Aortica di grado severo. Non in grado di deambulare correttamente,
      necessita di sedia a rotelle in ore serali. Si segnala inizio di sindrome
      paranoica e COPD.
    </text>
    ```
  * ``<entry>``: RACCOMANDATO ma non obbligatorio. Rappresentare in modo strutturato le informazioni di dettaglio riferite nel blocco narrativo (``<text>``).