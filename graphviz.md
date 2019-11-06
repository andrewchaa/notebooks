# GraphViz

#### Large Graphs

Lay the graph out left to right

```
graph {
    rankdir=LR; // Left to Right, instead of Top to Bottom
    a -- { b c d };
    b -- { c e };
    c -- { e f };
    d -- { f g };
    e -- h;
    t -- z;
}
```

#### Rendering image

```
d3.graphviz("#graph")
    .addImage("images/first.png", "400px", "300px")
    .addImage("images/second.png", "400px", "300px")
    .renderDot('digraph { a[image="images/first.png"]; b[image="images/second.png"]; a -> b }');
```

