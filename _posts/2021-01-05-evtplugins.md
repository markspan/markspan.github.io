---
layout: posts
title: "Plugin set for OpenSesame. Plugins from Research Support BSS University of Groningen"
tags: eeg EventExchanger ButtonBox coding opensesame python plugin
---

# EventExchanger

GitHub link:
[https://github.com/markspan/evtplugins](https://github.com/markspan/evtplugins)

## Short Description:

Set of [OpenSesame](https://osdoc.cogsci.nl/) toolbox items to use hardware the University of Groningen in 0penSesame,
faculty of Behavioural and Social Sciences, department of Research Support developed.

Code Written by Eise Hoekstra and Mark M. Span, Maintained by Mark M. Span

## Usage:

Start OpenSesame *as Administrator*.
Open the console (ctrl-D) of OpenSesame, and type:

```
 pip install evtplugins
```
and then, if this results in success, close OpenSesame, and open it again, but now no need for administrators rights any more.

If all went well, the plugins are now available in your toolbox.


*at the moment* there are four (4) plugins available. 

- EVTXX item: send codes through an "EventExchanger" to a physiology recording, to synchronise the behavioural data with the physiological data.
- ResponseBox item: Alternative to the default 'JoyStick' plugin. Made for the custom made buttonboxes of Research Support.
- RGB_Led_Control item: Extended Responsebox item for use with the RGB Responsebox, enabling colour use and feedback on the buttonbox.
- VAS item: a "Visual Analogue Scale". I tried to make it as customizable as possible, so its up to the user to stay close to the original VAS, or design their own.


## Usage of each item:

### EVTXX
The purpose of this item/device is to send codes through an "EventExchanger" to a physiology recording, to synchronise the behavioural data with the physiological data.
What we call an "EventExchanger", usually is an **EVT-2**, see picture below. This is an USB version of the **EVT-1**, which did/does the same, but was/is connected to a printer-port. 
There is also an **EVT-3**. This is a larger version of the EVT-2, and is only used in the EEG cabins at the basement. The EVT-2 and EVT-3 are compatible, and only differ in extra (external) connections for the EVT-3.
![EVT-2](/images/EVT-2.jpg)

The toolbox item for the EVTXX is to be used for either the EVT-2 and the EVT-3. When you place the item in your task, it will look for *attached* EVT devices. 
In the configuration page of the item you can select the to-be-used EVT from a list. This enables the use of multiple EVT devices in the same task. During the development of your task it is
advised to select the "DUMMY", which is always available, and will output the codes straight to the debug window of OpenSesame. **Be aware:** the list of devices is populated at startup, so connecting 
the device after startup will not enable it. 

![EVT-2 config](/images/EVT-config.png)

The item has two modes: *set output lines*, and *pulse output lines*. The first mode will place the code value on the output lines of the EVT until changed, the second mode will
do so for a number of milliseconds, and then change back to zero. Because the devices have eight outputlines, the code you can convey through these devices can vary between 0 and 255.

Normally, "0" means no code, no event, and the values between 1 and 255 can be used to code your stimuli. 

---
*In the "Pulse Output Lines" mode, OpenSesame will **not** pause until the code is reset. Be careful not to send codes when the EVT is still "pulsing", as the identification of the stimuli will be affected. We also had reports of the EVTs "getting confused" and pulsing codes that were not defined after possibly overlapping pulses. We therefore advice pulsing for approximately 4 times the time between two samples on the amplifier. No code will the be "missed" and the chance of overlapping pulses is nil.*

--- 

#### Using in code:

The easiest way to use the code is to use the underlying library 'pyEVT'. Somewhere in the start of your task create and select the device you want to use:

```
 from pyEVT import EvtExchanger 
 EE = EvtExchanger.Device() 
```

Then, to select the device you need:

``` 
EE.Select("EVT") 
```
---
*Using 'EVT' will select the USB device that has 'EVT' as part of the name. This will usually suffice, but if there are more devices that comply, you should use the serial number here to select ONE*

---

and then set the channels to 0.

``` 
EE.SetLines(0) 
```
Further on in your task you can now use the *SetLines* and *PulseLines* functions when you need to inform the physiology about an event.
```
EE.PulseLines(255, 1000) 
```
 

## ResponseBox
![RSP-12](/images/RSP-12.jpg)
![RSP-12 config](/images/RSP-config.png)
### RGB-Led-Control
![RSP-RGB config](/images/RSP-RGB-config.png)
### VAS
![VAS](/images/VAS1.png)
![VAS config](/images/VAS-config.png)

---
