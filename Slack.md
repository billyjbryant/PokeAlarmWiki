## Overview
* [Prerequisities](#prerequisites)
* [Introduction](#introduction)
* [Basic Config](#basic-config)
  * [Required Parameters](#required-parameters)
  * [Example: Basic Alarm Configuration using Required Parameters](#example-basic-alarm-configuration-using-required-parameters)
* [Advanced Config](#advanced-config)
  * [Optional Parameters](#optional-parameters)
  * [Example: Alarm Configuration Using Optional Parameters](#example-alarm-configuration-using-optional-parameters)
  * [Mini Map Configuration](#mini-map-configuration)
  * [Formatting alarms text](#formatting-alarms-text)
* [How to Get a Slack API Key](#how-to-get-a-slack-api-key)

## Prerequisites
This guide assumes

1. You are familiar with [JSON formatting](https://www.w3schools.com/js/js_json_intro.asp)
2. You have read and understood the [Alarms](alarms) Wiki
3. You are comfortable with the layout of `alarms.json`.
4. You are using the latest version of PokeAlarm.

Please familiarize yourself with all of the above before proceeding.

## Introduction
![](images/slack_demo.PNG)

**Slack** is a cloud-based team collaboration tool that is available on Windows, Mac OS X, Linux, iOS, Android, and Windows Phone. Slack offers a lot of IRC-like features: persistent chat rooms (channels) organized by topic, as well as private groups and direct messaging. All content inside Slack is searchable, including files, conversations, and people.

PokeAlarm offers the following for Slack:

* Custom username for posting
* High resolution icons for pokemon, gym, or pokestop notifications
* Notifications to multiple Slack channels and/or teams
* Customizable Google Map image of the pokemon, gym, and/or pokestop location
* Personalized notifications via [Dynamic Text Substitution](Dynamic-Text-Substitution)

## Basic Config

### Required Parameters
These parameters below are required to enable the Slack alarm service:

| Parameters        | Default     | Description                           |
|-------------------|-------------|---------------------------------------|
| `type`            | `slack`     | `slack`                               |
| `active`          | `False`     | `True` for alarm to be active         |
| `api_key`         |             | Your Slack API key                    |
| `channel`         | `"general"` | Default channel to post notifications |

### Example: Basic Alarm Configuration using Required Parameters
```json
{
	"active":"True",
	"type":"slack",
	"api_key":"YOUR_API_KEY",
	"channel":"general"
}
```
**Note:** The above code is to be inserted into the alarms section of alarms.json. It does not represent the entire alarms.json file.

## Advanced Config

### Optional Parameters

In addition to the 3 required parameters, several optional parameters are available to personalize your Slack notifications. Below is an example of these optional parameters and how they are incorporated into a functional alarm layout for Slack.

These optional parameters are entered at the same level as `"type":"slack"`.

| Parameters         | Description                                  |
|--------------------|----------------------------------------------|
| `startup_message`  | Confirmation post when PokeAlarm initialized |

These optional parameters below are applicable to the `pokemon`, `pokestop`, `gym`, `egg`, and `raid` sections of the JSON file.

| Parameters       | Description                                       | Default                                      |
|------------------|---------------------------------------------------|----------------------------------------------|
| `channel`        | Send messages to this channel. `#<pkmn>` pushes to pokemon name* | `#general`                    |
| `username`       | Username the bot should post the message as       | `<pkmn>`                                     |
| `icon_url`       | URL path to pokemon icon                          |                                              |
| `title`          | Notification text to begin the message            | `A wild <pkmn> has appeared!`                |
| `url`            | Link to be added to notification text             | `<gmaps>`                                    |
| `body`           | Additional text to be added to the message        | `Available until <24h_time> (<time_left>).`  |
| `map`            | Specify a json object to describe the map         | See Mini Map Configuration for more details  |

*Note: Nidorans will be `nidoranf` or `nidoranm`, Farfetch'd will be `farfetchd`, and Mr. Mime will be `mrmime`. Channels that do not exist (channels cannot be created by bots) will default to general instead.

## Example: Alarm Configuration Using Optional Parameters
```json
{
    "active":"True",
    "type":"slack",
    "api_key":"YOUR_API_KEY",
    "channel":"general",
    "startup_message":"True",
    "pokemon":{
        "channel":"general",
        "username":"<pkmn>",
        "icon_url*":"<YOUR CUSTOM URL HERE>/<pkmn_id_3>_<form_or_empty>.png",
        "title":"A wild <pkmn> has appeared!",
        "url":"<gmaps>",
        "body":"Available until <24h_time> (<time_left>).",
        "map":{
            "enabled":"true",
            "width":"250",
            "height":"125",
            "maptype":"roadmap",
            "zoom":"15"
        }
    },
    "pokestop":{
        "channel":"general",
        "username":"Pokestop",
        "icon_url*":"<YOUR CUSTOM URL HERE>/pokestop.png",
        "title":"Someone has placed a lure on a Pokestop!",
        "url":"<gmaps>",
        "body":"Lure will expire at <24h_time> (<time_left>)."
    },
    "gym":{
        "channel":"general",
        "username":"Pokemon Gym",
        "icon_url*":"<YOUR CUSTOM URL HERE>/gym_<new_team_id>.png",
        "title":"A Team <old_team> gym has fallen!",
        "url":"<gmaps>",
        "body":"It is now controlled by <new_team>."
    },
    "egg":{
        "username":"Egg",
        "icon_url*":"<YOUR CUSTOM URL HERE>/egg_<raid_level>.png",
        "title":"A level <raid_level> raid is incoming!",
        "url":"<gmaps>",
        "body":"The egg will hatch <begin_24h_time> (<begin_time_left>)."
    },
    "raid":{
        "username":"<pkmn> Raid",
        "icon_url*":"<YOUR CUSTOM URL HERE>/<pkmn_id_3>_<form_or_empty>.png",
        "title":"A Raid is available against <pkmn>!",
        "url":"<gmaps>",
        "body":"The raid is available until <24h_time> (<time_left>)."
    }
}
```
**Note1:** *THESE LINES ARE USED TO OVERRIDE DEFAULT VALUES. IF YOU DO NOT WISH TO USE CUSTOM IMAGES, DO NOT ADD THESE LINES TO YOUR ALARMS.JSON.

**Note2:** The above code is to be inserted into the alarms section of alarms.json. It does not represent the entire alarms.json file.


### Mini Map Configuration
![](images/minimap.png)

You can enable a small Google Static Maps image after your post, showing the location of the alarmed pokemon, gym, or pokestop. This is done by adding the `map` parameter at the Alarm level (which will apply maps for any notification), or individually to the `pokemon`, `pokestop`, `gym`, `egg`, or `raid` sections of your alarm.



Below is an example of enabling the mini map for pokemon.
```json
	"pokemon":{
		"channel":"general",
		"username":"<pkmn>",
		"title":"A wild <pkmn> has appeared!",
		"url":"<gmaps>",
		"body":"Available until <24h_time> (<time_left>).",
		"map":{
			"enabled":"true",
			"width":"250",
			"height":"125",
			"maptype":"roadmap",
			"zoom":"15"
		}
	},
```

| Parameters     | Description                                       | Default                                   |
|----------------|---------------------------------------------------|-------------------------------------------|
| `enabled`      | Turns the map on or off                           | `True`                                    |
| `width`        | Width of the map                                  | `250` px                                  |
| `height`       | Height of the map                                 | `150` px                                  |
| `maptype`      | Link to be added to notification text             | `roadmap`                                 |
| `zoom`         | Specifies the zoom of the map                     | `15`                                      |

### Formatting alarms text

Here is a basic guide to apply custom styles to alarm text:

| Style                              | Example                          |
|------------------------------------|----------------------------------|
| `_italics_`                        | *italics*                        |
| `*bold*`                           | **bold**                         |
| `_*bold italics*_`                 | ***bold italics***               |
| `~Strikethrough~`                  | ~~Strikethrough~~                |

You can see other options in the official Slack information about formatting text [here](https://get.slack.help/hc/en-us/articles/202288908-Format-your-messages).

## How to get a Slack API Key

1. Visit [slack.com](https://www.slack.com). Enter your email address and click 'Create your team'. Follow the instructions to setup and activate your account.

2. Go to the [create a bot page](https://my.slack.com/services/new/bot). Enter a username and click create.

3. Copy the API Token given. Fill out any more information you want, and click 'Save Integration'.
