---
layout: posts
title: "Plugin set for OpenSesame. Plugins from Research Support BSS University of Groningen"
tags: eeg EventExchanger ButtonBox coding opensesame python plugin
---

# EventExchanger

Github link:
[https://github.com/markspan/evtplugins](https://github.com/markspan/evtplugins)

## Short Description:

Set of [OpenSesame](https://osdoc.cogsci.nl/) toolbox items to use hardware the University of Groningen in 0penSesame,
faculty of Behavioural and Social Sciences, department of Research Support developed.

Code Written by Eise Hoekstra and Mark M. Span, Maintained by Mark M. Span

## Usage:

Start Opensesame *as Administrator*.
Open the console (ctrl-D) of OpenSesame, and type:

```
 pip install evtplugins
```
and then, if this results in succes, close OpenSesame, and open it again, but now no need for administrators rights anymore.

If all went well, the plugins are now availeable in your toolbox.


*at the moment* there are four (4) plugins availeable. 

- EVTXX item: send codes throught an "EventExchanger" to a fysiology recording, to synchronise the behavioural data with the fysiological data.
- ResponseBox item: Alternative to the default 'JoyStick' plugin. Made for the custom made buttonboxes of Research Support.
- RGB_Led_Control item: Extended Responsebox iterm for use with the RGB Responsebox, enabeling color use and feedback on the buttonbox.
- VAS item: a "Visual Analogue Scale". I tried to make it as customizeable as possible, so its upto the user to stay close to the original VAS, or design theitr own.


## Usage of each item:

### EVTXX
The purpose of this item/device is to send codes throught an "EventExchanger" to a fysiology recording, to synchronise the behavioural data with the fysiological data.
What we call an "EventExchanger", usually is an **EVT-2**, see picture below. This is an usb version of the **EVT-1**, which did/does the same, but was/is connected to a printerport. 
There is also an **EVT-3**. This is a larger version of the EVT-2, and is only used in the EEG cabins at the basement. The EVT-2 and EVT-3 are compatible, and only differ in extra (exyternal) connections for the EVT-3.
![EVT-2](/images/EVT-2.jpg)

The toolbox item for the EVTXX is to be used for either the EVT-2 and the EVT-3. When you place the item in your task, it will look for *attached* EVT devices. 
In the configurationpage of the item you can select the to-be-used EVT from a list. This enables the use of multiple EVT devices in the same task. During the development of your task it is
advised to select the "DUMMY", which is always availeble, and will output the codes straight to the debugwindow of OpenSesame. **Be aware:** the list of devices is populated at startup, so connecting 
the device after startup will not enable it. 

![EVT-2 config](/images/EVT-config.png)

The item has two modes: *set output lines*, and *pulse output lines*. The first mode will place the codevalue on the output lines of the EVT untill changed, the second mode will
do so for a number of milliseconds, and then change back to zero. Because the devices have eight outputlines, the code you can convey through these devices can vary between 0 and 255.

Normally, "0" means no code, no event, and the values between 1 and 255 can be used to code your stimuli. 

---
*In the "Pulse Output Lines" mode, Opensesame will **not** pause untill the code is reset. Be carefull not to send codes when the EVT is still "pulsing", as the identification of the stimuli will be affected. We also had reports of the EVTs "getting confused" and pulsing codes that were not defined after possibly overlapping pulses. We therefore advice pulsing for aproximately 4 times the time between two samples on the amplifier. No code will the be "missed" and the chance of overlapping pulses is nihil.*

--- 

#### Using in code:

'Examples of how to use the EVT in Python inline code items'

## ResponseBox
![RSP-12](/images/RSP-12.jpg)
![RSP-12 config](/images/RSP-config.png)
### RGB-Led-Control
![RSP-RGB config](/images/RSP-RGB-config.png)
### VAS
![VAS](/images/VAS1.png)
![VAS config](/images/VAS-config.png)

---
