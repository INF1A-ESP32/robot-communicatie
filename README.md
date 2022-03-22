# Websocket communicatie tussen de server en de robot
In deze git repo staat de communicatie bescheven tussen de server en de robot. 

Om er voor te zorgen dat de server weet welke robots er verbonden zijn word er elke 5 seconden een ping naar de robot toe gestuurd. Dit zal automatisch gebeuren en de robot zal automatisch pong terug sturen. Mocht een robot niet binnen 5 seconden reageren word de verbinding verbroken.


## Authenentication
De robot zal een autenticatie verzoek sturen naar de server op het moment van connecten. Het `id` veld dient het mac-adress van de robot te worden ingevuld
```json
{
     "action": "login",
     "id": "xx:xx:xx:xx:xx:xx"
} 
```
Op een ongeldige login zal de server het volgende terugsturen
```json
{
    "loggedin": false
}
```
Op een geldige login zal de server het volgende terugsturen
```json
{
    "loggedin": true
}
```

## Voorbereiden spel
Wanneer er op de interface op een knop word gedruikt om het spel te starten zal de server het volgende naar de robot sturen. Hierbij word als voorbeeld het spel `maze` gekozen. Door deze actie kan de robot zich voorbereiden op het spel
```json
{
    "action": "prepare",
    "game": "maze"
}
```
Wanneer de robot klaar is om het spel te gaan spelen zal deze het volgende terugsturen
```json
{
    "status": true,
    "game": "maze"
}
```
Als er om wat voor een reden het spel niet gestart kan worden, stuurt de robot het volgende terug en speeld deze robot niet mee met het spel
```json
{
    "status": false,
    "game": "maze"
}
```

## Spel starten
Om het spel te starten zal de server het volgende signaal sturen naar de robot
```json
{
    "action": "start",
    "game": "maze"
}
```

## Spel stopen
Als het spel voorbij is of het spel moet gestopt worden zal de interface het volgende naar de robots sturen
```json
{
    "action": "ended",
    "game": "maze"
}
```
Dit commando zorgt er voor dat alle robots ***stoppen***.

## Errors
Wanneer de robot tegen een fout aanloopt kan deze verschillende errors teruggeven naar de server

### Spel niet gevonden
```json
{
    "error": "GAME_NOT_FOUND"
}
```

### Spel kon niet worden voorbereid
```json
{
    "error": "GAME_NOT_PREPARED"
}
```

### Het is niet mogelijk om de finish lijn te halen
```json
{
    "error": "UNABLE_TO_FINISH"
}
```

### Wanneer de robot nog bezig is met een spel
```json
{
    "error": "ALREADY_PLAYING_GAME"
}
```

### Wanneer er ongeldige json verstuurd word
Deze error kan van de robot naar de server worden verstuurd en andersom

```json
{
    "error": "INVALID_JSON"
}
```

## Statistieken
De robot zal elke 5 seconden statistieken naar de server sturen met de volgende gegevens

### Verschillende status meldingen
```
preparing       - De robot is nog niet klaar met opstarten
preparing_game  - De robot is bezig met het voorbereiden van een spel
ready           - De robot is klaar om een verzoek te ontvangen/spel starten/spel voorbereiden
in_game         - De robot is bezig met een spel
finished        - De robot is klaar met het spel en bij het einde aangekomen
```
### isDriving
isDriving kan de volgende waardes hebben `true` of `false`

### Acceleration
acceleration heeft een heel getal met de waarde van de acceleratie
```json
{
    "status": "in_game",
    "isDriving": false,
    "acceleration": 22 
}
```