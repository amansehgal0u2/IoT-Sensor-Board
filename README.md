## Hardware
* ATSAMW25H18 system on module
* RH/T and Ambient Light Sensor
* Brushed DC motor driver
* DC/DC buck converter
* Li-Po charger
* 1MB off chip SPI flash

### System Block Diagram

![Block Diagram with Part Numbers](https://github.com/amansehgal0u2/IoT-Sensor-Board/blob/master/system.png)

## Inspiration
I was inspired by the growing need to help farmers better plan and allocate their resources and provide effective and actionable information about all things weather around local spots on their farm. Use the weather sense node, a farmer can manage and care for their crops better by getting real time data about humidity, temperature and ambient light. This is particularly useful for vertical farms that grow food in an indoor setting with a tightly controlled climate that requires effective and efficient feedback. Vertical farms are important because they can provide fresh food without the need to waste resources on transportation and storage in order to cater to urban populations in metropolitan areas with a quick turnaround time. Imagine walking into a grocery store that grows its food right above ?!



## What it does
It collects weather related data such as humidity, ambient light and temperature about the local environment and relays all the information to a remote server. The remote server serves a webpage dashboard allowing access to the collected data. The user can also send commands to the node to turn on an actuator such as a relay or a servo to open a valve. In this case I used a brushed DC motor


## sensors
Relative humidity, temperature and ambient light.

## How I built it
I designed the the PCBA using Altium, bought the parts on DigiKey, had the board fabricated at PCB:NG in brooklyn, NYC and wrote all the firmware in C using Atmel studio and the supplied APIs. The processor is an integrated module called the ATSAMW25 by Atmel that incorporates bluetooth and wifi along with a PCB antenna. The web platform was prototyped using node-red and deployed using IBM's bluemix hosting service. The messages were sent using MQTT with the broker hosted on a [cloud MQTT](http://www.cloudmqtt.com) instance. 

## Cloud Connection
The data was sent using MQTT & TCP/IP over WiFi to an MQTT broker. The MQTT broker then forwarded the data to a web app that was built using node-red and hosted on IBM Bluemix. All sensor data was sent to the cloud. All actuator states were received from the cloud. All diagnostics like heart-beats with a timestamp and sensor health were also sent to the cloud.

## Bootloading
Every-time the device starts up from either a reset state or from a power cycle, the bootloader ensures the integrity of the firmware and also ensures that the firmware that has been downloaded is valid by using a crc checker. The bootloader is also responsible for upgrading the executing firmware by programming the on-chip flash with the firmware that was downloaded into the SPI flash. The device and be forced into boot mode by resetting the device while holding down the user button. This will lock the bootloader into boot mode. The bootloader check to see if the firmware write flag has been set and then decides to upgrade the firmware. It also logs the number of resets that have occurred due to a watch dog reset and if this crosses a set threshold, then the bootloader reverts to a golden image at the root of flash. 

## Memory Partition
The off-chip flash is used to store multiple images of the main application code and also to download updated application firmware in order to upgrade the system firmware.

![](https://github.com/amansehgal0u2/IoT-Sensor-Board/blob/master/memPartition.PNG "Off-chip flash partition")
