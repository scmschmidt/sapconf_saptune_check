# sapconf_saptune_check

This tool checks if sapconf or saptune is set up correctly. +
It will check
  * the package version,
  * the status of the systemd services of sapconf and tuned,
  * the tuned profile,
  * and if solutions or notes are applied for saptune

In case of a misconfiguration some hints are given to rectify the problem.
Please follow the hints from top to down to mitigate side effects.

**It will not dig deeper to check if the tuning itself is working correctly!**


## Usage
```
sapconf_saptune_check overview|sapconf|saptune
```

## Examples

Just getting an overview:
```
./sapconf_saptune_check overview

This is sapconf_saptune_check v1.0.0.

It verifies if sapconf or saptune are set up correctly.
Please keep in mind:
 - Only *one* of both can be used at the same time!
 - This tool does not check, if the tuning itself works correctly.
 - Follow the hints from top to down to minimize side effects.


Package overview
================
[NOTE]  tuned has version 2.4.1
[NOTE]  sapconf has version 4.1.14
[NOTE]  saptune has version 2.0.3

Systemd unit overview
=====================
[NOTE]  tuned.service is active
[NOTE]  tuned.service is enabled
[NOTE]  sapconf.service is inactive
[NOTE]  sapconf.service is disabled

Profile overview
================
[NOTE]  tuned profile is saptune
[NOTE]  saptune configuration is missing
[NOTE]  saptune is configured for version: 2
```

A sapconf check with everything perfect:
```
# ./sapconf_saptune_check sapconf


This is sapconf_saptune_check v1.0.0.

It verifies if sapconf or saptune are set up correctly.
Please keep in mind:
 - Only *one* of both can be used at the same time!
 - This tool does not check, if the tuning itself works correctly.
 - Follow the hints from top to down to minimize side effects.


Checking sapconf (tuned-less)
=============================
[ OK ]  sapconf package has version 5.0.0
[ OK ]  sapconf.service is active
[ OK ]  sapconf.service is enabled
[ OK ]  tuned.service is inactive
[ OK ]  tuned.service is disabled

Sapconf is set up correctly.
```

Not so well in case of saptune:
```
# ./sapconf_saptune_check saptune

This is sapconf_saptune_check v1.0.0.

It verifies if sapconf or saptune are set up correctly.
Please keep in mind:
 - Only *one* of both can be used at the same time!
 - This tool does not check, if the tuning itself works correctly.
 - Follow the hints from top to down to minimize side effects.


Checking saptune (tuned-based)
==============================
[ OK ]  saptune package has version 2.0.3
[FAIL]  sapconf.service is active               --> Run 'systemctl stop sapconf.service' to stop the tuning now.
[FAIL]  sapconf.service is enabled              --> Run 'systemctl disable sapconf.service' to deactivate sapconf at boot.
[ OK ]  tuned package has version 2.4.1
[FAIL]  tuned.service is inactive               --> Run 'saptune daemon start' to start the tuning now.
[FAIL]  tuned.service is disabled               --> Run 'saptune daemon start' to enable tuned at boot.
[FAIL]  No solutions or notes applied           --> Run 'saptune solution|note apply <id>' to configure saptune.

5 error(s) have been found.
Saptune will not work properly!
```


## Exit Codes
| exit code | description                                                                    |
|-----------|--------------------------------------------------------------------------------|
|     0     | All checks ok. Sapconf/saptune have been set up correctly.                     |
|     1     | Some warnings occured. Sapconf/saptune should work, but better check manually. |   
|     2     | Some errors occured. Sapconf/saptune will not work.                            |
|     3     | Wrong parameters given to the tool on commandline.                             | 


## Changelog

|    date    | version  | comment                                     |
|------------|----------|---------------------------------------------|
| 24.06.2019 | v0.1     | First release.                              |
| 24.06.2019 | v0.1.1   | Small bug fixed / Some messages rephrased   |
| 25.06.2019 | v0.1.2   | Disclaimer added                            |
| 22.11.2019 | v0.1.3   | Bug fix: Now handles missing rpm packages   |
| 02.07.2020 | v1.0.0   | Rework to fix some bugs regarding missing   |
|            |          | packages and support for tuned-less sapconf |
