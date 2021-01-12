#   START RECORDING
##  INITIALIZE THE MUTATION BUFFER
The mutation buffer is needed to implement more efficiently mutations that occur. Indeed it resolve a lot of mutation problems like :
+   Duplication de nœuds ajoutés
+   Considering moved nodes as deleted
+   ...

##  TAKE THE FIRST FULL NODE CAPTURE
This is the base full node capture of the page. It enables to have all node present in the page as formated node at the start. First at all we capture meta informations of the page (location, width, height) and record in the events time line array. Take care that no DOM modification occur during full node capture. To do that we freeze the mutation buffer before calling the capture method of the node captor. After that the full capture is saved as an event in array and unfreeze the mutation buffer.

##  INITIALIZE ALL WATCHER AND START WATCHING
Create instance of all needed watchers class then call the watch method for each of those watchers. 

```mermaid

```

#   STOP RECORDING