## Overview

* [Prerequisities](#prerequisites)
* [Introduction](#introduction)
* [Parameters](#parameters)

## Prerequisites
This page assumes:

1. You have a working scanner.
2. You are familiar with
[JSON formatting](https://www.w3schools.com/js/js_json_intro.asp).
3. You are using the latest version of PokeAlarm.
4. You have read and understood the [Filters Overview](filters_overview)
page.

## Introduction

The `"monsters"` section has three distinct settings.

| Setting Name         | Description                                   |
| -------------------- |---------------------------------------------- |
| enabled              | Process Monster Events only if `true`         |
| defaults             | See [filters](fitlers#defaults) page on defaults|
| filters              | See below parameters                           |

## Parameters

Monster Filters can use the following parameters to filter Events:

| Parameter            | Description                         | Example |
| -------------------- |------------------------------------ |---------|
| monsters | Array of allowed monsters, by id or name. | `[ "Bulbasaur", "2", 3 ]`|
| min_dist | Min distance of event from set location in miles or meters (depending on settings). | `0.0` *|
| max_dist | Max distance of event from set location in miles or meters (depending on settings). | `1000.0` *|

`*` - Floats can use `"inf"` to represent infinity

To do:
```json
                "min_dist": 0, "max_dist": "inf",
                "min_lvl": 0, "max_lvl": 40, "min_atk": 0, "max_atk": 15,
                "min_def": 0, "max_def": 15, "min_sta": 0, "max_sta": 15,
                "min_iv": 0.0, "max_iv": 100,
                "form_ids": [ 0 ],
                "quick_moves": [ "Vine Whip", "Tackle"],
                "charge_moves": [ "Sludge Bomb", "Seed Bomb"],
                "genders": [ "male", "female", "neutral"],
                "min_height": 0, "max_height": "inf",
                "min_weight": 0, "max_weight": "inf",
                "sizes": [ "tiny", "small", "normal", "large", "big" ],
                "geofences": [ "Central Park" ],
                "custom_dts": { "key1": "value1", "key2": "value2" },
                "is_missing_info": false
```