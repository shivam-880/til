# Convert a list of objects to an object of objects with indices of objects in the lists as keys

## Input

```
var arr = 
[
  {
    "name": "styles-conference",
    "url": "https://github.com/codingkapoor/styles-conference",
    "desc": "Bootstrapped version of 'StylesConference' website created by Shay Howe @learn.shayhowe.com.",
    "stack": ["HTML5", "CSS3", "BootStrap"]
  },
  {
    "name": "akka-scala",
    "url": "https://github.com/codingkapoor/akka-scala",
    "desc": "This repository includes projects that attempt to explore Akka toolkit in Scala.",
    "stack": ["Scala", "Akka"]
  }
]
```

## Output

```
{ 1: 
   { name: 'styles-conference',
     url: 'https://github.com/codingkapoor/styles-conference',
     desc: 'Bootstrapped version of \'StylesConference\' website created by Shay Howe @learn.shayhowe.com.',
     stack: [ 'HTML5', 'CSS3', 'BootStrap' ] 
   },
  2: 
   { name: 'akka-scala',
     url: 'https://github.com/codingkapoor/akka-scala',
     desc: 'This repository includes projects that attempt to explore Akka toolkit in Scala.',
     stack: [ 'Scala', 'Akka' ] 
   } 
 }
```

## Solution

```
var res = arr.reduce((acc, item, index) => { acc[index + 1] = item; return acc; }, {});
res
```
