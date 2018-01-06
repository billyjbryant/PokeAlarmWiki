## Overview
This guide contains an overview to the different parts of configuring and customizing PokeAlarm to fit your needs.

* [Prerequisites](#prerequisites)
* [Goals](#goals)
* [Setting up an Alarm](#setting-up-an-alarm)
* [Setting up Filters](#setting-up-filters)
  * [Pokemon Filters](#pokemon-filters)
  * [Gym Filters](#gym-filters)
  * [Raid Filters](#raid-filters)
* [Customizing Alert Text](#customizing-alert-text)
* [Adding Additional Managers](#adding-additional-managers)

## Prerequisites
This guide assumes the following:

1. You have correctly [installed PokeAlarm](installation), including setting up a source for your information.

2. You are using Notepad++, Nano, or Vi(m) to configure any files. Do **NOT** use or open any files with Notepad or TextEdit - they will break your files!

## Goals

This guide will walk you through setting up and customizing PokeAlarm to fit your needs. Specifically, we will go through setting up a PA server to do the following:

1. Use Discord
2. Alert when Bulbasaur, Squirtle, and Charmander are found.
3. Alert when a Dratini is found with 95% IV's or higher
4. Alert when an Instinct Gym falls (since that is clearly the superior team)
5. Alert when a Raid appears for Legendary Birds
6. Customize the Pokemon and Raid Alerts
6. Send different alerts to different channels

## Setting up an Alarm

In PokeAlarm, an 'Alarm' is a block in the Alarms.json section that represents how you want to receive an alert. For our example, we will use Discord, but the steps are similar between types of Alarms.

To create a Discord Alarm, you should be familiar with these two pages:
1. Learn how to configure your alarms file with the [Alarms](alarms) wiki page.
2. Learn how to set up a Discord webhook url with the [Discord](discord) wiki page.

Once we have our webhook url set up, we can edit the default discord alarm in the `alarms.json` file:  
```json
[
  {
    "active": "True",
    "type": "discord",
    "webhook_url": "https://discordapp.com/api/webhooks/1234567890"
  }
]
```
**Note**: for brevity only the discord section is shown

Try starting PokeAlarm - If you have configured your Alarm correctly, you should see a start up message posted in your discord channel!

## Setting up Filters

To learn all the ins and outs of Filters, you should checkout the [Filters](Filters-Overview) wiki page. There are 5 sections in a filters file: `pokemon`, `pokestops`, `gyms`, `eggs`, and `raids`.

### Pokemon Filters
In PokeAlarm, 'Filters' are used to determine if a notification needs to be sent or not. For example, when PokeAlarm receives information about a Pokemon, it compares it to the filters set for that Pokemon. If it fits the criteria, PA will send to the the Alarm we set up in the last section.

First we want to make sure that Bulbasaur, Squirtle, and Charmander notifications are sent. We can set them up with the default filter by setting them to `"True"`.
```json
{
  "pokemon":{
     "enabled":"True",
      "default": {
        "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "4760", "min_iv":"0", "max_iv":"100",
        "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
        "quick_move": null, "charge_move": null, "moveset": null,
        "size": null, "gender": null, "form": null,
        "ignore_missing": "False"
      },
      "Bulbasaur": "True",
      "Ivysaur": "False",
      "Venusaur": "False",
      "Charmander": "True",
      "Charmeleon": "False",
      "Charizard": "False",
      "Squirtle": "True",
      "Wartortle": "False",
      "Blastoise": "False"
  }
}
```
**Note**: for brevity only part of the filters file is shown

Next, we went to allow Dratini, but only for a certain IV range. For this to work, we want to override the default filter for ivs with the following:`"min_iv"` and `"max_iv"`. We can set Dratini as follows:
```
"Dratini": { "min_iv":"95", "max_iv":"100"},
```
This will create a filter that is identical to the default except for the two fields we overwrote.

### Gym Filters

Next we want to know whenever an Instinct gym goes down. Our Gym section is as follows:
```json
{
    "gyms":{
      "enabled":"False",
      "ignore_neutral":"True",
      "filters":[
        {
          "from_team":["Instinct"],
           "to_team":["Valor", "Instinct", "Mystic"],
           "min_dist":"0", "max_dist":"inf"
        }
      ]
    }
}
```
**Note**: for brevity only part of the filters file is shown

This filter will allow in any gym that switches from Instinct to any other team. Additionally, we don't want to know about when it switches to neutral, so we set `"ignore_neutral":"True"`. You can list multiple different filters in the filters section of gyms, and PA will check them one by one.


### Raid Filters

Raid filters work very similar to Pokemon Filters. You can set up alerts for any pokemon - not just the ones in the example. Here is the Raid section of a filters file set up for legendary birds:
```json
{
    "raids":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "999999", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null,
            "size": null, "gender": null, "form": null, "ignore_missing": "False"
        },
        "Articuno":"True",
        "Zapdos":"True",
        "Moltres":"True",
        "Lugia":"True",
        "Ho-Oh":"True"
    }
}
```
**Note**: for brevity only part of the filters file is shown

## Customizing Alert Text

PokeAlarm allows you to customize the Alerts that it sends out. We want to customize our Raid Alerts to show a more appropriate message. To add custom text to a Discord Alarm, you should be familiar with three pages:
1. Learn how to configure your alarms file with the [Alarms](alarms) wiki page.
2. Learn what fields Discord has to change with [Discord](discord) wiki page.
3. Learn what DTS options are available to you with the [Dynamic Text Substitution](dynamic-text-substitution) wiki page.
```json
[
  {
    "active": "True",
    "type": "discord",
    "webhook_url": "https://discordapp.com/api/webhooks/1234567890",
    "raids":{
      "title": "The Legendary Bird <pkmn> has appeared! It has <cp> CP!"
    }
  }
]
```
**Note**: for brevity only part of the alarms file is shown

 Dynamic Text Solutions change depending on the information present. If a Moltres were to appear, it should show up with the title "The Legendary Bird Moltres has appeared! It has 9999 CP!". If PA doesn't have that information, it will show either '?', 'unkn', or 'unknown'. If you get these in your alert, you will need to tweak your scanners to send the correct information.

You can customize any notification by adding the proper alert settings to your alarms file. If we wanted to customize pokemon alerts, we use the following:
```json
[
  {
    "active": "True",
    "type": "discord",
    "webhook_url": "https://discordapp.com/api/webhooks/1234567890",
    "pokemon":{
      "title": "The starter <pkmn> jumped out of the bushes!",
      "body": "It has an IV of <iv>%!"
    },
    "raids":{
      "title": "The Legendary Bird <pkmn> has appeared! It has <cp> CP!"
    }
  }
]
```
**Note**: for brevity only part of the alarms file is shown

You can customize more than just the title - each service have many different options. Check out the wiki page for your specific service for a full list.

## Adding Additional Managers

Now that we've customized our message, we have a little problem. We are getting messages for Dratini, that say "The starter Dratini jumped out of the bushes!". But Dratini isn't a starter pokemon! Let's add a second manager that allow us to customize even more.

You can find out more about Manager's and their settings on the [Managers](managers) wiki page.

First, lets make two different filter files. We will call the following filter file `starters_filter.json`:
```json
{
  "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "4760", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null,
            "size": null, "gender": null, "form": null,
            "ignore_missing": "False"
        },
        "Bulbasaur":"True",
        "Charmander":"True",
        "Squirtle": "True"
  }
}
```
**Note**: for brevity only part of the filters file is shown

Next, we can make a second filter file for Dratini. We will call this one `dragon_filters.json`:
```json
{
  "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "4760", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null,
            "size": null, "gender": null, "form": null,
            "ignore_missing": "False"
        },
        "Dratini":"True"
  }
}
```
**Note**: for brevity only part of the filters file is shown

We want the alerts from `starter_filters.json` to be different than the ones from `dragon_filters.json`. So we need two different alarms files. Here is what our `starter_alarms.json` file looks like:
```json
[
  {
    "active": "True",
    "type": "discord",
    "webhook_url": "https://discordapp.com/api/webhooks/1234567890",
    "pokemon":{
      "title": "The starter <pkmn> jumped out of the bushes!",
      "body": "It has an IV of <iv>%!"
    }
  }
]
```
As you can see, it is the same one as before. Here is what the `dragons_alarms.json` file looks like:
```json
[
  {
    "active": "True",
    "type": "discord",
    "webhook_url": "https://discordapp.com/api/webhooks/1234567890",
    "pokemon":{
      "title": "The high IV dragon <pkmn> appears with a roar!",
      "body": "It has an IV of <iv>%!"
    }
  }
]
```

Now we have two different filters and two different alarm files. Now we just need to set up the manager and link the filter to the alarm we want to use. When we start PokeAlarm, we can use the following:
```
python start_pokealarm.py -m 2 -f starter_filters.json -a starter_alarms.json -f dragon_filters.json -a dragon_filters.json
```

The `-m` flag tells PA that we want 2 managers. The first manager uses the first file specified by `-f` and the first file specified by `-a`. The second manager uses the second filter and second alarm file. You can keep going and add as many managers as you would like! Managers run in seperate processes, so they run concurrently and scale well with the number of cores on your machine.

Managers are a great tool that allow you to mix and match almost every setting to any filter or alarm settings. For full details on the power of Managers, don't forget to check out [Managers](managers) wiki page.
