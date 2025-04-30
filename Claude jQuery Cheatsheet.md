# Claude jQuery Cheatsheet

## Core jQuery

### Selectors
```javascript
// Basic selectors
$("#id")               // Select by ID
$(".class")            // Select by class
$("element")           // Select by tag name
$("[attribute]")       // Select by attribute
$("[attribute=value]") // Select by attribute value

// Combinators
$("parent > child")    // Direct children
$("ancestor descendant") // All descendants
$("prev + next")       // Adjacent sibling
$("prev ~ siblings")   // All siblings

// Multiple selectors
$("selector1, selector2") // Multiple selectors

// Pseudo-classes
$(":first")            // First matching element
$(":last")             // Last matching element
$(":eq(n)")            // Element at index n
$(":gt(n)")            // Elements with index greater than n
$(":lt(n)")            // Elements with index less than n
$(":even")             // Even elements
$(":odd")              // Odd elements
$(":not(selector)")    // Elements that don't match the selector
$(":has(selector)")    // Elements that contain elements matching selector
$(":contains(text)")   // Elements containing specific text
$(":empty")            // Elements with no children
$(":parent")           // Elements that are parents
$(":visible")          // Visible elements
$(":hidden")           // Hidden elements
$(":checked")          // Checked inputs
$(":selected")         // Selected options
$(":disabled")         // Disabled form elements
$(":enabled")          // Enabled form elements
```

### DOM Traversal
```javascript
// Traversing up
$element.parent()            // Direct parent
$element.parents([selector]) // All ancestor elements
$element.parentsUntil([selector]) // Ancestors until selector match
$element.closest(selector)   // Closest ancestor matching selector

// Traversing down
$element.children([selector]) // Direct children
$element.find(selector)      // All descendants matching selector

// Traversing sideways
$element.siblings([selector]) // All siblings
$element.next([selector])    // Next sibling
$element.nextAll([selector]) // All next siblings
$element.nextUntil([selector]) // Next siblings until selector match
$element.prev([selector])    // Previous sibling
$element.prevAll([selector]) // All previous siblings
$element.prevUntil([selector]) // Previous siblings until selector match

// Filtering
$elements.first()            // First element
$elements.last()             // Last element
$elements.eq(index)          // Element at index
$elements.filter(selector)   // Elements matching selector
$elements.not(selector)      // Elements not matching selector
$elements.slice(start, end)  // Elements in range
```

### DOM Manipulation
```javascript
// Content manipulation
$element.html()              // Get HTML content
$element.html(newHTML)       // Set HTML content
$element.text()              // Get text content
$element.text(newText)       // Set text content
$element.val()               // Get input value
$element.val(newValue)       // Set input value
$element.empty()             // Remove all child nodes

// Attribute manipulation
$element.attr(name)          // Get attribute
$element.attr(name, value)   // Set attribute
$element.removeAttr(name)    // Remove attribute
$element.prop(name)          // Get property
$element.prop(name, value)   // Set property
$element.removeProp(name)    // Remove property
$element.data(key)           // Get data attribute
$element.data(key, value)    // Set data attribute

// Insertion
$element.append(content)     // Append content
$element.prepend(content)    // Prepend content
$element.after(content)      // Insert after element
$element.before(content)     // Insert before element
$element.appendTo(target)    // Append element to target
$element.prependTo(target)   // Prepend element to target
$element.insertAfter(target) // Insert element after target
$element.insertBefore(target) // Insert element before target
$element.wrap(wrapper)       // Wrap element
$element.wrapAll(wrapper)    // Wrap all elements
$element.wrapInner(wrapper)  // Wrap inner content
$element.unwrap()            // Remove wrapper

// Removal
$element.remove()            // Remove element
$element.detach()            // Remove but keep data/events
$element.replaceWith(newContent) // Replace element

// Copying
$element.clone([withEvents]) // Clone element
```

### CSS & Classes
```javascript
// CSS manipulation
$element.css(property)       // Get CSS property
$element.css(property, value) // Set CSS property
$element.css({prop1: val1, prop2: val2}) // Set multiple properties

// Classes
$element.addClass(className)  // Add class
$element.removeClass(className) // Remove class
$element.toggleClass(className) // Toggle class
$element.hasClass(className)  // Check for class

// Dimensions
$element.width()              // Get width
$element.width(value)         // Set width
$element.height()             // Get height
$element.height(value)        // Set height
$element.innerWidth()         // Width + padding
$element.innerHeight()        // Height + padding
$element.outerWidth([includeMargin]) // Width + padding + border (+margin)
$element.outerHeight([includeMargin]) // Height + padding + border (+margin)

// Position
$element.position()           // Get position relative to parent
$element.offset()             // Get position relative to document
$element.scrollTop()          // Get scroll position
$element.scrollTop(value)     // Set scroll position
$element.scrollLeft()         // Get horizontal scroll position
$element.scrollLeft(value)    // Set horizontal scroll position
```

## Events

### Event Binding
```javascript
// Basic event binding
$element.click(handler)                // Click event
$element.dblclick(handler)             // Double-click event
$element.mouseenter(handler)           // Mouse enter event
$element.mouseleave(handler)           // Mouse leave event
$element.hover(enterHandler, leaveHandler) // Hover event
$element.focus(handler)                // Focus event
$element.blur(handler)                 // Blur event
$element.keydown(handler)              // Key down event
$element.keyup(handler)                // Key up event
$element.keypress(handler)             // Key press event
$element.submit(handler)               // Form submit event
$element.change(handler)               // Input change event
$element.resize(handler)               // Window resize event
$element.scroll(handler)               // Scroll event
$element.load(handler)                 // Load event

// General event binding
$element.on(event, [selector], [data], handler) // Bind event with optional delegation
$element.one(event, [selector], [data], handler) // One-time event binding
$element.off(event, [selector], [handler]) // Unbind event

// Event triggering
$element.trigger(event, [data])        // Trigger event
$element.triggerHandler(event, [data]) // Trigger event handler only

// Event objects
event.preventDefault()                 // Prevent default action
event.stopPropagation()                // Stop event bubbling
event.stopImmediatePropagation()       // Stop event and prevent other handlers
event.type                             // Event type
event.which                            // Key or button pressed
event.target                           // Event target
event.currentTarget                    // Current event target
event.pageX, event.pageY               // Mouse position
event.data                             // Event data
```

## Ajax

### Ajax Requests
```javascript
// Basic Ajax
$.ajax({
  url: "url",                  // URL to send request to
  type: "GET",                 // HTTP method (GET, POST, PUT, DELETE)
  dataType: "json",            // Type of data expected back
  data: {name: "value"},       // Data to send
  headers: {},                 // Custom headers
  success: function(data) {},  // Success callback
  error: function(xhr, status, error) {},  // Error callback
  complete: function() {},     // Complete callback
  beforeSend: function(xhr) {},// Before send callback
  timeout: 3000,               // Timeout in milliseconds
  async: true,                 // Asynchronous request
  cache: true,                 // Allow caching
  context: object,             // Context for callbacks
  contentType: "application/json", // Content type
  processData: true,           // Process data as query string
  xhr: function() {}           // Custom XHR object
});

// Shorthand methods
$.get(url, [data], [callback], [dataType])      // GET request
$.post(url, [data], [callback], [dataType])     // POST request
$.getJSON(url, [data], [callback])              // GET JSON
$.getScript(url, [callback])                    // Load and execute JS

// Request promises
$.ajax({...}).done(function() {})    // Success handler
            .fail(function() {})     // Error handler
            .always(function() {})   // Complete handler
            .then(success, error)    // Success and error handlers

// Global Ajax handlers
$(document).ajaxStart(function() {}) // Ajax request starts
$(document).ajaxStop(function() {})  // All Ajax requests complete
$(document).ajaxComplete(function() {}) // Ajax request completes
$(document).ajaxError(function() {}) // Ajax request error
$(document).ajaxSuccess(function() {}) // Ajax request succeeds
$(document).ajaxSend(function() {})  // Before Ajax request sent

// Ajax settings
$.ajaxSetup({                        // Set default values for future Ajax requests
  timeout: 5000,
  cache: false
});
```

## Effects & Animations

### Basic Effects
```javascript
// Visibility
$element.show([duration], [easing], [callback])       // Show element
$element.hide([duration], [easing], [callback])       // Hide element
$element.toggle([duration], [easing], [callback])     // Toggle visibility

// Fading
$element.fadeIn([duration], [easing], [callback])     // Fade in
$element.fadeOut([duration], [easing], [callback])    // Fade out
$element.fadeToggle([duration], [easing], [callback]) // Toggle fade
$element.fadeTo(duration, opacity, [easing], [callback]) // Fade to opacity

// Sliding
$element.slideDown([duration], [easing], [callback])  // Slide down
$element.slideUp([duration], [easing], [callback])    // Slide up
$element.slideToggle([duration], [easing], [callback]) // Toggle slide

// Custom animations
$element.animate({
  properties: value          // CSS properties to animate
}, 
  duration,                 // Duration in ms or "slow"/"fast"
  easing,                   // Easing function ("linear" or "swing")
  callback                  // Callback function when complete
);

// Animation control
$element.stop([clearQueue], [jumpToEnd])     // Stop animation
$element.delay(duration)                     // Delay before next animation
$.fx.off = true                             // Disable all animations
```

## Utilities

### Common Utilities
```javascript
// Array utilities
$.each(array|object, callback)              // Iterate over array or object
$.map(array|object, callback)               // Map array or object
$.grep(array, callback, [invert])           // Filter array
$.inArray(value, array, [fromIndex])        // Index of value in array
$.makeArray(object)                         // Convert to array
$.merge(array1, array2)                     // Merge arrays

// Object utilities
$.extend([deep], target, object1, [objectN]) // Extend objects
$.isArray(object)                           // Check if array
$.isEmptyObject(object)                     // Check if empty object
$.isFunction(object)                        // Check if function
$.isNumeric(value)                          // Check if numeric
$.isPlainObject(object)                     // Check if plain object
$.type(object)                              // Get type

// String utilities
$.trim(string)                              // Trim whitespace
$.param(object)                             // Serialize to query string
$.parseJSON(string)                         // Parse JSON string
$.parseXML(string)                          // Parse XML string

// DOM Ready
$(document).ready(function() {})            // DOM ready handler
$(function() {})                            // Shorthand for DOM ready

// Deferred objects
var dfd = $.Deferred()                     // Create deferred object
dfd.resolve(args)                          // Resolve deferred
dfd.reject(args)                           // Reject deferred
dfd.promise()                              // Get promise object
dfd.done(callback)                         // Add success callback
dfd.fail(callback)                         // Add error callback
dfd.always(callback)                       // Add complete callback
dfd.then(doneCallback, failCallback)       // Add callbacks
$.when(dfd1, dfd2).then(callback)          // When all deferreds resolved
```

## jQuery UI Core Concepts

### Widget Factory
```javascript
// Create custom widget
$.widget("namespace.widgetname", {
  options: {                              // Default options
    option1: value1
  },
  _create: function() {},                 // Constructor
  _init: function() {},                   // Initialization
  _setOption: function(key, value) {},    // Set option
  methodName: function() {},              // Public method
  _privateMethod: function() {},          // Private method
  destroy: function() {}                  // Destructor
});

// Use widget
$element.widgetname({option1: newValue});  // Initialize
$element.widgetname("methodName");         // Call method
$element.widgetname("option", "option1");  // Get option
$element.widgetname("option", "option1", newValue); // Set option
$element.widgetname("destroy");            // Destroy
```

## Common Patterns

### Document Ready
```javascript
// Standard document ready
$(document).ready(function() {
  // Code to run when DOM is ready
});

// Shorthand document ready
$(function() {
  // Code to run when DOM is ready
});
```

### Event Delegation
```javascript
// Delegate events to handle dynamically added elements
$(document).on("click", ".selector", function(event) {
  // Event handler code
});
```

### AJAX Form Submission
```javascript
$("#myForm").submit(function(event) {
  event.preventDefault();
  
  $.ajax({
    url: $(this).attr("action"),
    type: $(this).attr("method"),
    data: $(this).serialize(),
    success: function(response) {
      // Handle success
    },
    error: function(xhr) {
      // Handle error
    }
  });
});
```

### Chaining
```javascript
// Method chaining for concise code
$("#element")
  .hide()
  .addClass("highlight")
  .fadeIn(1000)
  .text("New content");
```

### Plugin Template
```javascript
// Basic jQuery plugin pattern
(function($) {
  $.fn.myPlugin = function(options) {
    // Default options
    var settings = $.extend({
      option1: "default1",
      option2: "default2"
    }, options);
    
    // Plugin code
    return this.each(function() {
      var $this = $(this);
      
      // Plugin implementation
    });
  };
})(jQuery);

// Usage
$(".elements").myPlugin({
  option1: "custom value"
});
```
