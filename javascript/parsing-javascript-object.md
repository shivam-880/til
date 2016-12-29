# Parsing JavaScript Object

We can parse nested javascript object like so:

``` javascript
// Parser.js

var obj = [{
    id: "A",
    children: [{
        id: "B",
        children: [{
            id: "C",
            children: [{
                id: "D",
                children: [{
                    id: "E",
                    children: [{
                        id: "F"
          }]
        }]
      }, {
                id: "G",
                children: {
                    id: "H"
                }
      }]
    }, {
            id: "I"
    }]
  }, {
        id: "J",
        children: [{
            id: "K"
    }]
  }]
}, {
    id: "L"
}, {
    id: "M",
    children: {
        id: "N",
        children: [{
            id: "O"
    }]
    }
}, {
    id: "P"
}];

function parse(obj) {
    for (var k in obj) {
        if (typeof obj[k] == "object" && obj[k] !== null)
            parse(obj[k]);
        else
            console.log(obj[k])
    }
}

parse(obj)
```

This would print all the **id**'s on the console:
``` shell
$ node Parser.js
A
B
C
D
E
F
G
H
I
J
K
L
M
N
O
P
```
