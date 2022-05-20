#   REBUILDING NODE TREE

##   UTILIY & DESCRIPTION
In in this [doc](../Recorder/Capturing%20Node%20Process.md) we describe the process of how the capturing of nodes in the DOM is done. In another hand, during the replay session, we need to reconstruct the recorded DOM. That's the goal of this class : rebuild all a the DOM from a NodeCaptured tree in the player. Its mission is to ensure :
+   nodes are created whether they are ElementNodes or TextNodes
+   nodes retrieve all their stored attributes
+   nodes append in the good parent
+   eventual iframe DOM are recreate in the corresponding iframeElement
##   MAIN FUNCTION PROCESS
The main function of the class is implemented in its build method. The process of this method is the following :
+   For each node
    +   Check if the node is ELEMENT_NODE
        +   Create a corresponding element
        +   Retrieve and apply all recorded attributes
    +   Otherwise if the node TEXT_NODE
        +   Change the `:hover` pseudo-selector to a `.:hover` class if it's a CSS Rules
        +   Create a text node element
    +   Append node in its parent
    +   Build their childNodes
    +   Build the iframe DOM if the originId correspond to an iframe
##   DATA STRUCTURE DESCRIPTION
This class simply import and use the type create with the NodeEncoder. Click [here](../Recorder/Capturing%20Node%20Process.md) to read more about this data structure.
