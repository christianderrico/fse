## Lettera di Dimissione Ospedaliera (LDO)
La lettera di dimissione è un documento che viene rilasciato al paziente al termine di una fase di ricovero ospedaliero e contiene le indicazioni per gli eventuali controlli o terapie da effettuare. Questo documento contiene le principali informazioni inerenti al passaggio di cura dal contesto ospedaliero a quello territoriale. Le informazioni contenute nel documento sono destinate al medico che seguirà il paziente successivamente. Una copia della lettera viene inviata anche al medico di famiglia, al prescrittore del trattamento o a eventuali altri istituti di cura interessati nel percorso diagnostico-terapeutico. La responsabilità della corretta compilazione della lettera di dimissione compete al medico responsabile della dimissione e la LDO DEVE riportare l’identificazione di detto medico e la sua firma. 
### Header
- inFulfillmentOf: elemento OPZIONALE che identifica la prescrizione di ricovero.
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
- **componentOf**: identifica il ricovero a cui si riferisce la dimissione, riferito da ``<componentOf>/<encompassingEncounter>``
- **id**:  rappresenta l’identificativo del ricovero, cioè riporta il numero nosologico corrispondente al ricovero.
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
- **effective time**: identifica le date di inizio e fine ricovero.

