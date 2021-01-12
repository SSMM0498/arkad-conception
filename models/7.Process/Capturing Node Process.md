+   For each node
    +   Capture and format the node
        +   Retrieve the origin id (check if this node is contain in the root document or in a iframe)
        +   Retrieve the following value from an ELEMENT_NODE
            +   Tag name
            +   Globals attributes
                +   HTMLElement.attributes
                +   Scrolling value (scroll left, scroll top)
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