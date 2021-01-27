#   CAPTURING NODE
##   UTILIY & DESCRIPTION
 This is class used for making instant capture of the whole DOM object in memory. If you only need to record and replay changes within the browser locally, then we can simply save the current view by deep copying the DOM object. But to do this properly, we need to implement a class which will capture and serialize the DOM in a specific format. This class follows a number of rules that solve some implementations issues. In the following list, all important handlings done by the node captor are detailed:
+   Scripts in the recorded source page should not be executed during the replay (simply the `script` tag is replace by a placeholder `noscript` tag in the capture). It'snt secure to retrieve the script content.A malicious script could run when replaying the arkast. Instead any changes to the DOM that scripts cause is observe using mutation observer and record as an event
+   There are some DOM changes that is not reflected in the HTML. For example, the value of `<input type="text" />` will not be reflected in its HTML, but we need to record the `value` attribute as a property when capturing this input element.
+   Relative paths need to be converted to absolute paths. During replay, we will place the recorded page in an `<iframe>`. The page URL at this time is the address of the replay page. If there are some relative paths in the recorded page, an error will occur when the user tries to open them, so when recording we need to convert relative paths. Relative paths in the CSS style sheet also need to be converted.
+   If the recorded page links to external style sheets which can be located on an intranet or localhost, we have to get its parsed CSS rules from the browser, generate an inline style sheet containing all these rules. This way all stylesheets are accessible and can be replayed correctly.
+   The complete DOM is reconstructed when replaying, that's why there is no way to associate the interacting DOM nodes in the recorded event with the existing DOM. For example if we recorded the click of a button on the same page and played it back, we must show in the recorded event json which is the specific button clicked. A unique identifier `nodeId` is add to each captured Node, which is used for being mention in triggered events associated to this node.
+   Iframe element is very special element used to embed another document within the current HTML document.This particularity involve that node captor add to the captured node an `originId` when its target document is not the top scope document object and collect then capture all node in the iframe. That allow us to rebuild properly and exactly all the DOM showed in the page while recording.
##   MAIN FUNCTION PROCESS
The main function of the class is implemented in its capture method. The process of this method is the following :
+   For each node
    +   Capture and format the node
        +   Retrieve the origin id (check if this node is contain in the root document or in a iframe)
        +   Retrieve the following value from an ELEMENT_NODE
            +   Tag name
            +   Globals attributes
                +   HTMLElement.attributes
                +   Scrolling position (scroll left, scroll top)
                +   Size (width as _width, height as _height)
            +   Css rules
                +   external css (from a link)
                +   internal css (from a style)
            +   Form fields attributes
                +   value
                +   checked
                +   selected
            +   Canvas image data
                +   toDataURL
            +   Media elements
                +   HTMLMediaElement.paused as _mediaState
        +   Retrieve the following value from a TEXT_NODE
            +   Parent node tag name
            +   Text.textContent
            +   is the text a CSS rule (isCSSRule)
        +   Generate the node id
    +   Capture and format the child node
    +   Capture the document iframe

##   DATA STRUCTURE DESCRIPTION
Capturing the DOM produces a specific type of data structures which are detailed in the following
+   NodType : represents the type of the captured node and lets us know if it's a HTML Element or Text Node.
```typescript
enum NodeType {
    Element,
    Text
}
```
+   ElementNode : represents the format in which HTML elements are stored. It contain properties which are :
    +   type : specifies that it is a NodeType.Element
    +   elementName : represents the element tag name
    +   attributes : contains all attribute retrieved during the capture
    +   childNodes : is a array which contain all the children of this element
```typescript
type ElementNode = {
    type: NodeType.Element
    elementName: string
    attributes: attributes
    childNodes: NodeCaptured[]
}
```
+   TextNode : represents the text node or css rules capturing format. It contain properties which are :
    +   type : specifies that it is a NodeType.Text.
    +   textContent : represents the whole text in the node as an unique string.
    +   isCssRules :is a boolean that indicates whether this textNode is a set of css rule or not.
```typescript
type TextNode = {
    type: NodeType.Text
    textContent: string
    isCSSRules: boolean
}
```
+   NodeCaptured : represents the captured node format. It simply add the `originId` and the `nodeId` to an ElementNode or TextNode.
```typescript
type NodeCaptured = {
    nodeId: number,
    originId: number &
    (ElementNode || TextNode)
}
```
+   NodeFormated : represents a merge between the NodeCaptured and the node DOM. The NodeCaptured is stored in a `_cnode` (captured node) properties.
```typescript
interface NodeFormated extends Node {
    _cnode: NodeCaptured
}
```
Here is an example of capture execution return :
```json
DocumentNodes =  {
    {
        "id": 0,
        "type": "Element",
        "ElementName": "h1",
        "attributes": [
            "class": "title bold gray",
            "id": "firsttitle"
        ],
        "childNodes": [
            {
                "type": "Text",
                "textContent": "My First Heading",
                "isStyle": false
            }
        ]
    },
    {
        "id": 1,
        "type": "Element",
        "ElementName": "ul",
        "attributes": [
            "class": "list-wrapper toggleon",
        ],
        "childNodes": [
            {
                "id": 2,
                "type": "Element",
                "ElementName": "li",
                "attributes": [
                    "class": "list-item active",
                ],
                "childNodes": [
                    {
                        "type": "Text",
                        "textContent": "The 1rst Item",
                        "isStyle": false
                    }
                ]
            },
            {
                "id": 3,
                "type": "Element",
                "ElementName": "li",
                "attributes": [
                    "class": "list-item",
                ],
                "childNodes": [
                    {
                        "type": "Text",
                        "textContent": "The 2nd Item",
                        "isStyle": false
                    }
                ]
            },
            {
                "id": 4,
                "type": "Element",
                "ElementName": "li",
                "attributes": [
                    "class": "list-item",
                ],
                "childNodes": [
                    {
                        "type": "Text",
                        "textContent": "The 3rd Item",
                        "isStyle": false
                    }
                ]
            },
            {
                "id": 5,
                "type": "Element",
                "ElementName": "li",
                "attributes": [
                    "class": "list-item",
                ],
                "childNodes": [
                    {
                        "type": "Text",
                        "textContent": "The 4rth Item",
                        "isStyle": false
                    }
                ]
            },
        ]
    }
}
```