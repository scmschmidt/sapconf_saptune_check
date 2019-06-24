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


## USAGE
```
sapconf_saptune_check sapconf|saptune
```

## EXAMPLES

A sapconf check with everything perfect:
```
# ./sapconf_saptune_check sapconf

This is sapconf_saptune_check v0.1.1

It verifies if sapconf or saptune are set up correctly.
Please keep in mind:
 - Only *one* of both can be used at the same time!
 - This tool does not check, if the tuning itself works correctly.
 - Follow the hints from top to down to minimize side effects.


Checking sapconf
================
[ OK ]  sapconf package version 4.1.14
[ OK ]  sapconf tuned profile 'sap-hana' is set
[ OK ]  sapconf.service is active
[ OK ]  sapconf.service is enabled
[ OK ]  tuned.service is active
[ OK ]  tuned.service is disabled

Sapconf is set up correctly.
```

Not so well in case of saptune:
```
# ./sapconf_saptune_check saptune

This is sapconf_saptune_check v0.1.1

It verifies if sapconf or saptune are set up correctly.
Please keep in mind:
 - Only *one* of both can be used at the same time!
 - This tool does not check, if the tuning itself works correctly.
 - Follow the hints from top to down to minimize side effects.


Checking saptune
================
[ OK ]  saptune package version 1.1.7
[FAIL]  sapconf.service is active               --> Run 'systemctl stop sapconf.service' to stop the tuning now.
[FAIL]  sapconf.service is enabled              --> Run 'systemctl disable sapconf.service' to deactivate sapconf at boot.
[ OK ]  tuned.service is active
[FAIL]  tuned.service is disabled               --> Run 'saptune daemon start' to enable tuned at boot.
[FAIL]  No saptune profile is set: sap-hana     --> Run 'saptune daemon start' to set the profile.
[FAIL]  No solutions or notes applied           --> Run 'saptune solution|note apply <id>' to configure saptune.

1 warning(s) have been found.
4 error(s) have been found.
Saptune will not work properly!
```


## EXIT CODES

0   All checks ok. Sapconf/saptune have been set up correctly.
1   Some warnings occured. Sapconf/saptune should work, but better check manually.
2   Some errors occured. Sapconf/saptune will not work.
3   Wrong parameters given to the tool on commandline.


## Changelog

24.06.2019  v0.1      First release.
24.06.2019  v0.1.1    Small bug fixed / Some messages rephrased

## Author / Disclaimer???
