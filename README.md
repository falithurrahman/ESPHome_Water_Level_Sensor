### Description
In this repository, i share my project on making water level sensor integrated with home assistant. The controller for this water level sensor is running on ESPHome Firmware. I will explain the details of this project below.

#### Tools and Equipment
  - Wemos D1 Mini
  - DC 5V Power Supply
  - Relay Optocoupler <br>
    This relay will be connected to water pump so that we will be able to autonomously turn the pump on and off. 
  - Ultrasonic Distance Sensor <br>
    There are various kind of this sensor like SRF04, HC-SR04, US-100. You can use any kind of sensor of your desire. They all are working on same principle.

#### Wiring Diagram
Here is the wiring diagram between WemosD1 Mini and Relay Optocoupler. <br>
| Wemos D1 Mini      | Relay Optocoupler |
| ----------- | ----------- |
| VCC         | VCC       |
| GND         | GND        |
|D8|Input|

Here is the wiring diagram between WemosD1 Mini and Ultrasonic Distance Sensor. <br>
| Wemos D1 Mini      | Ultrasonic Distance Sensor |
| ----------- | ----------- |
| VCC         | VCC       |
| GND         | GND        |
|D3|Trig|
|D4|Echo|

I have provided you the schematic and board file inside control board folder. The filename is WemosD1_Control. It will ease you to understand wiring diagram of the circuit.

#### Code
Since i use ESPHome firmware running on my Wemos D1 Mini, i write the code in yaml file. You can peek the code in this repository with WemosD1.yaml filename. The main part of the code is within the sensor and switch part. For pin mapping, we can use whether with D8, D3, D4 syntax or GPIO2, GPIO0, GPIO15 syntax. Both of them work fine.

````yaml
sensor:
  - platform: ultrasonic
    trigger_pin: GPIO0
    echo_pin: GPIO2
    name: "Ultrasonic Sensor"
    update_interval: 10s
    timeout: 4.0m
    filters:
    - lambda: return (1-x);
    unit_of_measurement: "meter"
    
switch:
  - platform: gpio
    name: "Kendali Pompa Air"
    id: kendali_pompa_air
    pin:
      number: GPIO15
      inverted: True
````

In filters, i inverse the distance result from sensor. It's because i place ultrasonic distance sensor on the top of my water tank, facing down the water tank. As the water rise, the tank will be full. But the ultrasonic distance sensor reading will get shorter. Then, i inverse sensor reading so that as the water rise, it'll also tell us that the tank is getting full.

#### Automation
The idea of using home assistant is because i want to add automation in this project. By using automation, i can have the water pump to turn on and off, filling water tank autonomously. The trigger of this automation is based on ultrasonic sensor reading. You can peek the automation code in this repository with automation.yaml filename. 

````yaml
  alias: PompaAir_TurnOn_Auto
  description: ''
  trigger:
  - above: 0
    below: 0.2
    device_id: 1c657803e15f46eebff68046b731fd01
    domain: sensor
    entity_id: sensor.ultrasonic_sensor
    for:
      hours: 0
      minutes: 0
      seconds: 5
    platform: device
    type: value
````
As you can see part of the automation.yaml i copy, trigger to turn on the water pump is the inverted result from ultrasonic distance reading. If the value shows between 0 to 0.2 meters, it will turn on water pump.
````yaml
  alias: PompaAir_TurnOff_Auto
  description: ''
  trigger:
  - above: 0.8
    device_id: 1c657803e15f46eebff68046b731fd01
    domain: sensor
    entity_id: sensor.ultrasonic_sensor
    for:
      hours: 0
      minutes: 0
      seconds: 10
    platform: device
    type: value
````
But if the value shows between 0.8 meter, it will turn on water pump. Why not use 1 meter? Because the height of my water tank is 1m. If the pump stop by 1m, it will make the sensor sink and i don't want it happen.