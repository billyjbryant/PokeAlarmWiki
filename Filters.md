## Overview
* [Prerequisites](#prerequisites)
* [Introduction](#introduction)
* [Gym Filters](#gym-filters)
* [Pokestop Filters](#pokestop-filters)
* [Pokemon Filters](#pokemon-filters)
  * [Example Pokemon Config](#example-pokemon-config)
  * [Specific Pokemon Level Options](#specific-pokemon-level-options)
    * [Filtering on multiple combinations of parameters, a.k.a., "Multifiltering"](#filtering-on-multiple-combinations-of-parameters-aka-multifiltering)
* [Egg Filters](#egg-filters)
* [Raid Filters](#raid-filters)
* [Final Words](#final-words)

## Prerequisites
This guide assumes:

1. You have a working RocketMap installation
2. You are familiar with [JSON formatting](https://www.w3schools.com/js/js_json_intro.asp)
3. You are using the latest version of PokeAlarm

## Introduction

The `filters.json` configuration file filters Gym, Pokestop, Pokemon, Egg, and Raid notifications so that you receive only the notifications you want.

The basic structure of `filters.json` contains 5 sections:

```
{
    "gyms":{
        GYM_FILTERS_ARE_HERE
    },
    "pokestops":{
        POKESTOP_FILTERS_ARE_HERE
    },
    "pokemon":{
        POKEMON_FILTERS_ARE_HERE
    },
    "eggs": {
        EGG_FILTERS_ARE_HERE
    },
    "raids": {
        RAID_FILTERS_ARE_HERE
    }
}
```


**Note for location:** To utilize the distance feature of PokeAlarm, set your location set by passing `-l LOCATION` at the command line or adding `location:YOUR_LOCATION` in `config.ini`, located in the `config` directory of your PokeAlarm installation.

**Note for locale:** PokeAlarm will only notify on correctly spelled Pokemon for the selected locale.

## Gym Filters
You may filter on gyms based on team activity, and within a minimum / maximum distance from your set location.  Gyms within these parameters will trigger your Gym alarm notification.  You may have have multiple filter conditions within the same "gyms" filter block.

| Parameter        | Default | Description
|:----------------:|:-------:|:-------
| `enabled`        | `False` | Set to `True` to enable gym filtering
| `ignore_neutral` | `False` | Set to `True` to prevent the `"is now controled by team Neutral"` gym notifications
| `from_team`      |         | Array of defeated gym team names. Use `"Instinct"`, `"Mystic"`, or `"Valor"`. Wrap list in brackets `[ ]`. Each element must be in quotes, separated by comma `,`. 
| `to_team`        |         | Array of new gym owner team names. Use `"Instinct"`, `"Mystic"`, or `"Valor"`. Wrap list in brackets `[ ]`. Each element must be in quotes, separated by comma `,`.
| `min_dist`       | `"0"`   | Minimum distance to alarm on gym notifications.
| `max_dist`       | `"inf"` | Maximum distance to alarm on gym notifications. Use `"Inf"` to notify on any distance.


**Example: Alarm on all gym team activity at any distance**
```json
    "gyms":{
        "enabled":"True",
        "ignore_neutral":"False",
        "filters":[
            {
                "from_team":["Valor", "Instinct", "Mystic"], "to_team":["Valor", "Instinct", "Mystic"],
                "min_dist":"0", "max_dist":"inf"
            }
        ]
    },
```

**Example:  Alert on Instinct defeated gyms within 500m, or all Valor gym activity anywhere**
```json
    "gyms":{
        "enabled":"True",
        "ignore_neutral":"False",
        "filters":[
            {
                "from_team":["Instinct"], "to_team":["Valor", "Instinct", "Mystic"],
                "min_dist":"0", "max_dist":"500"
            },
            {
                "from_team":["Valor"], "to_team":["Valor"]
            }
        ]
    },
```

## Pokestop Filters
You may filter on lured pokestops within a minimum and maximum distance from your set location.  Lured pokestops within this range will trigger your Pokestop alarm notification.

```json
    "pokestops":{
        "enabled":"True",
        "filters":{ "min_dist":"0", "max_dist":"inf" }
    },
```

| Parameter        | Default | Description
|:----------------:|:-------:|:-------
| `enabled`        | `False` | Set to `True` to enable gym filtering
| `min_dist`       | `"0"`   | Minimum distance to alarm on pokestop notifications.
| `max_dist`       | `"inf"` | Maximum distance to alarm on pokestop notifications. Use `"Inf"` to notify on any distance.

## Pokemon Filters
You may set filters for Pokemon at 2 levels of the `"Pokemon":` section of `filters.json`:

1. Default level
2. Specific Pokemon level, e.g., at the bulbasaur line

Filters at the Default level will act as the default for all Pokemon.  Filters at the Specific Pokemon level will override the default settings and only apply to that particular Pokemon.

Below is an example of the Default level options:

```json
    "pokemon":{
        "enabled":"True",
        "default":{
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "4760", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "form": null, "ignore_missing": "False"
        },
```

Below is a description of all filters available at both Default and Specific Pokemon level:

| Parameter  | Default | Description |
|:-----------|:--------|:------------------------------------------------------------|
| `min_dist` | `"0"`   | Minimum distance from current location by which all Pokemon will trigger configured alarm(s).  Pokemon farther than this value will not trigger configured alarm(s). Can be an integer, e.g., `"1000"`.
| `max_dist` | `"inf"`  | Maximum distance from current location by which the specific Pokemon will trigger configured alarm(s). Pokemon farther than this value will not trigger configured alarm(s). Can be an integer, e.g., `"1000"`, or `"inf"` for unlimited range.
| `min_iv`   | `"0"`   | Minimum percent IV for all Pokemon. 0 to 100.
| `max_iv`   | `"100"` | Maximum percent IV for all Pokemon. 0 to 100.
| `min_cp`   | `"0"`   | Minimum CP for all Pokemon. 0 to 4760.
| `max_cp`   | `"4760"` | Maximum CP for all Pokemon. 0 to 4760.
| `min_level` | `"0"` | Minimum level for all Pokemon. 0 to 30.
| `max_level` | `"30"` | Maximum level for all Pokemon. 0 to 30.
| `min_atk` | `"0"` | Value, 0-15, corresponding to the attack IV 
| `max_atk` | `"15"`| Value, 0-15, corresponding to the attack IV 
| `min_def` | `"0"` | Value, 0-15, corresponding to the defense IV 
| `max_def` | `"15"`| Value, 0-15, corresponding to the defense IV 
| `min_sta` | `"0"` | Value, 0-15, corresponding to the stamina IV 
| `max_sta` | `"15"`| Value, 0-15, corresponding to the stamina IV
| `quick_move`   |  | Quick moves for the specific Pokemon.  Can contain multiple moves for the specific Pokemon, separated by whitespace. Set to `"all"` to notify on any quick moves.  Leaving out this parameter is the equivalent of notifying on all quick moves.
| `charge_move`   |  | Charge moves for the specific Pokemon.  Can contain multiple moves for the specific Pokemon, separated by whitespace. Set to `"all"` to notify on any charge moves.  Leaving out this parameter is the equivalent of notifying on all charge moves.
| `moveset` |  | Combination of `quick_move` and `charge_move`, useful for filtering on best attacking or defending moves. Format is`"moveset":[ "QUICK_MOVE_NAME/CHARGE_MOVE_NAME" ]`. Example - `"moveset":[ "Dragon Breath/Dragon Claw", "Steel Wing/Dragon Pulse" ]`
| `ignore_missing` | `True` | Prevents PokeAlarm from notifying on Pokemon without moves or IVs.  WARNING: enabling this could potentially result in losing a snorlax, lapras, dragonite, etc., if RocketMap screws up and sends the webhook without info. A better way is to configure your blacklist or whitelist in RocketMap and set the unwanted Pokemon in filters.json to `False`.
| `size` | `null` | Useful for the tiny Rattata and big Magikarp badges.  Filter with  `"tiny"`, `"small"`, `"normal"`, `"large"`, and `"big"`
| `gender` | `null` | Useful to collect pairs of pokemon.  Filter with  `"male`, `"female"`, and `"neutral"`
| `ignore_missing` | `True` | Prevents PokeAlarm from notifying on Pokemon without moves or IVs.  WARNING: enabling this could potentially result in losing a snorlax, lapras, dragonite, etc., if RocketMap errors and sends the snorlax webhook without info. A better way is to configure your blacklist or whitelist in RocketMap and set the unwanted Pokemon in filters.json to `False`.
  
### Example Pokemon Config

By default, PokeAlarm will notify on any Pokemon set to `"True"`, at any distance and with any %IV.  A config to notify for any Bulbasaur or Venusaur would look like below:
```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "4760", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":"True",
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```
**Setting Default Distance for all Pokemon.** Let's say we want to be notified of any Pokemon within **5000m** from our current location.  We update `"max_dist"` to accomplish this:
```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":"True",
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```
**Setting Default Minimum IV for all Pokemon.** Now we want to be notified of any **90%** IV Pokemon within **5000m** from our current location.  The `"max_dist"` and `"min_iv"` parameters apply to all Pokemon that are set to `"True"`. Our config would look like below:
```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":"True",
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```
## Specific Pokemon Level Options
At the lower specific Pokemon level, each Pokemon can have a configuration that will override the defaults that was set at the Default level.

**Filtering on a maximum range for a specific Pokemon.**  Add a `max_dist` key-value pair to the specific Pokemon.  Remember this will override the `max_dist` at the top `"pokemon":` level.  In the example below, Bulbasaur within 1000m will trigger the alarm.  A Venusaur at any distance will trigger the alarm.
```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":{ "max_dist":"1000" },
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```

**Filtering on minimum %IV for a specific Pokemon.**  Add a `min_iv` key-value pair to the specific Pokemon.  Remember this will override the `min_iv` at the top `"pokemon":` level.  In the example below, a 100% perfect Bulbasaur  will trigger the alarm.  Any Venusaur will trigger the alarm.
```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":{ "min_iv":"100" },
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```

**Filtering on minimum Level for a specific Pokemon.**  Add a `min_level` key-value pair to the specific Pokemon.  Remember this will override the `min_level` at the top `"pokemon":` level.  In the example below, a Level 20+ Bulbasaur  will trigger the alarm.  Any Venusaur will trigger the alarm.
```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":{ "min_level":"20" },
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```

**Filtering on quick and charge moves.** Add the `move_1` and/or `charge_move` key-value pair to the specific Pokemon.  Wrap each move in double quotes.  Separate multiple moves with a comma. In the example below, We want to be notified of Bulbasaur with either Power Whip or Seed Bomb as a charge move. Separate each move by a comma, and wrap the list in brackets `[ ]`. The Pokemon example configuration for Bulbasaur would be as below:

```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":{ "quick_move": ["Power Whip", "Seed Bomb"] },
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```



**Filtering on a moveset.** A `moveset` is a combination of `move_1` and `charge_move`, useful for filtering on best attacking or defending moves.  Below is an example Pokemon configuration for Bulbasaur with two movesets:

```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":{ "moveset": ["Vine Whip/Seed Bomb", "Tackle/Power Whip"] },
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```

**Filtering on a combination of parameters.** Let's say we want to be notified of any Pokemon with at least 90% IVs within 5000m of our set location. We also want to be notified of 100% IV Bulbasaur within 1000m of our location with either Power Whip or Seed Bomb as a charge move.  We don't want Ivysaur or Charmander.  We do want 90%+ Venusaurs within 5000m. The Pokemon configuration would be as below:
```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":{ "max_dist":"1000", "min_iv":"100", "charge_move":["Power Whip", "Seed Bomb"] },
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```

**Other combination of parameters with CP and IV.** Let's say we want to be notified of any Pokemon with at least 90% of IVs and 600 minimum CP. The Pokemon configuration would be as below:
```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "600", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur":"False",
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```

### Filtering on multiple combinations of parameters, a.k.a., "Multifiltering".
You can have multiple filter sets for a particular pokemon.  For example, if we want either:

1) A perfect Bulbasaur within 100m that has either Power Whip or Seed Bomb as a charge move, or
2) Any Bulbasaur within 150m

we would configure our `filters.json` for Bulbasaur like so:

```json
    "pokemon":{
        "enabled":"True",
        "default": {
            "min_dist":"0", "max_dist":"5000", "min_cp": "0", "max_cp": "4760", "min_iv":"90", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null, "min_level":"0", "max_level":"30",
            "size": null, "gender": null, "ignore_missing": "False"
        },
        "Bulbasaur": [
            { "max_dist":"1000", "min_iv":"100", "charge_move":["Power Whip", "Seed Bomb"] },
            { "max_dist":"150" }
        ],
        "Ivysaur":"False",
        "Venusaur":"True",
        "Charmander":"False"
    }
```

**Filtering on size.**

You may filter on pokemon size.  Options are, from smallest to largest: `"tiny"`, `"small"`, `"normal"`, `"large"`, or `"big"`.  Options must be wrapped in `[ ]`, similar to `quick_move`, `charge_move`, and `moveset`.

Example:  Filtering for tiny Ratatta or big Magikarp
```json
"Rattata":{"size":["tiny"] },
"Magikarp":{ "size":["big"] },
```

**Filtering on gender.**

You may filter on pokemon gender.  Options are: `"male"`, `"female"` or `"neutral"`.  Options must be wrapped in `[ ]`, similar to `quick_move`, `charge_move`, and `moveset`.

Example: Filtering for female Venusaur or male Pikachu:
```json
"Venusaur":{"gender":["female"] },
"Pikachu":{ "gender":["male"] },
```

## Egg Filters
You may filter eggs based on raid levels, gym sponsorship, or choose to ignore all eggs.

The raid levels go from 1 to 5, where 1 is easiest and 5 is a legendary raid.

These are filter settings that controls what kind of eggs will get an alarm.

| Parameter        | Default | Description
|:----------------:|:-------:|:-------
| `enabled`        | `"False"` | Set to `"True"` to enable egg alarms
| `min_level`      | `"1"`   | Minimum raid level to send egg alarm
| `max_level`      | `"5"` | Maximum raid level to send egg alarm
| `sponsored_gym`  | `"False"` | Must be at sponsored gym to send egg alarm


Example eggs filter:
```json
     "eggs": {
        "enabled":"True",
        "sponsored_gym":"False",
        "min_level": "1",
        "max_level": "5"
    },
```

## Raid Filters
You may filter raids by the specific raid pokemons in a similar fashion as the pokemon filter.

Raid filters has 4 sections: enabled, gym sponsorship, default pokemon filtering and individual pokemon filtering

| Parameter        | Default | Description
|:----------------:|:-------:|:-------
| `enabled`        | `"False"` | Set to `"True"` to enable raid alarms
| `sponsored_gym`  | `"False"` | Must be at sponsored gym to send raid alarm

```json
    "raids": {
        "enabled": "False",
        "sponsored_gym":"False",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "999999", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null,
            "size": null, "gender": null, "form": null,"ignore_missing": "False"
        },
        "Tyranitar": { "True" } 
    }
```

You may set filters for Raid pokemon at 2 levels of the `"raid":` section of `filters.json`:

1. Default level
2. Specific Pokemon level, e.g., at the bulbasaur line

Filters at the Default level will act as the default for all Pokemon.  Filters at the Specific Pokemon level will override the default settings and only apply to that particular Pokemon. 

**NB** Do note that any pokemon missing in the filter will be treated as disabled and will not trigger an alarm.

Below is an example of the Default level options:

```json
    "default":{
        "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "999999", "min_iv":"0", "max_iv":"100",
        "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
        "quick_move": null, "charge_move": null, "moveset": null,
        "size": null, "gender": null, "form": null, "ignore_missing": "False"
    }
```

**NB** The raid pokemon notification can currently ONLY be filtered by CP, quick move and charge move. The other filters are simply included for completeness and will not truly filter your raid alarms. 

Below is a description of all meaningful filters available at both Default and Specific Pokemon level:

| Parameter  | Default | Description |
|:-----------|:--------|:------------------------------------------------------------|
| `min_dist` | `"0"`   | Minimum distance from current location by which all Pokemon will trigger configured alarm(s).  Pokemon farther than this value will not trigger configured alarm(s). Can be an integer, e.g., `"1000"`.
| `max_dist` | `"inf"`  | Maximum distance from current location by which the specific Pokemon will trigger configured alarm(s). Pokemon farther than this value will not trigger configured alarm(s). Can be an integer, e.g., `"1000"`, or `"inf"` for unlimited range.
| `min_iv`   | `"0"`   | Minimum CP for the raid pokemon
| `max_iv`   | `"999999"` | Maximum CP for the raid pokemon
| `quick_move`   |  | Quick moves for the specific Pokemon.  Can contain multiple moves for the specific Pokemon, separated by whitespace. Set to `"all"` to notify on any quick moves.  Leaving out this parameter is the equivalent of notifying on all quick moves.
| `charge_move`   |  | Charge moves for the specific Pokemon.  Can contain multiple moves for the specific Pokemon, separated by whitespace. Set to `"all"` to notify on any charge moves.  Leaving out this parameter is the equivalent of notifying on all charge moves.
| `moveset` |  | Combination of `quick_move` and `charge_move`, useful for filtering on best attacking or defending moves. Format is`"moveset":[ "QUICK_MOVE_NAME/CHARGE_MOVE_NAME" ]`. Example - `"moveset":[ "Dragon Breath/Dragon Claw", "Steel Wing/Dragon Pulse" ]`

### Example Egg and Raid Configuration

**Report all Eggs and Raids**

Reports all known egg levels and raid pokemon

```json
    "eggs": {
        "enabled":"True",
        "min_level": "1",
        "max_level": "5"
    },
    "raids": {
        "enabled": "True",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "999999", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null,
            "size": null, "gender": null, "form": null, "ignore_missing": "False"
        },
        "Bayleef":"True",
        "Croconaw":"True",
        "Magikarp":"True",
        "Quilava":"True",
        "Electabuzz":"True",
        "Exeggutor":"True",
        "Magmar":"True",
        "Muk":"True",
        "Weezing":"True",
        "Alakazam":"True",
        "Arcanine":"True",
        "Flareon":"True",
        "Gengar":"True",
        "Jolteon":"True",
        "Machamp":"True",
        "Vaporeon":"True",
        "Blastoise":"True",
        "Charizard":"True",
        "Lapras":"True",
        "Rhydon":"True",
        "Snorlax":"True",
        "Tyranitar":"True",
        "Venusaur":"True",
        "Articuno":"True",
        "Zapdos":"True",
        "Moltres":"True",
        "Mewtwo":"True",
        "Mew":"True",
        "Raikou":"True",
        "Entei":"True",
        "Suicune":"True",
        "Lugia":"True",
        "Ho-Oh":"True",
        "Celebi":"True"
    }
```

**Only report raids once we know the pokemon, ignore eggs**

```json
    "eggs": {
        "enabled":"False",
        "min_level": "1",
        "max_level": "5"
    },
    "raids": {
        "enabled": "True",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "999999", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null,
            "size": null, "gender": null, "form": null, "ignore_missing": "False"
        },
        "Bayleef":"True",
        "Croconaw":"True",
        "Magikarp":"True",
        "Quilava":"True",
        "Electabuzz":"True",
        "Exeggutor":"True",
        "Magmar":"True",
        "Muk":"True",
        "Weezing":"True",
        "Alakazam":"True",
        "Arcanine":"True",
        "Flareon":"True",
        "Gengar":"True",
        "Jolteon":"True",
        "Machamp":"True",
        "Vaporeon":"True",
        "Blastoise":"True",
        "Charizard":"True",
        "Lapras":"True",
        "Rhydon":"True",
        "Snorlax":"True",
        "Tyranitar":"True",
        "Venusaur":"True",
        "Articuno":"True",
        "Zapdos":"True",
        "Moltres":"True",
        "Mewtwo":"True",
        "Mew":"True",
        "Raikou":"True",
        "Entei":"True",
        "Suicune":"True",
        "Lugia":"True",
        "Ho-Oh":"True",
        "Celebi":"True"
    }
```

**Only report Eggs level 4 and 5, do not send alarms about pokemon**
 
```json
    "eggs": {
        "enabled":"True",
        "min_level": "4",
        "max_level": "5"
    },
    "raids": {
        "enabled": "False",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "999999", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null,
            "size": null, "gender": null, "form": null, "ignore_missing": "False"
        }
    }
```

**Report Eggs level 3 and higher, but only some specific raid pokemon**

 ```json
    "eggs": {
        "enabled":"True",
        "min_level": "3",
        "max_level": "5"
    },
    "raids": {
        "enabled": "True",
        "default": {
            "min_dist":"0", "max_dist":"inf", "min_cp": "0", "max_cp": "999999", "min_iv":"0", "max_iv":"100",
            "min_atk": "0", "max_atk":"15", "min_def": "0", "max_def":"15", "min_sta": "0", "max_sta":"15",
            "quick_move": null, "charge_move": null, "moveset": null,
            "size": null, "gender": null, "form": null, "ignore_missing": "False"
        },
        "Alakazam":"True",
        "Machamp":"True",
        "Blastoise":"True",
        "Charizard":"True",
        "Lapras":"True",
        "Rhydon":"True",
        "Snorlax":"True",
        "Tyranitar":"True",
        "Venusaur":"True",
        "Articuno":"True",
        "Zapdos":"True",
        "Moltres":"True",
        "Mewtwo":"True",
        "Mew":"True",
        "Raikou":"True",
        "Entei":"True",
        "Suicune":"True",
        "Lugia":"True",
        "Ho-Oh":"True",
        "Celebi":"True"
    }
 ```

**Filter specific raid pokemon**

Filters for specific raid pokemon is the same as for the pokemon section of filters.json, see [Example Specific Pokemon Config](#example-specific-pokemon-config).

## Final Words
The `filters.json.example` file contains the entire list of generation 1 and 2 Pokemon, and includes some examples of how the various parameters can be used to fit your needs.  Add the optional key-value pairs for each of your specific Pokemon. Remember that keys such as `max_dist` and `min_iv` set at the specific Pokemon level will override the values of `max_dist` and `min_iv` set at the Default level.
