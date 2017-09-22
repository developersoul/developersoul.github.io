## Making wordpress shortcodes with react

When I used to create wordpress themes, I start first doing shortcodes, to that way have some sort of modularity that allow me to move things around without any problem and reuse it easily on all the pages and layouts.

So the components growth along with complexity so jquery falls short and the code starts to looks messy.

So the solution was vue or react, because of his own nature of everything is a component like shortcodes.

I decide for react cause his way of handle state and jsx.
The complicate at first was how to render the component if was necessary twice or more with his own props…

I look out for packages on npm and I don’t find anything that fit with that requirement.

So I create a package to help me to render dynamically each component multiple times with his own props.
Below his basic implementation:

```javascript
import React from "react";
import { render } from "react-dom";

export default function multipleRender(selector, component) {
  if (document.querySelectorAll(selector).length >= 1) {
    let containers = [...document.querySelectorAll(selector)];

    containers.forEach(el => {
      let props = el.getAttribute("data-props")
        ? JSON.parse(el.getAttribute("data-props"))
        : {};

      render(React.createElement(component, { ...props }), el);
    });
  }
}
```
What makes is simple:

First check if exists the selector checking if has more then one existing node:

```javascript
if (document.querySelectorAll(selector).length >= 1)
```

Second convert nodelist to array. the three dots is a feature of es6 call spread operator.

```javascript
let containers = [...document.querySelectorAll(selector)];
```

Third get the data props attribute of each node and parse it to json and if hasn't the attribute return a empty object.

```javascript
let props = el.getAttribute("data-props")
  ? JSON.parse(el.getAttribute("data-props"))
  : {};
```

And finally render the component with the api createElement that get a component and create a new one with the props pass it. we use object rest spread to pass all props that we get from the attribute data-props.

```javascript
render(React.createElement(component, { ...props }), el);
```
