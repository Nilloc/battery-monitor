# Car Battery Monitor

Particle [Photon](https://docs.particle.io/datasheets/photon-datasheet/) based WiFi 12v car battery monitor. The idea is to monitor the battery voltage in order to get early warning that the battery is becoming discharged. The project is intended to be directly connected to the battery via its main terminals.

I used [IFTTT](https://ifttt.com/) to send notifications to my phone for the low battery events generated by the program.

The program uses the deep sleep mode of the Photon between voltage measurements, in this state the current draw from the battery is ~1mA. It takes about 15 seconds to wake-up, connect to WiFi, take a measurement and send an event containing the measured voltage. With a sleep time of 1 hour, a 1mA current should have negligible impact on charge state of the battery in a car which is driven every day.

Over the air (OTA) firmware update is support by the node.js script [ota-update.js](ota-update.js), which when the Photon wakes, sends an event to prevent it going back to sleep. Updated firmware can then be flashed using Particle Dev or Particle Build. To run the script add a file **auth.js** containing:

```javascript
module.exports = {
    username: '<your email>',
    password: '<password>',
    device: '<deviceId>',
    token: '<auth token>'
};
```

## Circuit

![circuit diagram](circuit/battery-monitor.png?raw=true "Battery Monitor Circuit")

The 20V Zenner and the ratio of R1/R2 means that voltage seen by the Photon at A0 cannot exceed 1.8V.

## Completed Project

![project](circuit/circuit-box.jpg?raw=true "Battery Monitor Project")

## BOM

* Particle Photon
* Traco Power [TSR-1-2450](docs/tsr1.pdf) DC/DC Step-Down Converter
* 1N4747 Zener Diode, 20V, 1 Watt
* 1000uF 35V Electrolytic Capacitor
* 0.1uF 50V Ceramic Capacitor
* 150uH RF Choke, 4.2&Omega;, 175mA max
* 1K&Omega; Resistor
* 220K&Omega; Resistor
* 22K&Omega; Resistor
* 2 core cable (2 Meters)
* Solder tags (6mm ID) x 2

## References

I used the following to design the components which provide protection from noise in the car electrical system caused by the alternator and hopefully more severe events like jump starting.

* [Power Filter/Regulator Circuit](http://linuxcar.sone.jp/reg.en.html)
* [Powering Arduino and switches from 12V automotive battery](http://www.eevblog.com/forum/projects/powering-arduino-and-switches-from-12v-automotive-battery/msg687838/#msg687838)
