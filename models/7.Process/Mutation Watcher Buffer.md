#   MUTATION WATCHER & BUFFER

##   UTILIY & DESCRIPTION
Since we don't execute any JavaScript during replay, we instead need to record all changes scripts make to the document. This changes are :
+   Node creation, deletion
+   Node attribute changes
+   Text changes
But to record the DOM changes we can not use a classic watcher. Usually watcher works like this :
+   They have a watch method where they add an event listener to the specific event they watch.
+   When this event occurs they catch it and send it to the main recorder class which can store it.
But there is no event listener for DOM changes. Fortunately, modern browsers have provided us with a very powerful API which can do this: **[MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)**. The MutationObserver interface provides the ability to watch for changes being made to the DOM tree. It is designed as a replacement for the older Mutation Events feature, which was part of the DOM3 Events specification. However it is not the only class we need to record DOM changes, recording DOM Changes requires to use a Mutation Buffer to handle and store more efficiently mutation that occur. Indeed mutation observer uses a **Bulk Asynchronous** callback. Specifically, there will be a single callback after a series of DOM changes occur, and it is passed an array of multiple mutation records. This mechanism is not problematic for normal use, but create a number of **indesirable behavior** in our case :
+   Duplicate an added nodes
    +   The browser MutationObserver emits multiple mutations after a delay for performance reasons, making tracing added nodes hard in our `processMutationss` callback function.
    For example, if we append an node `n1` into body, and then append another node `n2` into `n1`, these 2 mutations may be passed to the callback function together when the 2 operations were done. In this case, although `n1` has no child node when it is added, due to the above-mentioned batch asynchronous callback mechanism, when we receive the mutation record and process the `n1` node the it already has the child node `n2` in the DOM. Generally we need to trace child nodes of newly added nodes, but in this case if we count `n2` as `n1`'s child node in the first mutation record, then we will count `n2` again in the second mutation record which was duplicated.
    Therefore, to avoid of duplicate counting added nodes when dealing with multiple mutation records in a callback, we need to "lazily" process the newly-added nodes, that is, first use a Set to store added nodes and its child nodes during iterate mutation records and then after browsing into all mutation records, we determine the order in which the nodes were added to the DOM.
+   Forget children of added node
    +   In another hand, when we create nodes `n1` and `n2`, then append `n2` as child to of `n1`, then append `n1` as child of body only one mutation record will be generated, that is, node `n1` (course including children) is added. In this case, when recording new nodes we must browse all its descendants to ensure that all new nodes are recorded.
+   Add an deleted nodes
    +   When processing mutation records, we may encounter a removed node that has not yet been serialized. That indicates that it is a newly added node, and the "add node" mutation record is also somewhere in the mutation records we received. We label these nodes as "aborted nodes". In this case since the node was removed already, there is no need to record it, and thus we remove it from the newly added node queue, this also applies to their descendants by checking if it has a aborted node as an ancestor.
+   Delete a moved nodes
    +   Consider a `n1` which is moved from a node `n2` to a node `n3`, mutation observer will trigger 2 mutations (delete from `n2` then create in `n3`), we must optimized this by differentiating the moved node to a delete.
+   Trigger mutations while making a full nodes capture
    +   The fact that a new DOM mutation was trigger asynchronously while a full nodes capture is done leads to a situation where, in replay a mutation is executed on a DOM tree that already has the mutation applied. This issue could be fixed by freezing and buffering any mutations until the full nodes capture is completed and then emitting them after unpause.
+   Add attribute to a fresh created node
    +   Imagine we create a node and right after add a attribute to it. Of course 2 mutations are trigger. For the sake of optimisation, we must remove all attribute modifications from a mutation event that occur in a nodes present in the added node queue.

##   MAIN FUNCTION PROCESS
###  MUTATION WATCHER
The watch method is the main function of the class. Its process is the following :
+   Init the mutation buffer with the capture method which will be used as the emission callback to save mutations that occur as an event
+   Create an MutationObserver and use the processMutations method of the mutation buffer as the mutation callback function.Thus, each time the DOM is modified, it will be called.
+   Start observing the document DOM with all options parameter to true :
    +   subtree : Set to true to extend monitoring to the entire subtree of the document node.
    +   childList : Set to true to monitor the document node (and its descendants) for the addition of new child nodes or removal of existing child nodes.
    +   attributes : Set to true to watch for changes to the value of attributes on the document node (and its descendants).
    +   attributeOldValue : Set to true to record the previous value of any attribute.
    +   characterData : Set to true to monitor the document node (and its descendants) for changes to the character data contained within the node or nodes.
    +   characterDataOldValue : Set to true to record the previous value of a node's text whenever the text changes on document node (and its descendants).

###  MUTATION BUFFER
We notice earlier that the mutation watcher create the mutation obsever by using the processMutations method of this class as callback function. That's why we consider the processMutations as the main method of this class. The method will receive an array of MutationRecord.
```typescript
type mutationRecord = {
    type: string
    target: Node
    oldValue: string | null
    addedNodes: NodeList
    removedNodes: NodeList
    attributeName: string | null
}
```
However It simply do 2 actions but they are very complex.
First, for each MutationRecord it call bufferizeMutation method which is used for optimisizing the bufferisation of those mutations and will perform the following work:
+   Check the type of the MutationRecord
    +   If it's `characterData` then retrieve
        +   target node
        +   textContent of this node
    +   If it's `attributes` then retrieve
        +   If for the target node some attributes mutations are already record in the buffer and are not emit yet
            +   Overwrite old buffered attribute and apply the new one
        +   Otherwise that is to say for the attributes which are newly triggered we retrieve
            +   Target node
            +   Attributes
    +   If it's `childList` that's mean a nod creation or deletrion is occurs then
        +   In Case of a node creation
            +   Reject if it is a blocked node
            +   Push it to movedNodeSet if the node is already formated
            +   Push it from addedNodeSet if the node is newly add
            +   And do the same for each childNodes
        +   Otherwise for a node deletion
            +   Reject if it is a blocked node
            +   Reject if it is an ancestor of the node is removed
            +   Remove it from the addedNodeSet if the removed node has not been serialized yet
            +   Remove it from the movedNodeSet if it was in meaning that in the MutationRecord the node is added to right after being removed
            +   Push it from addedNodeSet

After doing this we can emit a very optimise mutation event without all this indesirable behaviour. This MutationEvent is emit only if the MutationBuffer are not freeze, the emission is done like this :
+   This method use an array of addedNodeMutation which are a data structure with all properties to add correctly a node and so sometimes child node may be pushed before its newly added parent, so we init a queue to reorder these nodes.
    +   ```typescript
            type addedNodeMutation = {
                parentId: number
                previousId?: number | null
                nextId: number | null
                node: NodeCaptured
            }
        ```
    +   In pair with this addedNodeMutation we have a pushAdd function used for filling it with
    +   First of all, we remove all the nodes in the removedNodeSet on the map of the NodeFormatedHandler
    +   Next we add all nodes from movedNodeSet after checking 
        +   if their parents are not already removed
        +   and if their parents are not in the movedNodeSet
    +   Next we add all nodes from addedNodeSet after checking 
        +   if one of their ancestor are not removed
        +   or if one of their ancestor are moved
        +   else push the node in the droppedNodeSet
    +   Create an optimise event from
        +   addedNodeMutation array
        +   removedNodeMutation array
        +   attributeNewValue array
        +   textNodeNewValue array
    +   Reset the buffer
    +   Emit the event
To sum up the emit method take a look of the state of buffer and reorganise it for creating an optimise event with
+   any duplication of added nodes
+   a consideration of their children
+   any temporarily added nodes (nodes which added then deleted right after)
+   a differentiation of moved nodes and removed node
+   any attributes mutations for fresh created node

##   DATA STRUCTURE DESCRIPTION
The buffering of mutation are done thanks to a large kind of data structure and also produces number which are :
+   DoubleLinkedList : a typical double linked list class with methods :
    +   add a node
    +   remove a node
    +   retrieve a node with its position
```typescript
class DoubleLinkedList { ... }
```

+   DoubleLinkedListNode : 
```typescript
type DoubleLinkedListNode = {
    previous: DoubleLinkedListNode | null;
    next: DoubleLinkedListNode | null;
    value: NodeInLinkedList;
}
```

+   NodeInLinkedList :
```typescript
type NodeInLinkedList = Node & {
    _ln: DoubleLinkedListNode;
}
```

+   textNodeNewValue :
```typescript
type textNodeNewValue = {
    node: Node
    value: string | null
}
```

+   textMutation : 
```typescript
type textMutation = {
    id: number
    value: string | null
}
```

+   attributeNewValue : 
```typescript
type attributeNewValue = {
    node: Node
    attributes: { [key: string]: string | null }
}
```

+   attributeMutation : 
```typescript
type attributeMutation = {
    id: number
    attributes: { [key: string]: string | null }
}
```

+   removedNodeMutation : 
```typescript
export type removedNodeMutation = {
    parentId: number
    id: number
}
```

+   addedNodeMutation : 
```typescript
export type addedNodeMutation = {
    parentId: number
    previousId?: number | null
    nextId: number | null
    node: NodeCaptured
}
```