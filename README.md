# signalk-threshold-notifier

A [Signal K Node Server](https://github.com/SignalK/signalk-server-node) plugin
which raises notifications based on the values of one or more monitored Signal
K paths.

Thanks are due to Scott Bender for his
[signalk-simple-notifications](https://github.com/sbender9/signalk-simple-notifications)
plugin which this work simply elaborates.
## System requirements

__signalk-threshold-notifier__ has no special system requirements.
## Installation

Download and install __signalk-threshold-notifier__ using the _Appstore_ link
in your Signal K Node server console.

The plugin can also be downloaded from the
[project homepage](https://github.com/preeve9534/signalk-threshold-notifier)
and installed using
[these instructions](https://github.com/SignalK/signalk-server-node/blob/master/SERVERPLUGINS.md).
## Usage

 __signalk-threshold-notifier__ is configured through the Signal K Node server
plugin configuration interface.
Navigate to _Server_->_Plugin config_ and select the _Threshold notifier_ tab.

![Configuration panel](readme/screenshot.png)

Configuration is simply a matter of maintaining the __Monitored paths__ list
of Signal K Node server paths which the plugin should monitor and specifying
the conditons under which notifications should be raised and the attributes
which should be associated with them.

On first use the list of monitored paths will include a single, empty, entry
which should be completed.
Additional new monitored paths can be added by clicking the __[+]__ button and
any existing, unwanted, paths can be deleted by clicking the __[x]__ button,
both located in the control panel to the right of the list. 

Each monitored path configuration entry consists of the following fields.

__Enabled__  
Checkbox specifying whether or not the path should be monitored.
Default is yes (checked).
Change this to temporarily disable individual notifications rather than
permanently deleting them.

__Signal K path to monitor__  
The Signal K Node server path which should be monitored.
There is no default value.
Enter here the full Signal K path for the value which you would like to
monitor, for example, `tanks.wasteWater.0.currentValue`.

__Notification message__  
The text of the message which will be issued as part of any notification.
Default is the empty string.
Enter here the text of the message you would like to be issued when the
monitored path value crosses one of the defined thresholds.
If any of the following tokens are used they will be interpolated when the
notification message is composed:

_${vessel}_ will be replaced with Signal K's idea of the vessel name.

_${test}_ will be replaced by one of "above", "below" or "between"
dependant upon the threshold being crossed and the direction of crossing.

_${threshold}_ will be replaced with the value of the threshold or, in the
case of the path value being between thresholds with the string "_n_ and _m_"
where _n_ is the low threshold and _m_ is the high threshold.

_${value}_ is the instantaneous value of the monitored path that caused the

For examle `${vessel}: waste water tank level is ${test} ${threshold} (currently ${value})`

__Thresholds__

This entry consists of a list of one or more threshold specifications each of
which defines a value threshold value range and the type of notification which
will be issued if the monitored path value moves into or out of this range.
Typically, multiple threshold entries can be used to escalate the severity of
a notification as the monitored path value makes increasingly significant
excursions beyond one or other threshold. 

Each threshold item has the following properties.

__--> Low__  
If supplied, specifies the lower threshold for raising a notification: if
the monitored path value falls below this value then a notification will
be issued.
The default value is undefined which means that there is no lower
thresold.

__--> High__  
If supplied, specifies the upper threshold for raising a notification: if
the monitored path value rises above this value then a notification will
be issued.
The default value is undefined which means that there is no upper
thresold.

__--> Alarm__  
The type of notification to be raised when one or other threshold is
passed.
Default is _Alert_.

__--> Request__  

__--> Options__  
The _audio_ and _visual_ checkboxes allow a suggestion to be made for thes
alert medium to be used when this notification is ultimately processed by
some notification handler.

The _normal_ option requests that a notification also be issued when the
monitored path value re-enters the 'between-thresholds' region after
transiting one or other threshold.

The defaults are to make no suggestions and not to issue an in-range
notification.
## Messages

__signalk-threshold-notifier__ outputs the following messages to the Signal K
Node server console and the host system logs.

__Monitoring _n_ path__[__s__]  
Output when the plugin initialises to report the number, _n_, of Signal K
paths that are being monitored for threshold transition events.

__Notifying on path__   
Output when the plugin issues a threshold transition notification.
The message on the server console persists for a few seconds before
the normal status message is restored.
