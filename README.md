# eis_port_transsioncamera
This is an attempt to port the EIS functionality that came with the stock rom camera (aka Transsion Camera) to other custom ROMs. Infinix Note 30 would be used as the testing device, support for other device is unplanned.

## Objectives
- Get the cameraserver binary running
- ~~Port the camera app~~

## Resources
- https://gitlab.com/rama-firmware-dumps/infinix/Infinix-X6833B_dump (Infinix Note 30 firmware dump)
- https://gitlab.com/rama-firmware-dumps/infinix/Infinix-X6851_dump (Infinix Note 40 Pro 5G firmware dump, used for A14 base)

## Porting the camera server
This is the hardest part where I've been stuck for days, it is extremely painful. You can continue where I left off by installing the zip file as a Magisk/KSU module or start by your own.
If you want to start by your own, you can get the `cameraserver` binary in `/system/bin/cameraserver` and resolving its dependencies by using `ldd <executable path>` or `linker64 --list <executable/library path>`.

Once you get all the dependencies put in place (or if you continue where I left off), the camera server will still not run. This is because it is missing some symbols/functions from other dependencies. You can get an insight to what library is using/defining the function, but you can't simply retrofit it. If you do, then it will cause a chain reaction where that library is missing some functions call or other libraries used in the system is missing the functions defined in that library.

## Porting the camera app
This is relatively the easiest part, you just need to get the camera apk file itself from `/system_ext/app/TranssionCamera`, set-up an error logger (you can use adb logcat or AppErrorsTracking with LSposed), then place all the required libraries to run it.
However, porting the app itself won't get you the EIS feature (which is the primary reason I intend to port the camera in the first place), you also need to get the proprietary cameraserver to run.
