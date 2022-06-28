# ADB-android-debug-bridge-[ Under Construction ] 


<pre> 




<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/92098387/175825784-c2d38156-4e33-494c-b49b-c79465b07912.png">
</p>







</pre>

ALL you need to know about adb &amp; all the command that you will be using &amp; some of my experiences with adb


ADB, Android Debug Bridge, is a command-line utility included with Google's Android SDK. ADB can control your device over USB from a computer, copy files back and forth, install and uninstall apps, run shell commands, and more.

ADB commands can be used to debug Android devices, installing or uninstalling apps, and getting information about a connected device. ADB works with the aid of three components called Client, Daemon, and Server. If you are curious about how these 3 components work together to make ADB and ADB shell commands functions, see below:

   * Client: It’s is very computer on which you use a command-line terminal to issue an ADB command. which sends commands.
   * Daemon: Or, ADBD is a background process that runs on both the connected devices. It’s responsible for running commands on a connected emulator or          Android device.
   * Server: It runs in the background and works as a bridge between the Client and the Daemon and manages the communication. which manages communication        between the client and the daemon.


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
