## Mongoose OS

Vi bruker programmet `mos` fra terminalen for å flashe firmware til dingsen og for å sette opp koblingen mellom dingsen og AWS.

[Følg instruksene her for å installere mos (kun steg 1)](https://mongoose-os.com/docs/mongoose-os/quickstart/setup.md).

Appen vi skal utvikle får vi så og si gratis. Den legger du inn ved å skrive

```
$ mos clone https://github.com/mongoose-os-apps/demo-js app1
$ cd app1
```

Vi må gjøre en liten justering i konfigurasjonen for å få appen til å fungere.
I `mos.yml` legger du til

```
build_vars:
  APP_SLOT_SIZE: 0x190000
```

Vi kompilerer appen

```
$ mos build --platform=esp32
```

(dette tar litt tid)

og flasher dingsen

```
$ mos flash
```

Om du ikke får koblet til dingsen:

Sjekk om USB-til-serieport-adapteren finnes:

```
$ ls /dev/cu.*
```

Den heter typisk noe som `SLAB_USBtoUART`.

Skriv deretter

```
$ mos flash --port=/dev/cu.SLAB_USBtoUART
```

Dingsen må ha parametere for Wifi. Disse setter vi ved å skrive 

```
$ mos wifi knowit-workshop QuantumDot # wifinavn er knowit-workshop og passordet er QuantumDot
```

Denne kommandoen må skrives etter hver gang vi flasher dingsen.

Vi må deretter konfigurere dingsen for AWS

```
$ mos aws-iot-setup
```

Denne kommandoen må skrives etter hver gang vi flasher dingsen.

Dingsen vår blir opprettet i AWS og sertifikater synkroniseres

```
$ aws iot list-things
{
    "things": [
        {
            "thingArn": "arn:aws:iot:eu-west-1:057359675476:thing/esp32_F7F2AD",
            "version": 1,
            "thingName": "esp32_F7F2AD",
            "attributes": {}
        }
    ]
}
```

Vi kan nå åpne mos i grafisk modus og se på logger fra dingsen

```
$ mos
```

![](./mos.png)

Om man trykker på knappen til høyre for USB-kontakten (Boot) sendes det en melding fra dingsen til skyen. Meldingen og kanalen den blir sendt på vises i loggene i mos.

Vi kan se på meldingene i AWS-konsollen ved å søke opp AWS "IoT Core" og gå til `Test`.

![](./aws-iot-sub.png)

__OBS!__ Hvis du blir får et velkomstbilde, klikk "Getting started" og deretter "Monitor" (i menyen til venstre) 

1. Gå til menyen Manage/Things i menyen til venstre 
1. Noter ned navn på dingsen, f.eks "esp32_929D8C"
1. Klikk på Test i venstremenyen
1. Velg "Subscribe to a topic"
1. Legg inn topic på dingsen din på formatet devices/NAVN/events, f.eks: devices/esp32_929D8C/events
1. Trykk på knappen
1. Det skal vises en melding hver gang knappen trykkes



__TIPS!__ For å bygge ny versjon:
1. Hvis ikke grafisk modus er åpen, skriv ```mos``` i kommandolinjen
1. Skriv: ```mos build --platform=esp32```
1. Skriv: ```mos flash```
1. Skriv: ```mos wifi``` ssid pass
1. Skriv: ```mos aws-iot-setup```

...og alt kan gjøres på en linje slik:

``` mos build --platform=esp32 ; mos flash ; mos wifi knowit-guest Blockchain ; mos aws-iot-setup ; mos```
