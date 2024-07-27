# Quandes Robot

Mit dem Quandes Robot lassen sich repetitive Web-interaktionen automatisieren. Hierzu wird in einer Sequenz eine Abfolge von Interkationen definiert, die das System dann automatisch ausführt. Eine Sequenz hat das folgende Format:

```json
{
  "input": ["string"], // Liste von Input-Parametern  (Diese Variablen werden bei einem API-Aufruf als Übergabeparameter erwartet.)
  "output": ["string"], // Liste von Output-Parametern (Diese Variablen werden bei einem API-Aufruf als Result-JSON zurückgegeben.)
  "sequence": ["Action"] // Liste von Actions (s.u.)
}
```

# Actions

Eine Aktion hat folgendes Format:

```json
{
  "type": "string", // Typ der Aktion
  "mode": "mode", // Aktion wird nur nicht ausgeführt, wenn der als Parameter übergebene Mode des Aufrufs ungleich 'mode' ist
  "[PARAM]": "any" // Typ-spezifische Parameter
}
```

## Go To

**type**: goto

**Beschreibung:** Navigiert Browser zur angegebenen Url.

**Beispiel:**

```json
{
  "type": "goto",
  "value": "https://google.de" // URL
}
```

## Click

**type**: click

**Beschreibung:** Führt eine Klickt-Aktion auf ein DOM-Objekt aus.

**Beispiel:**

```json
{
  "type": "click",
  "selector": "#ButtonEdit" // Selektor des zu klickenden DOM-Objekts
}
```

## Enter Text

**type**: enterText

**Beschreibung:** Gibt in ein Eingabe-Feld ein Text ein.

**Beispiel:**

```json
{
  "type": "enterText",
  "selector": ".native-input",
  "value": "Text von ${textInput}" // Einzugebender Text (Templating mit ${var} möglich)
}
```

## Press

**type**: press

**Beschreibung:** Führt eine Tastatur-Eingabe aus.

**Beispiel:**

```json
{ "type": "press", "value": "Escape" }
```

## Wait For

**type**: waitFor

**Beschreibung:** Wartet für eine fest definierte Zeit.

**Beispiel:**

```json
{
  "type": "waitFor",
  "value": "number" // Anzahl von Millisekunden, die der Prozess wartet.
}
```

## Wait For Selector

**type**: waitForSelector

**Beschreibung:** Setzt die Sequenzausführung aus, bis ein spezifisches
DOM-Objekt verfügbar ist.
**Beispiel:**

```json
{
  "type": "waitForSelector",
  "selector": "#ButtonEdit", // Selektor des erwarteten DOM-Objekts
  "value": "number" // Anzahl von Millisekunden, die der Prozess maximal wartet.
}
```

## Wait For Navigation

**type**: waitForNavigation

**Beschreibung:** Wartet, bis die Webseite die Navigationselemente darstellt.
**Beispiel:**

```json
{ "type": "waitForNavigation" }
```

## Wait For Function

**type**: waitForNavigation

**Beschreibung:** Wartet, bis die definierte Funktion einen positiven Wert zurückgibt.

**Beispiel:**

```json
{
  "type": "waitForFunction",
  "selector": "document.querySelector(\"#Tweetstorm-dialog\").style.display == 'none'", // Selektor-Funktion
  "timeout": 18000 // Maximales Timeout
}
```

## Log

**type**: log

**Beschreibung:** Gibt auf der Konsole einen Text aus

**Beispiel:**

```json
{
  "type": "log",
  "value": "Renew tokens for environment: ${environment}..." // Auszugebender Text (Templating mit ${var} möglich)
}
```

## Request

**type**: request

**Beschreibung:** Führt einen Url-Aufruf durch

**Beispiel:**

```json
{
  "type": "request",
  "options": "options", // Optionales Parameter-Objekt (z.B. {method: 'POST', form: {...}}, vgl. https://github.com/request/request)
  "value": "${server_url}/action/initialTestCleanUp", //Aufzurufende Url (Templating mit ${var} möglich)
  "variable": "output" // Rückgabewert
}
```

## Set Variable

**type**: setVariable

**Beschreibung:** Setzt eine Variable.

**Beispiel:**

```json
{
  "type": "setVariable",
  "variable": "client_url", // Output
  "mapping": "(vars)=>{return (vars && vars.client_url) || 'http://localhost:8100'}" // Javascript-Inject als Mappingfunktion, Signatur: (vars) => { return 'any'}
}
```

## Get Attribute

**type**: getAttribute

**Beschreibung:** Ließt ein Attribut eines DOM-Elements aus.

**Beispiel:**

```json
{
  "type": "getAttribute",
  "selector": [".js-photo-page-image-img", "src"], // 1.Parameter: Selektor, 2. Attribut
  "variable": "imageUrl" // Zielvariable
}
```

## Get Text

**type**: getText

**Beschreibung:** Ließt den Text-Inhalt eines DOM-Elements aus.

**Beispiel:**

```json
{
  "type": "getText",
  "selector": ".js-photo-page-mini-profile-full-name", // Selektor
  "variable": "author" // Zielvariable
}
```

## Get List

**type**: getList

**Beschreibung:** Ließt alle Elemente aus, die dem Selektor entsprechen.

**Beispiel:**

```json
{
  "type": "getList",
  "selector": "evo-node-page div.scroll-content evo-aggregate ion-list > ion-item-sliding", // Selektor für mehrere DOM-Elemente
  "variable": "blogEntries" // Zielvariable für Liste
}
```

## Get URL

**type**: getUrl

**Beschreibung:** Ließt die aktuelle Browser-URL aus.

**Beispiel:**

```json
{ "type": "getUrl", "output": "url" }
```

# Datei-Behandlung

## Upload File

**type**: uploadFile

**Beschreibung:** Lädt eine Datei in ein File-Input-Formular hoch.

**Beispiel:**

```json
{
  "type": "uploadFile",
  "selector": "div#qbfilebp > input", // Selektor des DOM-File-Input-Feldes
  "variable": "image" // Dateiname der hochzuladenden Datei
}
```

## Download File

**type**: downloadFile

**Beschreibung:** Lädt eine Datei von einer URL herunter.

**Beispiel:**

```json
{
  "type": "downloadFile",
  "variable": "image", // Dateiname zum Speichern der Datei
  "value": "${url}" // URL der zu speichernden Datei. (Templating mit ${var} möglich)
}
```


# Prozessfluss

## Condition

**type**: condition

**Beschreibung:** Führt eine Untersequenz abhängig von einer Bedingung aus.

**Beispiel:**

```json
{
  "type": "condition",
  "value": "(variables)=>{return !!variables[variables.tokenName]}", // Javascript-Inject zwecks Test, Signatur: (var) => {return true}
  "sequence": ["SEQUENCE"], // Sequenz wird bei positiven Test ausgeführt
  "fallback": ["SEQUENCE"], // Sequenz wird bei negativen Test ausgeführt
  "fallbackOnException": true // Führt die Fallback-Sequenz auch aus, wenn die Sequenz in eine Ausnahme läuft
}
```

## Loop

**type**: loop

**Beschreibung:** Führt eine Untersequenz für jedes Element einer List aus.

**Beispiel:**

```json
{
  "type": "loop",
  "variable": "platforms", // Liste an Elementen, über die Iteriert wird
  "sequence": ["SEQUENCE"] // Sequenz wird für jedes Element der Variable ausgeführt (Zugriff auf aktuelles Element über 'iterator'-Variable)
}
```

## Load

**type**: load

**Beschreibung:** Lädt eine Variablen-Satz aus dem lokalen Dateisystem.

**Beispiel:**

```json
{
  "type": "load",
  "value": "env-${environment}", // Dateiname (Templating mit ${var} möglich)
  "config": ["${tokenName}"] // Einschränkung auf Liste von Variablen
}
```

## Save

**type**: save

**Beschreibung:** Speichert einen Variablen-Satz in das lokale Dateisystem.

**Beispiel:**

```json
{
  "type": "save",
  "value": "tokens", // Dateiname (Templating mit ${var} möglich)
  "config": ["${tokenName}"] // Einschränkung auf Liste von Variablen
}
```

## Run Sequence

**type**: runSequence

**Beschreibung:** Führt eine weitere Sequenz aus. Die Variablen werden übergeben.

**Beispiel:**

```json
{
  "type": "runSequence",
  "sequence": "mysuricate-select-game" // Name der Sequenz
}
```

## Run Parallel

**type**: runParallel

**Beschreibung:** Führt eine Liste von Sequenzen parallel aus.

**Beispiel:**

```json
{
  "type": "runParallel",
  "sequences": ["parallelsequence1", "parallelsequence2"] // Liste an auszuführenden Sequenzen
}
```

## Sys Enter Key

**type**: sysEnterKey

**Beschreibung:** Führt einen Tastatur-Befehl auf Systemebene aus.

**Beispiel:**

```json
{ "type": "sysEnterKey", "value": "escape" }
```

## Close

**type**: close

**Beschreibung:** Schließt das aktuell genutzte Fenster im Robot.

**Beispiel:**

```json
{ "type": "close" }
```

## Exit

**type**: exit

**Beschreibung:** Beendet den Robot.

**Beispiel:**

```json
{ "type": "exit" }
```


# Visuelle Effekte

## Set Border

**type**: setBorder

**Beschreibung:** Setzt einen Rahmen um ein DOM-Element. (Für Test oder Nutzerinteraktion hilfreich)

**Beispiel:**

```json
{
  "type": "setBorder",
  "selector": "#Tweetstorm-tweet-box-0 button.SendTweetsButton", // Selektor
  "value": "4px solid red" // CSS-Eigenschaft für Border
}
```

## Set Style

**type**: setStyle

**Beschreibung:** Setzt CSS-Styling für ein DOM-Element. (Für Test oder Nutzerinteraktion hilfreich)

**Beispiel:**

```json
{
  "type": "setStyle",
  "selector": "ion-fab-button", // Selektor
  "value": {
    // CSS
    "border": "4px solid #FA664E",
    "border-radius": "20px"
  }
}
```

## Set View Port

**type**: setViewPort

**Beschreibung:** Set die Größe des Browserfensters.

**Beispiel:**

```json
{
  "type": "setViewPort",
  "value": {
    "width": 1024, // Breite des Fensters
    "height": 1024 // Höhe des Fensters
  }
}
```

## Scroll

**type**: scroll

**Beschreibung:** Führt eine Scroll-Interaktion auf einem DOM-Element aus.

**Beispiel:**

```json
{
  "type": "scroll",
  "value": -1000, // Wert in Pixel, um die gescrollt wird
  "selector": "ion-modal ion-content", // Selektor
  "params": {
    "shadow": ".inner-scroll" // Spezielle Option zur Auswahl eines DOM-Elements in Shadow-Umgebung
  }
}
```

## Set Mobile Agent

**type**: setMobileAgent

**Beschreibung:** Setzt die Browsereinstellung auf mobile Agent.

**Beispiel:**

```json
{ "type": "setMobileAgent" }
```


# Recording

Es ist möglich, die automatisierte Interation per Screen-Recoding aufzunehmen. Dazu legt man mit 'recordStart' den Zeitpunkt fest, an dem die Aufnahme gestartet werden soll, sowie mit 'recordEnd' den Endpunkt, zu dem die Aufnahme gestoppt wird.

## Record Start

**type**: recordStart

**Beschreibung:** Startet das Screen-Recording.

**Beispiel:**

```json
{
  "type": "recordStart",
  "params": {
    "seconds": 40, // Maximale Laufzeit des Videos
    "fps": "20" // Frames per Second
  }
}
```

## Record End

**type**: recordEnd

**Beschreibung:** Beendet das Screen-Recording und speichert das Video in einer Datei ab.

**Beispiel:**

```json
{
  "mode": "record",
  "type": "recordEnd",
  "variable": "animatedGif", // Dateiname für die generierte Video-Datei
  "params": {
    "fps": 16 // Frames per Second
  }
}
```

# Tests

Tests können verwendet werden, um mittels des Robots spezielle Zustände einer Web-Applikation zu testen. Im Fehlerfall liefert die Sequenz einen entsprechenden Test-Fehler-Report. So lassen sich beliebig komplexe Regressionstest-Szenarien definieren.

## Test Element

**type**: testElement

**Beschreibung:** Testet, ob ein DOM-Element existiert.

**Beispiel:**

```json
{
  "type": "testElement",
  "selector": "#loginForm > div > div:nth-child(1) > div > label > input", // Selektor
  "variable": "loginScreen" // Variable zum Speichern des Ergebnisses
}
```

## Test Text

**type**: testText

**Beschreibung:** Testet die Übereinstimmung von DOM-Inhalten mit einem Text.

**Beispiel:**

```json
{
  "type": "testText",
  "selector": "#goToAccount", // Selektor des zu testenden DOM-Inhalts
  "value": "Max Mustermann" // Erwarteter Text
}
```

## Test Equal

**type**: testEqual

**Beschreibung:** Testet die Übereinstimmung zweier Objekte.

**Beispiel:**

```json
{
  "type": "testEqual",
  "variable": "resultObject", // Zu testende Variable
  "value": {
    "error": "insufficient rights"
  } // Erwarteter Wert
}
```

## Test To Be Defined

**type**: testToBeDefined

**Beschreibung:** Testet, ob eine Variable definiert ist.

**Beispiel:**

```json
{
  "type": "testToBeDefined",
  "variable": "default.result.id" // Zu testende Variable
}
```
