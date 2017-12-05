# Slice objects from a list of objects to an object of objects with keys as indices

## Input

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

## Output

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

## Solution

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
