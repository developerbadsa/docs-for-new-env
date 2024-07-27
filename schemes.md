# Schemes

## Basis-Scheme-Struktur

```json
{
  "_id": "string", // Id des Schemes
  "bid": "string", // Id des zugehörigen Blocks zur Definition der Node-Darstellung
  "icon": "string", // Icon des Schemas (Wird bei Erstellung eines neuen Nodes verwendet)
  "name": "string", // Name des Schemas (Wird bei Erstellung eines neuen Nodes verwendet)
  "aggregate": "Aggregate", // Definition der Aggregation des Nodes
  "create": "Create" // Definiert Kontext zum Erstellen von Child-Nodes
}
```

## Scheme-Spezifikation

```json
{
  "_id": "string", // ID des Schemas
  "bid": "string", // Zugehöriger Block zur Darstellung des Nodes
  "icon": "string", // Icon des Schemas (Wird bei Erstellung eines neuen Nodes verwendet)
  "name": "string", // Name des Schemas (Wird bei Erstellung eines neuen Nodes verwendet),
  "description": "string", // Beschreibung des Schemas
  "create": {
    // Definiert Kontext zum Erstellen von Child-Nodes
    "color": "string", //Auswahl der Hauptfarbe für Button (default: 'primary')
    "inline": "boolean", // Anstatt ein Modal wird der Kontext im Inline-Formular angezeigt
    "mode": "string", // Kontext ist nur bei diesem Mode sichtbar
    "icon": "string", // Icon für Create-Button (default: 'add')
    "children": [
      {
        "sid": "string", // Id des Schemes als Formular-Vorlage.
        "sidKey": "string", // Node-Key, der die Id des Schemas speichert 
        "access": ["string"], // Einschränkung des Schemas auf Liste von Rollen
        "mode": "string" // Kontext für dieses Schema ist nur bei diesem Mode sichtbar
      }
    ]
  },
  "aggregate": "Aggregate", // Aggregation und Darstellung von assoziierten Nodes (Siehe Aggregate unten)
  "aggregates": "Aggregate[]", // Liste von Aggregates (für Mode-spezifische Aggregationen)
  "buttons": [
    {
      "label": "string", // Label des Buttons
      "icon": "string", // Icon des Buttons
      "action": "string", // ID der Action, die bei Klick ausgeführt wird (vgl. Actions)
      "mode": ["string"], // Sichtbarkeit wird auf  Liste an Modes eingeschränkt
      "visible": "string", // Javascript-Inject zur Regelung der Sichtbarkeit des Buttons, Signatur: (node, mode)=> {return true}
      "access": ["string"], // Liste von Rollen zwecks Zugriffsberechtigung
      "color": "string" // Farbe des Buttons (Werte: 'danger', 'second', 'success', 'green')
    }
  ],
  "server": {
    // Server-seitige Interaktionen
    "onSave": "string", //Javascript-Inject für serverseitige Modifikation von Node-Updates, Signatur: (node, modifier, workers) => { return modifier }
    // Mögliche Workers:
    // -----------------------------------------
    // workers.math: Durchführung von mathematikschen Operationen mittels 'mathjs' (https://mathjs.org/), Beispiel: workers.math.round(workers.math.e, 3)
    // workers.request(options): Durchführung eines Remote Requests (vgl. https://github.com/request/request)
    // workers.getEntities(model, query): Führt eine Mongodb-Suche auf die Datenbank 'model' mit dem Parameter 'query' aus
    // userMembership(node, role): Prüft, ob aktueller Nutzer für 'node' die Rolle 'role' besitzt
    // action(id, params): Führt die Action mit 'id' und Parameter 'params' aus
    // getUser(): Liefert die ID des aktuellen Nutzers
    // getTime(): Aktuelle Zeit
    // getEnvVar(key): Liefert den Env-Parameter des Node-Prozesses
    // updateNode(node, model): Aktualisiert den Node in der Datenbank 'model'
    // getApprover(): Liefert den Approver des Parents
    // getGeoCoordinates: Deprecated -> Zu Robot migrieren
    // workers.generateMultiImage (Deprecated -> Zu Robot migrieren)
    "customRoles": [
      // Passt Rollenberechtigung für dieses Schema an
      {
        "op": "string", // Typ der Operation
        // Mögliche Werte:
        // 'replace': Ersetzt für die Rolle 'role' die Nutzer durch die Node-Nutzer mit den Rollen 'param' bzw. die Parent-Rollen mit den Rollen 'inherit'
        // 'add': Fügt für die Rolle 'role' die Node-Nutzer mit den Rollen 'param' bzw. die Parent-Nutzer mit den Rollen 'inherit' hinzu
        "role": "string", // siehe oben
        "param": ["string"], // siehe oben
        "condition": {
          // Bedingung für Anpassung der Berechtigung
          "key": "string", // Wenn Node-Parameter mit 'key' den Wert 'value' hat
          "value": "any" // siehe oben
        }
      }
    ],
    "postAction": "string", // ID der Action nach Ausführung eines Node-Updates (Übergabe-Parameter "input": ["node", "modifier", "previous", "userId"])
    "protect": {
      // Nur Nutzer mit ['role'] dürfen 'key' modifizieren
      "[key]": ["role"]
    }
  },
  "onSave": "string", //Client-seitiges Javascript-Inject für Modifikation von Node-Updates, Signatur: (node, modifier, workers) => { return modifier }
  // Mögliche Workers:
  // getApp(): Aktuelle App
  // generateUid(len): Generiert eine Unique-ID mit Länge 'len'
  // getBaseUrl(): Base-Url des Clients
  // getUser(): Aktueller Nutzer
  // pruneObjectTree(object): Löscht Äste ohne Inhalt entlang Baum aus JSON-Objekt 'object'
  // getNode(id): Node mit ID 'id'
  // getNodes(query): Liste von Nodes per MongoDB-Query
  // action (id, params): Führt serverseitige Action 'id' mit 'params' aus
  // upsertNode(id, modifier): Führt einen Server-seitigen Node-Update von 'id' mit 'modifier' aus
  "events": {
    "update": "string" // Client-seitiges Javascript-Inject bei Form-Update, Signatur: (form, previousForm, user)=> {return modifier}
  },
  "transform": {
    "onInit": "string", // Client-seitiges Javascript-Inject vor Node-Darstellung, Signatur: (node, profile, helpers)=> {return node}
    "onInitDomains": ["string"] // Optionale Einschränkung auf Domains (z.B. 'modal', 'aggregation')
  },
  "updater": [
    {
      // Deprecated: Zeitbasierte Updates -> TODO: Ersetzen durch Action
      "collection": "string",
      "timer": {
        "cron": "string"
      },
      "query": {
        "where": "[JSON]"
      },
      "set": "[JSON]"
    }
  ], //
  "dynamicGroups": [
    {
      // Generiert automatisch Gruppen (id = prefix + nodeId) aus Eigenschaft 'key' des Nodes und weist Nutzern die Rollen 'roles' zu
      "prefix": "string",
      "key": "string",
      "roles": ["roles"]
    }
  ], //
  "hideBreadcrumb": "boolean", // Verstecke Breadcrumb bei Nodes mit diesem Schema
  "hideContent": "boolean", // Block wird ausgeblendet (für Aggregation hilfreich)
  "persist": [
    // Aktiviert Autospeichern bei jeder Änderung (Speichern-Button nicht nötig)
    {
      "mode": "string", // Einschränkung auf 'mode'
      "keys": ["key"] // zu persistierende 'key' Property-Liste
    }
  ],
  "customBlocks": {
    // Bedingung zur Verwendung einen alternativen Block zur Darstellung des Nodes
    "switch": "string", // Kriterium zur Block-Auswahl Default: 'mode'
    "cases": {
      "value": "string", // zu prüfendender Wert
      "block": "string" // ID des zu verwendenden Nodes
    }
  },
  "collection": "string", // Collection in Datenbank zum Speichern von Nodes  dieses Schemas (default: 'nodes')
  "mergeNode": "boolean", // Der Modifier wird entlang des Eigenschaftsbaums mit dem Node zusammengeführt
  "modalSize": "string", // 'large': Modal-Breite 768px, 'xlarge': Modal-Breite 1280px, 'small': Modal-Breite 480px
  "keyForInitialMode": "string", // Eigenschaft mit 'string' als key des Nodes wird für den initialen Mode beim Öffnen eines Modals verwendet
  "keyForModalTitle": "string", // Eigenschaft mit 'string' als key des Nodes wird für den Modal-Titel verwendet
  "base64ToFile": { "key": "string", "volume": "string" },
  "hideMainCard": "boolean", // Versteckt Card (Unterschied hideContent?)
  "keysForUpdateChildren": ["key"], // Verhindert das automatische Aktualisieren beim Speichern eines Nodes von Kind-Nodes, solange sich die Werte der Keys nicht ändern.
  "customModeBlocks": {
    "[mode]": "blockId" // Verwendet für 'mode' alternativen Block mit 'blockId'
  },
  "persistModeOnNodeChange": "boolean", // Mode wird bei Nodewechsel nicht auf 'view' zurückgesetzt (für Navigation in speziellem Mode sinnvoll)
  "sid": "schemeScheme", // Konstante
  "generators": "any", // deprecated, Todo: Ersetzen durch Action
  "removers": "any", // deprecated, Todo: Ersetzen durch Action
  "inherit": "Inherit", // deprecated
  "sum": "string", // deprecated
  "atoms": "any", // deprecated
  "masses": "any" // deprecated
}
```

> Für Aggregation von Nodes siehe [Aggregate](aggregate.md)
