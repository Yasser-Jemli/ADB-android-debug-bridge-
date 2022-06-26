# ADB-android-debug-bridge-

ALL you need to know about adb &amp; all the command that you will be using &amp; some of my experiences with adb


ADB, Android Debug Bridge, is a command-line utility included with Google's Android SDK. ADB can control your device over USB from a computer, copy files back and forth, install and uninstall apps, run shell commands, and more.


# ADB Basics

adb devices (lists connected devices)
adb root (restarts adbd with root permissions)
adb start-server (starts the adb server)
adb kill-server (kills the adb server)
adb reboot (reboots the device)
adb devices -l (list of devices by product/model)
adb shell (starts the backround terminal)
exit (exits the background terminal)
adb help (list all commands)
adb -s <deviceName> <command> (redirect command to specific device)
adb –d <command> (directs command to only attached USB device)
adb –e <command> (directs command to only attached emulator)

