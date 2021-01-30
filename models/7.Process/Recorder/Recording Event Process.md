#   RECORDING EVENT PROCESS

##  UTILITY & DESCRIPTION
This class is important to :
+   Initialize a record environnment
    +   Indeed this is the main class of the record component, it will be up to him to initialize all classes concerned by record :
        +   The mutation buffer
        +   The node captor
        +   All watchers
        +   The listener
    +   In another hand it must link DOM elements with the corresponding action in the recorder :
        +   A button to toggle between the differents states of the recorder (start/pause/resume/stop) and thus enable user to handle the recording process.
        +   A button to activate or deactivate the microphone.
+   Save the arkast as a file on server disk after pack down the JSON file of all events timeline and audio file in order to reduce size of output file.
##  INITIALISATION OF THE MAIN CLASS
### INITIALIZE THE NODE CAPTOR
Create an instance of the node captor. This is class used for making instant capture of the whole DOM object in memory. The functionnement of the node captor class is detailed and explain [here](./Capturing%20Node%20Process.md)
### INITIALIZE THE MUTATION BUFFER
Create an instance of the mutation buffer. The mutation buffer is needed to record more efficiently mutations that occur. The functionnement of the mutation buffer class is detailed and explain [here](../8.Workflow/Mutation%20Workflow.md)
### INITIALIZE ALL WATCHERS
Create an instance for each watchers classes. There are a number of watchers classes which are :
+   InputWatcher
+   MouseInteractionWatcher
+   MouseMovementWatcher
+   MutationWatcher
+   ScrollWatcher
+   TextSelectionWatcher
Click [here](WatchersProcess.md) to learn more about watchers.
### INITIALIZE THE MICRO LISTENER
Create an instance of the microphone listener. It allow us to record the voice of the user.

---
##  START RECORDING
### TAKE THE FIRST FULL NODE CAPTURE
This is the base full nodes capture of the page. It enables to have all node present in the page as formated node at the start. First at all we capture meta informations of the page (location, width, height) and record it in the events time line array. This is the type of event that is produced with information about :
+   href the current location of the browser tab (url)
+   the current width of the page
+   the current height of the page
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

Take care that no DOM modification occur during full node capture to avoid some incoherence. To do that we freeze the mutationBuffer before calling the capture method of the nodeCaptor. After that the full capture is saved as an event in array and unfreeze the mutationBuffer. Take a look of the [mutationBuffer](Mutation%20Watcher%20Buffer.md) doc for more understanding that. The full capture return the following data structure :
+   fullCaptureEvent : contains the payload (data) of a full capture event with :
    +   NodeCaptured is the current capture of all element in the page.
    +   initialOffset is scroll position of document.
```typescript
type fullCaptureEvent = {
    type: EventType.FullCapture
    data: {
        node: NodeCaptured
        initialOffset: {
            top: number
            left: number
        }
    }
}
```

### START WATCHING
Call the watch method for each watchers classes. Click [here](Watchers.md) to learn more about watchers classes.

### START LISTENING
Call the listen method of the microphone listener class.
##  ADD NEW RECORDED EVENT
This method is very usefull to save a capture event in the events time line. This is callback function of all watchers classses. It allows them to emit their recorded events. But particularities of these method are :
+   To detect if an user event is occur while the MutationBuffer was freeze and so apply all DOM changes that have been buffering during paused state.
+   Check if it is time to do a new full node capture. Indeed, a serie of full node captures is carried out recurrently according to a time interval. This mecanism is very important during the replay and allows us to provide similar functionality to video players, such as dragging and dropping to the progress bar to any point in time. Without it, if a user play the arkast in a specific point of the progress bar all the record event before this timestamp must be execute to ensure that the DOM is in the desired state (have all mutations applied) at that timestamp. This process is expensive in terms of performance. That's why by doing frequently a full node capture of the DOM we just need to retrive the near one before that timestamp and just perform a reduce number of actions done before that point.
##  PAUSE/RESUME RECORDING
When
##  STOP RECORDING
When
##  SAVE TO FILE
When