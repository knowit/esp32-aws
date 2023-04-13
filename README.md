# ESP32 mot AWS Iot

## Om kurset

Dette kurset viser hvordan man kan lage dingser (Things) som benytter serverless teknologi for å kommunisere med skyen og hverandre.

Vi benytter AWS som skyteknologi og Arduino som rammeverk på mikrokontrolleren.

## Utstyr

For å delta trenger du en datamaskin med en eller to USB-A-kontakter. Kurset har blitt testet på macOS, men skal i teorien fungere på alle operativsystemer.

Du trenger også [en konto på AWS](https://portal.aws.amazon.com/billing/signup#/start).

## Forkunnskaper

Fokuset handler mest mest på konfigurasjon av serverless-teknologi. Det blir lite fikling med maskinvare.

Det er en fordel om du har tatt kurset [esp32-intro](https://github.com/knowit/esp32-intro) tidligere, men det er ikke nødvendig for å følge dette kurset. 

## Innhold

### IoT i skyen

Mange skyleverandører (Google, Amazon, Azure, etc) har plattformer for Internet-of-Things (IoT).

Plattformene har gjerne noen fellestrekk:

* Dingsene kommuniserer over en protokoll som heter MQTT. MQTT er en sikker publish-subscribe-protokoll med relativt lav overhead og betydelig fleksibilitet som egner seg godt for IoT.

* MQTT krever en meldings-broker som tar i mot og sender meldingene videre. Brokeren har forskjellige "topics" som dingsene kan sende eller lytte etter meldinger på. Hos Amazon ligger brokeren i "IoT Core", hos Azure i "IoT Hub", osv.

* Dingsen har en virtuell kopi av seg selv i skyen. Hos Amazon heter kopien "Shadow", hos Azure "Device twin", osv. Den virtuelle kopien er ofte et JSON-skjema som speiler tilstanden til dingsen. Dingsen og den virtuelle kopien kan synkroniseres.

* Regler, konfigurert hos skyleverandøren, kan trigge andre utfall av meldingene. Dette kan være å lagre meldinger i en database, gjøre analyse av dataene, kjøre lambda-funksjoner, eller andre ting som er tilgjengelig i en serverless-verden.

* Sikkerheten mellom dingsene og skyen er som regel realisert ved bruk av sertifikater, hvor hver dings har fått et unikt sertifikat.

### Hva vi skal gjøre

Vi programmerer en dings for å kommunisere med skyen. Dingsen vi bruker heter ESP32. En ESP32 er en mikrokontroller med innebygget støtte for Wifi, har lav kostnad, og er mye utbredt.

Programvaren på mikrokontrolleren utvikles på rammeverket Arduino.  Arduino er et utviklingsrammeverk som støtter mange forskjellige dingser, og som abstraherer vekk mye av de underliggende tekniske detaljene. Vi benytter et bibliotek som tar seg av konfigurering av MQTT mot skyleverandøren slikt som generering av sertifikater og selve oppkoblingen.


## Leksjoner
- [Hello AWS](./hello-aws/README.md)
