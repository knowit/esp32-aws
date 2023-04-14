# Hello AWS

For å kommunisere med ESP32-enheten må den kobles til AWS IoT Core og autentisere seg. Du må også spesifisere hvilke MQTT- topics den har tillatelse til å publisere og abonnere på.

I denne oppgaven skal vi:

* Forberedelser: sette opp utviklingsmiljø og legge inn et testprogram på ESP32
* Leksjon 2-3: konfigurere opp tingen vår i AWS IoT 
* Leksjon 4: legge inn et program på ESP32 som lar oss kommunisere med AWS IoT
* Leksjon 5: konfiguerere WiFi-innstillinger og sertifikater
* Leksjon 6-7: sende og motta meldinger ved hjelp av testklient på AWS IoT
* Leksjon 8: sende og motta meldinger ved hjelp av en testklient på PC
* Leksjon 9: sende meldingene fra AWS IoT videre til en lambda og sende meldinger fra lambda tilbake til ESP32
* Leksjon 10: lagre meldinger i en DynamoDB-database

Vi kommer til å bruke SnappySense-devicer til dette, disse kombinerer en ESP32 med et display og et antall sensorer.  Koden vi legger inn kommer til å bruke displayet og PIR-sensoren som registrerer bevegelser.  For mer om SnappySense, se [her](https://github.com/knowit/snappysense).

![Deployment diagram](./doc/deployment.png)

## Legg inn Hello World på ESP32

Begynn med å test at programmering mot - og kommunikasjon med - ESP32 fungerer ved å legge inn og teste Hello World.

[Klikk her for instruksjoner](HelloWorld/README.md)

Gå videre til: [AWS IoT Console](./2_AWS_IoT_Console.md)
