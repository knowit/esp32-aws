# Hello AWS: Konfigurasjon og sertifikater


## Legg til hemmeligheter
Lag en ny fil som heter ```secrets.h``` i katalogen ```src``` og lim inn følgende:

```cpp
#include <pgmspace.h>

#define SECRET
#define THINGNAME "MyNewESP32"

const char WIFI_SSID[] = "XXXXXXX";
const char WIFI_PASSWORD[] = "XXXXXX";
const char AWS_IOT_ENDPOINT[] = "xxxxxx-ats.iot.eu-west-1.amazonaws.com";

// Amazon Root CA 1
static const char AWS_CERT_CA[] PROGMEM = R"EOF(
-----BEGIN CERTIFICATE-----
XXXXXX
-----END CERTIFICATE-----
)EOF";

// Device Certificate
static const char AWS_CERT_CRT[] PROGMEM = R"KEY(
-----BEGIN CERTIFICATE-----
XXXXXX
-----END CERTIFICATE-----
)KEY";

// Device Private Key
static const char AWS_CERT_PRIVATE[] PROGMEM = R"KEY(
-----BEGIN RSA PRIVATE KEY-----
XXXXXX
-----END RSA PRIVATE KEY-----
)KEY";
```

## Konfigurer WiFi
Sett disse verdiene i henhold til korrekte verdier for WiFi- nettet du vil bruke:
```cpp
const char WIFI_SSID[] = "XXXXXXX";
const char WIFI_PASSWORD[] = "XXXXXX";
```


## Lim inn navn på AWS IoT endepunkt
1. Åpne [AWS IoT-konsollen](https://eu-west-1.console.aws.amazon.com/iot/home?region=eu-west-1#/home) 
1. Kontroller at korrekt region er satt: ```eu-west-1```
1. Velg ```Settings``` i menyen til venstre
1. Under ```Device data endpoint```, kopier verdien under teksten ```Endpoint```.
1. Lim inn i konstanten ```AWS_IOT_ENDPOINT``` : 

```cpp
const char AWS_IOT_ENDPOINT[] = "xxxxxx-ats.iot.eu-west-1.amazonaws.com";
```

## Lim inn sertifikater
Du skal nå lime inn innholdet i sertifikatfilene du har lastet ned fra AWS IoT.

1. Åpne filen ```AmazonRootCA1.pem``` og lim inn innholdet i verdien ```AWS_CERT_CA```:

```cpp
// Amazon Root CA 1
static const char AWS_CERT_CA[]
```

Koden skal se f.eks slik ut:
```cpp
// Amazon Root CA 1
static const char AWS_CERT_CA[] PROGMEM = R"EOF(
-----BEGIN CERTIFICATE-----
MIIDQTCCAimgAwIBAgITBmyfz5m/jAo54vB4ikPmljZbyjANBgkqhkiG9w0BAQsF
ADA5MQswCQYDVQQGEwJVUzEPMA0GA1UEChMGQW1hem9uMRkwFwYDVQQDExBBbWF6
b24gUm9vdCBDQSAxMB4XDTE1MDUyNjAwMDAwMFoXDTM4MDExNzAwMDAwMFowOTEL
MAkGA1UEBhMCVVMxDzANBgNVBAoTBkFtYXpvbjEZMBcGA1UEAxMQQW1hem9uIFJv
b3QgQ0EgMTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALJ4gHHKeNXj
ca9HgFB0fW7Y14h29Jlo91ghYPl0hAEvrAIthtOgQ3pOsqTQNroBvo3bSMgHFzZM
9O6II8c+6zf1tRn4SWiw3te5djgdYZ6k/oI2peVKVuRF4fn9tBb6dNqcmzU5L/qw
IFAGbHrQgLKm+a/sRxmPUDgH3KKHOVj4utWp+UhnMJbulHheb4mjUcAwhmahRWa6
VOujw5H5SNz/0egwLX0tdHA114gk957EWW67c4cX8jJGKLhD+rcdqsq08p8kDi1L
93FcXmn/6pUCyziKrlA4b9v7LWIbxcceVOF34GfID5yHI9Y/QCB/IIDEgEw+OyQm
jgSubJrIqg0CAwEAAaNCMEAwDwYDVR0TAQH/BAUwAwEB/zAOBgNVHQ8BAf8EBAMC
AYYwHQYDVR0OBBYEFIQYzIU07LwMlJQuCFmcx7IQTgoIMA0GCSqGSIb3DQEBCwUA
A4IBAQCY8jdaQZChGsV2USggNiMOruYou6r4lK5IpDB/G/wkjUu0yKGX9rbxenDI
U5PMCCjjmCXPI6T53iHTfIUJrU6adTrCC2qJeHZERxhlbI1Bjjt/msv0tadQ1wUs
N+gDS63pYaACbvXy8MWy7Vu33PqUXHeeE6V/Uq2V8viTO96LXFvKWlJbYK8U90vv
o/ufQJVtMVT8QtPHRh8jrdkPSHCa2XV4cdFyQzR1bldZwgJcJmApzyMZFo6IQ6XU
5MsI+yMRQ+hDKXJioaldXgjUkK642M4UwtBV8ob2xJNDd2ZhwLnoQdeXeGADbkpy
rqXRfboQnoZsG4q5WTP468SQvvG5
-----END CERTIFICATE-----
)EOF";
```

1. Åpne filen ```Device Certificate.pem.crt``` og lim inn innholdet i verdien ```AWS_CERT_CRT```:

```cpp
// Device Certificate
static const char AWS_CERT_CRT[]
```


1. Åpne filen ```Device Private Key.pem.key``` og lim inn innholdet i verdien ```AWS_CERT_PRIVATE```:

```cpp
// Device Private Key
static const char AWS_CERT_PRIVATE[]
```

Gå videre til: [Test programmet](./esp32awsiot_6.md)
