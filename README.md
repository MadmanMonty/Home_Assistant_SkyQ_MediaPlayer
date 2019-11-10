# Custom Component for SkyQ Integration for Home Assistant

The skyq platform allows you to control a SkyQ set top box.

There is currently support for the following device types within Home Assistant:

-   Media Player

## Installation

To begin with it is recommended you ensure your SkyQ set top box or boxes have static IP addresses.

Download the custom component in to your folder <config_directory>/custom_components/skyq

# Media Player Configuration 

### Example of basic configuration.yaml
```
media_player:
 - platform:  skyq
   name: SkyQ Living Room
   host: 192.168.0.10
   sources:
      SkyOne: '1,0,6'
      SkyNews: '5,0,1'
```

## Configuration variables

media_player:
**platform** (string)(Required) 
Must be set to skyq

**host** _(string)(Required)_
The IP of the  SkyQ  set top box, e.g., 192.168.0.10.

**name** _(string)(Required)_
The name you would like to give to the  SkyQ  set top box.

**sources** _(list)(Required)_
List of channels or other commands that will appear in the source selection.

**name** _(string)(Required)_
The name you would like to give to the  SkyQ  set top box.


### Sources

To configure sources, set as 
```
<YourChanneName> : ‘<button>,<button>,<button>’.
```
### Supported buttons

sky, power,  tvguide  or home,  boxoffice, search, sidebar, up, down, left, right, select,  channelup,  channeldown,  i, dismiss, text, help,

play, pause, rewind,  fastforward, stop, record

red, green, yellow, blue

0, 1, 2, 3, 4, 5, 6, 7, 8, 9

### Next/Previous Buttons

The behaviour of the next and previous buttons is fastforward and rewind (multiple presses to increase speed, play to resume)


# Switch Generation Helper
A utility function has been created to generate yaml configuration for SkyQ enabled media players to support easy usage with other home assistant integrations, e.g. google home

Usage based on google home:  _“turn on <source name / channel name> in the ”_

## Configuration

**generate_switches_for_channels** _(boolean)(Optional)_ Default False
Generate switches for each item listed in source.
The files will be generated in <config folder>/skyq<room>.yaml

**config_directory** _(string)(Optional)_ Default '/config/'
The location of your configuration folder. 

The correct path required if generate_switches_for_channels is set to True to enable output generation of yaml files to the correct location

Hassbian default would be -  config_directory: '/home/homeassistant/.homeassistant/' or '/config/' for hassio

**room**
_(string)(Optional)_ Default 'Default Room'
The room where the  SkyQ  set top box is located. 

Avoid using [ ] in the name: or room: of your device. This field is required if you have more than one SkyQ box being configured with switches

 
### Example configuration.yaml with switch generation
```
media_player:
 - platform:  skyq
   name: SkyQ Living Room
   host: 192.168.0.10
   sources:
      SkyOne: '1,0,6'
      SkyNews: '5,0,1'
   room: Living Room
   config_directory: '/home/homeassistant/.homeassistant/'
   generate_switches_for_channels: true
```

To integrate these generated switch configuration files, add the generated yaml to your configuration.yaml. The following example configuration implements the generated switches from the generate_switches_for_channels function.

```
switch:
- platform: template
  switches: !include  skyq<room*>.yaml
```

# Additional usage configuration examples

Below this point is a collation of usage examples for forums and user feedback to help ease integration usability

## HACS Mini Media Player

Credit to Matt Barnes for this example, integrating SkyQ into the mini-media-player

See https://community.home-assistant.io/t/custom-component-skyq-media-player/140306/49

### Media Player Configuration:
```
  - platform: skyq
    host: 192.168.0.9
    name: TestSkyL2
    sources:
      CUP: 'channelup'
      CDOWN: 'channeldown'
      UP: 'up'
      DOWN: 'down'
      LEFT: 'left'
      RIGHT: 'right'
```

Lovelace configuration

```
        - artwork: cover
        background: >-
          https://wi-images.condecdn.net/image/m9qkOnPpZnz/crop/1620/f/screen-shot-2015-11-18-at-100440.jpg
        entity: media_player.testskyl2
        hide:
          power_state: false
          source: true
          volume: true
        icon: 'mdi:blank'
        name: TestSkyL2
        shortcuts:
          buttons:
            - icon: 'mdi:arrow-up'
              id: CUP
              type: source
            - icon: 'mdi:arrow-down'
              id: CDOWN
              type: source
            - icon: 'mdi:arrow-left'
              id: LEFT
              type: source
            - icon: 'mdi:arrow-right'
              id: RIGHT
              type: service
            - icon: 'mdi:arrow-up'
              id: skyq_chuplounge
              type: service
            - icon: 'mdi:arrow-up'
              id: skyq_chuplounge
              type: service
          columns: 6
        type: 'custom:mini-media-player'
```

