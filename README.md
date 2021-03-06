# theDOMinator

The DOMinator is an open source, lean DOM manipulation library inspired by jQuery. This library enables users to

  * Select single or multiple DOM elements
  * Manipulate and Traverse DOM element's ```innerHTML```, class, etc.
  * Perform AJAX requests


## Getting Started

To use theDOMinator library, simply clone the github repo into your existing project and include ```theDOMinator.js``` file in the head of your Index or Root html file as so:

```html
  <script type="text/javascript" src="lib/theDOMinator.js"></script>
```
Remember to properly navigate from your HTML file to the ```theDOMinator.js``` file for the script to be included.

## theDOMinator API

theDOMinator is built using a custom class called ```DOMNodeCollection```. All functions used within theDOMinator will return an instance of ```DOMNodeCollection```.

### `$l(selector)`

The core function, ```$l(selector)```, receives one argument and returns a array like object otherwise known as a ```NodeList```. Depending on wether your ```(selector)``` argument is a string, HTML element, Array, or function, the ```DOMNodeCollection``` collection will return the correct ```NodeList``` based on the argument passed in as the ```(selector)```.

``` js
  $l = function(argument){
    //if HTML element
    if (argument instanceof HTMLElement) {
      return new DOMNodeCollection([argument]);
    // if String
    }else if (typeof argument === 'string'){
      let nodeList = document.querySelectorAll(argument);
      return new DOMNodeCollection(Array.prototype.slice.call(nodeList));
    // if Array of HTMLElements
    } else if (Array.isArray(argument)){
      return new DOMNodeCollection(argument);
    //if Function
    } else if (typeof argument === 'function'){

      if (document.readyState === 'complete') {
        return func();
      } else {
        funcQueue.push(argument);
      }
    } else {
      console.log("error");
    }
  };
  ```
### `DOMNodeCollection.prototype methods`

  theDOMinator has a series of prototype functions which can be called upon the selection of a ```DOMNodeCollection``` using the ```$l``` function.

#### `html(argument)`

  Using the selector and calling the ```html``` method without an argument will return the ```innerHTML ```of the selected DOM. If an argument is provided, the ```innerHTML``` of the selected DOM will be replaced with the provided argument.

#### `empty()`

  This function clears out the ```innerHTML``` of the selected DOM.

#### `append(argument)`

  Append accepts an HTML element, theDOMinator wrapped collection, or string. The ```outerHTML``` of each element provided in the argument is then appended to the ```innerHTML``` of each node in the ```DOMNodeCollection```.

#### `attribute(attributeName, value)`

  For the attribute function you must pass in two arguments. ```attributeName``` to select the attribute of the ```DOMNodeCollection```. Passed in as an argument alone, this will return the ```value``` corresponding to the ```attributeName``` of the first node of the ```DOMNodeCollection```. If a ```value``` is also passed in, The ```attributeName``` is then set equal to the passed in ```value``` for all nodes in the ```DOMNodeCollection```.

#### `addClass(className)`

  Takes a ```string``` argument as ```className``` and sets the ```class``` attribute of each node in the ```DOMNodeCollection``` to the ```className``` provided.

#### `removeClass(className)`

  Takes a ```string``` argument as ```className``` and
  removes the current ```class``` attribute of each node in the ```DOMNodeCollection```.

#### `toggleClass(className)`

  Takes a ```string``` argument as ```className``` and resets the ```class``` attribute of each node in the ```DOMNodeCollection``` to the ```className``` provided.


#### `parent()`

  Returns an array of all parents of each of the nodes in the ```DOMNodeCollection```.

#### `find(selector)`

  Returns a ```DOMNodeCollection``` of all the nodes matching the selector passed in as an argument that are descendants of the nodes.

#### `remove()`

  Removes the ```innerHTML``` of all nodes in the selected ```DOMNodeCollection```.

#### `on(eve, callback)`

  Adds an Event Listener to each of the nodes in the ```DOMNodeCollection```. Pass in your desired event as ```eve``` and desired callback function as ```callback```.

#### `off(eve)`
  Removes Event Listener passed in as ```eve``` from each of the nodes in the ```DOMNodeCollection```.

### AJAX requests

  You can make AJAX requests using theDOMinator's ```$l.ajax(options)``` function. You must pass in an object in place of the ```options``` argument specifying the option values you wish to specify. You can see the default ```options``` object below.


| Keys        | Defaults           |
| ------------- |:-------------:|
| contentType    | 'application/x-www-form-urlencoded; charset=UTF-8' |
| method     | "GET"     |   
|  url | ""    |
|  dataType | "JSON"     |
| success -- Callback function upon success | (s) => console.log("No Success Callback")      |
| error -- Callback function upon error| (e) => console.log("No Error Callback")      |

an example ```$l.ajax``` function call to change the background image of the body within an HTML file can be seen below.


  ```js
  $l.ajax({
       method: 'GET',
       url: 'http://www.exampleurl.com/imagetoDisplay',
       success: (data) => {
         let body = $l("body").elements[0]
         body.style.backgroundImage = ``
         body.style.background = `url(${JSON.parse(data).url})`
         body.style.backgroundSize = 'cover'
       }
     });
```

## Demo

  You can view a live demo of theDOMinator in action [here](http://www.chrishakos.com/theDOMinator).
