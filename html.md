# HTML

#### Height

In order for a percentage value to work for height, the parent's height must be determined. The only exception is the root element &lt;html&gt;, which can be a percentage height \([https://stackoverflow.com/questions/7049875/why-doesnt-height-100-work-to-expand-divs-to-the-screen-height](https://stackoverflow.com/questions/7049875/why-doesnt-height-100-work-to-expand-divs-to-the-screen-height)\)

```text
html {
    height: 100%;
}
```

```text
* { padding: 0; margin: 0; }
html, body, #fullheight {
    min-height: 100% !important;
    height: 100%;
}
#fullheight {
    width: 250px;
    background: blue;
}
```

```text
<div id=fullheight>
  Lorem Ipsum        
</div>
```

#### Notable HTML Codes

|  |  |
| :--- | :--- |
| € | &euro; |
| £ | &pound; |



