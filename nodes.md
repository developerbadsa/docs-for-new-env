# Node-Spezifikation

```json
{
  "_id": "string",    // Id des Nodes
  "name": "string",   // Name des Nodes, wird an diversen Stellen zur Darstellung verwendet
  "pid": "string",    // Id des Node Parents
  "sid": "string",    // Id des zugehörigen Schemes
  "path": "string[]", // Pfad der Node-Hierarchie, wird automatisch generiert
  "icon": "string",   // Icon des Nodes, wird an diversen Stellen zur Darstellung verwendet
  "image": "string",  // Id des Bildes, wird an diversen Stellen zur Darstellung verwendet
  "sys": {          // System-Informationen, wird automatisch generiert
    "cAt": "string",  // Zeitpunkt: Created At
    "cBy": "string",  // Id des Nutzers: Created By
    "mAt": "string",  // Zeitpunkt: Last Modified At
    "mBy": "string",  // Id des Nutzers: Last Modified By
    "roles": "Roles"  // Rollen-Berechtigungen: User/Gruppen, die Zugriff auf den Node haben
  },
  "roles": "Roles",   // Manuelle Anpassung der Rollen-Berechtigungen
  "PARAM_XYZ": "any"  // Beliebige Key-Value-Werte, die in dem Node als Payload gespeichert werden (vgl. Blöcke)
}
```

## Role-Spezifikation

```json
{
  "owners": "string[]",  // Liste von Nutzer-Ids und/oder Gruppen-Ids für Owners
  "viewers": "string[]", // Liste von Nutzer-Ids und/oder Gruppen-Ids für Viewers
  "editors": "string[]", // Liste von Nutzer-Ids und/oder Gruppen-Ids für Editors
  "admins": "string[]",  // Liste von Nutzer-Ids und/oder Gruppen-Ids für Admins
  "ROLE_XYZ": "string[]" // Liste von Nutzer-Ids und/oder Gruppen-Ids für beliebige Custom Roles
}
```
+ Der Admin eines Nodes hat alle Rechte auf den Node.


+ Der Author eines Nodes ist standardmäßig der Owner. Ein Owner hat mit Ausnahme an die Anpassung der Rollen alle Rechte für einen Node.

+ Ein Editor kann Inhalte des Nodes bearbeiten bzw. kuratieren.

+ Ein Creator kann Kind-Knoten erstellen. 

+ Ein Viewer hat ein Zugriffsrecht auf einen Node.

Als Nächstes: [Blocks](blocks.md)