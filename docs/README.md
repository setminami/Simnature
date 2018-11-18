# Simnature

```uml
@startuml

title a Day Use Case Diagram


actor Raspi001
note top: 3B or 3B+ which have cam device
node peripherals01 [
USB
---
    hdd
    cam
    highSpeedDisk
]

agent Simnature as SimN
Raspi001 - SimN : startup
note right: start up on central raspi

Raspi001 .. peripherals01 : peripherals

actor Raspi002
note top: ZeroW he has temparature sensor and lird

node peripherals02 [
GPIO
---
    ir sender
    ir reciever
    SD1331
    in warter temparature sensor
]

Raspi002 .. peripherals02: peripherals

agent SunlightControl as SC
(IoTConnections) as IoTC
SimN --> SC : kick
SimN --> IoTC : kick

Raspi002 -- SC : startup
file schedules as pubSchData
note right: NGINX publish this

(schedule daemon) as daemon #lightblue
SC --> pubSchData : construct daily lighting schedules
Raspi001 <--> Raspi002 : Connect
SC <-- SC : running \nto check temparature

IoTC --> pubSchData : (1) check out
SC --> daemon : generate and run
daemon <-- daemon : running for a day

(schedule daemon as backup) as sdh #lightblue
IoTC --> sdh: (2) generate helper daemon
sdh <-- sdh
daemon <--> sdh : collaborate to run punctual
interface IFTTT as ifttt
interface irsend as ir
daemon --> ifttt
daemon --> ir
sdh --> ifttt: when couldnot run a schedule, he delegate them.
@enduml
```
