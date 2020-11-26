#   Data Formated
```typescript
NodeType {
    Element,
    Text,
}

Node = {
    id: number,
    ElementNode | textNode
}

ElementNode {
    type: NodeType.Element
    ElementName: string
    attributes: attributes
    childNodes: NodeShot[]
}

textNode = {
    type: NodeType.Text
    textContent: string
    isStyle?: true
}
```

#   Class Required
##  Main Class : Node Captor
### Description
Used for retrieve, reformat and save HTML Node in JSON format.
### Methods
captureNode : Parse the node and call the getAttribute method corresponding to this kind of node.
formatNode  : Capture the node and Attribute ids (node id and root id).
getGlobalAttribute : return the HTMLElement.attributes, scolling value, size (width - height).
getExternalCSSAttribute : return the css rules and the absolute css path.
getInternalCSSAttribute : return the css rules and the absolute css path.
getFormFieldsAttribute : return the input value for input, textarea and select, the checked value for checkbox and radio, the selected value for option.
getCanvasAttribute : return the dataUrl.
getMediaAttribute : return the mediaState.

#   Storing Example
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