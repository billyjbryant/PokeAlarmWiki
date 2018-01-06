## Overview

* [Prerequisites](#prerequisites)
* [Available DTS](#available-dts)

## Prerequisites

This page assumes:

1. You have a working scanner.
2. You read and understood the [DTS](Dynamic-Text-Substitution) page.
3. You are using the latest version of PokeAlarm.


## Available DTS

| DTS          | Description                                           |
|------------- |------------------------------------------------------ |
| gym_id       | The gym id. Unique per gym.                           |
| time_left    | Time remaining until the lure expires.                |
| lat          | Latitude of the stop.                                 |
| lng          | Longitude of the stop.                                |
| lat_5        | Latitude of the stop, truncated to 5 decimal places.  |
| lng_5        | Longitude of the stop, truncated to 5 decimal places. |
| distance     | Distance of the monster from the set location.        |
| direction    | Cardinal direction of the monster, from the set location.|
| gmaps        | Google Maps link to the location of the monster.      |
| applemaps    | Apple Maps link to the location of the monster.       |
| geofence     | Geofence around the event. See 'Geofences' section from [filters](Filters-Overview#geofence) page.|
| old_team     | The team in control of the gym previously.            |
| old_team_id  | The id of the team in control of the gym previously.  |
| old_team_leader| The leader of the team in control of the gym previously.|
| new_team     | The team currently in control of the gym.             |
| new_team_id  | The id of the team currently in control of the gym.   |
| new_team_leader| The leader of the team currently in control of the gym.|
| gym_name     | * The name of the Gym.                                |
| gym_description | * The description of the Gym.                      |
| gym_image     | * The url to the image of the Gym.                   |

\* Gym Info require caching. See the
[Object Caching](Object-Caching) page for more information.
