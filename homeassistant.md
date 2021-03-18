## Configuing Simple HomeAssistant

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

## I went for a dedicated RPI4

Having supervisor and addons.

Configured [HACS](https://hacs.xyz/docs/installation/manual)

Tips:

- CTRL+F5 to refresh your browser cache
- the resources are pointing to "hacsfiles" and I had to duplicate the line to the correct URL:
  - /hacsfiles/mini-media-player/mini-media-player-bundle.js
  
  added:
  - /local/community/mini-media-player/mini-media-player-bundle.js
- Lovelace icons can be found here: https://iconify.design/icon-sets/mdi/

### Configuration blocks:

For Soundtouch, as it was not recognized automatically:

in configuration.yaml:

``` yaml
media_player:
  - platform: soundtouch
    host: <IP>
    port: 8090
    name: Soundtouch Sala
```    

Lovelace card with mini media player:
``` yaml
type: 'custom:mini-media-player'
entity: media_player.soundtouch_sala
name: Bose Soundtouch 30
artwork: cover
toggle_power: true
group: true
volume_stateless: true
icon: 'mdi:radio'
show_source: true
tts:
  platform: google_translate
shortcuts:
  columns: 3
  buttons:
    - icon: 'mdi:numeric-1-box'
      type: playlist
      id: 1
    - icon: 'mdi:numeric-2-box'
      type: playlist
      id: 2
    - icon: 'mdi:numeric-3-box'
      type: playlist
      id: 3
    - icon: 'mdi:numeric-4-box'
      type: playlist
      id: 4
    - icon: 'mdi:numeric-5-box'
      type: playlist
      id: 5
    - icon: 'mdi:numeric-6-box'
      type: playlist
      id: 6
volume_step: '10'
max_volume: '100'
min_volume: '10'
```    
