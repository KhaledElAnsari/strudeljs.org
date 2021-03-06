---
type: api
---

# Strudel API

## Decorators

### @Component

- **Arguments:**
  - `{String} [selector]`

- **Details:**

  Register new component that will be instantiated for all occurrences of selector
  
- **Usage:**
  ```js
  @Component('.foo')
  class Foo { }
  ```
  
### @Evt

- **Arguments:**
  - `{String} [event descriptor]`
  
- **Details:**  

   Add DOM event handler for specific descriptor and make decorated function a callback. Event descriptor need to follow format *eventName [selector]*
    
- **Usage:**
  ```js
  @Evt('click .bar')
  onClick() { }
  ```
### @El

- **Arguments:**
  - `{String} [element selector]`
  
- **Usage:**
  
  Find element by selector on component initialisation and substitute for variable.
  ```js
  @El('.bar')
  bar
  ```

## Instance Properties

<blockquote class="alert">Instance properties are injected into `Component` default constructor, so changing the `constructor` for class is forbidden.</blockquote>
  
### $element

- **Type:** `{Element}`

- **Details:**

  Gets the DOM element correlated with the component instance.
  
- **Usage:**
  ```js
  init() {
    this.$element.text('Hello world')
  }
  ```
  
### $data

- **Type:** `{object}`

- **Details:**
  
  Reference to the `data` attributes map for DOM element correlated with component instance.
  
- **Usage:**
  ```js
  init() {
    console.log(this.$data.attribute);
  }
  ```
 
## Instance Methods / Events

### $teardown
  
- **Details:**  

  Triggers destroy of the component - unbinds event listeners and binding to DOM
  
- **Usage:**
  ```js
  this.$teardown()
  ```
  
### $emit

- **Arguments:**
  - `{String} [event descriptor]`
  
- **Details:**  

  Emits application event with provided descriptor and invokes all the handlers registered under that descriptor  
  
- **Usage:**
  ```js
  this.$emit('eventName');
  ```

### $on

- **Arguments:**
  - `{String} [event descriptor]`
  - `{Function} [callback]`
  
- **Details:**
  
  Registers `callback` which will fire when event with matching `event descriptor` is emitted
  
- **Usage:**
  ```js
  this.$on('eventName', function (data) {
    console.log(data);
  });
  ```

### $off

- **Arguments:**
  - `{String} [event descriptor]`
  - `{Function} [callback]`
  
- **Details:**  

  Unregisters `callback` registered for provided `event descriptor`
  
- **Usage:**
  ```js
  this.$off('eventName', callback);
  ```
  
## Instance Methods / Lifecycle Hooks

<blockquote class="alert">All lifecycle hooks have their `this` context bound to the instance. All of them have  **Instance Properties** available.</blockquote>

### beforeInit

- **Type:** `{Function}`
  
- **Details:**
  
  Called synchronously after `DOMContentLoaded` event, before the `init` and binding of events and elements. This can be used to fetch the data or manipulate the dom.

### init

- **Type:** `{Function}`
  
- **Details:**
  
  Called synchronously after `DOMContentLoaded` event.

### beforeDestroy

- **Type:** `{Function}`
  
- **Details:**
  
  Called synchronously after `$teardown()` function is invoked on component instance before event listeners and DOM detach

### destroy

- **Type:** `{Function}`
  
- **Details:**
  
  Called synchronously after `$teardown()` function is invoked on component instance when everything is destroyed.
  
# DOM API

<blockquote class="alert">Note: Element is internal class for handling DOM manipulation that simulates jQuery but is 800% lighter, below is a reference of available methods. Other methods need to be accessed through DOM API. `this.$element` and all elements found using `@El` decorator are Element instances.</blockquote>

### Element

- **Arguments:**
  - `{String} [selector]`
  - `{Node} [context]`
  
- **Details:**

    Creates instance of `Element` class for the `selector`, can also have context being provided to find elements within Node.
  
- **Usage:**
  ```js
    Element('div');
    Element('.list-item', document.querySelector('ul'));
  ```

## Traversing

### find
- **Arguments:**
  - `{String} selector`
  
- **Details:**
  
  Finds element specified by selector in the element context
  
- **Usage:**
  ```js
  Element('body').find('.className');
  ```
  
### children
- **Arguments:**
  - `{String} selector`
  
- **Details:**
  
  Get the direct children of all of the nodes with an optional filter
  
- **Usage:**
  ```js
  Element('body').children();
  Element('body').children('<div>');
  ```
  
### parent

- **Arguments:**
  - `{String} selector`
  
- **Details:**
  
  Travel the matched elements one node up.
   
- **Usage:**
  ```js
  Element('a').parent();
  ```
  
### eq

- **Arguments:**
  - `{number} index`
  
- **Details:**
  
  Reduce the set of matched elements to the one at the specified index.
  
- **Usage:**
  ```js
  Element('.list-items').eq(3);
  ```
  
### first

- **Details:**
  
  Reduce the set of matched elements to the first in the set. **Returns native HTML Element.**
  
- **Usage:**
  ```js
  Element('body').find('.className').first();
  ```

### array

- **Arguments:**
  - `{Function} callback`

- **Details:**
  
  Extracts structured data from the DOM. Can have callback passed to return different format of data.
  
- **Usage:**
  ```js
  Element('ul li').array();
  // ['Item 1', 'Item 2', 'Item 3']
  ```
  
## Filtering

### filter

- **Arguments:**
  - `{string|Element|Function} filter`

- **Details:**
  
  Remove all the nodes that doesn't match the criteria.
  
- **Usage:**
  ```js
  Element('ul').filter('li')
  Element('nav').filter(Element('a'))
  Element('nav').filter((node) => Element(node).is('a'))
  ```

### map

- **Arguments:**
  - `{Function} callback`

- **Details:**
  
  Change the content of the current instance by looping each element
  
- **Usage:**
  ```js
  Element('ul li').map((node, i) => '<a>' + Element(node).text() + '</a>');
  ```

### each

- **Arguments:**
  - `{Function} callback`

- **Details:**
  
  Loop through all of the nodes and execute a callback for each
  
- **Usage:**
  ```js
  Element('ul li').each((node, i) => Element(node).attr('href', '#'));
  ```

### is

- **Arguments:**
  - `{string|Element|Function} filter`

- **Details:**
  
  Check whether any of the nodes matches the selector
  
- **Usage:**
  ```js
  Element('nav').is('.is-active');
  ```

## Manipulation

### append

- **Arguments:**
  - `{string|Element} html`

- **Details:**
  
  Insert content, specified by the parameter, to the end of each element in the set of matched elements. Additional data can be provided, which will be used for populating the html
  
- **Usage:**
  ```js
  Element("ul").append("<li>Item 1</li>");
  ```
  
### prepend

- **Arguments:**
    - `{string|Element} html`

- **Details:**

Insert content, specified by the parameter, to the beginning of each element in the set of matched elements. Additional data can be provided, which will be used for populating the html

- **Usage:**
```js
Element("ul").prepend("<li>Item 1</li>");
```

### remove

- **Details:**
  
  Remove the set of matched elements from the DOM.
  
- **Usage:**
  ```js
  Element("ul").remove();
  ```

### text

- **Arguments:**
  - `{string} text (optional)`

- **Details:**
  
   Gets the text contents of the first element in a set. When parameter is provided set the text contents of each element in the set.
  
- **Usage:**
  ```js
  Element("span").text();
  Element("span").text("Hello world!");
  ```

### html

- **Arguments:**
  - `{string} htmlString (optional)`

- **Details:**
  
  Gets the HTML contents of the first element in a set. When parameter is provided set the HTML contents of each element in the set.
  
- **Usage:**
  ```js
  Element("div").html();
  Element("div").html("<span>Hello world!</span>");
  ```

### attr

- **Arguments:**
  - `{string|object} attrName`
  - `{string} value`

- **Details:**
  
  Gets the value of an attribute for the each element in the set of matched elements or set one or more attributes for every matched element.
  
- **Usage:**
  ```js
  Element("a").attr("href");
  Element("a").attr("href", "#");
  Element("a").attr({"href": "#", "target": "_blank"});
  ```

### data

- **Arguments:**
  - `{string|object} attrName`
  - `{string} value`

- **Details:**
  
  Gets the value of an data attribute for the each element in the set of matched elements or set one or more attributes for every matched element.
  
- **Usage:**
  ```js
  Element("div").data().msg;
  Element("div").data("msg");
  Element("div").data("msg", "Hello world!");
  Element("div").data({"msg": "Hello world!", "value": "200"});
  ```

### addClass
- **Arguments:**

  - `{...String} class(es)
  `
- **Details:**
  
  Adds the specified class(es) to each element in the set of matched elements.
  
- **Usage:**
  ```js
  Element('.button').removeClass('.is-active');
  ```
### removeClass
- **Arguments:**

  - `{...String} class(es)`
- **Details:**
  
  Remove the specified class(es) to each element in the set of matched elements.
  
- **Usage:**
  ```js
  Element('.modal').removeClass('.is-visible');
  ```
### toggleClass
- **Arguments:**
  - `{...String} class(es)`
- **Details:**
  
  Toggles the specified class(es) to each element in the set of matched elements.
  
- **Usage:**
  ```js
  Element('.main-nav').toggleClass('.is-expanded');
  ```
## Events

### trigger

- **Arguments:**
  - `{String} events`
  
- **Details:**
  
  Execute all handlers attached to the event type.
  
- **Usage:**
  ```js
  Element('button').trigger('click');
  ```


### on

- **Arguments:**
  - `{String} event`
  - `{string|Function} selector or listener`
  - `{Function} listener (only when selector provided)` 
  
- **Details:**
  
  Attach event handlers to element or element descendants
  
- **Usage:**
  ```js
  Element('button').on('click', callback);
  Element('div').on('click', 'button', callback);
  ```

### off

- **Arguments:**
  - `{...String} events`
  
- **Details:**
  
  Remove an event handler(s)
  
- **Usage:**
  ```js
  Element('button').off('click');
  ```