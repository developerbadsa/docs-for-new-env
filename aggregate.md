# Aggregate

## Aggregate-Spezifikation

```json
{
  "type": "string", // Typ der Aggregation
  // Werte:
  // 'cards': Liste von Karten
  // 'grid': Kachel-Ansicht
  // 'list': Listenansicht
  // 'raw': Darstellung untereinander (ohne zusätzliche Formatierung)
  "query": {
    "[key]": "[value]" // MongoDB-Query; MongoDB-Parameter mit $ durch @ ersetzen, deep-Values mit ':' (z.B. 'sys:cAt'), dynamische Parameter: 
    // #NODE: Parameter mit Node-Eigenschaft (z.B. { #NODE: pid }), 
    // #USER: Parameter mit User-Eigenschaft (z.B. { #USER: _id })
  },
  "onClick": "string | json", // Aktion bei Klick auf Aggregations-Element:
  // viewModal: Öffnet ein Modal mit dem Node im Mode 'view'
  // editModal: Öffnet ein Modal mit dem Node im Mode 'edit'
  // navigateTo: Navigiert zu dem Node
  // { 'editModal':'schemeId' }: Öffnet den Modal im Mode 'edit' unter Verwendung von 'schemeId'
  "options": {
    "sort": { "[key]": "1|-1" }, // Sortiert Ergebnis der Aggregation mit [key] absteigend (1) oder aufsteigend (-1); Deep-Keys mit ':' trennbar
    "paging": {
      "type": "string", // infiniteScroll: Ermöglicht kontinuierliches Scrollen der Ergebnisse, weitere Elemente werden dynamisch nachgeladen
      "size": "number" // Anzahl der Einträge je Segment
    },
    "limit": "number" // limitiert Anzahl der Einträge des Suchergebnisses
  }, //
  "config": {
    "autoScroll": "boolean", // Springt bei Darstellung ans Ende der Ergebnisliste (z.B. bei Chat)
    "emptyText": "string" // Text wird angezeigt, wenn keine Elemente verfügbar sind.
  }, //
  "itemNameKey": "string", // Diese Node-Property wird bei der Darstellung der Liste anstatt 'name' verwendet
  "collection": "string", // Die für das Schema verwendete Datenbank
  "filters": [
    // Filter-Definition
    {
      "bid": "string", // ID des zugehörigen Blocks zwecks Übernahme einiger Parameter
      "alias": "letter", // Buchstabe für die Filter-Referenz in der Url (muss sich von den anderen Aliases unterscheiden)
      "color": "string", // Farbe des Filter-Blocks (z.B. 'toolbar'),
      "colorLight": "string", // Hover-Farbe des Filter-Blocks (z.B. 'toolbar-light')
      "key": "string", // Key der Node-Eigenschaft, zu der gefiltert wird.
      "searchThreshold": "number", // Grenzwert der Filter-Optionen, ab dem die Suche angegeben wird
      "match": "user", // Match wird gegen User durchgeführt
      "lazyLoading": "boolean", // Filter wird erst nach Aufklappen geladen
      "visibilityRange": {
        // Anzahl der Filter-Optionen, zwischen denen der Filter angezeigt wird
        "min": "number",
        "max": "number"
      },
      "icon": "string", // Icon des Filter-Blocks
      "icon-collapsed": "string", // Icon des eingeklappten Filter-Blocks
      "collapsed": "boolean", // Filter-Block aus- oder eingeklappt
      "type": "string", // Filter-Typ, Mögliche Werte:
      // 'tags' (default): Filter als Tag-Baum
      // 'list': Vordefinierte Liste
      // 'toggle': Toggle-Filter
      "options": [
        // Auswahl-Optionen für type = 'list'
        "string", // Option 1) einfacher 'string'
        {
          // Option 2) Option als 'key'/'label'
          "key": "string", // Filter-Wert
          "label": "string" // Label
        }
      ],
      "mapping": {
        // Optionale Transformation der Listen-Auswahl
        "remove": "string", // Löscht 'string' aus dem String der ausgewählten Option
        "replace": ["von", "zu"] // Ersetzt 'von' im String der ausgewählten Option durch 'zu'
      },
      "match": "string",
      // 'intersection': Node ist ein Treffer des Filters, wenn das numerische Intervall von Node und Filter sich überschneiden
      // 'search': Filter wird anhand des Suchfelds gesetzt
      "keys": ["string"] // Liste von Property-Keys, auf denen die Suche angewand wird (für match = 'search')
    }
  ],
  "search": {
    "keys": ["string"], // Liste von Property-Keys, auf denen die Suche angewand wird
    "noPopup": "boolean" // Filter-Popup wird ausgeschaltet
  },
  "sorts": [
    {
      "icon": "string", // Icon
      "sort": "string", // Key der Node-Eigenschaft für die Suche (kann mit ':' auf Untereigenschaften verweisen)
      "ascending": "boolean", // Auf- oder absteigende Reihenfolge
      "label": "string" // Label
    }
  ],
  "forcedBlock": "string", // Id des Blocks zur Anzeige in Aggregation
  "forcedScheme": "string", // Id des Schemes zur Anzeige in Aggregation (z.B. für angepasste OnInit-Transformation)
  "gridSize": {
    // Legt fest, wie viele Kacheln bei Grid angezeigt werden (Teiler von 12, z.B. 6 bei 2 Spalten)
    "default": "number", // Default-Ansicht (Default: 4)
    "sm": "number", // Bei kleinen Displays
    "md": "number", // Bei mittelgroßen Displays
    "lg": "number", // Bei großen Displays
    "xl": "number" // Bei extragroßen Displays
  }
}
```
Als Nächstes: [Actions](actions.md)