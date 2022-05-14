# FSE

## Referto di Radiologia (RAD)
Il Referto di Radiologia è un documento che riassume i risultati di tutte le indagini afferenti alla specialità radiologica, attestando quanto effettuato per l’inquadramento diagnostico e terapeutico.

Il Referto di Radiologia può essere indirizzato sia allo Specialista sia al Medico di Medicina Generale. Può essere richiesto come accertamento diagnostico per un paziente non ricoverato, ma anche come consulenza interna tra specialisti.

### HEADER
*  ***ClinicalDocument***: Elemento root per la struttura XML che rappresenta il documento CDA. Ogni documento CDA DEVE iniziare con questo elemento, che comprende gli attributi speciali xsi:schemaLocation, xmlns e xmlsn:xsi.
*  ***realmCode***: indica il dominio di appartenenza del documento. Nel dominio ITALIANO ```<realmCode code="IT"/>```
*  ***typeId***: indica che il documento è strutturato secondo le specifiche HL7-CDA Rel 2.0. Nel dominio ITALIANO ```<typeId root="2.16.840.1.113883.1.3" extension="POCD_HD000040"/>```
* ***templateId***: indica il template di riferimento per il documento CDA, utile per individuare restrizioni/linee guida da applicare. Nel dominio ITALIANO ```<templateId root= “2.16.840.1.113883.2.9.10.1.7.1” extension=”1.0”/>```
* ***id***: identifica univocamente l'istanza di ogni documento CDA. 
  ```xml
  <id root="2.16.840.1.113883.2.9.2.30967"
      extension="HMS.RAD.20171018.123456"
      assigningAuthorityName="A.O. OSP.NIGUARDA CA'GRANDA - MILANO"/>
  ``` 
* ***code***: indica la tipologia di documento. Il valore DEVE fare riferimento al sistema di codifica LOINC. In particolare,  per identificare il RAD si dovrà utilizzare il codice ```LOINC "68604-8 – Referto Radiologico”```. Esempio nel contesto RAD
  ```xml
  <code code="68604-8”
    codeSystem="2.16.840.1.113883.6.1"
    codeSystemName="LOINC"
    codeSystemVersion="2.19"
    displayName="Referto Radiologico"/> 
  ```
* ***effectiveTime***: indica la data di creazione del documento CDA. L'elemento DEVE essere valorizzato tramite un tipo Time Stamp (TS). ```<effectiveTime value="20050729183023+0100"/>```
* ***confidentialityCode***: specifica il livello di riservatezza del documento. Nel caso del Referto di Radiologia, l'elemento DEVE essere valorizzato nel modo seguente. Si suggerisce che nel contesto italiano, il valore di default sia pari a “N”.
  | Attributo      | Tipo | Valore                   | Dettagli                |
  |----------------|------|--------------------------|-------------------------|
  | codeSystem     | OID  | "2.16.840.1.113883.5.25" | OID codifica.           |
  | code           | ST   | "N", "R", "V"            | Regole di riservatezza. |
  | codeSystemName | ST   | "Confidentiality"        | Nome della codifica.    |
  
  Esempio di utilizzo:
  ```xml
    <confidentialityCode code="N"
      codeSystem="2.16.840.1.113883.5.25"
      codeSystemName="Confidentiality"/>
  ```
  N.B: Il documento DEVE contenere uno ed un solo elemento ``confidentialityCode``
* ***languageCode***: indica la lingua in cui è redatto il documento. Nel caso del Referto di Radiologia, l'elemento DEVE essere così valorizzato: ```<languageCode code="it-IT"/>```. N.B: il documento DEVE contenere un solo elemento ``languageCode```.
* ***setId*** e ***versionNumber***: consentono di gestire le revisioni del documento, o di suoi eventuali addendum integrativi.  l'elemento ```<setId>``` ha un valore costante tra le diverse versioni del medesimo documento, mentre l'elemento ```<versionNumber>``` cambia al variare della revisione. Il nuovo documento sostitutivo DEVE comprendere un elemento <relatedDocument> che punta al documento sostituito.
* ***recordTarget***: identifica il soggetto della prestazione, ovvero il paziente a cui il Referto si riferisce. Per il Referto di Radiologia l'elemento deve pertanto essere strutturato come mostrato di seguito.
  ```xml
   <recordTarget>
     <patientRole>
       <patient>
         ...
        </patient>
    </patientRole>
  </recordTarget>
  ```
  
  L'elemento ***patientRole*** DEVE prevedere al suo interno almeno due elementi di tipo ```<id>```. Diverse sono le casistiche possibili:
  * Cittadino italiano o straniero residente (iscritto SSN);
  * Soggetti assicurati da istituzioni estere;
  * Europei Non Iscritti (ENI) al SSN;
  * Stranieri Temporaneamente Presenti (STP)
  
  Esempio di cittadino iscrito SSN:
  ```xml
  <patientRole>
    <id root="2.16.840.1.113883.2.9.2.4.3.2"
      extension="XYILNI99M22G999T"
      assigningAuthorityName="Ministero Economia e Finanze"/>
    <id root="[OID DELLO SPAZIO DI IDENTIFICAZIONE USATO NELL’AZIENZA CHE CUSTODISCE CUSTODE DEL PACS]"
      extension="[NUMERO IDENTIFICATIVO PERSONALE]"
      assigningAuthorityName="[NOME DELLO SPAZIO DI IDENTIFICAZIONE USATO
    NELL’AZIENZA CUSTODE DEL PACS]"/>
    <patient>...</patient>
  </patientRole>
  ```
  L'elemento ***patient*** contiene i dettagli anagrafici relativi al paziente.
* ***author***: identifica il soggetto che ha creato il documento, Nel caso del Referto di Radiologia almeno un autore è rappresentato dal Medico Refertante.
* ***custodian***: identifica l'organizzazione incaricata della custodia del documento originale, corrispondente al conservatore dei beni digitali.
* ***legalAuthenticator***: riporta il firmatario del documento.
* participant: rappresenta tutti coloro che partecipano all’atto
descritto dal documento.
* ***inFulfillmentOf***: identifica la richiesta che ha determinato la produzione del documento di Referto di Radiologia od agni altro tipo di ordine ad esso relativo.
* ***componentOf***: descrive l’incontro tra assistito e la struttura sanitaria durante il quale l’atto documentato è avvenuto. Tra i campi da definire l'unico obbligatorio è ```healthCareFacility``` che specifica il luogo.
  ```xml
  <location>
    <healthCareFacility>
      <id root="2.16.840.1.113883.2.9.4.1.6" extension="[CODICE UNITA’ OPERATIVA]"/>
      <location>
        <name>Cardiologia Terapia Intensiva</name>
      </location>
      ...
    </healthCareFacility> 
  </location>
  ```
## Body
Lo standard CDA prevede che il corpo di un documento possa essere formato in modo strutturato (```<structuredBody>```) o in modo destrutturato (```<nonXMLBody>```).
  Un referto di radiologia è organizzato in una serie di sezioni autoconsistenti, definiti dall’elemento ```<section>```.
  Le sezioni OBBLIGATORIE nel caso del CDA:
  * ***Esame eseguito***: descrive l’esame radiologico oggetto del referto. È caratterizzato dalla data di esecuzione, dalla modalità di esecuzione e dalla dose assorbita (qualora l’esame preveda l’esposizione del paziente a radiazioni ionizzanti);
    * ```<code>```: definisce la tipologia di ```<section>``` in base alla codifica LOINC;
    * ```<title>```: rappresenta il titolo della sezione;
    * ```<entry>```: consente di rappresentare in modo strutturato le informazioni di dettaglio riferite nel blocco narrativo
  * ***Referto***:  rappresenta l’elemento centrale e riportata al proprio interno una descrizione delle valutazioni del medico.
    * ```<code>```;
    * ```<title>```.
  
