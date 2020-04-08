---
layout: posts
title: "Info on the Library for use with the 'EventExchanger'"
tags: eeg analysis EventExchanger coding
---

# EventExchanger

Github link:
[https://github.com/markspan/EventExchanger](https://github.com/markspan/EventExchanger)

## Disclaimer:

C-Sharp code to communicate with the hardware the University of Groningen,
faculty of Behaviour ans Social Sciences, department of Research Support developed.

Code Written by Eise Hoekstra and Mark M. Span, Maintained by Mark M. Span

## Usage:

Any environment that can read and use .net code will be able to use this library.

e.g., Matlab and Python.

For Python:

```
 pip install pythonnet
```
and then things like this:

``` python

import clr
clr.AddReference("System.Reflection")
from System.Reflection import Assembly

directory = os.path.dirname(__file__)
Assembly.UnsafeLoadFrom(directory+'\HidSharp.dll')
Assembly.UnsafeLoadFrom(directory+'\HidSharp.DeviceHelpers.dll')
Assembly.UnsafeLoadFrom(directory+'\EventExchanger.dll')

EVT2 = clr.ID.EventExchanger()
EVT2.Start()
```

after wich you can:

``` python

EVT2.PulseLines(code,ms)

```

For Matlab:

``` matlab
% Put the three (3) dll's in the current directory for the task!
warning ('off', 'MATLAB:NET:AddAssembly:nameConflict');
NET.addAssembly([pwd '\EventExchanger.dll']);
warning ('on', 'MATLAB:NET:AddAssembly:nameConflict');

% the needed dll are:
% EventExchanger.dll,
% HidSharp.dll and
% HidSharp.DeviceHelpers.dll

% Create and start the object
EE = ID.EventExchanger;
EE.Start();

% if multiple EVT-2s are connected add a serialnumber as parameter to start
% e.g.,
% EE.Start('11007');

% to pulse the port with number x for y millisecs do:
% EE.PulseLines(x,y);

EE.PulseLines(255,4);

% Thank you, come again!
```
