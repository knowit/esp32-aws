# Hello AWS: Send og motta meldinger i Python

Disse stegene viser deg hvordan du kan få AWS til å lage en bundle med scripts som gjør at du kan abonnere og sende meldinger fra for eksempel din PC eller Raspberry Pi.
Dette eksempelet vil ta utgangspunkt i Python. 

## Gå til ```Connect one device```
1. Åpne [AWS IoT-konsollen](https://eu-west-1.console.aws.amazon.com/iot/home?region=eu-west-1#/home) 
1. Kontroller at korrekt region er satt: ```eu-west-1```
1. Velg ```Connect``` i menyen til venstre
1. Velg ```Connect one device``` i menyen til venstre
1. Under ```Prepare your device``` siden, kjør en ping test mot AWS for å sjekke tilkoblingen din som i instruksene på denne siden
1. Klikk ```Next```
1. Klikk på ```Create a new thing``` på siden og gi enheten din et navn
1. Klikk ```Next```

## Velg platform og SDK
1. Velg ønsket platform
1. Velg ønsket SDK. Vi har tatt utgangspunkt i Python her. Påse at du har et fungerende Python 3 miljø for ditt system(*).
1. Klikk ```Next```

## Last ned connection kit og installer
1. Klikk ```Download connection kit```
1. Unzip zip filen i en ønsket folder
1. Du vil nå ha en folder som heter ```connect_device_package``` i ønsket folder
1. Klikk ```Next``` på AWS siden

## Run connection kit steg
1. Følg stegene som er listet på denne web siden, de vil variere litt etter valgt miljø/SDK
1. (Bruk ```Copy``` knappene på web siden for å få riktig kommandoer kopiert inn i ditt shell)
1. Sett riktige rettigheter på scriptet og start scriptet iht til instruksjonene på siden
1. Ved første gangs kjøring, så vil den sjekke/installere sine dependencier as-needed

## Verifiser oppsett
1. En forenklet MQTT klient er å finne nederst på ```Run connection kit``` siden
1. Verifiser at meldinger blir sendt fra scriptet til web siden
1. Klikk ```Next``` på AWS siden

## Device is connected
1. Wohoo!  Du har nå et fungerende miljø for en IoT Thing på din PC!
1. Neste steg kan være å tilpasse policy og scriptet til din thing til å kunne snakke med din ESP32

## Konfigurer policy for din nye Thing
1. Velg ```Security``` i menyen til venstre
1. Velg ```Policies``` i menyen til venstre
1. Klikk på policien som ble laget av wizarden til din nye Thing
1. Klikk ```Edit active version```
1. Klikk ```JSON```
1. Slett innhold i feltet ```Policy document```
1. Lim inn Policy JSON (se under) i feltet ```Policy document```
1. Erstatt ```ACCOUNT_ID``` med din AWS Account ID. Erstatt ```THING_NAME``` med valgt navn for din nye Thing
1. Klikk ```Save as new version```
1. Under ```All versions```, så vil du se en version 2, velg denne og gjør den aktiv med ```Set as active```

### Policy JSON
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": "arn:aws:iot:eu-west-1:ACCOUNT_ID:client/THING_NAME"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Subscribe",
      "Resource": "arn:aws:iot:eu-west-1:ACCOUNT_ID:topicfilter/esp32/pub"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Receive",
      "Resource": "arn:aws:iot:eu-west-1:ACCOUNT_ID:topic/esp32/pub"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Publish",
      "Resource": "arn:aws:iot:eu-west-1:ACCOUNT_ID:topic/esp32/pub"
    }
  ]
}
```

## Konfigurer og test scriptet for din nye Thing
1. Naviger til folderen ```connect_device_package``` og lag en kopi av filen ```start.ps1```
1. Åpne ```start.ps1``` i din favorit editor.  Finn feltene ```--client_id basicPubSub``` og ```--topic sdk/test/Python```, endre disse til ```--client_id THING_NAME``` og ```--topic esp32/pub```
1. Lagre endring i filen
1. FYI: Scriptet og policy er satt opp til å abonnere og sende meldinger til samme topic(**)
1. Kjør scriptet ```start.ps1``` (fil vil variere mht sdk type/os valg)
1. Klikk på ```MQTT test client``` i AWS IoT core konsolen

## Se meldinger fra Python, samme topics som ESP32
1. Klikk på ```Subscribe to a topic``` (hvis det ikke allerede er valgt)
1. ```Topic filter```, legg inn denne verdien: ```esp32/pub```
1. Klikk ```Subscribe```
1. Du skal nå se meldinger som sendes fra Python scriptet! Hvis du har ESP32 også tilkoblet, så vil du se at meldingene fra denne ender opp i Python scriptet også.

**Gratulerer!** Du har nå satt opp en PC/Raspberry Pi til å sende og motta meldinger fra AWS IoT og din ESP32.

Gå videre til: [Lag en Lambda i AWS](./9_Lage_en_Lambda_Sky.md)

(*) Typiske issues er at du ikke kjører Python 3 og/eller ikke har satt opp Path variablen til Python folderen i 'Edit the system environment variables' seksjonen i Windows

(**)  Dette kan du endre på selv etter behov.  Sjekk ut filen ```pubsub.py``` i folderen ```connect_device_package/aws-iot-device-sdk-python-v2/samples```. Relevant seksjon er mellom kodelinjene 90-119. Kanskje legge til separate publish/subscribe topics som argumenter til funksjonen? Husk å endre policy i AWS tilsvarende. Alternativt bruk wildcard * for å dekke alle topics :)