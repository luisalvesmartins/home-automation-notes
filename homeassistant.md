## Configuing HomeAssistant

Home assistant running on docker on the Raspberry PI 3:

```
docker run --init -d --name homeassistant --restart=unless-stopped -v /etc/localtime:/etc/localtime:ro 
-v ~/homeassistant:/config 
--network=host 
homeassistant/raspberrypi3-homeassistant:stable
```

Then run it:

```
http://<IP>:8123
```

HA will discover some devices already in your network.
