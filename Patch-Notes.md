## Patch History

* [Patch 3.5](#patch-35)
* [Patch 3.4](#patch-34)
* [Patch 3.3](#patch-33)
* [Patch 3.2](#patch-32)
* [Patch 3.1](#patch-31)

---

## Patch 3.5

This patch contains several breaking changes including changes made to completely re-work the Filters subsystem.
*Please see the [**Wiki**](https://github.com/PokeAlarm/PokeAlarm/wiki) and `filters.json.example` for information on how these changes impact your setup.*

### Filters

* **Breaking Change** - Reworked PA Filter system to improve performance and reduce overall resource requirements.

* Expanded Filter system to allow multiple combinations of filter options, this change allows filter files to be combined and reduces the total number of managers necessary. 

* **All**
    * All instances of `pkmn` replaced with `mon`
    * `missing_info` - Multiple changes have been made, refer to [**Missing Info**](Filters-Overview#missing-info) for more details on new functionality.
    * `image_url` changed to `gym_image`
    * `custom_dts` added to pass a Custom Dynamic Text Substitution to the Alert system, see [**Custom DTS**](Filters-Overview#custom-dts) for more details.

* **Gyms/Raids/Eggs**
    * `gym_name_contains` - Added functionality to filter Gym/Raid/Egg events based on the cached name of the gym using a list of RegExs.  See [**Filters**](Gym-Filters#parameters) for more details.

* **Gyms**
    * `min_slots` - Allows filtering gyms based on the minimum number of guard slots available.
    * `max_slots` - Allows filtering gyms based on the maximum number of guard slots available.

* **Raids**
    * `min_lvl` & `max_lvl` changed to `min_raid_lvl` and `max_raid_lvl`
    * `quick_move_id` changed to `quick_id`
    * `charge_move_id` changed to `charge_id`

### Server Settings

* Reworked PA Event handling system to improve performance and reduce resource utilization.

### Alarms

* **Discord/Slack/Facebook Pages**
    * Updated Image Structure for `icon_url` and `avatar_url`
    * Implemented changes to the default image urls to help bust Discord and Slack image caching

* **Discord** `webhook_url` now accepts DTS elements

* **Twilio** now supports using an array of numbers for the `to` field in Alert configurations.


### Data Files

* Added Gen3 Movesets
* Updated Kyogre and Groudon stats
* Added Portuguese language translations

### Dynamic Text Substitutions

* **All**
    * **Custom_DTS**
        Added support for Custom DTS Fields to be passed using the `custom_dts` Filter
    * `pkmn` changed to `mon_name`
    * `pkmn_id` changed to `mon_id`
    * `pkmn_id_3` changed to `mon_id_3`
    * `gym_image_url` changed to `gym_image`

* **Pokemon**
    * `tiny_rat` - Updated to use weight instead of size.
    * `big_karp` - Updated to use weight instead of size.
    * `enc_id` changed to `encounter_id`
    * `form_id_3_or_empty` replaced with `form_id_3`
    * `form_name` added to retrieve the name of a monster's form

* **Pokestops**
    * `id` changed to `stop_id`

* **Raids**
    * `team_leader` - Reports the name of the team leader of the team in control of the gym.
    * `raid_level` changed to `raid_lvl`
    * `raid_end` changed to `raid_time_left`

* **Eggs**
    * `raid_level` changed to `egg_lvl`
    * `raid_begin` changed to `hatch_time_left`
    * `raid_end` changed to `raid_time_left`

* **Gyms**
    * `slots_available` - displays the number of open guard slots available in a gym
    * `guard_count` - displays the number of guards assigned to a gym

---

## Patch 3.4

### Server Settings

* Implemented caching for managers with `-ct` or `--cache_type`. Cache is used to handling remembering things like pokemon history and gym names.
    * (default) `mem` - data persists only in memory.
    * `file` - data is saved and loaded from a file.
* PA now gracefully exits on ctrl+c, finishing items in queue and logging the status of managers.

### Dynamic Text Substitutions

* **All**

    * `lat_5` and `lng_5` - Latitude and Longitude for the event, truncated to 5 decimal places.
    * `address_eu` - same as `<street> <street_num>`.

* **Pokemon**

    * `form_or_empty` - Reports the form name of the pokemon if known, or an empty string if unknown.
    * `pkmn_id_3` - pokemon id padded to 3 digits.
    * `form_id_or_emtpy` - prints form_id padded to 3 digits if known, or and empty string if unknown.

* **Raids**
    * `form_or_empty` - Reports the form name of the pokemon if known, or an empty string if unknown.
    * `team_name` - Reports the current team for a Raid gym if known, or 'unknown' if not.
    * `team_id` - Reports the current team id for a Raid gym if known, or '?' if not.
    * `min_cp` and `max_cp` - min and max potential cp for the raid boss if caught.
    * `pkmn_id_3` - pokemon id padded to 3 digits.
    * `form_id_or_emtpy` - prints form_id padded to 3 digits if known, or and empty string if unknown.

### Tools

* Webhook Tester
    * Added `webhook_tester.py` for users to simulate sending events.

---

## Patch 3.3

### Dynamic Text Substitution
* **Pokemon**
    * (Re) added `'form'` DTS (and made it more robust).

#### Server Settings
* **Reverse Location**
    * Location services have been completely reworked to be more reliable.
    * Location results now retrieve results set in whatever language is set for that manager.
    * Location results now cache in memory - lowering overall API usage.

---

## Patch 3.2

#### Filters
* **Pokemon**
    * Pokemon can now be filtered by their form_id (example": `"form": [0, 1, 2]`).
    * Pokemon can now be filtered by their gender (example": `"gender": ["male", "female"`).
    * `'min_cp'` and `'max_cp'` are used to limit the CP of pokemon to alert.
* **Egg**
    * "Egg" section is now added to the filters.json.
    * `'enabled'` allows incoming raid alerts to be toggled on or off.
    * `'min_level'` and `'max_level'` are used to limit the levels of incoming raids to alert on.
* **Raids**
    * "Raids" section is now added to the filters.json.
    * `'enabled'` allows raid alerts to be toggled on or off.
    * `'default'` allows setting default settings for filters, and has the same options as pokemon filters do.
    *  Filters for pokemon raids can be set similar to the "pokemon" section, with the `"name": {}` scheme.

#### Dynamic Text Substitution
* **General**
    * Added `'applemaps'` to most alerts for AppleMaps link for iOS users.
    * Time DTS now switch to ##h##m format when longer than one hour.
* **Pokemon**
    * Added support for the following substitutions: `'cp', 'form', 'form_id', 'level'`.
    * Added `'big_karp'` that displays 'big' on big magikarp and `'tiny_rat'` that displays 'tiny' on small rattata.
* **Egg**
    * "Egg" alerts have the following substitutions: `'type', 'id', 'raid_level', 'raid_end', 'raid_being', 'lat', 'lng', 'gym_name', 'gym_description', 'gym_url'`.
* **Raid**
    * "Raid" alerts have the following substitutions: `'type', 'id', 'pkmn_id', 'cp', 'quick_id', 'quick_damage', 'quick_dps', 'quick_duration', 'quick_energy', 'charge_id', 'charge_damage', 'charge_dps', 'charge_duration', 'charge_energy', 'raid_level', 'raid_end', 'raid_begin' 'lat', 'lng', 'gym_name', 'gym_description', 'gym_url'`.
* **Gyms**
    * Added the following Gym substitutions: `'name', 'description', 'url'`.
    * Added the following 'Gym Leader' substitutions: `'old_team_id', 'new_team_leader', 'old_team_leader'`.

#### Alarms
* **Twitter**
    * Twitter now automatically truncates to 140 characters - it will preserve <gmaps> links if added to the end.
* **Discord**
    * Discord usernames now automatically truncates usernames to 32 characters.
    * Added `'avatar_url'` to change the avatar of the post.
    * Added `content` field that posts a message before the embed, allows for taging by user or roll.
    * Added `'disable_embed'` that disables the discord embeds when set to `true`.

---

## Patch 3.1

#### Filters
* **Multifilters** - Each filter can now be one of the following:
     * `'True'` for all defaults, `'False'` for entirely disabled.
     * A single filter in a json object (Ex: `{ "min_iv":"0",  "max_iv" : "0" ... }`).
     * Multiple filters inside an array: `[{"min_iv":"0", "max_iv":"25" }, {"min_iv":"75", "max_iv":"100"} ]`.

* **Pokemon**
    * All default filter parameters are now encapsulated in a single filter under the label `default`.
     * `size` requires an array of valid sizes from the following: `['tiny', 'small', 'normal', 'large', 'big']`.
     * `move_1` and `move_2` are now `quick_move` and `charge_move`.
     * `size`, `quick_move`, `charge_move`, `moveset` filter parameters will accept any value when set to `null`.

* **Pokestops**
    * Now only two fields: `enabled` and `filters`.
    * `filters` supports the Multifilter format with `min_dist` and `max_dist` fields.

* **Gym**
    * Now only three fields `enabled`, `ignore_neutral`, and `filters`.
    * `filters` supports multifilters with the following fields:
        * `from_team` - Array of valid previous team names.
        * `to_team` - Array of valid current team names.
        * `min_dist` and `max_dist` working as before.

#### Dynamic Text Substitution
* `<geofence>` added - the name of the first geofence in which the notification is in.
* `<size>` now list either `'tiny'`, `'small'`, `'normal'`, `'large'`, or `'big'`.
* Quick moves now use the following:  `<quick_move>`, `<quick_id>`, `<quick_damage>`, `<quick_dps>`, `<quick_duration>`, `<quick_energy>`.
* Charges moves now use the following: `<charge_move>`, `<charge_id>`, `<charge_damage>`, `<charge_dps>`, `<charge_duration>`, `<charge_energy>`.

#### Alarms
* All Services
    * `startup_list` is officially gone.
* Boxcar
    * No longer supported.
* Discord
    * `api_key` renamed to `webhook_url`.
    * Will now retry if webhook was not received correctly.
    * New optional alarm and alert level field: `map`.
        * `enabled` - True of False to enabled/disabled map.
        * Added other static map parameters.
* Slack
    * `channel` parameter is now required at the Alarm level.
        * Slack will no longer default to general if the correct channel cannot be found.
        * Slack will still default to the Alarm level channel over the Alert level channel is not found (so everyone can still use `<pkmn>`!).
* Pushover
    * No longer supported.
