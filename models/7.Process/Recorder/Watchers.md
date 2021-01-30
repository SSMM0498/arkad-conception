#   WATCHER CLASSES

##  UTILIY & DESCRIPTION
A set of action are done by users and we need to record each of them as events. Right now, we record the following events :
+   DOM changes
   +   Node creation, deletion
   +   Node attribute changes
   +   Text changes
+   Mouse movement
+   Mouse interaction
   +   mouse up, mouse down
   +   click, double click, context menu
   +   focus, blur
   +   touch start, touch move, touch end
   +   Page or element scrolling
+   Scroll
+   Input
+   Text selection
+   Window size changes

Watchers are here keep watch on user and brief the recorder by sending it a corresponding event related to this action. Thus we have 6 watchers which are :
+   MutationWatcher
+   MouseMovementWatcher
+   MouseInteractionWatcher
+   ViewPortWatcher
+   InputWatcher
+   TextSelectionWatcher

In this doc all watchers are detailled except the mutation watcher. Click [here](Mutation%20Watcher%20Buffer.md) to know more about this kind of watcher.

### MOUSE MOVEMENT & SCROLL WATCHERS
For both, as their name suggest, they watch respectively all movement of the mouse and scroll action and report to the recorder an event with the current scroll or mouse position. Those positions will be used to simulate the mouse movement trajectory during replay or redo a scroll action. However we must ensure that the mouse moves smoothly during replay and also minimize the number of corresponding, so we need to perform two layers of throttling while listening to mousemove.
+   The first layer records the mouse coordinates at most once every 20 ms.
+   The second layer transmits the mouse coordinate set at most once every 500 ms to ensure a single event doesn't accumulate a lot of mouse position data and becomes too large.

The process of the throttle function will be done later. Just keep in mind that a throttle function is a common technique used in the browser to improve an application’s performance by limiting the number of events your code needs to handle.

The scroll watcher use also the throttle function but with a single layer and at shorter interval.

### MOUSE INTERACTION WATCHER
We use an unique watcher for all mouse interaction event. And for each of them we just need to get
+   id of the target node
+   position (x, y) where the event is triggered

### INPUT WATCHER
We need to observe the input of the three elements `<input>`, `<textarea>`, `<select>`, including human input and programmatic changes.

### HUMAN INPUT
For human input, we mainly rely on listening to the input and change events. It is necessary to deduplicate different events triggered for the same the human input action. In addition, `<input type="radio" />` is also a special kind of control. If the multiple radio elements have the same name attribute, then when one is selected, the others will be reversed, but no event will be triggered on those others, so this needs to be handled separately.

### PROGRAMMATIC CHANGES
Setting the properties of these elements directly through the code will not trigger the MutationObserver. We can still achieve monitoring by hijacking the setter of the corresponding property. The sample code is as follows:

### TEXT SELECTION WATCHER

### TIME HANDLING
We record a timestamp when each incremental snapshot is generated so that during replay it can be applied at the correct time. However, due to the effect of throttling, the timestamps of the mouse movement corresponding to the incremental snapshot will be later than the actual recording time, so we need to record a negative time difference for correction and time calibration during replay.

##  MAIN FUNCTION PROCESS
All watchers have nearly the same process. A public method to start watching the corresponding events. For all, it is simply a single or multiple call of the needed event listenner function except for the MutationWatcher which use the MutationObserver WebAPI. The callback function of this event listenner will be the capture method of the class. They are method used for put the event in the good format and return it to the MainRecorder class.

##  DATA STRUCTURE DESCRITPTION
+   event
```typescript
type event =
    | fullCaptureEvent
    | incrementalCaptureEvent
    | metaEvent
```

+   eventWithTime
```typescript
type eventWithTime = event & {
    timestamp: number
    delay?: number
}
```

+   incrementalCaptureEvent
```typescript
type incrementalCaptureEvent = {
    type: EventType.IncrementalCapture
    data: incrementalData
}
```

+   metaEvent
```typescript
type metaEvent = {
    type: EventType.Meta;
    data: {
        href: string;
        width: number;
        height: number;
    }
}
```

+   IncrementalSource
```typescript
enum IncrementalSource {
    Mutation,
    MouseMove,
    MouseInteraction,
    Scroll,
    ViewportResize,
    Input,
    TouchMove,
    MediaInteraction,
    StyleSheetRule,
    TextSelection
}
```

+   mutationData
```typescript
type mutationData = {
    source: IncrementalSource.Mutation
} & mutationCallbackParam
```

+   mousemoveData
```typescript
type mousemoveData = {
    source: IncrementalSource.MouseMove | IncrementalSource.TouchMove
    positions: mousePosition[]
}
```

+   mouseInteractionData
```typescript
type mouseInteractionData = {
    source: IncrementalSource.MouseInteraction
} & mouseInteractionParam
```

+   textSelectionData
```typescript
type textSelectionData = {
    source: IncrementalSource.TextSelection,
    selection: Selection
}
```

+   scrollData
```typescript
type scrollData = {
    source: IncrementalSource.Scroll
} & scrollPosition
```

+   viewportResizeData
```typescript
type viewportResizeData = {
    source: IncrementalSource.ViewportResize
} & viewportResizeDimension
```

+   inputData
```typescript
type inputData = {
    source: IncrementalSource.Input
    id: number
} & inputValue
```

+   mediaInteractionData
```typescript
type mediaInteractionData = {
    source: IncrementalSource.MediaInteraction
} & mediaInteractionParam
```

+   styleSheetRuleData
```typescript
type styleSheetRuleData = {
    source: IncrementalSource.StyleSheetRule
} & styleSheetRuleParam
```

+   incrementalData
```typescript
type incrementalData =
    | mutationData
    | mousemoveData
    | mouseInteractionData
    | scrollData
    | viewportResizeData
    | inputData
    | mediaInteractionData
    | styleSheetRuleData
    | textSelectionData
```