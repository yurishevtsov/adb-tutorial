# ADB 101

See full documentation here: https://developer.android.com/studio/command-line/adb

### Basics

Check if you have any devices connected:
`adb devices`

If more than one device is connected, you need to pass device serial to every adb command:
`adb -s 12345678 install com.package`

### Install/Uninstall

Install an APK:
`adb install /path/to/apk/com.pocketgeek.android-3.4.5-12345.apk`

Uninstall a package:
`adb uninstall com.package.name`


### Working with logs
Print all logs in terminal:
`adb logcat`

Filter logs by priority level (D for 'debug', W for 'warning', E for 'error'):
`adb logcat *:w`
>Note: when you specify the level in filter, it will print that level and higher. E.g., 'w' will include 'warning' and 'error'. Priority levels [here](https://developer.android.com/studio/command-line/logcat#filteringOutput) for reference.

Record logs to a file:
`adb logcat > file.log`

Print logs to terminal **and** record to file:
`adb logcat | tee file.log`

Filter for crashes:
* Using native ADB tags:
`adb logcat AndroidRuntime:E *:S`
* You can also **grep** for 'FATAL EXCEPTION' + x lines after:
`adb logcat | grep FATAL -A 30`



#### PM (package manager)
List all installed packages:
`adb shell pm list packages`

Only list 3rd party apps (exculde system apps):
`adb shell pm list packages -3`

Filter by specific package name (using **grep** and pattern):
`adb shell pm list packages | grep pocketgeek`

Clear app data:
`adb shell pm clear com.package.name`

#### WM (window manager)

##### Change your screen density (for simulating small/large screen device)
To see current and/or default DPI:
`adb shell wm density`

Change the DPI to a desired value:
`adb shell wm density 200`

Reset to deafault:
`adb shell wm density reset`

>You can also change the screen size/resolution: replace 'density' with 'size' in the commands above.

#### Mocking battery states
To see current battery stats:
`adb shell dumpsys battery`

To mock your battery level:
`adb shell dumpsys set level <number b/w 1-100>`

To mock your battery status (charging ac, charging usb, charging wireless, not charging):
`adb shell dumpsys set status 1/2/3/4`
>See all battery status constants [here](https://developer.android.com/reference/android/os/BatteryManager#BATTERY_STATUS_CHARGING)

Alternative way to set status 'unplugged':
`adb shell dumpsys battery unplug`

Reset mocked battery state:
`adb shell battery reset`

### Misc
Launch an app:
`adb shell monkey -p com.package name 1`