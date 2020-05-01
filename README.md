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