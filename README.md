# Limp.js
## The best popup boxplugin for jQuery.

Limp.js is a popup box plugin for jQuery that is self contained (no css or images) with a focus on newer client-side usecases.


### Features

  * Content Caching.
  * Template support. Just pass limp a json file and a template name and render.
  * Callback everywhere.
  * Theme Support: Easy to style and change at any time. A style object is passed to limp during render.
  * Resize that works - limp will ajust at any resolution and no matter the content size so it never overflows.

### Example Config

```
var limpOptions = {
  cache: false,
  adjustmentSize: 0,
  loading: true,
  alwaysCenter: true,
  animation: "",
  shadow: "0 0px 20px rgba(0,0,0,0.5)",
  round: 3,
  distance: 10,
  overlayClick: true,
  enableEscapeButton: true,
  dataType: 'html',
  centerOnResize: true,
  closeButton: true,
  style: {
    '-webkit-outline': 0,
    color: '#000',
    position: 'fixed',
    border: 'none',
    outline: 0,
    zIndex: 1000001,
    opacity: 0,
    // overflow: 'auto',
    background: 'transparent'
  },
  inside: {
    background: 'transparent',
    padding: '0',
    display: 'block',
    border: '1px solid #000',
    overflow: 'visible'
  },
  overlay: {
    background: '#3c4242',
    opacity: 0.8
  },
  onOpen: function() {},
  afterOpen: function() {},
  afterDestroy: function() {},
  onTemplate: function(html, data, limp) {
  
    try {
      var template = Handlebars.compile(html);
      return template(data);
    } catch(e) {
      return false;
    }
  
  }
};

```

### Now how do we use it?

There are two pretty simple ways to use it. Below is a more advanced way that Threat Stack, Inc uses it when we are building clinet-side web apps.
We use handlebars to precompile templates and then use the Hnadlebars.templates call to pass the name of the templaye into. It works great and
make the whole process super easy. First you build a function to do the setup.

```
  var limpBox = function(template, data, args) {
    var self = this;

    var options = $.extend({}, self.limpOptions, args);
    options.template = template;
    options.templateData = data;

    var box = $.limp(options);

    return box;
  },
```

Then you can call it like this.

```
$(document).on('click', 'a.remove-item', function(e) {
  e.preventDefault();  
  limpBox.open('helpers/confirm-box', {
    title: "Are you sure you want to delete this file?",
    icon: "icon-remove",
    message: "If you remove this file it will be lost forever!"
  },
  onAction: function() {
    // On action callbacks will find you any element that has the class `limp-action`
    // This is super useful for having two buttons in your popup box: Remove item abd Cancel.

    // Delete Item Here.
  });
});
```

### An easier way to use limp.js is inline with the html.

```js
  $('.open-with-limp').limp(limpOptions);
```

then

```html
  <a href="#" data-limp-href="/ajax/uri" class="open-with-limp">Open Me</a>
```

## Demo Website


## TODO
  * Expire Cache
  * More Template Support - althought it should support pretty much anything since the onTemplate function is changeable.

