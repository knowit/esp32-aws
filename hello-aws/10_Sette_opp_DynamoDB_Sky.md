# Legge til data i dynamodb fra ESP32

# 1. Sett opp ESP32 med potmeter

Følg guiden i https://github.com/knowit/esp32-aws/tree/main/hardware/potmetertilsky

# 2. Opprette en Collection i dynamoDB

Først så må vi opprette et sted for lagring av data fra ESP32. Vi bruker NoSQL databasen dynamoDB. 

1. Søk opp DynamoDB på AWS
2. Gå til tables og trykk create table
3.  Velg navn på tabellen, kall denne IoTCatalog
4. På partition key skriv "device"
5. På sort key skriv "timestamp" og velg number
6. Trykk create table

# 3. Opprett en lambda
1. Søk på lambda i AWS
2. Trykk create function
3. På function name skriv hva du vil. F.eks. IoTStoreData
4. Velg Node.JS 14 på runtime.
5. Create Function
6. Legg inn følgende kode i lambdaen
```
    const AWS = require('aws-sdk');
	const dynamo = new AWS.DynamoDB.DocumentClient();

	const collection = "IoTCatalog"

	// Handler lamda function

	exports.handler = function(event, context) {

		const params = {

			TableName: collection,
			Item: {
				"device": "ESP32",
				"timestamp": event.time,
				"value": event.value,
			}
		};
		dynamo.put(params, function(err, data) {
			if (err) {
				console.error("Unable to add device.Error JSON: ", JSON.stringify(err, null, 2));
				context.fail();
			} else {
				console.log(data)
				console.log("Data saved: ", JSON.stringify(params, null, 2));
				context.succeed();
				return {
					"message": "Item created in DB"
				}
			}
		});
	}
```
# 4. Gi rettigheter til lambdaen
1. Gå til IAM via søkemenyen på toppen av AWS konsollen. 
2. Gå til policies
3. Trykk Create Policy
4. Bytt til JSON i stedet for visual-editor
5. Kopier inn følgende policy
```
    {
		"Version": "2012-10-17",
		"Statement": [
			{
				"Sid": "VisualEditor0",
				"Effect": "Allow",
				"Action": "dynamodb:PutItem",
				"Resource": "*"
			}
		]
	}
```
6. Trykk next til du kommer til navnsetting.
7. Velg et navn - dette er ikke viktig så lenge du husker hva du kaller den
8. Forbli på policy og trykk inn på den du akkurat opprettet
9. Gå til Policy Usage
10. Trykk på Attach
11. Huk av på rollen med samme navn som lambdaen du opprettet tidligere (den har sannsynligvis noen tall/bokstaver bak seg)
12. Trykk attach policy

# 5. Opprett en trigger for lambdaen
Dette gjør at den automatisk kjører når du sender melding fra ESP32. Her kan du også bruke den du lagde for den forrige oppgaven. Hvis du ikke gjorde den oppgaven så kan du følge oppskriften under. Husk å fjerne triggeren fra forrige oppgave

1. Gå til IoT core
2. Under manage på venstresiden velg Message Routing -> Rules
3. Create Rule
4. Gi den et valgfritt navn og trykk next
5. La det stå på SQL Version 2016-03-23
6. Skriv inn følgende SQL statement og trykk next
```
    SELECT * FROM 'esp32/pub'
```
7. Under Rule Actions velg Lambda
8. På Lambda function velger du navnet på lambdaen din
9. Trykk next, og så create.

# 6. Få inn data
Gjør endringer på "device name" på ESP32 og se at dataen kommer inn. Pass på kostnader. Det skal være 1 million månedlige requests gratis på AWS, men står ting å tikker så kommer beløpene. Ingen lambdaer kan kjøre uten at det skrives noe fra ESP32. 

