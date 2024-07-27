# Erste Schritte der App-Konfiguration

Die initale Json einer Beispiel-App hat die folgende Struktur:

```json
{
  "_id": "myFirstApp", //ID der App
  "name": "my First App", //Name der App
  "version": "0.0.1", //App-Version wird bei jeder Änderung inkrementiert
  "default": true, //Falls mehrere Apps eingerichtet sind, kann Start-App mit 'default = true' aktiviert werden, alle anderen erhalten 'default = false'
  "debug": true, //Aktiviert die Logausgabe zur Fehleranalyse in der Konsole des Browsers
  "roles": { "owners": ["1"], "viewers": ["registered"] }, //Standard-Rollen der App
  "root": "ob4Angai7shaef", //Root-Node der App
  "account": {
    //Einstellungen für Account-Schnittstelle
    "loginServices": []
  },
  "nodes": [
    //Liste der initialen, fixen Nodes im Node-Baum
    {
      "_id": "ob4Angai7shaef",
      "sid": "tieheiH8usheiF",
      "app": "myFirstApp",
      "title": "Hallo Welt",
      "checked": true
    }
  ],
  "schemes": [
    //Liste der Schemata zur Definition des Node-Typs
    {
      "_id": "tieheiH8usheiF",
      "app": "myFirstApp",
      "aggregate": {
        "type": "list",
        "onClick": "viewModal"
      },
      "create": {
        "children": [{ "sid": "wiesie4Yoohica" }, { "sid": "yieC8hoh5EeZe2" }]
      },
      "bid": "ouseiyah9gie3E"
    },
    {
      "_id": "wiesie4Yoohica",
      "name": "Kind",
      "icon": "airplane",
      "bid": "Quel3te7jeisah"
    },
    {
      "_id": "yieC8hoh5EeZe2",
      "icon": "bug",
      "name": "Tier",
      "bid": "Quel3te7jeisah"
    }
  ],
  "blocks": [
    //Liste der Blöcke zur Definition der Darstellungen von Node-Typen
    {
      "_id": "Quel3te7jeisah",
      "properties": [
        {
          "key": "name",
          "type": "text",
          "label": "Name"
        }
      ]
    },
    {
      "_id": "ouseiyah9gie3E",
      "app": "myFirstApp",
      "properties": [
        { "key": "title", "type": "text", "label": "Titel" },
        { "key": "check", "type": "checkbox", "label": "Ist gecheckt?" }
      ]
    }
  ],
  "menu": [
    //Liste der Hauptmenüeinträge
    {
      "label": "Edit",
      "icon": "create",
      "config": { "action": "openModal", "mode": "edit" },
      "showDirectly": true
    }
  ]
}
```

_Hinweis:_ Ein Update der App-Version in der Instanz lässt sich auch durch das folgendes Script durchführen:

> Voraussetzung: [nodejs](https://nodejs.org/en/) muss installiert sein.

```bash
node updateJson.js
```

Dabei wird automatisch die Version um 0.0.1 inkrementiert und ein curl-Befehl zum App-Update ausgeführt.

## Grundstruktur der App

Grundsätzlich ist eine App in einer Node-Tree-Struktur angeordnet:

- **RootNode** (Jede App hat genau einen Root-Knoten)
  - **node1** (Ein Node hat immer genau einen Parent: pid)
    - **scheme1** (Jeder Node hat genau ein Scheme: sid)
      - **block1** (Jedes Scheme hat genau einen Block: bid)
  - **node2**
    - **scheme2**
      - **block2**
    - **node3** (Ein node kann beliebig viele Child-Nodes haben: pid)
      - **scheme3**
        - **block3**
          - **block4** (Ein Block kann aus mehreren Teil-Blöcken bestehen)
          - **block5**
          - ...
    - **node4**
      - **scheme3** (Ein Schema kann für mehrere Nodes gelten)
    - **node5**
      - **scheme4**
        - **block3** (Schemata können gleiche Blöcke verwenden)
## Die Kernstrukturen einer App

- [Nodes](nodes.md)
- [Blocks](blocks.md)
- [Schemes](schemes.md)

> Eine Übersicht aller Konfigurationsoptinonen für eine App ist in der [vollständigen Spezifikation](app-definition.md) aufgeführt.

  Als Nächstes: [Nodes](nodes.md)