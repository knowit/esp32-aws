# Hello AWS: Legg data inn i dynamodb fra lambda

## Opprett en Collection i dynamoDB

Først så må vi opprette et sted for lagring av data fra ESP32. Vi bruker NoSQL databasen dynamoDB. 

1. Søk opp DynamoDB i AWS
2. Gå til *Tables* og trykk `Create table`
3. Velg navn på tabellen, kall denne `IoTCatalog`
4. På partition key skriv `device`
5. På sort key skriv `timestamp` og velg `Number`
6. Trykk `Create table`

## Gi nye rettigheter til lambdaen

1. Søk opp IAM i AWS
2. Gå til *Policies*
3. Gå inn på `IoT-ESP32-Policy`
4. Klikk `Edit policy`
5. Bytt til JSON i stedet for visual-editor
6. Bytt ut innholdet med dette:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iot:Publish",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "dynamodb:PutItem",
            "Resource": "*"
        }
    ]
}
```

8. Trykk `Review policy` og `Save changes`

## Modifiser koden i lambdaen

1. Søk opp Lambda i AWS
2. Klikk *Functions* og gå inn på `IoT-ESP32-Lambda`
3. Legg inn følgende kode i lambdaen i stedet for det som står der:

```python
import boto3
import json

db_client = boto3.client("dynamodb")
iot_client = boto3.client("iot-data")

def lambda_handler(event, context):
    timestamp = event["time"]
    value = event["value"]

    db_client.put_item(
        TableName="IoTCatalog",
        Item={"device":   {"S":"ESP32"},
              "timestamp":{"N":str(timestamp)},
              "value":    {"N":str(value)}})

    iot_client.publish(
        topic="esp32/sub",
        qos=1,
        retain=False,
        payload=json.dumps({"message":"Item created in DB"})
    )
```
4. Lagre endringene
5. Trykk `Deploy`

## Få inn data

Skru på devicet og la det koble seg til.  Følg evt med i MQTT Test client for å se at det går trafikk.

I DynamoDB, velg Tables og IoTCatalog og deretter `Explore table items`.  Observer at det er data i tabellen.  Om du laster tabellen om igjen vil du se at det er kommet flere elementer i tabellen.

Kolonnen `value` representerer faktisk sensor-verdien fra PIR-sensoren (bevegelse).  Sitt musestille en stund og du vil se at det kommer inn rader med 0-verdier, mens hvis du beveger deg vil verdiene være 4095.  Du kan sortere tabellen på timestamp (synkende) for å følge med lettere.

Pass på kostnader. Det skal være 1 million månedlige requests gratis på AWS, men står ting og tikker
så kommer beløpene.  (Ingen lambdaer vil kjøre uten at det skrives noe fra ESP32 eller fra PC-klienten.)
