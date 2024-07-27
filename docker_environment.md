# Lokale Quandes Ui-Umgebung

Mit der lokalen Quandes Ui-Umgebung ist es möglich, eine Applikation lokal auf dem eigenen Rechner zu entwickeln. Zu diesem Zweck muss der Zugriff auf das Quandes-Docker-Repository eingerichtet werden. 
## Generieren eines neuen Tokens

+ Auf code.quandes.com einloggen
+ Einen neuen Token erstellen: Nutzermenü > Einstellungen > Zugangs-Token 
  + Name: Beliebig
  + Scopes: api, read_registry
  + Auf 'Create personal access token' klicken
  + Zugangstoken kopieren und idealerweise in Keepass zwischenspeichern

>*Hinweis für Windows-Nutzer:* Im Folgenden jeweils das initale Wort "sudo" weglassen.

## Bei Docker-Repository von Quandes Code anmelden

```bash
sudo docker login code.quandes.com:50670
```

> Zur Anmeldung den Gitlab-Nutzernamen (@...) und den Token als Passwort verwenden.

## Docker-Image von Quandes UI herunterladen

```bash
sudo docker pull code.quandes.com:50670/matthias/quandes-ui-local:latest
```

## Starten des Docker-Images


```bash
sudo docker run -p 8082:8082 -p 3000:8081 code.quandes.com:50670/matthias/quandes-ui-local:latest
```
Unter http://localhost:8082/ sollte nun im Browser eine leere Seite aufrufbar sein. Zu diesem Zeitpunkt ist das Quandes Ui-Framework noch nicht mit einer App initialisiert. Dies passiert im nächsten Schritt.

# Einspielen der App-Konfiguration
Mit dem folgenden Befehl wird eine Standard-App für die lokale Umgebung eingerichtet. Hierzu wird das [Sandbox-Projekt](setup.md#sandbox) benötigt.

* Eine weitere Konsole öffnen
* In den Ordner des Sandbox-Projekts navigieren
* Den folgenden curl-Befehl ausführen:

```bash
curl -X POST localhost:3000/methods/updateAppByJson -H "Content-Type: application/json" -d @app.config.json
```

Dadurch wird die Initale App in die lokale Quandes-UI-Instanz geladen. Im Browser sollte nun unter  http://localhost:8082/ die App aufrufbar sein. Es erfolgt eine Weiterleitung zur Login-Seite. Im nächsten Arbeitsschritt wird der Admin-Account initialisiert.

## Erstellen eines neuen Nutzers

```bash
curl -X POST localhost:3000/methods/generateUser -o user.json -d email=admin@quandes.com -d id=1 -d username=admin
```

Mit diesem Befehl wird die Datei **user.json** erstellt. Sie enthält die Login-Informationen, mit der man sich anschließend in der lokalen App einloggen kann.

# Arbeiten mit der Docker-Konsole
An dieser Stelle werden ein paar nützliche Konsolen-Befehle zur Arbeit mit dem Quandes Ui-Frameworks gegeben.
## Konsolen-Login in den Docker-Container
Für die Quandes-Ui wird im Hintergrund mittels Docker eine eigene Linux-Umgebung gestartet. Den Zugriff auf die Bash-Konsole erhält man mit diesem Befehl:

**Ubuntu:**
```bash
sudo docker exec -it $(sudo docker ps -ql) /bin/bash
```

**Windows:**
```bash
docker ps -ql
docker exec -it "ID des Docker Containers" /bin/bash
```
## Login zur Mongo-DB
Alle Daten des Quandes Ui-Frameworks werden in einer [MongoDB](https://www.mongodb.com/de) gespeichert. Der Login zu der lokalen MongoDB erfolgt in der Kommandozeile des Docker-Containers durch den folgenden Befehl:

```bash
mongo
```

### Anzeige der aktuellen Datenbanken
```
show dbs
```

### Auswahl einer Datenbank
```
use test
```

### Anzeige der Collections
```
show collections
```

### Anzeige der App Version in der Datenbank

In der Mongo-DB der Docker-Konsole folgendes ausführen:
```bash
db.apps.findOne().version
```
Als Ergebnis sollte die Version der eingespielten App ausgegeben werden.

>*Hinweis:* Derzeit ist nur eine App eingerichtet. Deshalb findet findOne ohne Parameter-Angabe die richtige App. 

Als Nächstes: [App-Konfiguration](app.md)