##  DATA STRUCTURE DESCRIPTION
```typescript
enum NodeType {
    Element,
    Text
}

type ElementNode = {
    type: NodeType.Element
    elementName: string
    attributes: attributes
    childNodes: NodeCaptured[]
}

type TextNode = {
    type: NodeType.Text
    textContent: string
    isCSSRules: boolean
}

type NodeCaptured = {
    nodeId: number,
    originId: number &
    (ElementNode || TextNode)
}

interface NodeFormated extends Node {
    _cnode: NodeCaptured
}
```
##  EXAMPLE
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