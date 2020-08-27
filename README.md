# Master Thesis : Comparison of Smoothness in Progressive Web Apps and Mobile Applications on Android

This repository includes one of the major contributions of my master thesis : 
* the model of Chrome frame rendering workflow i.e. the pattern of events each frame goes through frome the moment Chrome decides to start a frame, until it is either dropped or displayed
* the tool used to checked the correspondance of the model (the identified pattern of events) with the log of events recorded and to extract key timestamps of each frame computed during the recording

## Model of Chrome frame rendering workflow

The identified pattern of events each frame goes through is available here as a picture or here https://drive.google.com/file/d/1mV4hpf-9-lWhRUSZyzA7cwhj7-CbfqoF/view?usp=sharing (this drawing is kept in my google drive, but may bee deleted sometimes in the future).

A new frame can follow overall 3 different patterns though some combinations of the three can occur : 
* the browser surface changes (pool 'Browser Frame')
* the app surface changes slightly (pool 'Basic Frame')
* the app surface changes entirely (pools 'Basic Frame' + 'Main Frame')

Each pool is divided into rows representing the thread the events inside belong to. The arrows connecting the events represent either a chronological order (one event happens after the other) or a parent/child relationship (one event 'contains' the other event, the former starting before and ending after the latter).

For more detailed information about this model, please refer to my Master Thesis report.

## FrameTracker

*Frametracker* is a tool written written in javascript able to extract key timestamps of the frames from a log of events, according to the model presented previously. The log of events comes from Chrome Tracing tool, which can export a recording as a JSON file.
To use *FrameTracker*, do the following :

```javascript
const frameTracker = require('FrameTracker.js');
const frameModel = frameTracker.processEvents(event);
```

Upon execution, frameTracker.processEvents(events) will log on the console any mismatch detected between the model and the events observed. It will output a FrameModel object, which contains:
* a list of Browser Frames
```javascript
frameModel._frames['CrBrowserMain'] = new FramesList()
```
* a list of Basic Frames
```javascript
frameModel._frames['Compositor'] = new FramesList()
```
* a list of Main Frames
```javascript
frameModel._frames['CrRendererMain'] = new MainFramesList()
```

All objects (FramesList, MainFramesList, MainFrame, Frame, FrameModel) are defined in FrameTracker.js


