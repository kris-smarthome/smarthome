# smarthome

Create networks:
```
docker network create inside_network
```

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