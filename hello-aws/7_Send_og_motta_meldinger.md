# Hello AWS: Send og motta meldinger

## Gå til ```MQTT test client```

1. Åpne [AWS IoT-konsollen](https://eu-west-1.console.aws.amazon.com/iot/home?region=eu-west-1#/home) 
1. Kontroller at korrekt region er satt: ```eu-west-1```
1. Velg ```Manage``` i menyen til venstre
1. Velg ```All devices``` i menyen til venstre
1. Velg ```Things``` i menyen til venstre
1. Klikk på ```MyNewESP32```
1. Klikk på ```Activity``` (det er en tab under info om tingen)
1. Klikk på ```MQTT test client```

## Se meldinger fra ESP32

1. Klikk på ```Subscribe to a topic``` (hvis det ikke allerede er valgt)
1. ```Topic filter```, legg inn denne verdien: ```esp32/pub```
1. Klikk ```Subscribe```
1. Du skal nå se meldinger som sendes fra ESP32. Se eksempel under.

```json
{
  "time": 253580,
  "value": 187
}
```

## Send meldinger til ESP32

1. Klikk på ```Publish to a topic``` 
1. ```Topic name```, legg inn denne verdien: ```esp32/sub```
1. Legg inn en melding i ```Message payload``` (se eksempel under)
1. Klikk ```Publish```

Eksempel på melding:
```json
{
  "message": "Hei hei ESP32!",
  "value": "2.14159"
}
```

Hvis alt fungerer, skal du se dette i Serial Monitor:
```
Message received.
Topic: esp32/sub
Payload:
{
  "message": "Hei hei ESP32!",
  "value": "2.14159"
}
Message: Hei hei ESP32!
Value: 2.14159
```

**Gratulerer!** Du har nå satt opp en ESP32 til å sende og motta meldinger til AWS IoT.

Gå videre til: [Send og motta meldinger i Python](./8_Send_og_motta_meldinger_med_python.md)
