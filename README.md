### Description
In this repository, i share my project on making water level sensor integrated with home assistant. The controller for this water level sensor is running on ESPHome Firmware. I will explain the details of this project below.

#### Tools and Equipment
  - Wemos D1 Mini
  - DC 5V Power Supply
  - Relay Optocoupler 
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

In filters, i inverse the distance eesult from sensor. It's because i place ultrasonic distance sensor on the top of my water tank, facomg down the water tank. As the water rise, the tank will be full. But the ultrasonic distance sensor reading will get shorter. Then, i inverse sensor reading so that as the water rise, it'll also tell us that the tank is getting full.