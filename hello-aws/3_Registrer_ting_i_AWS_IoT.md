# Hello AWS: Registrer ting i AWS IoT


## Opprett Policy
1. Velg ```Security``` i menyen til venstre
1. Velg ```Policies``` i menyen til venstre
1. Klikk ```Create policy```
1. Angi navn: ```Esp32Policy```
1. Klikk ```JSON```
1. Slett innhold i feltet ```Policy document```
1. Lim inn Policy JSON (se under) i feltet ```Policy document```
1. Erstatt ```ACCOUNT_ID``` med din AWS Account ID. ([klikk her for tips om hvordan finne Account ID](https://www.apn-portal.com/knowledgebase/articles/FAQ/Where-Can-I-Find-My-AWS-Account-ID))
1. Klikk ```Create```
1. ```Esp32Policy``` skal nå komme opp i listen over policies

### Policy JSON
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": "arn:aws:iot:eu-west-1:ACCOUNT_ID:client/MyNewESP32"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Subscribe",
      "Resource": "arn:aws:iot:eu-west-1:ACCOUNT_ID:topicfilter/esp32/sub"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Receive",
      "Resource": "arn:aws:iot:eu-west-1:ACCOUNT_ID:topic/esp32/sub"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Publish",
      "Resource": "arn:aws:iot:eu-west-1:ACCOUNT_ID:topic/esp32/pub"
    }
  ]
}
```



## Registrer ting
1. Velg ```Manage``` i menyen til venstre
1. Velg ```All devices``` i menyen til venstre
1. Velg ```Things``` i menyen til venstre
1. Velg ```Create things```.
1. Velg ```Create single thing```
1. Klikk ```Next```
1. Gi den nye tingen navnet ```MyNewESP32```. La de resterende feltene stå til standardverdiene. 
1. Klikk ```Next```
1. Velg ```Auto-generate a new certificate (recommended)```
1. Klikk ```Next```
1. Kryss av på ```Esp32Policy``` i listen
1. Klikk ```Create thing```
1. Klikk ```Download``` på ```Device certificate```
1. Endre navn på den nedlastede filen til: ```Device Certificate.pem.crt``` 
1. Klikk ```Download``` på ```Private key file```.
1. Endre navn på den nedlastede filen til: ```Device Private Key.pem.key``` 
1. Klikk ```Download``` på ```Public key file```. Den filen kan forkastes. Amazon tvinger deg til å laste den ned.
1. Under ```Root CA certificates```, klikk ```Download``` på ```Amazon trust services endpoint```. Velg sertifikat som ender på "CA 1".
1. Pass på at den nedlastede filen heter ```AmazonRootCA1.pem``` . Dersom den har et annet navn, gi den det navnet
1. Klikk ```Done```
1. ```MyNewESP32``` skal nå komme opp i listen over things

Gå videre til: [Legg inn hovedprogram](./4_Legg_inn_hovedprogram.md)
