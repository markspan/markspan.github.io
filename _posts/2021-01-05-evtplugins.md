---
layout: posts
title: "Plugin set for OpenSesame. Plugins from Research Support BSS University of Groningen"
tags: EEG EventExchanger ButtonBox coding opensesame python plugin
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
 !pip install evtplugins
```
and then, if this results in success, close OpenSesame, and open it again, but now no need for administrators rights any more.

If all went well, the plugins are now available in your toolbox.

![EVTPLUGINS](/images/evtplugins.png)

*at the moment* there are four (4) plugins available. 

- [EVTXX](#EVTXX) item: send codes through an "EventExchanger" to a physiology recording, to synchronise the behavioural data with the physiological data.
- [ResponseBox](#ResponseBox) item: Alternative to the default 'JoyStick' plugin. Made for the custom made buttonboxes of Research Support.
- [RGB_Led_Control](#RGB_Led_Control) item: Extended Responsebox item for use with the RGB Responsebox, enabling colour use and feedback on the buttonbox.
- [VAS](#VAS) item: a "Visual Analogue Scale". I tried to make it as customizable as possible, so its up to the user to stay close to the original VAS, or design their own.

## Usage of each item:

### <a name="EVTXX">EVTXX</a>
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
*In the "Pulse Output Lines" mode, OpenSesame will **not** pause until the code is reset (non-blocking!). Be careful not to send codes when the EVT is still "pulsing", as the code that will be recorded can be affected. We  have had reports of the EVTs "getting confused" and sent erroneous codes after possibly overlapping pulses. We therefore advice pulsing with short latencies, for about 4 times the time between two samples on the amplifier. No code will the be "missed" and the chance of overlapping pulses is minimal.*

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
 

### <a name="ResponseBox">ResponseBox</a>

There are multiple devices that work with this plugin. They mostly differ in number and position of the buttons that are attached, but they are also available as voice-keys, and most of them can even record the occurrence of an r-top.
This is the generic form:

![RSP-12](/images/RSP-12.jpg)

But there are many variations, with touch-buttons, different layouts etc. The can also be made bespoke.
There is also a footpedal-form:

![RSP-12-f](/images/RSP-12-F.jpg)


The devices are known to windows as joysticks, and can also be used with the generic joystick plugin.
The configuration is also remarkably similar to the generic joystick plugin. The 'Responsebox' plugin is also compatible with the *Psycho* back-end.

![RSP-12 config](/images/RSP-config.png)

### <a name="RGB_Led_Control">RGB_Led_Control</a>
The RGB-Led control is a version of the response-box that has keys that has RGB LEDs inside. The keys are quite a bit larger then the default ResponseBox keys.

![RSP-RGB config](/images/RSP-RGB-config.png)

The configuration is again similar to the ResponseBox configuration, but also has fields where the "current" colour of the keys can be defined.
If needed, you can also define a colour the pressed key will get, either when it is the correct, or if its one of the incorrect options.

### <a name="VAS">VAS</a>
The item itself is a bit more complex then the previous items.The VAS item is designed to be used with the Rotary Encoder. It also has the option to be used with a standard mouse. 
The item interacts with a standard [sketchpad](https://osdoc.cogsci.nl/3.3/manual/stimuli/visual/#using-the-sketchpad-and-feedback-items) item. In particular,
the item refers to a canvas with some [named](https://osdoc.cogsci.nl/3.3/manual/python/canvas/#naming-accessing-and-modifying-elements) elements on it. 
The names of the elements can be entered in the configuration of the item.

![VAS](/images/VAS1.png)

There are 3 names relevant for the VAS item: the name of the sketchpad to interact with, the name of the line element (on this sketchpad) on which the cursor moves, and the name of the cursor.
Optionally, there is the possibility to animate a timer (again, on the named sketchpad), counting down to the "end of response". If this element is used, it also must be named.

![VAS config](/images/VAS-config.png)

There is also a configuration item "Start value". This value sets the starting point of the VAS cursor, when the rotary encoder is used. At the moment it is not functional (:(). Because of this, the item can now only be used with the mouse as input device.

The relative complexity of the VAS item led to the inclusion of a bare-bones example of its use. This example can be found under Tools - Example experiments in OpenSesame.

---
