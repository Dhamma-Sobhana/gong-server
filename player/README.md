# Gong - Client

A node service for playing gong when told by [Gong Backend][gong-backend].

Planned to be deployed on Raspberry Pi's using [balenaCloud](https://www.balena.io/cloud/).

# Dependencies

- mpg123: For playing sound
- mqtt server: For communication with backend

# Developement

    npm install
    npm run start-nodemon

# Configuration
Configure by setting environment variables:

## NAME
Name of instance (male_house, dhamma_hall, kitchen, ...). Used to identify device.

Default: First found mac address

## AREAS
Areas this client handles. Separate multiple with ',' (0 (all), 1-16).

Default: 0

## MQTT_SERVER
IP or hostname of MQTT server.

Default: localhost

## TZ
Timezone of device. For correct logging of time.

Default: Europe/Stockholm

# Communication

Service connects to MQTT message broker and subscribes to topics.

## ping
Respond by publish a *pong*.

## pong
Containing this data:

- name: string
- areas: array of areas: 0 (all), 1-16
- timestamp-millis: unix time
- timestamp: human readable date time

## play
Wait until the next even second, then play gong sound.

Expected data:

- type: string
- areas: array of areas: 0 (all), 1-16

Publishes *scheduled*.

## scheduled
Containting this data:

- name: string
- areas: array of areas: 0 (all), 1-16
- timestamp-millis: unix time
- timestamp: human readable date time
- scheduled-millis: unix time when sound will be played
- scheduled: human readable date time when sound will be played

## played
Publish after gong has been played with this data:

- name: string
- areas: array of areas: 0 (all), 1-16
- timestamp-millis: unix time
- timestamp: human readable date time

# Time keeping
To improve synchronization of sound being played, sync time to local NTP server regularly. Instead of playing sound directly when message is received, which can take different amount of time to different devices, play based on time in short future. 

# Optional hardware

To prevent static noise from speakers a relay can be used to keep the circuit open when not playing.

If using a device with GPIO a simple relay can be connected to this.

This can also be used to handle more than one area using one device by opening circuits based on areas received.

# See also

[Gong Backend][gong-backend]

[Gong Frontend][gong-frontend]

[gong-backend]: https://github.com/Dhamma-Sobhana/gong-backend

[gong-frontend]: https://github.com/Dhamma-Sobhana/gong-frontend
