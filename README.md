# ADB-android-debug-bridge-

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
  
<br />  adb shell install <apk> (install app)
<br />  adb shell install <path> (install app from phone path)
<br />  adb shell install -r <path> (install app from phone path)
<br />  adb shell uninstall <name> (remove the app)
