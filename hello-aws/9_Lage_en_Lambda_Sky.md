# Hello AWS: Kjør en lambda når melding kommer inn

## Opprett en lambda

Gå til Lambda inne på AWS, velg så `Create function`. Denne fylles ut etter følgende oppskrift:

1. Function name: Bruk `IoT-ESP32-Lambda`
2. Runtime: Velg Python
3. Architecture: La det stå på x86_64
4. Ikke gjør noen endring på Execution Role
5. Trykk på `Create function` nede til høyre

## Legg inn koden i lambdaen

1. Kopier koden og legg den inn under code på lambdaen
   
```python
import boto3
import json
        
iot_client = boto3.client('iot-data')
iam_client = boto3.client('iam')
        
def return_name():
    response = iam_client.list_account_aliases()
    aliases = response.get('AccountAliases', [])
    if aliases:
        return aliases[0]
    else:
        return 'world'
        
def lambda_handler(event, context):
    name = return_name()
    response = iot_client.publish(
                   topic='esp32/sub',
                   qos=1,
                   retain=False,
                   payload=json.dumps(f"Hello {name}")
               )
```

2. Lagre endringene
3. Trykk `Deploy`

## Gi lambdaen ekstra rettigheter

En liten advarsel: IAM burde alltid gjøres etter least privilege principle. For å slippe å tenke på mulige 
endringer på oppsett og for å gjøre workshoppen så streamlined og bug-free som mulig har vi ikke gjort 
dette, så det vi viser er ikke best practice. 

1. Gå til IAM via søkemenyen på toppen av AWS konsollen. 
2. Gå til *Policies* i menyen til venstre
3. Trykk `Create Policy`
4. Bytt til JSON i stedet for visual-editor
5. Kopier inn følgende policy

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:ListAccountAliases",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iot:Publish",
            "Resource": "*"
        }
    ]
}
```

6. Trykk `Next` til du kommer til navnsetting.
7. Velg et navn, bruk gjerne `IoT-ESP32-Policy`
8. Trykk `Create policy`, den blir opprettet og du kommer tilbake til listen over policies
9. Gå inn på `IoT-ESP32-Policy`
10. Gå til fliken *Policy Usage*
11. Trykk på `Attach`
12. Huk av på rollen med navn `IoT-ESP32-Lambda-XXXX` (samme navn som lambdaen du opprettet tidligere)
13. Trykk `Attach policy`

## Opprett en trigger for lambdaen

En trigger gjør at lambdaen automatisk kjører når en melding kommer fra ESP32. 

1. Gå til IoT core
2. Under *Manage* på venstresiden velg *Message Routing* og deretter *Rules*
3. Trykk på `Create Rule`
4. Kall den `IoT_ESP32_Rule` og trykk `Next`
5. La det stå på SQL Version 2016-03-23
6. Skriv inn følgende SQL statement og trykk `Next`:
```
    SELECT * FROM 'esp32/pub'
```
7. Under *Rule Actions* velg `Lambda`
8. På `Lambda` function velger du navnet på lambdaen din, `IoT-ESP32-Lambda`
9. Trykk `Next`, og så `Create`.

## Sluttresultat

Det skal nå komme en melding tilbake fra lambdaen til `esp32/sub` når den mottar en melding.

Dette kan du se i konsollet i VS Code.  Devicet sender en melding til `esp32/pub`, denne går til lambdaen, og denne svarer, og meldingen mottas på devicet:
``` 
Message received.
Topic: esp32/sub
Payload:
"Hello world"
Message: 
Value:
```

Du kan også se trafikken fra PC-klienten.  Denne sender meldinger til `esp32/pub` med innholdet `Hello World! [n]` (hvor _n_ varierer og merk stor `W` mens meldingen fra lambda har liten `w`, bl.a.).  I MQTT Test client i AWS IoT kan man se trafikken i begge retninger, og det kan man også i terminalvinduet hvor `start.ps1` / `start.sh` kjøres.

Gå videre til: [Sette opp DynamoDB i AWS](./10_Sette_opp_DynamoDB_Sky.md)


