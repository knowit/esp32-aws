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
4. Trykk `Deploy`

## Få inn data

FIXME.  Gjør endringer på "device name" på ESP32 og se at dataen kommer inn.

Pass på kostnader. Det skal være 1 million månedlige requests gratis på AWS, men står ting å tikker
så kommer beløpene. Ingen lambdaer kan kjøre uten at det skrives noe fra ESP32.

