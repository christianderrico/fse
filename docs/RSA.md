## Referto di Specialistica Ambulatoriale (RSA)
Il Referto di Specialistica Ambulatoriale è finalizzato a definire uno standard per la refertazione di prestazioni ambulatoriali specialistiche (visite mediche ed esami strumentali) che non ricadano nella sfera della medicina di laboratorio, della radiologia ed imaging, dell'anatomia patologica.
Il Referto di Specialistica Ambulatoriale può essere indirizzato sia ad un altro Specialista sia al Medico di Medicina Generale.
### Header
- ***code***: esempio di utilizzo
  ```xml
  <code code=“11488-4”
    codeSystem=“2.16.840.1.113883.6.1”
    codeSystemName=“LOINC”
    codeSystemVersion=“2.64”
    displayName=“Nota di consulto”>
  </code>
  ```
## Body
Lo standard CDA prevede che il corpo di un documento possa essere formato in modo strutturato (```<structuredBody>```) o in modo destrutturato (```<nonXMLBody>```).
Un referto di radiologia è organizzato in una serie di sezioni autoconsistenti, definiti dall’elemento ```<section>```.
Le sezioni OBBLIGATORIE nel caso del RSA:
 | Sezioni        | Codici LOINC | Descrizioni LOINC ShortName                     |
  |----------------|--------------|-------------------------------------------------|
  | Prestazioni | 62387-6      | Interventi |
  | Referto        | 47045-0      | Referto                     |