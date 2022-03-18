# Smarthome
Docker compose file for a basic smarthome stack, all application port numbers are shown in square brackets. zwave2mqtt and zigbee2mqtt are disabled by default, uncommenting those sections within the docker-compose file will create those conainers. If enabling either 2mqtt application ensure that devices are set correctly. Remember to create the inside_network and set environment variables before bringing up the stack.

Create networks:
```
docker network create inside_network
```

Set the following variables in the .env file, examples are given in square brackets:
- PUID= [the PUID of your user account - $ id] 
- PGID= [the PGID of your user account - $ id] 
- TZ= ["Europe/London"]
- DOCKERDIR= ["/home/user/docker"]
- SESSION_SECRET= [anythingyoulike]

Bring up the stack:
```
docker-compose -p smarthome up -d
```
Applications included with this stack:
- [9000] Home Assistant
- [9001] Home Bridge
- [9002] Mosquitto
- [9003] ESP Home
- [9004] Code Server

Available applications (not enabled by default):
- [9005] zwave2mqtt
- [9006] zigbee2mqtt

Additional applications:
- [10000] Portainer agent

## Post installation
Some applications require addtitional configuration, this is detailed below and also, comments are also included in the docker-compose.yml file.

### Home Assistant:
```
sudo chown -R <user> app_data/homeassistant
nano app_data/homeassistant/configuration.yaml
```

Add the following to set the Home Assistant port to 9000
```
  http:
    server_port: 9000 
```
Restart the container: 
```
sudo docker container restart homeassistant
```

### Mosquitto

Set MQTT username and password

```
sudo docker exec -it mosquitto sh
mosquitto_passwd - c mosquitto/config/mqtt_passwd mqtt
exit
```
Create a mosquitto configuration file
```
sudo nano app_data/mosquitto/config/mosqitto.conf 
```
Add the following to mosquitto.conf
```
listener 1883
allow_anonymous false
password_file /mosquitto/config/mqtt_passwd
persistence true
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log
```
Restart the container: 
```
sudo docker container restart homeassistant
```