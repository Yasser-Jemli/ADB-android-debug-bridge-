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

# All Android Key Events for usage with adb shell

{
<br />   "key_events": {
<br />     "key_unknown": "adb shell input keyevent 0",
<br />    "key_soft_left": "adb shell input keyevent 1",
<br />     "key_soft_right": "adb shell input keyevent 2",
<br />     "key_home": "adb shell input keyevent 3",
<br />    "key_back": "adb shell input keyevent 4",
<br />      "key_call": "adb shell input keyevent 5",
<br />        "key_endcall": "adb shell input keyevent 6",
<br />        "key_0": "adb shell input keyevent 7",
<br />        "key_1": "adb shell input keyevent 8",
<br />        "key_2": "adb shell input keyevent 9",
<br />        "key_3": "adb shell input keyevent 10",
<br />        "key_4": "adb shell input keyevent        
<br />        "key_5": "adb shell input keyevent 12",
<br />        "key_6": "adb shell input keyevent 13",
<br />        "key_7": "adb shell input keyevent 14",
<br />       "key_8": "adb shell input keyevent 15",
<br />        "key_9": "adb shell input keyevent 16",
<br />        "key_star": "adb shell input keyevent 17",
<br />        "key_pound": "adb shell input keyevent 18",
<br />        "key_dpad_up": "adb shell input keyevent 19",
<br />        "key_dpad_down": "adb shell input keyevent 20",
<br />       "key_dpad_left": "adb shell input keyevent 21",
<br />       "key_dpad_right": "adb shell input keyevent 22",
<br />        "key_dpad_center": "adb shell input keyevent 23",
<br />        "key_volume_up": "adb shell input keyevent 24",
<br />       "key_volume_down": "adb shell input keyevent 25",
<br />        "key_power": "adb shell input keyevent 26",
<br />        "key_camera": "adb shell input keyevent 27",
<br />        "key_clear": "adb shell input keyevent 28",
<br />       "key_a": "adb shell input keyevent 29",
<br />        "key_b": "adb shell input keyevent 30",
<br />        "key_c": "adb shell input keyevent 31",
<br />        "key_d": "adb shell input keyevent 32",
<br />       "key_e": "adb shell input keyevent 33",
<br />        "key_f": "adb shell input keyevent 34",
<br />        "key_g": "adb shell input keyevent 35",
<br />        "key_h": "adb shell input keyevent 36",
<br />        "key_i": "adb shell input keyevent 37",
<br />        "key_j": "adb shell input keyevent 38",
<br />        "key_k": "adb shell input keyevent 39",
<br />        "key_l": "adb shell input keyevent 40",
<br />        "key_m": "adb shell input keyevent 41",
<br />        "key_n": "adb shell input keyevent 42",
<br />        "key_o": "adb shell input keyevent 43",
<br />        "key_p": "adb shell input keyevent 44",
<br />        "key_q": "adb shell input keyevent 45",
<br />        "key_r": "adb shell input keyevent 46",
<br />        "key_s": "adb shell input keyevent 47",
<br />        "key_t": "adb shell input keyevent 48",
<br />        "key_u": "adb shell input keyevent 49",
<br />        "key_v": "adb shell input keyevent 50",
<br />        "key_w": "adb shell input keyevent 51",
<br />        "key_x": "adb shell input keyevent 52",
<br />        "key_y": "adb shell input keyevent 53",
<br />        "key_z": "adb shell input keyevent 54",
<br />        "key_comma": "adb shell input keyevent 55",
<br />        "key_period": "adb shell input keyevent 56",
<br />        "key_alt_left": "adb shell input keyevent 57",
<br />        "key_alt_right": "adb shell input keyevent 58",
<br />        "key_shift_left": "adb shell input keyevent 59",
<br />        "key_shift_right": "adb shell input keyevent 60",
<br />        "key_tab": "adb shell input keyevent 61",
<br />        "key_space": "adb shell input keyevent 62",
<br />        "key_sym": "adb shell input keyevent 63",
<br />        "key_explorer": "adb shell input keyevent 64",
<br />        "key_envelope": "adb shell input keyevent 65",
<br />        "key_enter": "adb shell input keyevent 66",
<br />        "key_del": "adb shell input keyevent 67",
<br />        "key_grave": "adb shell input keyevent 68",
<br />        "key_minus": "adb shell input keyevent 69",
<br />        "key_equals": "adb shell input keyevent 70",
<br />        "key_left_bracket": "adb shell input keyevent 71",
<br />       "key_right_bracket": "adb shell input keyevent 72",

<br />       "key_backslash": "adb shell input keyevent 73",
<br />        "key_semicolon": "adb shell input keyevent 74",
<br />        "key_apostrophe": "adb shell input keyevent 75",
<br />        "key_slash": "adb shell input keyevent 76",
<br />        "key_at": "adb shell input keyevent 77",
<br />        "key_num": "adb shell input keyevent 78",
<br />        "key_headsethook": "adb shell input keyevent 79",
<br />        "key_focus": "adb shell input keyevent 80",
<br />        "key_plus": "adb shell input keyevent 81",
<br />        "key_menu": "adb shell input keyevent 82",
<br />        "key_notification": "adb shell input keyevent 83",
<br />        "key_search": "adb shell input keyevent 84",
<br />        "key_media_play_pause": "adb shell input keyevent 85",
<br />        "key_media_stop": "adb shell input keyevent 86",
<br />        "key_media_next": "adb shell input keyevent 87",
<br />        "key_media_previous": "adb shell input keyevent 88",
<br />        "key_media_rewind": "adb shell input keyevent 89",
<br />        "key_media_fast_forward": "adb shell input keyevent 90",
<br />        "key_mute": "adb shell input keyevent 91",
<br />        "key_page_up": "adb shell input keyevent 92",
<br />        "key_page_down": "adb shell input keyevent 93",
<br />       "key_pictsymbols": "adb shell input keyevent 94",
<br />        "key_switch_charset": "adb shell input keyevent 95",
<br />        "key_button_a": "adb shell input keyevent 96",
<br />        "key_button_b": "adb shell input keyevent 97",
<br />        "key_button_c": "adb shell input keyevent 98",
<br />        "key_button_x": "adb shell input keyevent 99",
<br />        "key_button_y": "adb shell input keyevent 100",
<br />        "key_button_z": "adb shell input keyevent 101",
<br />        "key_button_l1": "adb shell input keyevent 102",
<br />        "key_button_r1": "adb shell input keyevent 103",
<br />        "key_button_l2": "adb shell input keyevent 104",
<br />        "key_button_r2": "adb shell input keyevent 105",
<br />        "key_button_thumbl": "adb shell input keyevent 106",
<br />        "key_button_thumbr": "adb shell input keyevent 107",
<br />        "key_button_start": "adb shell input keyevent 108",
<br />        "key_button_select": "adb shell input keyevent 109",
<br />        "key_button_mode": "adb shell input keyevent 110",
<br />        "key_escape": "adb shell input keyevent 111",
<br />        "key_forward_del": "adb shell input keyevent 112",
<br />        "key_ctrl_left": "adb shell input keyevent 113",
<br />        "key_ctrl_right": "adb shell input keyevent 114",
<br />        "key_caps_lock": "adb shell input keyevent 115",
<br />        "key_scroll_lock": "adb shell input keyevent 116",
<br />        "key_meta_left": "adb shell input keyevent 117",
<br />        "key_meta_right": "adb shell input keyevent 118",
<br />        "key_function": "adb shell input keyevent 119",
<br />        "key_sysrq": "adb shell input keyevent 120",
<br />        "key_break": "adb shell input keyevent 121",
<br />        "key_move_home": "adb shell input keyevent 122",
<br />        "key_move_end": "adb shell input keyevent 123",
<br />        "key_insert": "adb shell input keyevent 124",
<br />        "key_forward": "adb shell input keyevent 125",
<br />        "key_media_play": "adb shell input keyevent 126",
<br />        "key_media_pause": "adb shell input keyevent 127",
<br />        "key_media_close": "adb shell input keyevent 128",
<br />        "key_media_eject": "adb shell input keyevent 129",
<br />        "key_media_record": "adb shell input keyevent 130",
<br />        "key_f1": "adb shell input keyevent 131",
<br />        "key_f2": "adb shell input keyevent 132",
<br />        "key_f3": "adb shell input keyevent 133",
<br />        "key_f4": "adb shell input keyevent 134",
<br />        "key_f5": "adb shell input keyevent 135",
<br />        "key_f6": "adb shell input keyevent 136",
<br />        "key_f7": "adb shell input keyevent 137",
<br />        "key_f8": "adb shell input keyevent 138",
<br />        "key_f9": "adb shell input keyevent 139",
<br />        "key_f10": "adb shell input keyevent 140",
<br />        "key_f11": "adb shell input keyevent 141",
<br />        "key_f12": "adb shell input keyevent 142",
<br />        "key_num_lock": "adb shell input keyevent 143",
<br />        "key_numpad_0": "adb shell input keyevent 144",
<br />         "key_numpad_1": "adb shell input keyevent 145",
<br />         "key_numpad_2": "adb shell input keyevent 146",
<br />         "key_numpad_3": "adb shell input keyevent 147",
<br />         "key_numpad_4": "adb shell input keyevent 148",
<br />         "key_numpad_5": "adb shell input keyevent 149",
<br />         "key_numpad_6": "adb shell input keyevent 150",
<br />         "key_numpad_7": "adb shell input keyevent 151",
<br />         "key_numpad_8": "adb shell input keyevent 152",
<br />         "key_numpad_9": "adb shell input keyevent 153",
<br />         "key_numpad_divide": "adb shell input keyevent 154",
<br />         "key_numpad_multiply": "adb shell input keyevent 155",
<br />         "key_numpad_subtract": "adb shell input keyevent 156",
<br />         "key_numpad_add": "adb shell input keyevent 157",
<br />         "key_numpad_dot": "adb shell input keyevent 158",
<br />         "key_numpad_comma": "adb shell input keyevent 159",
<br />         "key_numpad_enter": "adb shell input keyevent 160",
<br />         "key_numpad_equals": "adb shell input keyevent 161",
<br />         "key_numpad_left_paren": "adb shell input keyevent 162",
<br />         "key_numpad_right_paren": "adb shell input keyevent 163",
<br />         "key_volume_mute": "adb shell input keyevent 164",
<br />         "key_info": "adb shell input keyevent 165",
<br />         "key_channel_up": "adb shell input keyevent 166",
<br />         "key_channel_down": "adb shell input keyevent 167",
<br />         "key_zoom_in": "adb shell input keyevent 168",
<br />         "key_zoom_out": "adb shell input keyevent 169",
<br />         "key_tv": "adb shell input keyevent 170",
<br />         "key_window": "adb shell input keyevent 171",
<br />         "key_guide": "adb shell input keyevent 172",
<br />         "key_dvr": "adb shell input keyevent 173",
<br />         "key_bookmark": "adb shell input keyevent 174",
<br />         "key_captions": "adb shell input keyevent 175",
<br />         "key_settings": "adb shell input keyevent 176",
<br />         "key_tv_power": "adb shell input keyevent 177",
<br />         "key_tv_input": "adb shell input keyevent 178",
<br />         "key_stb_power": "adb shell input keyevent 179",
<br />         "key_stb_input": "adb shell input keyevent 180",
<br />         "key_avr_power": "adb shell input keyevent 181",
<br />         "key_avr_input": "adb shell input keyevent 182",
<br />         "key_prog_red": "adb shell input keyevent 183",
<br />         "key_prog_green": "adb shell input keyevent 184",
<br />         "key_prog_yellow": "adb shell input keyevent 185",
<br />         "key_prog_blue": "adb shell input keyevent 186",
<br />         "key_app_switch": "adb shell input keyevent 187",
<br />         "key_button_1": "adb shell input keyevent 188",
<br />         "key_button_2": "adb shell input keyevent 189",
<br />         "key_button_3": "adb shell input keyevent 190",
<br />         "key_button_4": "adb shell input keyevent 191",
<br />         "key_button_5": "adb shell input keyevent 192",
<br />         "key_button_6": "adb shell input keyevent 193",
<br />         "key_button_7": "adb shell input keyevent 194",
<br />         "key_button_8": "adb shell input keyevent 195",
<br />         "key_button_9": "adb shell input keyevent 196",
<br />         "key_button_10": "adb shell input keyevent 197",
<br />         "key_button_11": "adb shell input keyevent 198",
<br />         "key_button_12": "adb shell input keyevent 199",
<br />         "key_button_13": "adb shell input keyevent 200",
<br />         "key_button_14": "adb shell input keyevent 201",
<br />         "key_button_15": "adb shell input keyevent 202",
<br />         "key_button_16": "adb shell input keyevent 203",
<br />         "key_language_switch": "adb shell input keyevent 204",
<br />         "key_manner_mode": "adb shell input keyevent 205",
<br />         "key_3d_mode": "adb shell input keyevent 206",
<br />         "key_contacts": "adb shell input keyevent 207",
<br />         "key_calendar": "adb shell input keyevent 208",
<br />         "key_music": "adb shell input keyevent 209",
<br />         "key_calculator": "adb shell input keyevent 210",
<br />         "key_zenkaku_hankaku": "adb shell input keyevent 211",
<br />         "key_eisu": "adb shell input keyevent 212",
<br />         "key_muhenkan": "adb shell input keyevent 213",
<br />         "key_henkan": "adb shell input keyevent 214",
<br />         "key_katakana_hiragana": "adb shell input keyevent 215",
<br />         "key_yen": "adb shell input keyevent 216",
<br />         "key_ro": "adb shell input keyevent 217",
<br />         "key_kana": "adb shell input keyevent 218",
<br />         "key_assist": "adb shell input keyevent 219",
<br />         "key_brightness_down": "adb shell input keyevent 220",
<br />         "key_brightness_up": "adb shell input keyevent 221",
<br />         "key_media_audio_track": "adb shell input keyevent 222",
<br />         "key_sleep": "adb shell input keyevent 223",
<br />         "key_wakeup": "adb shell input keyevent 224",
<br />         "key_pairing": "adb shell input keyevent 225",
<br />         "key_media_top_menu": "adb shell input keyevent 226",
<br />         "key_11": "adb shell input keyevent 227",
<br />         "key_12": "adb shell input keyevent 228",
<br />         "key_last_channel": "adb shell input keyevent 229",
<br />         "key_tv_data_service": "adb shell input keyevent 230",
<br />         "key_voice_assist": "adb shell input keyevent 231",
<br />         "key_tv_radio_service": "adb shell input keyevent 232",
<br />         "key_tv_teletext": "adb shell input keyevent 233",
<br />         "key_tv_number_entry": "adb shell input keyevent 234",
<br />         "key_tv_terrestrial_analog": "adb shell input keyevent 235",
<br />         "key_tv_terrestrial_digital": "adb shell input keyevent 236",
<br />         "key_tv_satellite": "adb shell input keyevent 237",
<br />         "key_tv_satellite_bs": "adb shell input keyevent 238",
<br />         "key_tv_satellite_cs": "adb shell input keyevent 239",
<br />         "key_tv_satellite_service": "adb shell input keyevent 240",
<br />         "key_tv_network": "adb shell input keyevent 241",
<br />         "key_tv_antenna_cable": "adb shell input keyevent 242",
<br />         "key_tv_input_hdmi_1": "adb shell input keyevent 243",
<br />         "key_tv_input_hdmi_2": "adb shell input keyevent 244",
<br />         "key_tv_input_hdmi_3": "adb shell input keyevent 245",
<br />         "key_tv_input_hdmi_4": "adb shell input keyevent 246",
<br />         "key_tv_input_composite_1": "adb shell input keyevent 247",
<br />         "key_tv_input_composite_2": "adb shell input keyevent 248",
<br />         "key_tv_input_component_1": "adb shell input keyevent 249",
<br />         "key_tv_input_component_2": "adb shell input keyevent 250",
<br />         "key_tv_input_vga_1": "adb shell input keyevent 251",
<br />         "key_tv_audio_description": "adb shell input keyevent 252",
<br />         "key_tv_audio_description_mix_up": "adb shell input keyevent 253",
<br />         "key_tv_audio_description_mix_down": "adb shell input keyevent 254",
<br />         "key_tv_zoom_mode": "adb shell input keyevent 255",
<br />         "key_tv_contents_menu": "adb shell input keyevent 256",
<br />         "key_tv_media_context_menu": "adb shell input keyevent 257",
<br />         "key_tv_timer_programming": "adb shell input keyevent 258",
<br />         "key_help": "adb shell input keyevent 259",
<br />         "key_navigate_previous": "adb shell input keyevent 260",
<br />         "key_navigate_next": "adb shell input keyevent 261",
<br />         "key_navigate_in": "adb shell input keyevent 262",
<br />         "key_navigate_out": "adb shell input keyevent 263",
<br />         "key_stem_primary": "adb shell input keyevent 264",
<br />         "key_stem_1": "adb shell input keyevent 265",
<br />         "key_stem_2": "adb shell input keyevent 266",
<br />         "key_stem_3": "adb shell input keyevent 267",
<br />         "key_dpad_up_left": "adb shell input keyevent 268",
<br />         "key_dpad_down_left": "adb shell input keyevent 269",
<br />         "key_dpad_up_right": "adb shell input keyevent 270",
<br />         "key_dpad_down_right": "adb shell input keyevent 271",
<br />         "key_media_skip_forward": "adb shell input keyevent 272",
<br />         "key_media_skip_backward": "adb shell input keyevent 273",
<br />         "key_media_step_forward": "adb shell input keyevent 274",
<br />         "key_media_step_backward": "adb shell input keyevent 275",
<br />         "key_soft_sleep": "adb shell input keyevent 276",
<br />         "key_cut": "adb shell input keyevent 277",
<br />         "key_copy": "adb shell input keyevent 278",
<br />         "key_paste": "adb shell input keyevent 279",
<br />         "key_system_navigation_up": "adb shell input keyevent 280",
<br />         "key_system_navigation_down": "adb shell input keyevent 281",
<br />         "key_system_navigation_left": "adb shell input keyevent 282",
<br />         "key_system_navigation_right": "adb shell input keyevent 283",
<br />         "key_all_apps": "adb shell input keyevent 284",
<br />         "key_refresh": "adb shell input keyevent 285"
     }
}



# LOG file Analyse Examples 

<pre>

example on log here:

BT playback on going, Door is opened while Engine OFF:

 

08-18 17:19:36.608850  1662  1878 I PowerMultiMediaEventController: handleMultiMediaEvent, appHmiState=268435458, vehicleState= 3, micomState=4, driverDoorOpenTriggered=1, mmCustomerActionTriggered=0

=> Media is paused

 

08-18 17:19:36.609961  6833 32192 W MediaSourceResumeService: MediaSinkAvailable onMute
08-18 17:19:36.609998  6833 32192 D MediaSourceResumeService: pause controller 

 

=> Audio bus are disconnected

 

08-18 17:19:37.044946  1746  1941 W CAR.AUDIO.CONFIGURABLE.ROUTE: Sink Device Port @=BUS00_MEDIA, type=bus not available

 

=> Connectivity is disabled

 

08-18 17:19:37.085230  1746  1746 D CAR.AUDIO.CONFIGURABLE.INTERACTIONS: BtActivityListener onReceive intent=Intent { act=android.bluetooth.a2dp-sink.profile.action.CONNECTION_STATE_CHANGED flg=0x4000010 (has extras) }
08-18 17:19:37.085279  1746  1746 D CAR.AUDIO.CONFIGURABLE.INTERACTIONS: BtActivityListener onReceive connected=false

 

And strategy applies: BT A2DP is lost, so Radio is started... 

08-18 17:19:37.086641  1746  3626 D CAR.MEDIA: Changing media source to: com.renault.radio

And radio becomes the current media source, after Audio Focus granted

08-18 17:19:37.690666  4052  4052 I RadioPlayer: requestAudioFocus

 

=> Need that the the current power state/audio state shall be taken into account before CAR.MEDIA applies the source backup to radio strategy.


</pre>
