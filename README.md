# MongooseOS with mJS on AWS serverless IoT

## Om kurset

Dette kurset viser hvordan man kan lage dingser (Things) som benytter serverless teknologi for å kommunisere med hverandre.

Vi benytter [Mongoose OS](https://mongoose-os.com/) som utviklingsrammeverk og AWS som backend. Mongoose OS gjør konfigurasjon og utvikling av dingsen til en enkel lek, og AWS tar seg av den bråkete og tunge infrastrukturen. Programmeringsspråket på dingsen er [mJS](https://github.com/cesanta/mjs).

Det ferdige produktet består av en dings som har en knapp man kan trykke på, og en annen dings som har en lysdiode som kan lyse.

## Utstyr

For å delta trenger du en datamaskin med to USB-A-kontakter. Kurset har blitt testet på macOS, men skal i teorien fungere på alle operativsystemer.

Du trenger også [en konto på AWS](https://portal.aws.amazon.com/billing/signup#/start).

## Forkunnskaper

Fokuset ligger mest på konfigurasjon av serverless-teknologi. Det blir lite fikling med maskinvare. Basiskunnskaper om Javascript er en fordel.

## Innhold

### IoT i skyen

Mange skyleverandører (Google, Amazon, Azure, etc) har nå lansert plattformer for Internet-of-Things (IoT).

Plattformene er sikkert mye forskjellige, men kan gjerne oppsummeres slik:

* Dingsene kommuniserer over en protokoll som heter MQTT. MQTT er en publish-subscribe-protokoll som tilbyr lav overhead og dermed små pakkestørrelser.

* MQTT krever en meldings-broker som tar i mot og sender meldingene videre. Brokeren har forskjellige "topics" som dingsene kan sende eller lytte etter meldinger på. Hos Amazon ligger brokeren i "IoT Core", hos Azure i "IoT Hub", osv.

* Dingsen har en virtuell kopi av seg selv i skyen. Hos Amazon heter kopien "Shadow", hos Azure "Device twin", osv. Den virtuelle kopien er ofte et JSON-skjema som speiler tilstanden til dingsen. Dingsen og den virtuelle kopien kan synkroniseres.

* Regler, konfigurert hos skyleverandøren, kan trigge andre utfall av meldingene. Dette kan være å lagre meldinger i en database, gjøre analyse av dataene, kjøre lambda-funksjoner, eller andre ting som er tilgjengelig i en serverless-verden.

* Sikkerheten mellom dingsene og skyen er som regel realisert ved bruk av sertifikater, hvor hver dings har fått et unikt sertifikat.

### Hva vi skal gjøre

Vi setter opp Mongoose OS på en dings. Dingsen vi bruker heter ESP32. En ESP32 er en mikrokontroller med innebygget støtte for Wifi, har lav kostnad, og er mye utbredt. Mongoose OS er et utviklingsrammeverk som støtter mange forskjellige dingser, og som abstraherer vekk mye av den underliggende SDKen til dingsen. Mongoose tar seg også av konfigurering av MQTT mot skyleverandøren slikt som generering av sertifikater og selve oppkoblingen.

![](./awsiot.png)

Vi velger skyleverandør litt etter magefølelsen, og lander da på Amazon.

* [Sette opp awscli](./tools)
* [Installer driver for ESP32](./usb-uart)
* [Sette opp dingsen med Mongoose OS](./mongoose-os)
* [Reagere på meldinger](./mongoose-messaging)
