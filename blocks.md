# Block-Spezifikation

Blöcke dienen zum einen zur Festlegung der Datenstruktur eines Nodes und regeln zum anderen die Darstellung im Lese- bzw. Bearbeitungsmodus in dem Client. Ein Block hat die folgende Struktur.

```json
{
  "_id": "string", // Id des Blocks
  "mode": "string", // Darstellungs-Mode des Blocks, default: view
  "properties": "Property[]", // Definition der Properties
  "style": "any" // Optionale CSS-Style-Konfiguration
}
```

# Properties

Eine Property repräsentiert eine Eigenschaft eines Nodes. Sie hat die folgende Minimal-Konfiguration:

**Beispiel:**

```json
{ "label": "Titel", "key": "title", "type": "text" }
```

Der key ist der Schlüssel, in dem der Eingabewert des Nutzers in dem zugehörigen Node gespeichert wird. Dies könnte zum Beispiel für _type = text_ der Inhalt des Text-Inputs der Formulareingabe sein.

<details>
  <summary>Property-Spezifikation</summary>

```json
{
  "key": "string", // Schlüssel zur Speicherung des Inhalts der Eigenschafts im Node
  "type": "string", // Eigenschaftstyp zur Festlegung des Komponententyps
  "label": "string", // Label zur Beschreibung der Eigenschaft
  "config": "any", // Optionale Typ-spezifische Konfiguration
  "config": {
    "style": "{CSS}", // Allgemein: Definiert CSS-Style eines Elements
    "flex": "main", // Allgemein: Priorisiert die Expansion des Elements, falls das übergeordnete Container-Element 'layout = "row"' und 'align = "flex"' hat
    "colSize": "number", //Größe der Spalte, falls übergeordneter Container layout = 'row'  (Summe aller Spalten = 12)
    "autoColSize": "boolean" // Automatische Zuweisung der Spaltengröße
  },
  "conditions": "any[]", // Optionale Bedingungen zur Einschränkung der Sichtbarkeit
  "properties": "Property[]", //Geschachtelte Eigenschaften (für "type = container")
  "icon": "string", // Das Icon wird bei der Darstellung einer Eigenschaft verwendet
  "mode": "string", // Optionale Festlegung des Darstellungs-Modes eines Containers und den zugehörigen Eigenschaften (für "type = container")
  "order": "number", // Optionale Festlegung der Reihenfolge der Eigenschaften
  "options": "any[]", // Optionale Typ-spezifische, weitere Optionen (für type = 'dropdown')
  "isArray": "boolean", // Legt fest, dass die Eigenschaften eine Liste von Eigenschaften beschreibt (Falls der Typ ein Container ist, muss ein key angegeben werden)
  "default": "any", // Festlegung eines Default-Werts
  "showAlways": "boolean", // Diese Option legt fest, dass die Eigenschaft immer angezeigt wird, auch wenn kein Inhalt im Node vorhanden ist.
  "required": "boolean", // Setzt die Eigenschaft auf erforderlich. Die Formulareingabe ist nur dann gültig, wenn alle erforderlichen Felder gefüllt sind.
  "access": "string[]" //Einschränkung der Sichtbarkeit auf Rollen (owners, admins, editors, creators, viewers)
}
```
**Bemerkung:** Für Icons (Schlüsselwort "icon") können folgende Bibliotheken verwendet werden:
- [Ion Icons](https://ionic.io/ionicons)
- [Fontawesome Free](https://fontawesome.com/icons) (**Verwendung**: fa:%name%, wobei %name% der Icon-Name ohne vorangestelltes "fa-" ist)

</details>


# Basis-Properties

Die Basiskomponenten gehören zu den am häufigsten verwendeten Properties.

## Container

**Beschreibung**: Diese Property dient zur zur Strukturierung und Darstellung von Inhalten.

**type**: container

```json
{
  "type": "container",
  "config": {
    "layout": "string" //Definiert das Layout des Containers:
    // 'column' Standardlayout
    // 'card' Kartenlayout
    // 'rows' Anordnung nebeneinander anstatt untereinander
    // 'tabs' Anordnung in Tabs
    // 'expand' Aufklappbarer Container
  },
  "properties": ["..."] // Verschachtelte Properties des Containers
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "type": "container",
  "config": {
    "layout": "string", //Definiert das Layout des Containers:
    // 'column' Standardlayout
    // 'card'   Kartenlayout
    // 'rows'   Anordnung nebeneinander anstatt untereinander
    // 'tabs'   Anordnung in Tabs
    // 'expand' Aufklappbarer Container
    "tabStyle": "{CSS}", // CSS-Style für Tab-Container (wenn layout = 'tabs')
    "block": "string", // Id eines anderen Blocks zwecks Template-basiertem Import
    "blockOptions": "JSON", // Optionale key-value-Parameter, die dem zu importierenden Blocks als condition-context übergeben werden.
    "align": "flex", // Wenn layout = 'row': Definert Flex-Grow-Container
    "colSize": "number", // Wenn align = 'flex': Definiert Flex-Breite des Containers (Wert: 1-12)
    "colSizeSM": "number", // Wenn align = 'flex': Definiert Flex-Breite des Containers für kleine Bildschirme (Wert: 1-12)
    "collapsed": "string", // Wenn layout = 'expand': Zeigt den Container initial eingeklappt
    "onClick": {
      "action": "string",
      // 'viewModal':   öffnet Referenz in neuem Modal
      // 'changeMode':  wechselt Mode des Nodes
      // 'externalUrl': ruft external Url auf
      "mode": "string",
      "key": "string", // Id des nodes (wenn action = 'viewModal')
      "mapping": { "[key]": "[value]" } // Optinales Mapping wenn action = 'viewModal' || 'externalUrl'
    },
    "expandColumn": "boolean", // Der Container wird bis zur maximalen Höhe der Spalte expandiert
    //-------------------------------------
    //Array-spezifische Konfiguration
    //(Wenn übergeortnet "isArray: true")
    "search": {
      // Listenbasierte Suchabfrage zum Hinzufügen zur Liste
      "query": "any", // Query-Abfrage für Nodes
      "sort": { "[key]": "number" }, // Sortierreihenfolge der Suchergebnisse (key: property, number: 1 oder -1
      "mapping": { "[key]": "[nodeKey]" }, // Properties [nodeKey] des ausgewählten Nodes werden auf [key] gemappt
      "collection": "string" // Optional: Wenn die MongoDB-Collection von 'nodes' abweicht
    },
    "relation": {
      // Modal-basierte Selektion eines Nodes zum Hinzufügen zur Liste
      "selector": "string", // Id des Nodes zur Darstellung der Auswahl-Liste
      "collection": "string", // Optional: Node aus dieser Datenbank (default: 'nodes')
      "mapping": { "[key]": "[value]" } // Mapping: [value] zu [key]
    },
    "hideAdd": "boolean", //   Add-Button ausblenden
    "hideSort": "boolean", //  Buttons zur Sortierung ausblenden
    "addOnTop": "boolean", //  Add-Button über anstatt unter der Liste anzeigen
    "arrayIndex": "boolean", // Zeigt eine fortlaufende Zahl vor dem Container an
    "addLabel": "string", //   Spezielles Label für Button zum Hinzufügen von neuen Elementen
    "interAdd": "boolean", //  Add-Button auch zwischen den Containern anzeigen
    "addOptions": [
      // Generiert Auswahl-Menü für Add-Button:
      {
        "label": "string", // Auswahl-Label
        "icon": "string", //  Auswahl-Icon
        "value": { "[key]": "[value]" } //[key] des Elements wird mit [value] vorausgefüllt
      }
    ]
    //-------------------------------------
  },
  "properties": ["..."] // Verschachtelte Properties des Containers
}
```

</details>

## Text

**Beschreibung**: Erzeugt im Edit-Modus ein einzeiliges Texteingabefeld und zeigt im View-Modus den Text-Inhalt an.

**type**: text

**Beispiel:**

```json
{ "key": "label", "label": "Label", "type": "text" }
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "type": "text",
  "config": {
    "type": "string", // Spezifischer Eingabe-Modus: 'number' | 'password'
    "clearInputButton": "boolean", //Zeigt Button zum Löschen des Inputs an
    "placeholder": "string", //Platzhalter-Text, wenn Texteingabe leer
    //-------------------------------------
    //Spezifische Konfiguration für type = 'number':
    "round": "number", // Anzahl der Nachkommastellen zum Runden im View-Modus
    "min": "number", // Minimal-zulässiger Wert
    "max": "number", //  Maximal-zulässiger Wert
    "step": "number", // Sprung-Länge bei der manuellen Erhöhung des Wertes,
    "selectOptions": [
      // Erzeugt ein Kontext-Menu zur schnellen Auswahl von Standard-Werten
      {
        "label": "string", // Label der Auswahl
        "value": "any" // Inputfeld wird mit 'value' gefüllt
      }
    ],
    //-------------------------------------
    //-------------------------------------
    //Spezifische Konfiguration für mode = 'view':
    "postfix": "string" // Wird bei der Darstellung an Text angehängt
    //-------------------------------------
  }
}
```

</details>

## HTML

**Beschreibung**: Generiert eine dynamische Ansicht zur Anzeige eines Werts.

**type**: html

**Beispiel:**

```json
{
  "type": "html",
  "key": "time",
  "config": {
    "customProperties": [{ "key": "lower" }, { "key": "upper" }],
    "custom": "{1} - {2} Min."
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "type": "html",
  "key": "weight",
  "config": {
    "custom": "string", // HTML-String mit Platzhalter {x} (Verweis auf customProperties mit {key} bzw. {n} für n-tes Element)
    "customProperties": [
      {
        "key": "string", // Key der Node-Property (deep-Key 'key1.key2' auch möglich)
        "inner": "boolean" // Schlüssel relativ zu aktuellem Formular-Element
      }
    ],
    "calc": "string", //Mathematische Kalkulation vor Übergabe an Custom-String mittels {html} (Ermöglicht Verwendung von customProperties-Platzhaltern (vgl. oben))
    "setZero": "boolean", //Zeige 0 an anstatt ausblenden, wenn Element keinen Wert hat
    "hoverLabel": "string", //Label, welches beim Überfahren des Mauszeigers angezeigt wird
    "round": "number", // Anzahl der Nachkommastellen zum Runden eines Zahlenwertes
    "replace": [{ "key": "string", "label": "string" }], //Statische Lookup-Liste zum Ersetzen eines Wertes
    "dynamicNumber": "number", //Lässt bei dynamischen Wechsel des Wertes die Zahl langsam laufend in- bzw. dekrementieren
    "ellipsis": "string", //Länge des Umbruchs zur Ellipsis-Ansicht (z.B. "100px")
    "htmlStyle": "{CSS}" // CSS-Styling der HTML-Komponente
  }
}
```

</details>

## Checkbox

**Beschreibung**: Erzeugt eine Checkbox mit dem Status ''Aktiv/Inaktiv''.

**type**: checkbox

**Beispiel:**

```json
{
  "label": "Ich akzeptiere die AGBs",
  "type": "checkbox",
  "key": "acceptAGBs",
  "custom": { "multiLineLabel": true } // Erlaubt Zeilenumbruch bei Label
}
```

## User

**Beschreibung**: Erzeugt ein Feld zur Auswahl eines Nutzers.

**type**: user

**Beispiel:**

```json
{
  "key": "members",
  "config": { "multi": true },
  "type": "user",
  "label": "Mitglieder"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "key": "members",
  "config": {
    "multi": "boolean", // ermöglicht Eingabe mehrerer Nutzer
    "display": "string", // Layout der Anzeige ('name', 'circle')
    "width": "number", // Breite des Bildes (für display = 'circle')
    "advancedSearch": [
      // Angepasste Suche gegen die Profil-Datenbank der App (Kombination mehrerer Suchen möglich, Priorisierung nach Reihenfolge)
      {
        "label": "string", // Label der Suche
        "onFocus": "boolean", // Die Suche startet direkt bei Nutzer-Focus
        "query": "string" // Angepasste Search-Query mit folgender Signatur: (node, form, userId, users, workers)=>{return {_id: {$in: ['1', '2', '3'] } } };}
      }
    ],
    "onClick": {
      // Öffnet bei On-Click ein Modal
      "sid": "string", //zur Darstellung des Modals verwendetes Scheme
      "mode": "string" //Initialer Mode des geöffneten Modal
    },
    "mode": "string", // 'initials': Zeigt lediglich die Initialien des Nutzers
    "currentUser": "boolean" // Darstellung des aktuell eingeloggten Nutzers
  },
  "type": "user",
  "label": "Mitglieder"
}
```

</details>

## Dropdown

**Beschreibung**: Erzeugt eine Dropdown mit Auswahloptionen.

**type**: dropdown

**Beispiel:**

```json
{
  "type": "dropdown",
  "label": "Dropdown",
  "key": "dropdown",
  "icon": "arrow-dropdown",
  "options": [
    { "key": "a", "label": "A" },
    { "key": "b", "label": "B" },
    { "key": "c", "label": "C" }
  ]
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "type": "dropdown",
  "label": "Dropdown",
  "key": "dropdown",
  "icon": "arrow-dropdown",
  "config": {
    "interface": "string", // 'alert' | 'popover' | 'action-sheet' (Standard)
    "selectFirst": "boolean", // Erste Eintrag der Liste wird automatisch gewählt
    "filter": "string" //Optional: js-Snippet zum Filtern der Optionen; Signatur: (options, node, form)=>{[ { "key": "a", "label": "A" },    { "key": "b", "label": "B" } ]}
  },
  "options": [
    // Auswahl Optionen mit 'key' (Wert) und 'label':
    { "key": "a", "label": "A" },
    { "key": "b", "label": "B" },
    { "key": "c", "label": "C" }
  ]
}
```

</details>

## Button

**Beschreibung**: Erzeugt einen Button mit einer Aktion.

**type**: button

**Beispiel:**

```json
{
  "type": "button",
  "label": "Edit",
  "key": "edit",
  "icon": "create",
  "config": { "action": "changeMode", "mode": "edit" }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "type": "button",
  "label": "Edit",
  "key": "edit",
  "icon": "create",
  "config": {
    "action": "{ACTION}", // vgl. Action-Config unten
    "clearButton": "boolean", // Transparenter Hintergrund
    "expand": "boolean", // Auf ganze Breite expandieren
    "hoverLabel": "string", // Label bei Überlauf des Mauszeigers
    "color": "string", // Farbe des Buttons
    "buttonStyle": "{CSS}" // Styling des Buttons
  }
}
```

- Action-Aufruf: [Action-Config](action-config.md)

</details>

## File

**Beschreibung**: Erzeugt ein Feld zum Auswählen und Hochladen von Dateien.

**type**: file

**Beispiel:**

```json
{
  "key": "image",
  "icon": "fa:image",
  "type": "file",
  "config": {
    "thumbnailStyle": { "height": "160px", "width": "200px" },
    "volume": "images",
    "maxNumber": 1,
    "accept": "image/*"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "volume": "string", // Verwendetes Volume zum Speichern der Dateien
  "maxNumber": "number", // Maximale Anzahl von hochladbaren Dateien je Node
  "accept": "string", // Einschränkung der erlaubten Dateitypen (z.B. 'image/*')
  "thumbnailStyle": "[CSS]", // Style für Thumbnail-Darstellung
  "hideName": "boolean", // Dateiname wird nicht angezeigt
  "livePasting": "boolean", // Ermöglicht das Einfügen von Bildern aus der Zwischenablage
  "secure": "boolean" // Aktiviert Einschränkung des Schreibzugriffs auf erstellenden Nutzer
}
```

</details>

Für mehr Informationen zum accept-Parameter vgl.: [Unique file type specifiers](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file#unique_file_type_specifiers).

## Image

**Beschreibung**: Stellt ein Bild dar.

**type**: image

**Beispiel:**

```json
{
  "type": "image",
  "key": "image",
  "config": {
    "volume": "images",
    "originalSize": "160x100x95",
    "preloadSize": "80x50x40"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "volume": "string", // Verwendetes Volume zum Laden der Bilder
  "disableOpenImage": "boolean", // deaktiviert das Öffnen des Bildes bei Klick
  "size": "XxYxTxC", // Größe des Bildes; X: Breite, Y: Höhe, T: 'PNG' oder '01-99' (Detailstufe für JPG), C: Optionaler Parameter zur Darstellung 'contain' anstatt 'cover'
  "imageStyle": "[CSS]", // Styling für Bild
  "constant": "string", // Konstante File-ID zum Laden des Bildes
  "preloadSize": "XxYxT", // deprecated, Größe des Vorschau-Bildes; X: Breite, Y: Höhe, T: 'PNG' oder '01-99' (Detailstufe für JPG)
  "originalSize": "XxYxT" // deprecated, Größe des finalen Bildes; X: Breite, Y: Höhe, T: 'PNG' oder '01-99' (Detailstufe für JPG)
}
```

</details>

# Erweiterte Komponenten

## Annex

**Beschreibung**:
Ermöglicht nutzerbasierte Annex-Funktion (z.B. Like-Funktion).

**type**: annex

**Beispiel:**

```json
{
  "key": "like",
  "type": "annex",
  "icon": "heart",
  "label": "Zu meinen Favoriten hinzufügen",
  "config": {
    "style": {
      "width": "max-content",
      "font-size": "90%",
      "margin-top": "-1px"
    },
    "color": "#155252",
    "outlineColor": "#155252",
    "outlineIcon": "far:thumbs-up",
    "disableClick": true
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "color": "string", // Farbe des Annex-Buttons
  "outlineColor": "string", // Farbe des Annex-Buttons, falls vom Nutzer noch nicht aktiviert
  "outlineIcon": "string", // Icon des Annex-Buttons, falls vom Nutzer noch nicht aktiviert (Standard-Icon wird über icon-Wert der Eigenschaft gesetzt)
  "disableClick": "boolean", // Deaktiviert Klick-Funktion
  "hideCount": "boolean", // Deaktiviert den Zähler der Annexes von  Nutzern
  "isButton": "boolean", // Annex als Button dargestellt
  "interceptWithAction": "string" // Führt die Action vor Click-Event aus. Click-Event wird nicht gestartet, wenn status = false zurückgegeben wird.
}
```

</details>

## Color

**Beschreibung**:

**type**: color

**Beispiel:**

```json
{
  "label": "Dark",
  "key": "dark",
  "type": "color"
}
```

<details>
  <summary>Config-Spezifikation</summary>
 
 keine

</details>

## Date

**Beschreibung**: Erzeugt ein Eingabefeld zur Auswahl von Datum und/oder Uhrzeit.

**type**: date

**Beispiel:**

```json
{
    "type": "date",
    "label": "Date",
    "key": "date",
    "icon": "calendar",
    "config": {
      "pickerFormat": "DD MM YYYY HH:mm",
      "displayFormat": "DD MM YYYY HH:mm",
      "minuteValues": "0,5,10,15,20,25,30,35,40,45,50,55",
      "pastYears": 5,
      "futureYears": 5
    }
},

```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "format": "string", // Wert: 'timestamp': Speichert einen Timestamp
  "pickerFormat": "string", // Definiert das Format des Date-Pickers (z.B. 'DD MM YYYY HH:mm'), vgl. https://www.w3.org/TR/NOTE-datetime
  "displayFormat": "string", // Definiert die Darstellung des Datums (z.B. 'DD MM YYYY HH:mm')
  "minuteValues": "string", // Schränkt die auswählbaren Werte des Minute-Pickers ein (z.B. '0,20,40')
  "pastYears": "number", // Legt die Anzahl der Jahre zurück bei der Jahresauswahl fest
  "futureYears": "number" // Legt die Anzahl der Jahre vor bei der Jahresauswahl fest
}
```

</details>

## Fetch

**Beschreibung**:
Ermöglicht die Aggregation von Nodes innerhalb eines Blocks.

**type**: fetch

**Beispiel:**

```json
{
  "type": "fetch",
  "config": {
    "type": "raw",
    "sort": {
      "sys:cAt": -1
    },
    "paging": {
      "type": "infiniteScroll",
      "size": 10
    },
    "reverse": true,
    "autoScroll": true,
    "onClick": "viewModal"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "type": "string", // Typ der Aggregation:
  // 'count': Nur Anzahl der Nodes wird angezeigt
  // 'raw': Die Nodes werden ohne Formattierung angezeigt
  // 'list': Die Nodes werden in einer Liste angezeigt
  // 'grid': Die Nodes werden als Kacheln angezeigt
  // 'kanban': Nodes werden in Spalten angeordnet (Kanban)
  // 'mindmap': Nodes werden in Mindmap angezeigt
  // 'slider': Es wird ein aktiver Node angezeigt, alle weiteren können per Slide-Effekt durchgeblättert werden.
  "query": {
    "[key]": "[value]" // MongoDB-Query; MongoDB-Parameter mit $ durch @ ersetzen, deep-Values mit ':' (z.B. 'sys:cAt'), dynamische Parameter:
    // #NODE: Parameter mit Node-Eigenschaft (z.B. { #NODE: pid }),
    // #USER: Parameter mit User-Eigenschaft (z.B. { #USER: _id }),
    // #VALUE: Form mit Eigenschaft ({ #VALUE: true }),
  },
  "filters": [
    {
      "type": "string", // Typ:
      // 'multi': Mehrfachauswahl von Filter-Optionen
      "key": "string", // zu filternde Node-Eigenschaft
      "label": "string", // Label
      "options": [
        // Auswahl-Optionen (für type = 'multi' oder type = 'dropdown')
        { "key": "string", "label": "string" }
      ],
      "default": "any" // Default-Wert
    }
  ],
  "limit": "number", // Limitiert Ergebnisse auf 'number'
  "loadMore": {
    "chunk": "number", // Anzahl der Ergebnisse je Chunk
    "label": "label" // Label für Aktion zum Laden weiterer Elemente
  },
  "reverse": "boolean", // Anzeige in umgekehrter Reihenfolge
  "onClick": "string|[string]", // Aktion bei Klick auf Aggregations-Element:
  // [string]: Führt die Liste an Actions mit 'string' als ID aus
  // viewModal: Öffnet ein Modal mit dem Node im Mode 'view'
  // editModal: Öffnet ein Modal mit dem Node im Mode 'edit'
  // navigate: Navigiert zu dem Node
  // disable: Deaktiviert die Klick-Option
  "options": {
    "sort": { "[key]": "1|-1" }, // Sortiert Ergebnis der Aggregation mit [key] absteigend (1) oder aufsteigend (-1); Deep-Keys mit ':' trennbar
    "paging": {
      "type": "string", // infiniteScroll: Ermöglicht kontinuierliches Scrollen der Ergebnisse, weitere Elemente werden dynamisch nachgeladen
      "size": "number" // Anzahl der Einträge je Segment
    },
    "columns": [
      // Spalten für Kanban-Typ
      { "key": "string", "label": "string" }
    ],
    "key": "string", // Node-Key für Einordnung in Spalten (für type = 'kanban')
    "create": {
      // Schema zum Erstellen von neuen Nodes (für type = 'kanban')
      "sid": "string"
    },
    "newNodeLabel": "string", // Name für neues Element im Mindmap-Baum (für type = 'mindmap')
    "itemActivities": [
      // Kontext-Aktivitäten in Mindmap (für type = 'mindmap')
      {
        "icon": "string", // Icon
        "label": "string", // Hover-Label
        "action": "string" // Aktion:
        // 'modal': Öffnet Modal mit Node im Edit-Mode
        // 'add': Hinzufügen eines Child-Nodes
        // 'navigate': Navigiere zu Node
      }
    ],
    "itemContent": [
      // Inhaltselement in Mindmap (für type = 'mindmap')
      {
        "type": "string", // Typ des Inhalts:
        // user: Darstellung eines Nutzers
        // icon: Anzeige eines Icons
        // number: Anzeige als Nummer
        "key": "string", // Node-Eigenschaft für Nutzer-Referenz (für type = 'user')
        "options": [
          // Optionen für Icon (für type = 'icon')
          {
            "key": "string", // Wert
            "label": "string", // Hover-Label
            "icon": "string" // Angezeigtes Icon
          }
        ],
        "postfix": "string", // String wird an Nummer angehängt (für type = 'number')
        "color": "string", // Farbe
        "label": "string", // Hover-Label
        "action": "string" // Aktion:
        // 'modal': Öffnet Modal mit Node im Edit-Mode
        // 'add': Hinzufügen eines Child-Nodes
        // 'navigate': Navigiere zu Node
      }
    ]
  },
  "iconColor": "string", // Farbe des Icons (für type = 'count')
  "manyLabel": "string", // Label für mehrere Ergebnisse (für type = 'count')
  "oneLabel": "string", // Label für ein Ergebnis (für type = 'count')
  "actionOnFormChagne": "string", // deprecated, ID der Action bei Form-Change ausgeführt
  "actionParameters": [], // deprecated
  "autoScroll": "boolean", // Springt bei Darstellung ans Ende der Ergebnisliste (z.B. bei Chat)
  "image": {
    // Bild wird in Liste angezeigt (für type = 'list')
    "size": "XxYxPNG" // X: Breite, Y: Höhe
  },
  "layout": "string" // 'card': Wird als Karte angezeigt (für type = 'list')
}
```

</details>

## Hidden

**Beschreibung**: Erzeugt einen nicht sichbaren Formular-Eintrag. Dies ist insbesondere nützlich, wenn eine bereits bestehende Eigenschaft einer Node-Unterstruktur mit übergeben werden soll, so dass sie beim Speichern nicht verloren geht.

**type**: hidden

**Beispiel:**

```json
{ "type": "hidden", "key": "image" }
```

## Icon

**Beschreibung**: Stellt ein Icon dar.

**type**: icon

**Beispiel:**

```json
{
  "type": "icon",
  "label": "Icon",
  "key": "icon"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "custom": "string" // Anstatt das Icon aus der key-Property auszulesen, wird  ein statisches Icon verwendet.
}
```

</details>

## Link

**Beschreibung**: Erzeugt ein Feld zur Eingabe eines Bildes.

**type**: link

**Beispiel:**

```json
{
  "type": "link",
  "label": "Link",
  "key": "link",
  "icon": "share-alt"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "clipboard": "boolean", // Zeigt einen Button mit Copy-To-Clipboard-Funktion
  "link": "string", // Zeigt einen statischen Link an, anstatt dynamische key-Property von Node zu verwenden
  "title": "string" // Titel des Links
}
```

</details>

## Radio

**Beschreibung**: Erzeugt mehrere Auswahloptionen, die als Radio dargestellt werden.

**type**: radio

**Beispiel:**

```json
{
  "type": "radio",
  "label": "Radio",
  "key": "radio",
  "icon": "radio-button-on",
  "options": [
    { "key": "green", "label": "Green" },
    { "key": "blue", "label": "Blue" }
  ]
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "keyForOptions": "key", // anstatt statische 'options' zu verwenden, werden die Options aus dem Node-Eigenschaft 'key' bezogen.
  "mapping": "string" // Javscript-Injection für dynamisches Mapping jeder einzelnen Option, Signatur: (option) => return {label: 'Label', value: 'Value' }
}
```

</details>

## Range

**Beschreibung**: Erzeugt einen Range-Slider zur Eingabe von nummerischen Werten.

**type**: range

**Beispiel:**

```json
{
  "type": "range",
  "label": "Range",
  "key": "range",
  "config": {
    "min": "10",
    "max": "100",
    "leftIcon": "arrow-dropdown",
    "leftLabel": "Min",
    "rightIcon": "arrow-dropup",
    "rightLabel": "Max",
    "snaps": true,
    "step": "10"
  },
  "icon": "pause"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "min": "number", // Minimal-Wert
  "max": "number", // Maximal-Wert
  "snaps": "boolean", // Zeigt Raster für Werteauswahl an
  "step": "decimal", // Größe der Stufe zwischen zwei Werten (Dezimalwert erlaubt)
  "showValue": "boolean", // Zeigt auf der rechten Seite den aktuell ausgewählten Wert nochmals an
  "leftLabel": "string", // Label links
  "leftIcon": "string", // Icon links
  "rightLabel": "string", // Label rechts
  "rightIcon": "string", // Icon rechts
  "pin": "boolean", // Zeigt einen Pin zur Werteauswahl an
  "dual": "boolean", // Ermöglicht die Auswahl eines Intervalls (Wert wird mit {lower: 1, upper: 5} im Node gespeichert)
  "eval": "string" // Javascript-Inject zur dynamischen Berechnung der Parameter, Signatur: (node form) => return {min: 1, max: 10, step: 1}
}
```

</details>

## Richtext

**Beschreibung**: Erzeugt ein mehrzeiliges Texteingabefeld.

**type**: richtext

**Beispiel:**

```json
{
  "key": "description",
  "type": "richtext",
  "label": "Beschreibung"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "placeholder": "string", // Platzhalter-Text
  "theme": "string", // Alternatives Design des Texteingabefelds, 'bubble': Editor-Einstellungen werden über markierten Text angezeigt
  "modules": "json" // Angepasstes Kontext-Menü für Eingabefeld, z.B.: {'toolbar': [[ "bold", "italic"], ["blockquote"]]} (vgl. https://quilljs.com/guides/how-to-customize-quill/)
}
```

</details>

## Search

**Beschreibung**: Auswahl eines Referenz-Objekts mittels durchsuchbarer, dynamischer Liste.

**type**: search

**Beispiel:**

```json
{
  "type": "search",
  "label": "Rezept",
  "key": "recipe",
  "config": {
    "query": {
      "sid": "recipeScheme"
    },
    "collection": "nodes",
    "sort": { "name": 1 },
    "mapping": {
      "name": "name",
      "image": "image",
      "_id": "_id"
    }
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "query": {
    "[key]": "[value]" // MongoDB-Query; MongoDB-Parameter mit $ durch @ ersetzen, deep-Values mit ':' (z.B. 'sys:cAt'), dynamische Parameter:
    // #NODE: Parameter mit Node-Eigenschaft (z.B. { #NODE: pid }),
    // #USER: Parameter mit User-Eigenschaft (z.B. { #USER: _id }),
    // #VALUE: Form mit Eigenschaft ({ #VALUE: true }),
  },
  "sort": { "[key]": "number" }, // Sortierreihenfolge der Suchergebnisse (key: property, number: 1 oder -1
  "mapping": { "[key]": "[nodeKey]" }, // Properties [nodeKey] des ausgewählten Nodes werden auf [key] gemappt
  "collection": "string", // Optional: Wenn die MongoDB-Collection von 'nodes' abweicht
  "advanced": [
    // Generiert eine oder mehrere dynamische Auswahllisten
    {
      "label": "string", // Überschrift der Auswahl-Liste
      "onFocus": "boolean", // Suche wird bereits bei Fokussierung des Suchfelds gestartet
      "query": { "[key]": "[value]" }, // Option 1: JSON-basierte MongoDB-Query
      "query": "string" // Option 2: Javascript-Injection, Signatur: (node, form) => { return query }
    }
  ]
}
```

</details>

## Switch

**Beschreibung**: Tab-Ansicht zum Schalten zwischen verschiedenen Modi.

**type**: switch

**Beispiel:**

```json
{
  "type": "switch",
  "config": {
    "options": [
      {
        "mode": "view",
        "label": "Übersicht"
      },
      {
        "mode": "meta",
        "label": "Info"
      },
      {
        "mode": "feedback",
        "label": "Feedback"
      }
    ]
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "options": [
    // Tabs;
    {
      "label": "string", // Label
      "mode": "string" // Bei Klick auf diesen Tab wechselt der Node in einen Mode
    }
  ],
  "steps": "boolean", // Anstatt Tabs werden Pfeile angezeigt
  "sticky": "boolean", // Beim Herunterscrollen bleiben die Tabs am obenen Rand sichtbar
  "threshold": "number", // Sticky-Tabs beginnen nach 'number' Pixeln gescrollt
  "minWidthForLabels": "number", // Minimal-Größe für Labels
  "key": "string" // Node-Property für Initialisierung der Auswahl
}
```

</details>

## Tags

**Beschreibung**: Auswahl von mehreren Tags entlang einer Baumstruktur

**type**: tags

**Beispiel:**

```json
{
  "key": "number",
  "label": "Personenzahl",
  "icon": "fa:users",
  "type": "tags",
  "config": {
    "rootNode": "tPwbaeyjpyuT3xdgy"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "rootNode": "string", // ID des Nodes als Wurzel des Tagbaums (entlang der Children)
  "noDivider": "boolean", // Zwischen den Tags wird kein Divider angezeigt
  "headerColor": "string" // Farbe des Headers
}
```

</details>

## Textarea

**Beschreibung**: Erzeugt ein mehrzeiliges Texteingabefeld mit Formatierungsoptionen.

**type**: textarea

**Beispiel:**

```json
{
  "type": "textarea",
  "label": "Beschreibung",
  "key": "description",
  "icon": "text"
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "autoGrow": "boolean", // Passt die Größe des Eingabefelds der Text-Länge an
  "rows": "number", // Anzahl der Reihen
  "placeholder": "string" // Platzhalter-Text
}
```

</details>

## Toggle

**Beschreibung**: Erzeugt einen Auswahl-Slider mit dem Status ''Aktiv/Inaktiv''.

**type**: toggle

**Beispiel:**

```json
{
  "type": "toggle",
  "label": "Toggle",
  "key": "toggle",
  "icon": "checkmark-circle-outline"
}
```

# Spezielle Komponenten

## Atom (deprecated)

**Beschreibung**: Nutzer-Punkte für Node

**type**: atom

**Beispiel:**

```json
{
  "key": "points",
  "type": "atom",
  "label": "Bugs",
  "icon": "megaphone"
}
```

## Chess

**Beschreibung**: Schachbrett mit Spielmöglichkeit.

**type**: chess

**Beispiel:**

```json
{
  "key": "chess",
  "type": "chess",
  "label": "Chess",
  "icon": "ribbon"
}
```

## Code

**Beschreibung**: Eingabefeld mit Syntax-Highlighting für Quellcode

**type**: code

**Beispiel:**

```json
{
  "key": "css",
  "label": "CSS",
  "type": "code",
  "config": {
    "format": "json"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "format": "string" // 'json': JSON-Syntax, 'javascript': Javascript-Syntax
}
```

</details>

## Map

**Beschreibung**: Kartenansicht für Geo-Location

**type**: map

**Beispiel:**

```json
{
  "type": "map",
  "label": "Map",
  "key": "map",
  "icon": "map"
}
```

## QR-Code

**Beschreibung**: Zeigt den String der Node-Eigenschaft als QR-Code an

**type**: qrcode

**Beispiel:**

```json
{
  "key": "qrcode",
  "type": "qrcode",
  "label": "QR-Code",
  "icon": "qr-scanner",
  "config": {
    "size": "160",
    "padding": "20"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "size": "number", // Größe des QR-Codes in Pixel
  "padding": "number" // Padding um QR-Code in Pixel
}
```

</details>

## Relation

**Beschreibung**: Referenzierung auf einen anderen Node

**type**: relation

**Beispiel:**

```json
{
  "label": "Scheme",
  "key": "sid",
  "type": "relation",
  "config": {
    "collection": "schemes",
    "key": "name",
    "selector": "arg4jj5sgs4i5s"
  }
}
```

<details>
  <summary>Config-Spezifikation</summary>

```json
{
  "selector": "string", // Id des Nodes, der eine Aggregation zur Auswahl der Referenz bereitstellt
  "key": "string", // Alternativer Key für Name der Referenz (default: 'name')
  "collection": "string" // Alternative monboDB-Collection (default: 'nodes')
}
```

</details>

Als Nächstes: [Schemes](schemes.md)
