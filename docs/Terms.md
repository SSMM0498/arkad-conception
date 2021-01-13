#   X State
##  Finite State Machines
A **Finite State Machine** is a mathematical model of computation that describes the behavior of a system that can be in only one state at any given time.
##  State Definition
A **State object** instance is JSON-serializable and has the following properties:
+   value - the current state value
+   context - the current context of this state
+   event - the event object that triggered the transition to this state
+   actions - an array of actions to be executed
+   activities - a mapping of activities to true if the activity started, or false if stopped.
+   history - the previous State instance
+   meta - any static meta data defined on the meta property of the state node
+   done - whether the state indicates a final state 4.7.1
It contains other properties such as historyValue, events, tree, and others that are generally not relevant and are used internally.

#   Video Player Term
##  Resolution
**Resolution** is a measure of the number of pixels a video contains both horizontally and vertically. Some common resolutions are 640×480 (SD) 1280×720 (HD), 1920×1080 (HD). Sometimes these are referred to just by their vertical dimension such as, 480p, 720p or 1080p.
##  Aspect Ratio
**Aspect Ratio** is the relationship between the width and the height of your video dimensions expressed as a ratio. The most common aspect ratios for video are 4:3, 5:4, 16:9 and 16:10.
##  Bit rate
**Bit rate** (also known as data rate) is the amount of data used for each second of video. In the world of video, this is generally measured in kilobits per second (kbps), and can be constant and variable.