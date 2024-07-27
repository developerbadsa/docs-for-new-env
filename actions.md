# Actions

Mittels Actions lassen sich automatisierte Vorgänge mittels einer JSON-Notation beschreiben. Auslösende Ereignisse können zum Beispiel der Klick auf einen Button oder die Speicherung eines Nodes in der Datenbank sein.

Eine Action hat das folgende Format:

```json
{
  "_id": "string", // ID der Action
  "steps": ["STEP"], // Definition der Arbeitsschritte (siehe unten)
  "input": ["string"], // Zulässige Input-Variablen
  "output": ["string"], // Rückgabe-Variablen
  "scope": "string", // Die Action wird serverseitig ('server') oder clientseitig ('client') ausgeführt. (Davon abhängig stehen unterschiedliche Step-Typen zur Verfügung, siehe unten.)
  "access": ["role"], // Liste an App-Rollen 'role', die zur Ausführung einer Action berechtigt sind.
  "app": "string" // ID der zugehörigen App
}
```

## Steps

Eine Action führt eine Reihe von Arbeitsschritten aus. Ein Step definiert einen solchen Arbeitsschritt.

```json
{
  "type": "string", // Typ des Arbeitsschritts
  "input": "string", // Input-Variable
  "output": "string[]", // Output-Variable
  "config": "string" // Typ-spezifische Konfiguration
}
```

# Server-seitige Steps

## Set

**Beschreibung**: Ermöglicht das Bearbeiten und Setzen von Variablen.

**type**: set

**scope**: Server

**Beispiel:**

```json
{
  "type": "set",
  "output": [
    { "key": "query", "constant": {} },
    { "key": "modifier", "constant": {} },
    { "key": "query.deactivated.@ne", "constant": true },
    { "key": "query.conditions:toDate.@lte", "value": "to" },
    { "key": "query.conditions:toDate.@gt", "constant": 0 },
    { "key": "modifier.deactivated", "constant": true },
    {
      "key": "root_url",
      "value": "ROOT_URL",
      "type": "system",
      "config": { "fallback": "http://localhost:8100" }
    }
  ]
}
```

<details>
  <summary>Output-Spezifikation</summary>

```json
{
  "key": "string", // Key der Ziel-Variable (Unter-Strukturen durch '.'-Separator zugänglich.)
  "value": "string", // Key der Werte-Variable (Unter-Strukturen durch '.'-Separator zugänglich.)
  "constant": "any", // Konstanter Ausdruck (beliebige Typen zulässig, z.B. string, boolean, Json)
  "strTransform": "template", // Optional: Führt eine String-Transformation mit 'template' als Template durch. {value} in Template wird ersetzt. (Bsp.: "Der Wert ist: {value}.")
  "type": "string", // 'system': Zugriff auf System-Variablen mit 'value' als Variablenamen
  "config": {
    "fallback": "string" // konstanter Alternativ-Wert, falls System-Variable nicht gesetzt ist. (für type = 'system')
  }
}
```

</details>

## Eval

**Beschreibung**: Führt eine Javascript-Injection aus.

**type**: eval

**scope**: Server

**Beispiel:**

```json
{
  "output": "name",
  "type": "eval",
  "config": {
    "function": "(vars) => { let d = new Date(); return 'Einkaufsliste vom ' + d.getDate() + '.' + (d.getMonth() + 1) + '.' + d.getFullYear();};"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "function": "string" // Javascript-Inject zur flexiblen Kalkulation von Variablen, Signatur vars => { return value; }
}
```

</details>

## Condition

**Beschreibung**: Prüft den Inhalt einer Variablen und führt davon abhängig optionale Steps aus.

**type**: condition

**scope**: Server

**Beispiel:**

```json
{
  "type": "condition",
  "input": "test",
  "config": {
    "constant": true,
    "fallback": [{}]
  },
  "steps": [{}]
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "value": "string", // Test gegen Variable
  "constant": "any", // Test gegen konstanten Wert
  "defined": "string", // Test, ob gesetzt
  "steps": [{}], // Liste von Arbeitsschritten, die bei erfolgreichem Test ausgeführt wird
  "else": [{}] // Liste von Arbeitsschritten, die bei fehlerhaftem Test ausgeführt wird
}
```

</details>

## FindOneDB

**Beschreibung**: Sucht einen Eintrag in der Datenbank.

**type**: findOneDB

**scope**: Server

**Beispiel:**

```json
{
  "type": "findOneDB",
  "config": { "collection": "profiles" },
  "input": "query",
  "output": "profile"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "collection": "string" // MonboDB-Collection (default: 'nodes')
}
```

</details>

## FindDB

**Beschreibung**: Sucht eine Liste von Einträgen in der Datenbank.

**type**: eval

**scope**: Server

**Beispiel:**

```json
{
  "output": "cards",
  "type": "findDB",
  "input": "query"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "collection": "string" // MonboDB-Collection (default: 'nodes')
}
```

</details>

## CountDB

**Beschreibung**: Sucht eine Liste von Einträgen in der Datenbank und liefert die Anzahl.

**type**: countDB

**scope**: Server

**Beispiel:**

```json
{
  "type": "countDB",
  "config": { "collection": "users" },
  "output": "count"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "collection": "string" // MonboDB-Collection (default: 'nodes')
}
```

</details>

## InsertDB

**Beschreibung**: Fügt einen Eintrag in die Datenbank ein.

**type**: insertDB

**scope**: Server

**Beispiel:**

```json
{
  "type": "insertDB",
  "config": { "collection": "nodes" },
  "input": "node",
  "output": "result"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "collection": "string" // MonboDB-Collection (default: 'nodes')
}
```

</details>

## UpdateDB

**Beschreibung**: Aktualisiert einen Eintrag in der Datenbank.

**type**: updateDB

**scope**: Server

**Beispiel:**

```json
{
  "type": "updateDB",
  "input": "profile",
  "config": { "collection": "profiles" }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "collection": "string" // MonboDB-Collection (default: 'nodes')
}
```

</details>

## RemoveDB

**Beschreibung**: Löscht einen Eintrag in der Datenbank.

**type**: removeDB

**scope**: Server

**Beispiel:**

```json
{
  "type": "removeDB",
  "input": "query",
  "config": { "collection": "nodes" },
  "output": "count"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "collection": "string" // MonboDB-Collection (default: 'nodes')
}
```

</details>

## Request

**Beschreibung**: Führt einen Web-Request aus.

**type**: request

**scope**: Server

**Beispiel:**

```json
{
  "type": "request",
  "input": "options", // Objekt mit Parametern: url (Request-Url), method (Web-Request-Methode), json (zu übergebendes JSON-Objekt)
  "output": "response" // Zurückgegebenes JSON
}
```

Beispiel für Definition von Options:

```json
{
  "type": "set",
  "output": [
    { "key": "options.url", "value": "url" },
    { "key": "options.method", "constant": "POST" },
    { "key": "options.json._id", "value": "node._id" },
    { "key": "options.json.data", "value": "data" }
  ]
}
```

## loop

**Beschreibung**: Führt eine Liste von Aktionsschritten für jeden Eintrag aus einer Variablen-Liste aus.

**type**: Loop

**scope**: Server

**Beispiel:**

```json
{
  "type": "loop",
  "input": "nodes",
  "steps": []
}
```

## log

**Beschreibung**: Gibt eine Variable auf der Konsole aus.

**type**: Loop

**scope**: Server

**Beispiel:**

```json
{ "type": "log", "input": "node" }
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "steps": [] // Liste von auszuführenden Arbeitsschritten. (Auf das Listenelement des aktuellen Schleifendurchlaufs kann über die Variable 'iterator' zugegriffen werden.)
}
```

</details>

## Action

**Beschreibung**: Führt eine weitere Action aus. (Server-seitige Actions können nur Server-seitige Actions ausführen, Client-seitige können sowohl Server-seitige als auch Client-seitige ausführen.)

**type**: action

**scope**: Server

**Beispiel:**

```json
{
  "type": "action",
  "input": "params",
  "config": { "id": "checkImprovementState" }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "id": "string" // ID der auszuführenden Action
}
```

</details>

## Array-Operation

**Beschreibung**: Führt eine Transformation auf eine Variablen-Liste aus.

**type**: arrayOp

**scope**: Server

**Beispiel:**

```json
{
  "type": "arrayOp",
  "config": {
    "op": "map",
    "mapping": "_id"
  },
  "input": "cards",
  "output": "cardIds"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "op": "string", // Typ der Operation:
  // 'add': Fügt das Objekt 'input' zum Array 'output' hinzu.
  // 'map': Bildet die Einträge des Arrays auf die Eigenschaft 'mapping' ab.
  // 'update': Ersetzt Eintrag an der Position 'config.index' durch 'input'
  // 'remove': Löscht das Objekt mit der Id 'input' aus dem Array 'output'
  "mapping": "string", // Mapping-key für type = 'map'
  "unique": "boolean", // Fügt das Object nur hinzu, wenn es noch nicht im Array vorhanden ist. (für type = 'add')
  "index": "number"
}
```

</details>

## String

**Beschreibung**: Überführt einen Variable-Template in einen String.

**type**: string

**scope**: Server

**Beispiel:**

```json
{
  "type": "string",
  "output": "url",
  "input": "${imageAPIUrl}/generate" // ${key} wird ersetzt durch den Wert der Variable 'key'
}
```

## Date

**Beschreibung**: Liefert das aktuelle Datum und speichert es in 'output'.

**type**: date

**scope**: Server

**Beispiel:**

```json
{
  "type": "date",
  "output": "from",
  "config": {
    "hours": 23,
    "minutes": 59,
    "seconds": 59
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "hours": "number", // Überschreibt Stunden durch 'number'
  "minutes": "number", // Überschreibt Minuten durch 'number'
  "seconds": "number" // Überschreibt Sekunden durch 'number'
}
```

</details>

## Math

**Beschreibung**: Führt eine mathematische Operation aus.

**type**: math

**scope**: Server

**Beispiel:**

```json
{
  "type": "math",
  "config": { "code": "default + 1" },
  "output": "newLevel"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "code": "string" // Mathematische Operation. ('input'-Wert, alternativ 'default' kann als variabler Parameter verwendet werden.)
}
```

</details>

## Robot

**Beschreibung**: Führt einen externen Quandes-Robot-Aufruf durch.

**type**: robot

**scope**: Server

**Beispiel:**

```json
{
  "type": "robot",
  "config": { "sequence": "imageLookup" },
  "input": ["url", "password"],
  "output": ["author", "id", "image"]
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "sequence": "string" // ID der aufzurufenden Robot-Sequenz. ('input': Input-Parameter, 'output': Output-Parameter)
}
```

</details>

## Annex

**Beschreibung**: Führt eine Annex-Operation (Annotation) für ein Node aus.

**type**: annex

**scope**: Server

**Beispiel:**

```json
{
  "type": "annex",
  "input": "cardIds",
  "config": {
    "key": "activated",
    "op": "add"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "key": "string", // Schlüssel des zu aktualisierenden Annex für die Liste an Nodes aus 'input'.
  "op": "string" // Operation:
  // 'add': Fügt Nutzer-Annotation hinzu.
  // 'remove': Löscht Nutzer-Annotation.
}
```

</details>

Als Nächstes: [Quandes Robot](robot.md)
