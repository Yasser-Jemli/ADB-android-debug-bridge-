# ADB-android-debug-bridge-


![index](https://user-images.githubusercontent.com/92098387/175825784-c2d38156-4e33-494c-b49b-c79465b07912.png)
ALL you need to know about adb &amp; all the command that you will be using &amp; some of my experiences with adb


ADB, Android Debug Bridge, is a command-line utility included with Google's Android SDK. ADB can control your device over USB from a computer, copy files back and forth, install and uninstall apps, run shell commands, and more.


# ADB Basics
<pre>
<br /> adb devices (lists connected devices)
<br /> adb root (restarts adbd with root permissions)
<br /> adb start-server (starts the adb server)
<br /> adb kill-server (kills the adb server)
<br /> adb reboot (reboots the device)
<br /> adb devices -l (list of devices by product/model)
<br /> adb shell (starts the backround terminal)
<br /> exit (exits the background terminal)
<br /> adb help (list all commands)
<br /> adb -s <deviceName> <command> (redirect command to specific device)
<br /> adb –d <command> (directs command to only attached USB device)
<br /> adb –e <command> (directs command to only attached emulator)
</pre> 

# Package Installation
  
 <pre>
 
<br />  adb shell install <apk> (install app)
<br />  adb shell install <path> (install app from phone path)
<br />  adb shell install -r <path> (install app from phone path)
<br />  adb shell uninstall <name> (remove the app)

</pre> 

# Paths

<pre> 
<br /> /data/data/<package>/databases (app databases)
<br /> /data/data/<package>/shared_prefs/ (shared preferences)
<br /> /data/app (apk installed by user)
<br /> /system/app (pre-installed APK files)
<br /> /mmt/asec (encrypted apps) (App2SD)
<br /> /mmt/emmc (internal SD Card)
<br /> /mmt/adcard (external/Internal SD Card)
<br /> /mmt/adcard/external_sd (external SD Card)

<br /> adb shell ls (list directory contents)
<br /> adb shell ls -s (print size of each file)
<br /> adb shell ls -R (list subdirectories recursively)
</pre>

#  File Operations
<pre>

<br /> adb push <local> <remote> (copy file/dir to device)
<br /> adb pull <remote> <local> (copy file/dir from device)
<br /> run-as -package- cat -file- (access the private package files)
</pre> 


# Phone Info
<pre>
<br /> adb get-statе (print device state)
<br /> adb get-serialno (get the serial number)
<br /> adb shell dumpsys iphonesybinfo (get the IMEI)
<br /> adb shell netstat (list TCP connectivity)
<br /> adb shell pwd (print current working directory)
<br /> adb shell dumpsys battery (battery status)
<br /> adb shell pm list features (list phone features)
<br /> adb shell service list (list all services)
<br /> adb shell dumpsys activity <package>/<activity> (activity info)
<br /> adb shell ps (print process status)
<br /> adb shell wm size (displays the current screen resolution)
<br /> dumpsys window windows | grep -E 'mCurrentFocus|mFocusedApp' (print current app's opened activity)
</pre>
