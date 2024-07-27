# Einrichtung der Arbeitsumgebung
## Quandes Code Account erstellen
Für die Nutzung diverser Applikationen von Quandes ist es notwendig, vorab eine Quandes ID zu erstellen.

1. Die Gitlab-Umgebung code.quandes.com aufrufen 
2. Eine neue Quandes-ID erstellen.

### 2-Faktor-Authentifizierung einrichten
Um eine höhere Sicherheit gewährleisten zu können, ist es notwendig ein zweiter Faktor zur Anmeldung mit dem Google Authenticator einzurichten. 

1. Google-Auth-App aus App-Store laden: [Google](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=de&gl=US) / [Apple](https://apps.apple.com/de/app/google-authenticator/id388497605)

2. Authentifizierung bei [code.quandes.com](https://code.quandes.com) einrichten: Nutzermenü > Einstellungen > Konto > Two-Factor Authentication

### Zugriffsberechtigungen für Projekt erhalten

Ein Admin schaltet anschließend die Berechtigung für ein Sandbox-Projekt frei.
## Software installieren

### Git

Zwecks professionellem Versionsmanagement insbesondere der Kommunikation mit dem Gitlab-Repository [code.quandes.com](https://code.quandes.com) wird Git verwendet.

**Linux:** Bereits vorinstalliert, **Windows:** [Download](https://gitforwindows.org/)

### Visual Studio Code

Als State-Of-The-Art-Code-Editor mit guter JSON-Unterstützung wie Syntax-Highlighting empfehlen wir den Editor *Visual Studio Code:*
[Download](https://code.visualstudio.com/)

#### VSCode Extensions installieren

- Better Comments
- GitLens — Git supercharged
- Prettier - Code formatter
- Pretty js/json

### Docker

Um das Quandes Ui-Framework in einer virtuellen Umgebung auf dem lokalen Rechner laufen lassen zu können, wird Docker benötigt.

**Installation unter Linux:**

```bash
apt install docker docker-compose
```

**Installation unter Windows:**

[Download](https://www.docker.com/get-started)

### Curl

Curl ist eine Kommandozeilen-basierte Applikation zur Durchführung von Web-Requests. Curl wird zur Interkation mit den Web-Schnittstellen von Quandes Ui und insbesondere zum Einspielen einer neuen App-Konfiguration verwendet.

**Linux:** Bereits vorinstalliert, **Windows:** [Download](https://curl.se/windows/)

### Keepass
Tool zum sicheren Speichern von Passwörtern: [Download](https://keepass.info/).

## Sicherheitshinweis
Für alle bei Quandes verwendeten Passwörter sollten stets autogenerierte Passwörter verwendet werden, die in einer Keepass-Datei auf einem separaten USB-Stick gespeichert werden. 

>Die Keepass-Datei muss mit einem sicheren Master-Passwort versehen werden (> 12 Zeichen / Groß-, Kleinschreibung, Zahl und Sonderzeichen verwenden). Dieser USB-Stick dient als digitaler Schlüssel und darf nicht an Dritte weitergegeben werden.

## SSH-Zugriff einrichten
Der Zugriff auf das Code-Repository code.quandes.com erfolgt über eine sichere Verbindung mittels ssh. Zur Einrichtung muss ein neues SSH-Schlüsselpar erzeugt werden, bei dem der Public Key anschließend im Code-Repository hinterlegt wird.

### SSH-Schlüsselpaar erstellen


>**Sicherheitshinweis:**
Für den private Key im Folgenden ein sicheres Passwort verwenden: > 12 Zeichen / Groß-, Kleinschreibung, Zahl und Sonderzeichen.

**Ubuntu**

Neues Schlüsselpaar generieren:

```bash
ssh-keygen
```

Public Key anzeigen:

```bash
cat .ssh/id_rsa.pub
```

**Windows**

- _Git Gui_ starten
- Help > Generate new Key Pair
- Public Key kopieren: _Copy To Clipboard_
### SSH-Key in Quandes Code eintragen

Abschließend muss der Public Keyim Code-Repository hinterlegt werden.

- In code.quandes.com einloggen
- Public Key eintragen: Nutzermenü > Einstellungen > SSH-Schlüssel > Key
- Schlüssel hinzufügen klicken

##  <a name="sandbox"></a>Sandbox-Projekt klonen
Sind alle Voraussetzungen erfüllt, können nun freigeschaltete Projekte zur Verwendung auf dem lokalen Rechner geklont werden:

```bash
git clone ssh://git@code.quandes.com:50681/[PROJECTGROUP]/[PROJECTNAME].git
```

Für den weiteren Arbeitsschritt wird folgendes Projekt benötigt:

```bash
git clone ssh://git@code.quandes.com:50681/matthias/quandes-ui-local.git
```
Als Nächstes: [Kommunikation](communication.md)