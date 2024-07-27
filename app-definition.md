# Vollständige App-Spezifikation

Eine vollständige App lässt sich wie folgt spezifizieren.

```json
{
  "_id": "string", //ID der App
  "name": "string", //Name der App
  "version": "string", //App-Version wird bei jeder Änderung inkrementiert
  "description": "string", //Beschreibung der App
  "root": "string", //Root-Knoten des Node-Baums
  "default": true, //Falls mehrere Apps eingerichtet sind, kann Start-App mit 'default = true' aktiviert werden, alle anderen erhalten 'default = false'
  "debug": true, //Aktiviert die Logausgabe zur Fehleranalyse in der Konsole des Browsers
  "roles": {
    //Definition der Default-Rollen in der App
    "owners": "string[]",
    "admins": "string[]",
    "editors": "string[]",
    "viewers": "string[]"
  },
  "nodes": "Node[]", //Liste der initialen, fixen Nodes im Node-Baum
  "users": "any[]",
  "blocks": "Block[]", //Vergleiche Block-Sektion
  "schemes": "Scheme[]", //Vergleiche Scheme-Sektion
  "groups": [
    //Gruppen können Rollen zugewiesen werden und erhalten somit weitere Berechtigungen
    {
      "_id": "string", //Id der Gruppe
      "name": "string", //Name der Gruppe
      "app": "string", //Id der zugehörigen App
      "sid": "groupScheme" //Konstante
    }
  ],
  "root": "string", //Root-Node der App
  "rootMass": "string[]", //Veraltet
  "translations": [
    {
      "_id": "string", //Id der Sprache (z.B. de),
      "[key]": "[value]" //übersetzt [key] nach [value] der entsprechenden Sprache
    }
  ],
  "device": {
    "tracking": "boolean" //(de-)aktiviert Tracking des Orts des mobilen Endgeräts
  },
  "menu": [
    {
      "label": "string", //Label des Menü-Eintrags
      "icon": "string", //Icon des Menü-Eintrags
      "mode": "string", //Mode, in dem der Eintrag sichtbar ist
      "condition": "string", //function: (item,node,context)=>{ return boolean } steuert Sichtbarkeit des Menü-Items (item = menu item; node = current node; context = { domain })
      "access": ["string"], //Liste an Rollen des aktuellen Nodes, die Menü-Eintrag sehen können (z.B. owners/admins/editors)
      "showDirectly": "boolean", //Zeigt das Menü-Element direkt in der Menü-Leiste an
      "config": "ActionConfig" //Siehe Spezifikation von ActionConfig unten
    }
  ],
  "debug": true, //Aktiviert die Logausgabe zur Fehleranalyse in der Konsole des Browsers
  "tracking": {
    "persist": "boolean", //Speichert die Nutzeraktivitäten in der Datenbank
    "console": "boolean" //Gibt die Nutzeraktivität in der Konsole aus
  },
  "account": {
    //Account-Einstellungen
    "profileBlock": "string", //Block zur Konfiguration der Parameter in den Account-Einstellungen
    "registrationBlock": "string", //Block zur Konfiguration der Parameter bei Registrierung
    "noPassword": "boolean", //Schaltet zwischen Passwort-Login und Email-Login um
    "codeSentMessage": "string", //Nachricht für den "Code gesendet"-Hinweis bei Email-Login
    "checkboxes": {
      //Diese Checkboxes werden bei der Registrierung angezeigt
      "required": "boolean", //Bestätigung erforderlich?
      "referenceId": "string", //Dient zur Verlinkung auf Node mit weiterführenden Informationen
      "text": "string", //Text der Checkbox (wird als Link gesetzt)
      "version": "string", //Wird bei jeder neuen Version inkrementiert. Dient zur Überprüfung, ob Nutzer bereits aktuelle Version akzeptiert hat.
      "key": "string", //Unter diesem Key wird der Status im Profil gespeichert (true/false, bzw. Version)
      "pre": "string" //Voranlaufender Text der Checkbox (wird nicht als Link gesetzt)
    },
    "config": {
      //Benennung der einzelnen Optionen in den Account-Einstellungen
      "profile": {
        //Profil-Konfiguration
        "buttonLabel": "string", //Label des Buttons
        "description": "string" //Beschreibung
      },
      "security": {
        //Sicherheitseinstellugnen
        "buttonLabel": "string", //Label des Buttons
        "description": "string" //Beschreibung
      },
      "account": {
        //Account-Aktionen
        "buttonLabel": "string", //Label des Buttons
        "description": "string" //Beschreibung
      }
    },
    "termsOfServices": "string", //Node zur Beschreibung der AGBs
    "dataProtection": "string", //Node zur Beschreibung der Datenschutzbestimmungen
    "invitationToken": "boolean", //Aktiviert die Option, Einladungstokens zu versenden
    "invitationMapping": "any", //TODO
    "atomActivationKey": "string", //Veraltet
    "sid": "string", //Schema zur Abbildung von Applikationslogik nach Speichern des Profils
    "loginServices": [
      //Konfiguration der Alternativ-Login-Optionen durch Drittanbieter
      {
        "service": "string", //'google' / 'facebook'
        "authUrl": "string", //Url zur Authentifizierung
        "config": "any", //Default: google: { "secret": "#SECRET", "clientId": "#ID" }, facebook: { "secret": "#SECRET", "appId": "#ID" }
        "mapping": {
          //Mapping der vom Drittanbieter übergebenen Profil-Informationen zu Quandes-Profil
          "[quandesKey]": "[thirdPartyKey]"
        }
      }
    ]
  },
  "email": {
    "siteName": "string", //Name der App
    "from": "string", //Name und Email des Absenders
    "resetPasswort": {
      //Konfiguration der Passwort-Reset-Email
      "subject": "string", //Betreff der Email
      "prefix_text": "string", //Text vor Reset-Link
      "suffix_text": "string" //Text nach Reset-Link
    },
    "verifyEmail": {
      //Konfiguration der Verifizierungs-Email
      "subject": "string", //Betreff der Email
      "prefix_text": "string", //Text vor Verifizierungs-Link
      "suffix_text": "string" //Text nach Verifizierungs-Link
    }
  },
  "volumes": {
    //Definition von Volumes zur Speicherung von Dateien
    "name": "string", //Identifier des Volumes
    "path": "string", //Pfad des Volumes
    "transform": {
      //Optionale Standard-Transformation bei Bild-Volumes
      "format": "string", //Werte: 'jpeg' oder 'png',
      "quality": "number", //jpeg-Qualität
      "width": "number", //Bildbreite,
      "height": "number" //Bildhöhe
    }
  },
  "colors": {
    //Definition der Standard-Farben (Format: rgb(x,y,z) oder Hexacode)
    "primary": "string",
    "secondary": "string",
    "tertiary": "string",
    "success": "string",
    "warning": "string",
    "medium": "string",
    "danger": "string",
    "light": "string",
    "dark": "string",
    "toolbar": "string",
    "toolbarLight": "string"
  },
  "assets": {
    //File-Ids für Standard-Assets
    "logoWhite": "string", //Logo für Ladebildschirm
    "logo": "string", //Logo zur Anzeige in der oberen, linken Ecke
    "banner": "string", //Banner
    "background": "string" //Hintergrundbild
  },
  "pdf": [
    //Konfiguration zur Autogenerierung von PDFs (vgl. https://pdfkit.org/)
    {
      "id": "string", //Id der Konfiguration (Lässt sich über /pdf/{id}/{nodeId} verlinken)
      "elements": [
        //Darstellungselemente im PDF
        {
          "type": "string", //Werte: 'text', 'image', 'line', 'textPos'
          "options": {
            //Konfigurationsoptionen (Hier nur ein Auszug der Konfiguration. Für vollständige Referenz vgl. Config-Json des jeweiligen Elements von https://pdfkit.org/)
            "align": "string", //Werte: 'center', 'left', 'right', 'justify',
            "width": "string", //Breite des Elements
            "continued": "boolean", //Ob Abstand zum nachfolgenden Textelement
            "fit": ["number1", "number2"], //Bild wird proportional skaliert; number1: Breite, number2: Höhe
            "link": "string", //Url zur Verlinkung
            "width": "string" //Breite
          },
          "keys": ["string"], //Liste von Keys der Eigenschaften vom übergebenen Node. Diese können im Text verwendet werden
          "content": "string", //Text-Inhalt (Platzhalter von Keys können mit {{key}} verwendet werden)
          "distance": "string", //Abstand zum vorherlaufenden Elements
          "x": "string", //X-Position bei type = textPos
          "y": "string", //Y-Position bei type = textPos
          "size": "string", //Fontsize bei Text
          "coords": {
            "x1": "number",
            "y1": "number",
            "x2": "number",
            "y2": "number"
          }, //Bei type = 'line': Linie von (x1, y1) zu (x2, y2)
          "color": "string", //Textfarbe bei type = 'text'
          "key": "string", //Key des Bildes bei type = 'image'
          "volumne": "string", //Id des Volumes bei type = 'image'
          "stripHtml": "boolean" //Vorverarbeitung: HTML zu Text bei type = 'text'
        }
      ]
    }
  ],
  "styles": "any", //Globale CSS-Styling-Optionen (z.B. { "card-border-top": "0px" })
  "actions": "Actions", //Spezifikation siehe unter Actions unten
  "footer": {
    "color": "string", // Hintergrundfarbe des Footers
    "main": [
      //Footer-Links in der unteren linken Ecke
      {
        "label": "string", //Label des
        "icon": "string", //Optionales Icon
        "nodeId": "string", //Alternative 1: Id des referenzierten Nodes,
        "link": "string" //Alternative 2: Link zu externer Website
      }
    ],
    "social": [
      //Social-Icons im Footer in der unteren linken Ecke
      {
        "label": "string", //Label des
        "icon": "string", //Optionales Icon
        "nodeId": "string", //Alternative 1: Id des referenzierten Nodes,
        "link": "string" //Alternative 2: Link zu externer Website
      }
    ]
  }
}
```

**Detailierte Spezifikationen**:

- [Action-Config](action-config.md) für Menü-Eintrag

- [Actions](actions.md)
