---
layout: posts
title: "Plugin set for OpenSesame. Update"
tags: EEG EventExchanger ButtonBox coding opensesame python plugin
---

# EventExchanger

GitHub link:
[https://github.com/markspan/evtplugins](https://github.com/markspan/evtplugins)

## Short Description:

Set of [OpenSesame](https://osdoc.cogsci.nl/) toolbox items to use hardware developed by the University of Groningen,
faculty of Behavioural and Social Sciences, department of Research Support, in OpenSesame.

Code Written by Eise Hoekstra and Mark M. Span, Maintained by Mark M. Span

## Usage:

Start OpenSesame (preferably in Admin mode)
Open the console (ctrl-D) of OpenSesame, and type:

```
 !pip install evtplugins 
```
or, if you cannot obtain root privileges:
```
 !pip install evtplugins --user
```

and then, if this results in success, close OpenSesame, and open it again.

---
*when installing on a MAC (**or another platform that has no pip executable available in the terminal**) you should use:*

```
import pip
pip.main(['install', 'evtplugins'])
```

If all went well, the plugins are now available in your toolbox. 

---
*They will be useable either when installed as Admin, or as user, as long as a user can connect to the USB port (this could be more difficult when running Linux)*

---

![EVTPLUGINS](/images/evtpluginsplus.png)

*at the moment* there are five (5) plugins available. 

- [EVTXX](#EVTXX) item: send codes through an "EventExchanger" to a physiology recording device, to synchronise the behavioural data with the physiological data.
- [ResponseBox](#ResponseBox) item: Alternative to the default 'JoyStick' plugin. Made for the custom made buttonboxes of Research Support (works / can be made to work with all HID devices).
- [RGB_Led_Control](#RGB_Led_Control) item: Extended Responsebox item for use with the RGB Responsebox, enabling colour use and feedback on the buttonbox.
- [VAS](#VAS) item: a "Visual Analogue Scale". I tried to make it as customizable as possible, so its up to the user to stay close to the original VAS, or design their own. 
- [SHOCKER](#Shocker) Officially: *Tactile Stimulator* The tactile stimulator is a device that can be used to administer unpleasant tactile feedback to a subject.

## Usage of each item:

### <a name="EVTXX">EVTXX</a>
The purpose of this device is to send codes through an "EventExchanger" to a physiology recording system in order to synchronize behavioral data with physiological data.

![EVT-2](/images/EVT-2.jpg)

When you add the item to your task, it will look for connected EVT devices. In the item's configuration page, you can select the EVT device from a list. This enables the use of 
multiple EVT devices in the same task. During the development of your task, it is recommended to select the "DUMMY" device, which is always available and outputs the codes straight 
to the debug window of OpenSesame. Note that the list of devices is populated at startup, so connecting the device after startup will not enable it.


![EVT-2 config](/images/EVT-config.png)

The item has two modes: "Set Output Lines" and "Pulse Output Lines". In the first mode, the code value is placed on the output lines of the EVT until it is changed. In the second mode, 
the code value is placed on the output lines for a specified number of milliseconds, and then changed back to zero. Because the devices have eight output lines, the code values that 
can be conveyed through these devices range from 1 to 255. Normally, "0" means no code or event, and the values between 1 and 255 can be used to code stimuli.

In the "Pulse Output Lines" mode, OpenSesame will not pause until the code is reset (non-blocking!). Be careful not to send codes when the EVT is still "pulsing", as the recorded 
code can be affected. We have received reports of the EVTs getting confused and sending erroneous codes after possibly overlapping pulses. Therefore, we recommend pulsing with short 
latencies, approximately 4 times the time between two samples on the amplifier. This way, no code will be missed, and the chance of overlapping pulses is minimal.

#### Using in code:

The easiest way to use the code is to use the underlying library 'pyEVT' [GitHub Link](https://github.com/markspan/pyEVT) . Somewhere in the start of your task create and select the device you want to use:

```
 from pyEVT import EvtExchanger 
 EE = EvtExchanger()
 EE.Select("EVT") 
```

---
*The Parameter used (Here 'EVT') will select the USB device that has 'EVT' as part of the name. This will usually suffice, but if there are more devices that comply, you should use the serial number here to select ONE*.
*Using no, or empty strings will look for devices with "EventExchanger" in the name.*
---

and then optionally set the channels to 0.

``` 
EE.SetLines(0) 
```
Further on in your task you can now use the *SetLines* and *PulseLines* functions when you need to inform the physiology about an event.
```
EE.PulseLines(255, 1000) 
```
*If you use multiple devices that use the EvtExchanger API, you need to select the device to be used before you call functions on it: this includes the use of Buttonboxes and EventExchangers. When you use the plugin ("dragged and dropped) this is taken care of in the plugin.*

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

#### Using in code:
The easiest way to use the code is to use the underlying library 'pyEVT' [GitHub Link](https://github.com/markspan/pyEVT) . Somewhere in the start of your task create and select the device you want to use:

```
 from pyEVT import EvtExchanger 
 RSP = EvtExchanger()
 RSP.Select("RSP") 
```

Now you can wait for a response using the command:

``` 
RSP.WaitForDigEvents(AllowedEventLines, responseTimeout)
```

Where *AllowedEventLines* is the bitpattern containing the buttons that should generate a response, and *responseTimeout* the timeout value in ms.

### <a name="RGB_Led_Control">RGB_Led_Control</a>
The RGB-Led control is a version of the response-box that has keys that has RGB LEDs inside. The keys are quite a bit larger then the default ResponseBox keys.

![RSP-RGB config](/images/RSP-RGB-config.png)

The configuration is again similar to the ResponseBox configuration, but also has fields where the "current" colour of the keys can be defined.
If needed, you can also define a colour the pressed key will get, either when it is the correct, or if its one of the incorrect options.

### <a name="VAS">VAS</a>
The item itself is a bit more complex then the previous items.The VAS item is designed to be used with the Rotary Encoder. 

![Rotary Encoder](/images/RSP-RDC1.jpg)

It also has the option to be used with a standard mouse. 
The item interacts with a standard [sketchpad](https://osdoc.cogsci.nl/3.3/manual/stimuli/visual/#using-the-sketchpad-and-feedback-items) item. In particular,
the item refers to a canvas with some [named](https://osdoc.cogsci.nl/3.3/manual/python/canvas/#naming-accessing-and-modifying-elements) elements on it. 
The names of the elements can be entered in the configuration of the item.

![VAS](/images/VAS1.png)

There are 3 names relevant for the VAS item: 
 - the name of the sketchpad to interact with, 
 - the name of the line element (on this sketchpad) on which the cursor moves, 
 - and the name of the cursor.
 
Optionally, there is the possibility to animate a timer (again, on the named sketchpad), counting down to the "end of response". If this element is used, it also must be named.

![VAS config](/images/VAS-config.png)

The relative complexity of the VAS item led to the inclusion of a bare-bones example of its use. This example can be found under Tools - Example experiments in OpenSesame.

There is also a **VAS2** item in the toolbox: this is a customized version, that has no coupling with any encoders and can only be used in combination with the mouse. The customizable parameters are therefore different. The main features are: the use of a GUI button to end the selection: that is, a rectangle on the canvas that is named in the parameters: when clicked after a selection, it will end the VAS2. (Optional) labels that can be clicked to the maximum and minimum values of the VAS, and the **appearance** of the cursor after the VASBODY is clicked.  


### <a name="Shocker">Shocker</a>
## Tactile Stimulator.

The tactile stimulator is a device that can be used to administer unpleasant tactile feedback to a subject. 

![LooksLike](/images/tactilestimulator.jpg)

***
*This maximum value of the applied current is 5 mA, which is reached
 when a byte with value 255 (being 100%) is sent to the Tactile Stimulator. 
 When a 0 is sent, the current will be 0 mA. This value can (and should) be limited by a careful calibration procedure!*
 
***

When using the Tactile Stimulator the first thing to do is to run a calibration. This will limit the current send to the subject to a maximum, that is being calibrated to the subjective experience of the subject.

You do so by dragging the plugin into you experiment. The configuration pane looks like this:

![Calib](/images/TS_Calibration.png)

Running this plugin in the 'calibration' mode (as seen above) will lead to the view below:

![CalibScreen](/images/TS_CalibrationScreen.png)

Clicking (with the mouse) on the bar in the middle of the screen will change the values for the maximum current to be used in the experiment. The newly chosen values are readable in the green field on this screen. 

Pressing the red "Test" button will administer this current to the subject. The calibration procedure will entail a steadily increasing current, and getting feedback from the subject on the subjective experience. The shock should probably be annoying, and should definitely not hurt. After each "Test" shock, a pause of 8 seconds will start, with the button turning blue. During this pause no test shock can be administered. This is to prevent accidental repetitive shocks.

When the calibration value is acquired, the green "OK" button can be pressed, and the calibration value is accepted. Your task will now continue.

Dragging a second "shock" plugin to the task, will enable the administering of shock upto the calibration value to your subject. The calibration screen in "shock" mode will look like this:

![Shock](/images/TS_Shock.png)
 
Main thing to note here is (next to the selection of the actual device: "Productname") is the activation of the 'Percentage' field. This field can be filled with a number between 0 and 100, and is used as a index, leading to a shock to be administered to the subject, with a current that is defined as a percentage of the calibration value.

So if the calibration led to the selection of a value of 2.5mAh (50%), setting the "Percentage" field to e.g., 30% will lead to a shock with a current of 30% *of the calibration value of 2.5mAh*, ie 0.75 mAh !

***
*The plugin will not allow fast repetitions of the shocks, as they usually are not what the researcher wants (or at least should want). The default minimum ISI (inter-shock-interval) is 1 second. The duration of the stimulation is normally 150ms. This cannot be changed through the interface, as it is meant to be constant. The value **is** visible, and will be logged in a logger item*

***