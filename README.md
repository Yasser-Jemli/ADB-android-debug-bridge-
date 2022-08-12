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


# Phone ( android device ) Info
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

# dumpsys
<pre>

dumpsys is a tool that runs on Android devices and provides information about system services. You can call dumpsys from the command line using the Android Debug Bridge (ADB) to get diagnostic output for all system services running on a connected device. This output is typically more verbose than you may want, so use the command line options described below to get output for only the system services you're interested in. This page also describes how to use dumpsys to accomplish common tasks, such as inspecting input, RAM, battery, or network diagnostics.
The general syntax for using dumpsys is as follows: adb shell dumpsys [-t timeout] [--help | -l | --skip services | service [arguments] | -c | -h]

To get a diagnostic output for all system services for your connected device, simply run adb shell dumpsys. However, this outputs far more information than you would typically want. For more manageable output, specify the service you want to examine by including it in the command. For example, the command below provides system data for input components, such as touchscreens or built-in keyboards: adb shell dumpsys input
For a complete list of system services that you can use with dumpsys, use the following command: adb shell dumpsys -l

for me the details visit : https://developer.android.com/studio/command-line/dumpsys
</pre>



# Logcat command-line tool

<pre>
Logcat is a command-line tool that dumps a log of system messages, including stack traces when the device throws an error and messages that you have written from your app with the Log class.

This page is about the command-line logcat tool, but you can also view log messages from the Logcat window in Android Studio. For information about viewing and filtering logs from Android Studio, see Write and View Logs with Logcat.
Logging system overview

The Android logging system is a set of structured circular buffers maintained by the system process logd. The set of available buffers is fixed and defined by the system. The most relevant ones are: main, which stores most application logs, system, which stores messages originating from the Android OS, and crash, which stores crash logs. Each log entry has a priority (one of VERBOSE, DEBUG, INFO, WARNING, ERROR or FATAL), a tag that identifies the origin of the log, and the actual log message.

The primary interface to the logging system is the shared library liblog and its header <android/log.h>. All language-specific logging facilities eventually call the function __android_log_write. By default, it calls the function __android_log_logd_logger, which sends the log entry to logd using a socket. Starting with API level 30, the logging function can be changed by calling __android_set_log_writer. More information is available in the NDK documentation.

Logs displayed by adb logcat undergo four levels of filtering:

    Compile-time filtering: depending on compilation settings, some logs may be completely removed from the binary. For example, Proguard can be configured to remove calls to Log.d from Java code.
    System property filtering: liblog queries a set of system properties to determine the minimum severity level to be sent to logd. If your logs have the tag MyApp, the following properties are checked, and are expected to contain the first letter of the minimum severity (V, D, I, W, E, or S to disable all logs):
        log.tag.MyApp
        persist.log.tag.MyApp
        log.tag
        persist.log.tag
    Application filtering: If none of the properties are set, liblog uses the minimum priority set by __android_log_set_minimum_priority. The default setting is INFO.
    Display filtering: adb logcat supports additional filters that can reduce the amount of logs shown from logd. See below for details.

Command-line syntax

To run logcat through the adb shell, the general usage is:

[adb] logcat [<option>] ... [<filter-spec>] ...

You can run logcat as an adb command or directly in a shell prompt of your emulator or connected device. To view log output using adb, navigate to your SDK platform-tools/ directory and execute:

adb logcat

For logcat online help, start a device and then execute:

adb logcat --help

You can create a shell connection to a device and execute:

$ adb shell
# logcat

Options

The following table describes the command line options of logcat.
Option 	Description
-b <buffer> 	Load an alternate log buffer for viewing, such as events or radio. The main, system, and crash buffer set is used by default. See Viewing Alternative Log Buffers.
-c, --clear 	Clear (flush) the selected buffers and exit. The default buffer set is main, system and crash. To clear all of the buffers, use -b all -c.
-e <expr>, --regex=<expr> 	Only print lines where the log message matches <expr> where <expr> is a regular expression.
-m <count>, --max-count=<count> 	Quit after printing <count> number of lines. This is meant to be paired with --regex, but will work on its own.
--print 	Paired with --regex and --max-count to let content bypass the regex filter, but still stop at the right number of matches.
-d 	Dump the log to the screen and exits.
-f <filename> 	Write log message output to <filename>. The default is stdout.
-g, --buffer-size 	Print the size of the specified log buffer and exits.
-n <count> 	Set the maximum number of rotated logs to <count>. The default value is 4. Requires the -r option.
-r <kbytes> 	Rotate the log file every <kbytes> of output. The default value is 16. Requires the -f option.
-s 	Equivalent to the filter expression '*:S', which sets priority for all tags to silent, and is used to precede a list of filter expressions that add content. To learn more, go to the section about filtering log output.
-v <format> 	Set the output format for log messages. The default is threadtime format. For a list of supported formats, go to the section about the Control log output format.
-D, --dividers 	Print dividers between each log buffer.
-c 	Flush (clear) the entire log and exit.
-t <count> 	Print only the most recent number of lines. This option includes -d functionality.
-t '<time>' 	Print the most recent lines since the specified time. This option includes -d functionality. See the -P option for information about quoting parameters with embedded spaces.

adb logcat -t '01-26 20:52:41.820'

-T <count> 	Print the most recent number of lines since the specified time. This option does not include -d functionality
-T '<time>' 	Print the most recent lines since the specified time. This option does not include include -d functionality. See the -P option for information about quoting parameters with embedded spaces.

adb logcat -t '01-26 20:52:41.820'

-L, --last 	Dump the logs prior to the last reboot.
-B, --binary 	Output the log in binary.
-S, --statistics 	Include statistics in the output to help you identify and target log spammers.
-G <size> 	Set the size of the log ring buffer. Can add K or M at the end to indicate kilobytes or megabytes.
-p, --prune 	Print (read) the allowlist and denylist and takes no arguments, as follows:

adb logcat -p

-P '<list> ...'
--prune '<list> ...' -P '<allowlist_and_denylist>' 	Write (set) the allowlist and denylist to adjust the logging content for a specific purpose. You provide a mixed content of allowed and denied list entries, where <allowlist> or <denylist> can be a UID, UID/PID or PID. With guidance from the logcat statistics (logcat -S), one can consider adjustments to the allowlist and denylist for purposes such as:

    Give the highest longevity to specific logging content through UID selections.
    Prevent someone (UID) or something (PID) from consuming these resources to help increase the logspan so you can have more visibility into the problems you are diagnosing.

By default the logging system automatically prevents the worst offender in the log statistics dynamically to make space for new log messages. Once it has exhausted the heuristics, the system prunes the oldest entries to make space for the new messages.

Adding an allowlist protects your Android Identification number (AID), which becomes the processes' AID and GID from being declared an offender, and adding a denylist helps free up space before the worst offenders are considered. You can choose how active the pruning is, and you can turn pruning off so it only removes content from the oldest entries in each log buffer.

Quotes

adb logcat does not preserve the quotes, so the syntax for specifying allowlist and denylist is as follows:

$ adb logcat -P '"<allowlist_and_denylist>"'

or

adb shell
$ logcat -P '<allowlist_and_denylist>'

The following example specifies an allowlist with PID 32676 and UID 675, and a denylist with PID 32677 and UID 897. PID 32677 on the denylist is weighted for faster pruning.

adb logcat -P '"/32676 675 ~/32677 897"'

Other allowlist and denylist command variations you can use are as follows:

~! worst uid denylist
~1000/! worst pid in system (1000)

--pid=<pid> ... 	Only print logs from the given PID.
--wrap 	Sleep for 2 hours or when the buffer is about to wrap whichever comes first. Improves efficiency of polling by providing an about-to-wrap wakeup.
Filtering log output

    The tag of a log message is a short string indicating the system component from which the message originates (for example, "View" for the view system).
    The priority is one of the following character values, ordered from lowest to highest priority:
        V: Verbose (lowest priority)
        D: Debug
        I: Info
        W: Warning
        E: Error
        F: Fatal
        S: Silent (highest priority, on which nothing is ever printed)

You can obtain a list of tags used in the system, with priorities, by running logcat and observing the first two columns of each message, given as <priority>/<tag>.

The following is an example of brief logcat output obtained with the logcat -v brief output command. It shows that the message relates to priority level "I" and tag "ActivityManager":

I/ActivityManager(  585): Starting activity: Intent { action=android.intent.action...}

To reduce the log output to a manageable level, you can restrict log output using filter expressions. Filter expressions let you indicate to the system the tags-priority combinations that you are interested in — the system suppresses other messages for the specified tags.

A filter expression follows this format tag:priority ..., where tag indicates the tag of interest and priority indicates the minimum level of priority to report for that tag. Messages for that tag at or above the specified priority are written to the log. You can supply any number of tag:priority specifications in a single filter expression. The series of specifications is whitespace-delimited.

Here's an example of a filter expression that suppresses all log messages except those with the tag "ActivityManager", at priority "Info" or above, and all log messages with tag "MyApp", with priority "Debug" or above:

adb logcat ActivityManager:I MyApp:D *:S

The final element in the above expression, *:S, sets the priority level for all tags to "silent", thus ensuring only log messages with "ActivityManager" and "MyApp" are displayed. Using *:S is an excellent way to ensure that log output is restricted to the filters that you have explicitly specified — it lets your filters serve as an allowlist for log output.

The following filter expression displays all log messages with priority level "warning" and higher, on all tags:

adb logcat *:W

If you're running logcat from your development computer (versus running it on a remote adb shell), you can also set a default filter expression by exporting a value for the environment variable ANDROID_LOG_TAGS:

export ANDROID_LOG_TAGS="ActivityManager:I MyApp:D *:S"

Note that ANDROID_LOG_TAGS filter is not exported to the emulator/device instance, if you are running logcat from a remote shell or using adb shell logcat.
Control log output format

Log messages contain a number of metadata fields, in addition to the tag and priority. You can modify the output format for messages so that they display a specific metadata field. To do so, you use the -v option and specify one of the supported output formats listed below.

    brief: Display priority, tag, and PID of the process issuing the message.
    long: Display all metadata fields and separate messages with blank lines.
    process: Display PID only.
    raw: Display the raw log message with no other metadata fields.
    tag: Display the priority and tag only.
    thread: A legacy format that shows priority, PID, and TID of the thread issuing the message.
    threadtime (default): Display the date, invocation time, priority, tag, PID, and TID of the thread issuing the message.
    time: Display the date, invocation time, priority, tag, and PID of the process issuing the message.

When starting logcat, you can specify the output format you want by using the -v option:

[adb] logcat [-v <format>]

Here's an example that shows how to generate messages in thread output format:

adb logcat -v thread

Note that you can only specify one output format with the -v option, but you can specify as many modifiers that make sense. Logcat ignores modifiers that do not make sense.
Format modifiers

Format modifiers change the logcat output in terms of any combination of one or more of the following modifiers. To specify a format modifier, use the -v option, as follows:

adb logcat -b all -v color -d

Every Android log message has a tag and a priority associated with it. You can combine any format modifier with any one of the following format options: brief, long, process, raw, tag, thread, threadtime, and time.

You can get the format modifier details by typing logcat -v --help at the command line.

    color: Show each priority level with a different color.
    descriptive: Show log buffer event descriptions. This modifier affects event log buffer messages only, and has no effect on the other non-binary buffers. The event descriptions come from the event-log-tags database.
    epoch: Display time in seconds starting from Jan 1, 1970.
    monotonic: Display time in CPU seconds starting from the last boot.
    printable: Ensure that any binary logging content is escaped.
    uid: If permitted by access controls, display the UID or Android ID of the logged process.
    usec: Display the time with precision down to microseconds.
    UTC: Display time as UTC.
    year: Add the year to the displayed time.
    zone: Add the local time zone to the displayed time.

Viewing alternative log buffers

The Android logging system keeps multiple circular buffers for log messages, and not all of the log messages are sent to the default circular buffer. To see additional log messages, you can run the logcat command with the -b option, to request viewing of an alternate circular buffer. You can view any of these alternate buffers:

    radio: View the buffer that contains radio/telephony related messages.
    events: View the interpreted binary system event buffer messages.
    main: View the main log buffer (default) does not contain system and crash log messages.
    system: View the system log buffer (default).
    crash: View the crash log buffer (default).
    all: View all buffers.
    default: Reports main, system, and crash buffers. 

The usage of the -b option is:

[adb] logcat [-b <buffer>]

Here is an example of how to view a log buffer containing radio and telephony messages:

adb logcat -b radio

You can also specify multiple -b flags for all of the buffers you want to print, as follows:

logcat -b main -b radio -b events

You can specify a single -b flag with a comma-separated list of buffers, for example:

logcat -b main,radio,events

Logging from code

The Log class allows you to create log entries in your code that display in the logcat tool. Common logging methods include:

    Log.v(String, String) (verbose)
    Log.d(String, String) (debug)
    Log.i(String, String) (information)
    Log.w(String, String) (warning)
    Log.e(String, String) (error)

For example, using the following call:
Kotlin
Java

Log.i("MyActivity", "MyClass.getView() — get item number $position")

The logcat outputs something like:

I/MyActivity( 1557): MyClass.getView() — get item number 1

</pre>
