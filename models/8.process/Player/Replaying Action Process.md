#   REPLAYING ACTION PROCESS

##  UTILITY & DESCRIPTION
After reading design principle behind the recorder it's to highlight that its process as little as possible minimize the impact on the recorded page. All this minimisation must perform by the player.
During replay, the complete event timeline list is get at one time. We need to synchronously initialize the first full event, and then apply the remaining incremental event asynchronously. Using a time interval we replay each incremental event one after the other, which requires a high-precision timer. The reason why **high precision** is used instead of classic `setTimeout` is because the native this one doesnt ensure accurate execution after the set delay time, for example, when the main thread is blocked.
For our replay package, this imprecise delay is unacceptable and can lead to various bizzard phenomena, so we implement a constantly calibrated timer with `requestAnimationFrame` to ensure that in most cases incremental event is performed as action. All type of event watchers catch a specific kind of event we need also a different type of action performers such as each on execute the corresponding actions from the recorded event.
As mention in the recording event process enabling the user to play the arkast any point of the progress bar is an essential feature. To make sure that is possible, each time a player fast forward to a point we load the closest full node capture and perform actions from it. But to garantee a effective user experience during the fast-foward some optimizations processings must be done:
+   First the player have to ignore some of the events during fast forward. For example, when we are going to fast forward to 10 minutes later, we do not need to perform mouse movement action during this period.
+   Second we must use a temporary list added element finally being appended into the document as a batch operation. That allow to append element in one operation instead of a several. In addition, this process garantee all added and then deleted node during fast forward are not append for being delete right after.
+   Finally we must make the fast forward process async and cancellable. This may make the drag and drop interactions in the player's UI looks smooth
Also for perfomance ee use a virtual DOM tree to store the mutations of DOM. This will minimize the number of DOM operations.

From now a good question is where all this actions will be perform. We can simply create a div in the player page and consider it as our root docuemtn player but there is a better solution : using an iframe as document. That's a better solution because of the feature provided by HTML for browser-level restrictions. That will garantee a isolation of all modifications done during the replay. This isolation will causes for example when an interactive element is click, replaying the JS `click` event is not nessecary because click events do not have any impact when JS is disabled. However, in order to optimize the replay effect, we can add special animation effects to visualize elements being clicked with the mouse, to clearly show the viewer that a click has occurred.

In another hand we need to provide enabling an interactive mode during the pause state. The implementation of this essential functionnality is performed ...

##  ARCHITECTURE

![PlayerArchiture](../../../out/models/6.Component/2.player/player%20component%20diagram.png)

##  INITIALISATION OF THE MAIN CLASS
### RETRIEVE THE RECORDED EVENTS AND AUDIO
Collect the recorded events from the arkast file and the audio.
+  Unpack the arkast file to get the event as an array
+  Get the path of the file audio

### SET ALL HANDLERS FOR EVENTS SENT TO THE EMITTER
+  Flush handler
+  Resize handler

### INITIALIZE THE UTILS CLASSES
+  Setup the player dom (wrapper, iframe, fake cursor mouse)
+  Create a Node builder to rebuild dom from a node formated
+  Create an action scheduler
+  Init the player state machine and run it
### REBUILD FIRST FULL SNAPSHOT AS THE POSTER OF THE PLAYER
rebuild first full snapshot as the poster of the player and cache it for performance optimization

### INIT UTILS
##  START PLAYING
This API was designed to be used as play method at any time offset. The implementation of play at any time offset will always iterate all of the events, perform action associated to it before the offset synchronously or perform the action after the offset asynchronously with actions buffers handler.
+   Switch to pause state first on player state machine if it's not the case
    +  Delete all buffered actions
    +  Retrieve need events
    +  Add delay to the current event according to the baseline
   +  Mouse move events was recorded in a throttle function, so we need to find the real timestamp by traverse the time offsets.
+  Switch to play state on player state machine if it's not the case
+  delete the pause class in iframe
##  PAUSE PLAYING AND INTERACT
When
##  RESUME PLAYING
When
##  STOP PLAYING
When
##  WHY NOT A LIVE MODE
If you want to replay the events in a real-time way, you can use the live mode API. This API is also useful for some real-time collaboration usage and enable a number of terminal to receive event send by a recording terminal instantly. This approach causes a lot of changes in the way the recorder works as well as the player. However we just need to answer to the following question :
+   Where should the recorder send the recorded events and the sound stream ?
+   How does it send this ?
+   Where should the player listen to receive the events and the sound ?
+   How will it synchronize the sound and the event being played ?

These are the only issues we need to fix as these are the only steps that will change in the recorder and player process.

Obviously we need both in the recorder and in the player an attribute to know which mode it is in. However in the player the attribute will be associate with a live state in the player state machine. We also need a liveMode server and client respectively for the recorder to emit event and a client to receive it. 