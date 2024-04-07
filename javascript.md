## Animated Logo in Reactjs
```javascript
import { useState } from "react"
import { Image, Transition } from "semantic-ui-react"
import { useInterval } from '../common/hooks' // Refer https://github.com/iamsmkr/til/blob/master/javascript.md#custom-react-hook-to-implement-intervals

const transitions = [
    'jiggle',
    'flash',
    'shake',
    'pulse',
    'tada',
    'bounce'
]

const AnimatedLogo = () => {
    const [anime, setAnime] = useState({
        animation: 'jiggle', duration: 1000, visible: true
    })

    const { animation, duration, visible } = anime

    useInterval(() => {
        setAnime({
            ...anime, 
            animation: transitions[Math.floor(Math.random() * transitions.length)], 
            visible: !anime.visible
        })
    }, 5000)

    return (
        <Transition
            animation={animation}
            duration={duration}
            visible={visible}
        >
            <Image
                src='https://icon-library.com/images/8-bit-icon/8-bit-icon-15.jpg'
                style={{ width: 40, height: 40, float: 'right', marginRight: 15 }}
            />
        </Transition>
    )
}

export default AnimatedLogo
```

<br/>

## Check if Javascript object is empty
```javascript
Object.keys({}).length === 0
```

<br/>

## Convert an array of objects to object of objects with indices as one of the key value
#### Input
```javascript
const arr = [
  {
      scope: "Row",
      index: 1
  },
  {
      scope: "File",
      index: 2
  },
  {
      scope: "Bundle",
      index: 3
  }
]
```

#### Output
```javascript
{
  1: {
      scope: "Row",
      index: 1
  },
  2: {
      scope: "File",
      index: 2
  },
  3: {
      scope: "Bundle",
      index: 3
  }
}
```

#### Solution
```javascript
import _ from 'lodash'
const obj = _.keyBy(arr, "index")
```

<br/>

## Convert a list of objects to an object of objects with indices of objects in the lists as keys

#### Input

``` javascript
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

#### Output

``` javascript
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

#### Solution

``` javascript
var res = arr.reduce((acc, item, index) => { acc[index + 1] = item; return acc; }, {});
res
```

<br/>

## Custom React Hook to implement intervals
`setInterval` doesn't work with react hooks as expected, hence this custom hook. It's a copy paste the following blog post.
Refer https://overreacted.io/making-setinterval-declarative-with-react-hooks/
```javascript
import { useEffect, useRef } from 'react'

const useInterval = (callback, delay) => {
    const savedCallback = useRef()

    // Remember the latest callback.
    useEffect(() => {
        savedCallback.current = callback
    }, [callback])

    // Set up the interval.
    useEffect(() => {
        function tick() {
            savedCallback.current()
        }
        if (delay !== null) {
            let id = setInterval(tick, delay)
            return () => clearInterval(id)
        }
    }, [delay])
}

export default useInterval

```

#### Usage
```javascript
import React, { useState, useEffect, useRef } from 'react';

function Counter() {
  let [count, setCount] = useState(0);

  useInterval(() => {
    // Your custom logic here
    setCount(count + 1);
  }, 1000);

  return <h1>{count}</h1>;
}
```

<br/>

## Enums in Javascript
```javascript
export const Scope = Object.freeze({ Row: "Row", File: "File", Bundle: "Bundle" });
```

<br/>


## How to properly terminate timeouts started in a loop
Calling `setTimeout` inside a loop will create multiple timers. The idea is to maintain a list of all such timers and clear them iteratively.

```javascript
let timeouts = []

timeouts.forEach((timer) => {
    clearTimeout(timer)
})

for (let x = 0; x < 10; x++) {
    let tick = () => {
        return () => {
            console.log("Hi")
        }
    }
    timeouts.push(setTimeout(tick(), 1000 * x))
}

```

<br/>

## Parsing JavaScript Object

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

<br/>

## Pausing setInterval when page/ browser is out of focus
The `requestAnimationFrame` solutions proposed in the blogs listed below won't work as per the documentation:
  > requestAnimationFrame() calls are paused in most browsers when running in background tabs or hidden <iframe>s, in order to improve performance and battery life.

**Refer**: 

(1) https://abhi9bakshi.medium.com/why-javascript-timer-is-unreliable-and-how-can-you-fix-it-9ff5e6d34ee0

(2) https://freedium.cfd/https://medium.com/jspoint/javascript-raf-requestanimationframe-456f8a0d04b0

<br/>

We can make use of <ins>web workers</ins> instead. 

**Refer**: 

(1) https://github.com/iamsmkr/seesaw 

(2) https://stackoverflow.com/questions/5927284/how-can-i-make-setinterval-also-work-when-a-tab-is-inactive-in-chrome/70870907#70870907




<br/>

## Sleep in Javascript
```javascript
await new Promise(r => setTimeout(r, 10000))
```

<br/>

## Slice objects from a list of objects to an object of objects with keys as indices

#### Input

``` javascript
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
  },
  {
    "name": "smf-dependency-graph",
    "url": "https://github.com/codingkapoor/smf-dependency-graph",
    "desc": "Create dependency graph of all the processes managed by SMF framework on a Solaris machine.",
    "stack": ["Java", "SigmaJS"]
  },
  {
    "name": "globalmart-java-spring-resteasy",
    "url": "https://github.com/codingkapoor/globalmart-java-spring-resteasy",
    "desc": "This repository demonstrates restful webservices in Java using Spring Core and Resteasy.",
    "stack": ["Java", "Spring", "Resteasy"]
  },
  {
    "name": "spring-boot-jscalendar",
    "url": "https://github.com/codingkapoor/spring-boot-jscalendar",
    "desc": "Full page calendar built using JavaScript and SpringBoot.",
    "stack": ["Java", "JavaScript", "JQuery", "Spring Boot"]
  },
  {
    "name": "play-slick-scala",
    "url": "https://github.com/codingkapoor/play-slick-scala",
    "desc": "This repository demonstrates restful webservices in Scala using Play framework and Slick.",
    "stack": ["Scala", "Play", "Slick"]
  },
  {
    "name": "wfh-calendar",
    "url": "https://github.com/codingkapoor/wfh-calendar",
    "desc": "A calendar web application.",
    "stack": ["HTML5", "SASS", "JQuery", "RequireJS", "Grunt"]
  },
  {
    "name": "youtube-search-app",
    "url": "https://github.com/codingkapoor/youtube-search-app",
    "desc": "Youtube search application in React.",
    "stack": ["HTML5", "CSS3", "ES6", "React"]
  },
  {
    "name": "tic-tac-toe-react-app",
    "url": "https://github.com/codingkapoor/tic-tac-toe-react-app",
    "desc": "Tic-Tac-Toe application from Reactjs tutorials.",
    "stack": ["HTML5", "CSS3", "ES6", "React"]
  },
  {
    "name": "books-library-react-app",
    "url": "https://github.com/codingkapoor/books-library-react-app",
    "desc": "Books library application in React.",
    "stack": ["HTML5", "CSS3", "ES6", "React", "Redux"]
  },
  {
    "name": "weather-forecast-react-app",
    "url": "https://github.com/codingkapoor/weather-forecast-react-app",
    "desc": "Weather forecast app in React.",
    "stack": ["HTML5", "CSS", "ES6", "React", "Redux"]
  },
  {
    "name": "blogging-react-app",
    "url": "https://github.com/codingkapoor/blogging-react-app",
    "desc": "Blogging application in React.",
    "stack": ["HTML5", "CSS", "ES6", "React", "Redux"]
  },
  {
    "name": "stupid-little-website",
    "url": "https://github.com/codingkapoor/stupid-little-website",
    "desc": "A small demo site for npm scripting adventures.",
    "stack": ["NPM"]
  },
  {
    "name": "technology-jukebox",
    "url": "https://github.com/codingkapoor/technology-jukebox",
    "desc": "A simple tool that lists and allows to search projects that I create as part of my experiments with various of languages, frameworks and libraries. ",
    "stack": ["HTML5", "CSS3", "ES6", "React"]
  }
]
```

#### Output

``` javascript
{ 2: 
   { name: 'akka-scala',
     url: 'https://github.com/codingkapoor/akka-scala',
     desc: 'This repository includes projects that attempt to explore Akka toolkit in Scala.',
     stack: [ 'Scala', 'Akka' ] 
   },
  3: 
   { name: 'smf-dependency-graph',
     url: 'https://github.com/codingkapoor/smf-dependency-graph',
     desc: 'Create dependency graph of all the processes managed by SMF framework on a Solaris machine.',
     stack: [ 'Java', 'SigmaJS' ] 
   },
  4: 
   { name: 'globalmart-java-spring-resteasy',
     url: 'https://github.com/codingkapoor/globalmart-java-spring-resteasy',
     desc: 'This repository demonstrates restful webservices in Java using Spring Core and Resteasy.',
     stack: [ 'Java', 'Spring', 'Resteasy' ] 
   },
  5: 
   { name: 'spring-boot-jscalendar',
     url: 'https://github.com/codingkapoor/spring-boot-jscalendar',
     desc: 'Full page calendar built using JavaScript and SpringBoot.',
     stack: [ 'Java', 'JavaScript', 'JQuery', 'Spring Boot' ] 
   },
  6: 
   { name: 'play-slick-scala',
     url: 'https://github.com/codingkapoor/play-slick-scala',
     desc: 'This repository demonstrates restful webservices in Scala using Play framework and Slick.',
     stack: [ 'Scala', 'Play', 'Slick' ] 
   },
  7: 
   { name: 'wfh-calendar',
     url: 'https://github.com/codingkapoor/wfh-calendar',
     desc: 'A calendar web application.',
     stack: [ 'HTML5', 'SASS', 'JQuery', 'RequireJS', 'Grunt' ] 
   } 
 }
```

#### Solution

``` javascript
function slice(offset, limit) {
  var newObj = {};
  for(var i = offset; i <= limit; i++){
    newObj[i] = arr[i];
  }
  return newObj;
}

slice(2,7);
```

<br/>

## Start local webserver to host dev apps
**Refer**: https://stackoverflow.com/questions/21408510/chrome-cant-load-web-worker
```javascript
$ npm install -g local-web-server
$ ws
```
Navigate to http://localhost:8000 (default port: 8000)

## Start React App in DEV/PROD mode
If you have created a react app from `create-react-app`:
```json
"scripts": {
    "dev": "react-scripts start",
    "start": "concurrently \"npm run server\" \"npm run dev\"",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "server": "pouchdb-server --host 0.0.0.0 -p 10102 -m -d /opt/pouchdb/data/visibility/ -n true",
    "eject": "react-scripts eject"
  },
```

Starting the server like `npm run start` will always start the server in the development mode. In fact, it will automatically set `NODE_ENV` as `development` which you can use in your code to do something like:
```javascript
 if (process.env.NODE_ENV === "development") {
        await import('rxdb/plugins/dev-mode').then(
            module => addRxPlugin(module.RxDBDevModePlugin)
        );
    }
```

When you deploy your app in the production using `serve` using following commands:
```bash
% npm install -g serve
% npm run build
% serve -s build
```

It will automatically set `NODE_ENV` as `production`.

You can also update your scripts to start the server in `dev` and `prod` mode like so:
```json
"scripts": {
    "dev": "react-scripts start",
    "start:dev": "concurrently \"npm run server\" \"npm run dev\"",
    "start": "concurrently \"npm run server\" \"serve -s build\"",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "server": "pouchdb-server --host 0.0.0.0 -p 10102 -m -d /opt/pouchdb/data/visibility/ -n true",
    "eject": "react-scripts eject"
  },
```

Now start the server in `dev` and `prod` mode respectively:
```bash
% npm run start:dev
% npm run start
```
