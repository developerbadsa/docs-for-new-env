# Action Config
# TODO!
```json
{
  "type": "string", //Art der Aktion (Werte: "createNode", "runAction", "changeMode", "openModal", "updateFrom", "login", "callMethod")
  "pre": [
    // Parameter-Mapping vor einer Aktion:
    {
      "key": "string", //key des Übergabeparameters
      "type": "string", //Mögliche Werte-Typen:
      // "node":      Eigenschaft 'config' von aktuellem Node
      // "formValue": Eigenschaft 'config' von aktuellem Formular
      // "formKey":   Aktueller Form-Key
      // "constant":  Wert 'config' als Konstante
      // "userId":    Id des aktuellen Users
      // "selector":  Aufruf eines Modals mit 'config' als Id des Nodes zu Selektion
      "config": "string" //Jeweilige Parameterübergabe (vgl. "type")
    }
  ],
  "post": [
    //Nach Ausführung der Action:
    {
      "type": "string", // Mögliche Aktionen:
      // (Rückgabe-Objekt von Action dient als 'result'-Parameter)
      // "closeModal":  // Schließt das aktuelle Modal-Fenster
      // "navigate":    // Navigiert zu Node mit result._id
      // "updateForm":  // Formular wird mit 'result' gepatcht
      // "callMethod":  // Ruft eine Server-Method mit 'result' auf (Rückgabe-Objekt überschreibt 'result')
      // "downloadFile":// Gibt 'result' als Datei aus
      // "sendToast":   // Sendet 'result' als Toast
      // "openModal":   // Öffnet einen Modal mit 'result' als Node
      // "changeMode":  // Ändert mode des aktuellen Nodes
      "mode": "string", // Ziel-Mode (type = 'openModal'|'changeMode')
      "template": "string", // Message-Template (wenn type = 'sendToast'; {{key}}-Templating aus 'result'-keys möglich)
      "filename": "string" // Dateiname für Download (wenn type = 'downloadFile'; default: file.json)
    }
  ],
  "id": "string", // Id der auszuführenden Action (bei "action = runAction")
  "eval": "string" // 
}
```
