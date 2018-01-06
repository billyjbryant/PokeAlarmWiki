## Overview
* [General Info](#general-info)
* [Troubleshooting](#troubleshooting)

## General Info

#### What scanners are supported?
* Currently RocketMap and Monocle are compatible with PA. Any others following our [Webhook Standards](webhook-standard) should work as well. The PA team does NOT help with troubleshooting scanners - they have their own resources you can use.

#### I have a great idea for a new feature!
* Open a Feature Request on our Github page. Use the [correct template](https://github.com/PokeAlarm/PokeAlarm/issues/new) - Do NOT post requests in Discord!

#### Do you support new services?
* Please check in the [New/Upcoming Services MEGA-THREAD](https://github.com/RocketMap/PokeAlarm/issues/147) to see if the service has already been requested. If it hasn't, feel free to leave a post requesting it.

## Troubleshooting

#### I am receiving an Error from RocketMap! What do I do?
* Check the [RocketMap Wiki](https://rocketmap.readthedocs.io/en/develop/) or the ask in the RocketMap Discord. We will not troubleshoot your RocketMap installation.

#### PokeAlarm outputs an alarm about 'ValueError' when starting up?
1. Make sure you didn't open any files with Notepad or TextEdit - they can break the encoding on your files.
2. If you sure that isn't the problem, then you probably have an error with your JSON Formatting. Look up how JSON formatting works and use a [JSON Editor](http://www.jsoneditoronline.org/) to find your problem.

#### PokeAlarm repeatedly spams Error 10053 on Windows or Error 53 on Linux?
* This error is when your machine can't keep up with the current number of sockets open. This is particularly common on Windows. You can try the following:
1. Lower the number of webhook threads (`wh-concurrent` or similar).
2. Decrease database threads (`db-concurrency` or similar).
3. If on Windows, try Linux (even a VM will see improvements).
4. If all of the above fails, you are being bottlenecked by hardware. It could be CPU is maxed out, HD is too slow, etc. You can upgrade your hardware or add on additional machine(s) to split the load.

#### RocketMap repeatedly spams Caused by ReadTimeoutError("HTTPConnectionPool(host='127.0.0.1', port=4000?
* Check to make sure your PokeAlarm instance is running.
If it is running properly, next, make sure that it is using the correct IP address and port.
If the PokeAlarm instance is on a remote server, try increasing RM's `wh-timeout` parameter (which defaults to 1s).

#### Why does `<iv>`, `<cp>`, or another DTS show up as `?`?
* This means that PA did not receive that information by your scanner. Make sure your scanner is configured to scan for the data you want and check that your accounts are not shadowbanned.
* Make sure that the scanner is also configured to send this data to PA via its webhook configuration.

#### My maps don't show up consistently... What can I do?
* Make sure you have added a [Google API Key](Google-Maps-API-Key) and have the Static Maps API enabled. If you already have it enabled, make sure that you haven't hit the limit for free users.

#### Do not you see the icons in your alerts?
* Remove your lines from `icon_url` and `avatar_url` from your [Alarms](alarms) to use the default images in the PA.

#### Do you want to use custom images in your alerts?
* Add in your alarms the option to add images (depends on the service that is used) and add your url where the images are.
For example, if you use [Discord](Discord) it will be something like this:

```
"icon_url":"https://raw.githubusercontent.com/user/PokeIcons/master/icons/<pkmn_id>.png"
````

#### Why isn't <gym_name> working right?
* You must have gym-details enabled in your scanner and webhook options.
* Most scanners don't send the gym-details with every gym related webhook - PA will cache the details and try to remember them, but this goes away upon restart. It takes time for the scanner to send all the details.

You can use file-caching to save the gym names to a file so that they are available upon restart. Check out the [Object Caching](Object-Caching) page for more information.

#### I have some other problem and can't figure it out.
* If you have already checked the [Wiki](Home) thoroughly, you can try asking in our [PokeAlarm Discord](https://discord.gg/S2BKC7p). Make sure to read the Rules before posting.
* Post your questions in the #troubleshooting channel (master branch) or in the #beta_chat (dev branch), support questions in other channels are not guaranteed to be answered.
