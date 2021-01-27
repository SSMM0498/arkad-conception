#   RECORDING EVENT PROCESS
##   INITIALISATION OF THE CLASS
###  INITIALIZE THE NODE CAPTOR
Create an instance of the node captor. This is class used for making instant capture of the whole DOM object in memory. The functionnement of the node captor class is detailed and explain [here](./Capturing%20Node%20Process.md)
###  INITIALIZE THE MUTATION BUFFER
Create an instance of the mutation buffer. The mutation buffer is needed to record more efficiently mutations that occur. The functionnement of the mutation buffer class is detailed and explain [here](../8.Workflow/Mutation%20Workflow.md)
###  INITIALIZE ALL WATCHER
Create an instance for each watchers classes. There are a number of watchers classes which are :
+   InputWatcher
+   MouseInteractionWatcher
+   MouseMovementWatcher
+   MutationWatcher
+   ScrollWatcher
+   TextSelectionWatcher
Click [here](WatchersProcess.md) to learn more about watchers.
###  INITIALIZE THE MICRO LISTENER
Create an instance of the microphone listener. It allow us to record the voice of the user.

---
##   START RECORDING
###  TAKE THE FIRST FULL NODE CAPTURE
This is the base full nodes capture of the page. It enables to have all node present in the page as formated node at the start. First at all we capture meta informations of the page (location, width, height) and record in the events time line array. Take care that no DOM modification occur during full node capture. To do that we freeze the mutation buffer before calling the capture method of the node captor. After that the full capture is saved as an event in array and unfreeze the mutation buffer.
###  START WATCHING
Call the watch method for each watchers classes. Click [here](WatchersProcess.md) to learn more about watchers classes.
###  START LISTENING
Call the listen method of the microphone listener class.
##   STOP RECORDING

##   SAVE TO FILE