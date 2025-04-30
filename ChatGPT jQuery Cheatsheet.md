Great. I’ll compile a comprehensive jQuery cheatsheet that covers all core categories including selectors, events, effects, DOM manipulation, AJAX, utilities, plugins, and common patterns. I’ll also include example syntax and usage notes for each.
I’ll let you know once it’s ready.

# jQuery Cheatsheet

**jQuery Overview:** jQuery is a fast, small, and feature-rich JavaScript library. It makes tasks like HTML document traversal and manipulation, event handling, animation, and Ajax much simpler with an easy-to-use API that works across many browsers ([jQuery API Documentation](https://api.jquery.com/#:~:text=jQuery%20is%20a%20fast%2C%20small%2C,that%20you%20check%20out%20the)). This cheatsheet covers the most common jQuery features with examples and tips.

## Selectors 
jQuery uses **CSS selectors** (and its own extensions) to select elements in the DOM ([Selectors | jQuery API Documentation](https://api.jquery.com/category/selectors/#:~:text=Borrowing%20from%20CSS%201%E2%80%933%2C%20and,of%20elements%20in%20a%20document)). You pass a CSS selector string into the `$()` function to get a jQuery collection of elements. You can then apply jQuery methods to this collection.

### Basic Selectors
- **Element Selector** – `$("tag")`: Selects all `<tag>` elements.  
- **ID Selector** – `$("#id")`: Selects the element with the specific id.  
- **Class Selector** – `$(".class")`: Selects all elements with the class.  
- **Universal Selector** – `$("*")`: Selects all elements (use sparingly for performance).  
- **Multiple Selectors** – `$("selector1, selector2, ...")`: Selects all elements matching any of the selectors (e.g. `$("div,myClass")` for all `<div>` and elements with class `myClass`) ([Multiple Selector (“selector1, selector2, selectorN”) | jQuery API Documentation](https://api.jquery.com/multiple-selector/#:~:text=You%20can%20specify%20any%20number,add%28%29%20method)).

```js
// Examples:
$("p")          // All <p> elements
$("#main")      // Element with id="main"
$(".highlight") // All elements with class="highlight"
$("h1, h2")     // All <h1> and <h2> elements
```

### Hierarchical (Relationship) Selectors
- **Descendant Selector** – `A B`: Selects all B elements that are inside A (at any depth).  
- **Child Selector** – `A > B`: Selects B elements that are direct children of A.  
- **Adjacent Sibling** – `A + B`: Selects the B element immediately following an A element.  
- **General Sibling** – `A ~ B`: Selects all B elements that share the same parent as A and come after A.

```js
// HTML: <ul id="list"><li>First</li><li class="sec">Second</li></ul>
$("#list li")    // all <li> inside element with id="list"
$("#list > li")  // direct children <li> of #list (same result here)
$("li.sec + li") // the <li> immediately after any <li class='sec'>
```

### Attribute Selectors
- **Has Attribute** – `$("[name]")`: Elements with the given attribute (any value).  
- **Exact Match** – `$("[type='text']")`: Elements with attribute equal to a value.  
- **Not Equal** – `$("[type!='text']")`: Elements that either lack the attribute or have a different value ([Selectors | jQuery API Documentation](https://api.jquery.com/category/selectors/#:~:text=Attribute%20Not%20Equal%20Selector%20)) (jQuery-specific extension).  
- **Contains Substring** – `$("[title*='hello']")`: Attribute value contains "hello".  
- **Starts With** – `$("[href^='https']")`: Attribute value starts with "https".  
- **Ends With** – `$("[href$='.pdf']")`: Attribute value ends with ".pdf".  

You can combine multiple attribute conditions as well. Remember to **escape special CSS characters** in selectors (like `.` in an ID) with a backslash `\\` ([Selectors | jQuery API Documentation](https://api.jquery.com/category/selectors/#:~:text=To%20use%20any%20of%20the,character%20escape%20sequences%20for%20identifiers)).

```js
// Examples:
$("input[name]")            // all <input> elements that have a name attribute
$("a[target='_blank']")     // all links that open in new tab
$("img[alt*='logo']")       // all images whose alt text contains 'logo'
$("script[src$='.js']")     // all script tags with src ending in .js
```

### Pseudo-class & Filter Selectors
jQuery supports CSS pseudo-classes and adds its own **filter selectors** for convenience:

- **:first / :last** – Select the first or last element in a set ([Basic Filter | jQuery API Documentation](https://api.jquery.com/category/selectors/basic-filter-selectors/#:~:text=%3Afirst%20Selector)).  
- **:even / :odd** – Elements with even or odd index within the selection (index starts at 0 = even) ([Basic Filter | jQuery API Documentation](https://api.jquery.com/category/selectors/basic-filter-selectors/#:~:text=%3Aeven%20Selector)).  
- **:eq(n)** – The element at index *n* within the matched set ([Basic Filter | jQuery API Documentation](https://api.jquery.com/category/selectors/basic-filter-selectors/#:~:text=%3Aeq)) (`:eq(0)` is same as `:first`). Also `:lt(n)` (indexes less than n) and `:gt(n)` (greater than n).  
- **:not(selector)** – Elements that do **not** match the given selector.  
- **:has(selector)** – Elements that contain an element matching the selector (as a descendant).  
- **:contains("text")** – Elements that contain the given text content.  
- **:empty** – Elements with no children (including no text).  
- **:visible / :hidden** – Elements that are currently visible or hidden (considering inline styles or layout; *note:* `:hidden` also selects elements with `display:none` or width/height 0).  
- **Form-related:** `:input` (all form controls), `:text` (text inputs), `:checkbox`, `:checked` (checked checkboxes/radios), `:selected` (selected `<option>`), etc.

```js
// Examples:
$("p:first")           // the first <p> element on the page
$("li:odd")            // all odd-indexed <li> elements (2nd, 4th, 6th, ...)
$("div:has(p.notice)") // all divs that have a <p class="notice"> inside
$("input:checked")     // all checked checkboxes/radios
$("tr:visible")        // all visible table rows
$("option:selected")   // the selected options in dropdowns
```

**Tip:** jQuery's `:first`, `:last`, etc. are **not the same as** CSS `:first-child` or `:last-child`. For example, `$("p:first")` finds the first `<p>` in the entire selection, whereas `$("p:first-child")` finds each `<p>` that is the first child of its parent. You can use any standard CSS3 pseudo-class as a selector in jQuery, as well as these jQuery-specific filters. For a full reference of selectors, see the **jQuery Selectors** documentation ([Selectors | jQuery API Documentation](https://api.jquery.com/category/selectors/#:~:text=Borrowing%20from%20CSS%201%E2%80%933%2C%20and,of%20elements%20in%20a%20document)).

## DOM Manipulation 
jQuery provides many methods to manipulate the DOM: altering element content or attributes, inserting or removing elements, and traversing the DOM tree. All these methods ultimately change the page’s HTML structure or element properties ([Manipulation | jQuery API Documentation](https://api.jquery.com/category/manipulation/#:~:text=All%20of%20the%20methods%20in,DOM%20elements%20for%20later%20use)). Most of these methods are **“setters”**, meaning they modify the DOM; many also act as **“getters”** if called without arguments (returning information instead of setting it).

### Inserting Elements
- **.append(content)** – Insert content at the *end* of each target element (as last child) ([Manipulation | jQuery API Documentation](https://api.jquery.com/category/manipulation/#:~:text=)).  
- **.prepend(content)** – Insert content at the *beginning* of each target element (as first child).  
- **.after(content)** – Insert content immediately *after* each target element (as a sibling) ([Manipulation | jQuery API Documentation](https://api.jquery.com/category/manipulation/#:~:text=)).  
- **.before(content)** – Insert content immediately *before* each target element.  
- **.appendTo(target)** / **.prependTo(target)** – Insert the selected elements into the target element (at end or beginning, respectively) ([Manipulation | jQuery API Documentation](https://api.jquery.com/category/manipulation/#:~:text=Also%20in%3A%20Manipulation%20%20,36)).  
- **.wrap(wrapper)** – Wrap each element in the selection with the given HTML structure (e.g. `$("p").wrap("<div class='wrapper'>")`). There are also `.wrapAll`, `.wrapInner`, `.unwrap` variants.

You can pass in HTML strings (`"<div>...</div>"`), DOM elements, or jQuery objects as content. If the content is a string containing HTML, jQuery will create the element(s). You can even insert existing elements (which moves them) or clone elements first if you want to duplicate them (using **.clone()**).

```js
$("<p>New paragraph</p>").appendTo("#main"); // create a <p> and append it to #main
$("#list").prepend("<li>First item</li>");   // add a new first <li> inside #list
$(".item").after("<hr>");                   // insert an <hr> after every .item element
```

### Removing & Replacing Elements
- **.remove()** – Remove the selected elements *and* their children from the DOM ([.remove() | jQuery API Documentation](https://api.jquery.com/remove/#:~:text=.remove%28%29%20,well%20as%20everything%20inside%20it)).  
- **.empty()** – Remove *all child nodes* from the selected elements (leaves the container in place).  
- **.detach()** – Like `.remove()`, but keeps the removed elements in memory (including their data/events) in case you need to re-insert them later.  
- **.replaceWith(newContent)** – Replace each element in the selection with the provided new content (returns the removed elements).  
- **.replaceAll(target)** – The opposite of replaceWith: replace the target elements with the selection.

```js
$("#old").remove();          // completely remove element with id="old"
$("#messages").empty();      // clear all content inside element with id="messages"
$("p").replaceWith("<div>Replaced!</div>"); // replace all <p> with a <div>
```

### Manipulating Content & Attributes
- **.text()** – Get or set the *text content* of elements. If used with an argument, it sets the text (treating the input as plain text). Without argument, it returns the combined text of the elements.  
- **.html()** – Get or set the *HTML content* (inner HTML) of elements. Setting HTML can include tags which will be interpreted. *(Be careful of HTML injection when using dynamic data.)*  
- **.val()** – Get or set the *value* of form elements (input, select, textarea). e.g. `$("#myInput").val("new value")`.  
- **.attr(name, value)** – Get or set an *attribute* value. With one argument, returns the attribute value of the first element; with two arguments or an object, sets attributes for each element ([.attr() | jQuery API Documentation](https://api.jquery.com/attr/#:~:text=The%20,method)) ([.attr() | jQuery API Documentation](https://api.jquery.com/attr/#:~:text=Note%3A%20Attribute%20values%20are%20strings,such%20as%20value%20and%20tabindex)).  
- **.prop(name, value)** – Get or set a *property* (DOM property) of elements. Use this for boolean properties like `checked`, `selected`, `disabled` instead of `.attr`. For example, `$("#check").prop("checked", true)` will check a checkbox. As of jQuery 1.6+, `.prop()` should be used for DOM properties, while `.attr()` is for HTML attributes ([.attr() | jQuery API Documentation](https://api.jquery.com/attr/#:~:text=As%20of%20jQuery%201,prop%28%29%20method)).
- **.removeAttr(name)** – Remove an attribute entirely.  
- **.addClass(name)** / **.removeClass(name)** / **.toggleClass(name)** – Add, remove, or toggle CSS class(es) on elements ([Manipulation | jQuery API Documentation](https://api.jquery.com/category/manipulation/#:~:text=)). You can also pass a function to determine the class name, or toggleClass with a force boolean.  
- **.hasClass(name)** – Check if at least one element in the set has the given class (returns boolean).  
- **.css(property, value)** – Get or set inline CSS styles. Pass a property name (or an object of prop-value pairs) and value to set, or omit the value to retrieve the current computed value.

```js
// Set text/HTML
$("#msg").text("Hello <b>World</b>");       // sets text content (tags will be escaped)
$("#msg").html("Hello <b>World</b>");       // sets HTML content (bold tag will render)

// Get values
let title = $("h1").text();                // get text of first <h1>
let firstInputVal = $("input").val();      // value of first input field

// Attribute vs Property
$("img#logo").attr("src", "logo2.png");    // set the src attribute of image
$("#check").prop("checked", false);        // uncheck the checkbox (property)
console.log( $("#check").attr("checked") ); // might show "" or undefined (attribute)

// Class manipulation
$("#box").addClass("active").css("border", "2px solid red");
$(".item").toggleClass("hidden");
```

**Tip:** Many of these methods are *getters* **and** *setters*. For example, `.html()` with no argument returns content, but with an argument it sets content. Similarly, `.attr()` and `.prop()` can retrieve or set values. When setting, these methods affect *all elements* in the jQuery selection. If you only want to affect one element, select or filter only that element first.

### DOM Traversal 
Use traversal methods to navigate the DOM relative to a starting element selection. jQuery’s traversal methods allow moving up, down, and sideways in the DOM tree:

- **.parent() / .parents()** – Get the immediate parent, or all ancestor elements up the chain. You can optionally filter ancestors by a selector.  
- **.closest(selector)** – Starting from the element itself, find the first ancestor (up to and including itself) that matches the selector ([Traversing | jQuery API Documentation](https://api.jquery.com/category/traversing/#:~:text=)). Useful for event delegation (e.g. find a container element).  
- **.children(selector)** – Get direct children of each element in the set (optionally filtered by selector) ([Traversing | jQuery API Documentation](https://api.jquery.com/category/traversing/#:~:text=)).  
- **.find(selector)** – Get *descendant* elements of each element in the current set that match the selector (searches *inside* the elements) ([Traversing | jQuery API Documentation](https://api.jquery.com/category/traversing/#:~:text=)).  
- **.siblings()** – All sibling elements (sharing the same parent) of each element in the set (excluding the element itself).  
- **.next() / .prev()** – The immediate next sibling, or previous sibling, of each element. Also `.nextAll()`, `.prevAll()` for *all* subsequent or preceding siblings, and `.nextUntil()/prevUntil()` to get siblings up to a certain selector.  
- **.first() / .last()** – Reduce the selection to only the first or last element (equivalent to using the `:first`/`:last` selector filter) ([Basic Filter | jQuery API Documentation](https://api.jquery.com/category/selectors/basic-filter-selectors/#:~:text=%3Afirst%20Selector)).  
- **.eq(index)** – Reduce the selection to the element at the given index (0-based) ([Basic Filter | jQuery API Documentation](https://api.jquery.com/category/selectors/basic-filter-selectors/#:~:text=%3Aeq)). Negative indices count from end (-1 is last element).  
- **.filter(selector/function)** – Filter the current set to those matching a selector or a truth-test function ([Traversing | jQuery API Documentation](https://api.jquery.com/category/traversing/#:~:text=)).  
- **.not(selector)** – Remove elements from the set that match the selector (inverse of filter).  
- **.add(selector)** – Add additional elements to the current jQuery collection (e.g. add a different selector’s results to current set) ([Traversing | jQuery API Documentation](https://api.jquery.com/category/traversing/#:~:text=)).  
- **.end()** – When you’ve navigated to a new set (e.g. via `.find()` or `.filter()`), `.end()` goes back to the previous set. This is useful for chaining multiple traversals and then returning to the original selection.

```js
let $item = $("#list .active");    // some active item in a list
$item.parent().addClass("highlight");       // add class to its parent
$item.closest("ul").addClass("has-active"); // nearest ancestor <ul> (could be the parent or higher) gets class
$item.siblings().hide();                   // hide all siblings of the active item
$item.find("span.title").text("Updated");  // find a descendant span inside the item and change text

// Chaining example with end():
$("#content").find("p").css("color","blue")
             .end().css("border","1px solid #000");
// Here, find("p") selects paragraphs in #content and colors them, 
// end() goes back to the $("#content") selection, and then we set a border on #content.
```

## Events 
jQuery makes it easy to **handle events** (such as clicks, form submissions, key presses, etc.) and bind event handlers to elements. These methods are used to register behaviors that run when the user interacts with elements ([Events | jQuery API Documentation](https://api.jquery.com/category/events/#:~:text=These%20methods%20are%20used%20to,further%20manipulate%20those%20registered%20behaviors)). jQuery normalizes event handling across browsers and provides an `event` object to your handlers.

### Binding Event Handlers
- **.on(events, selector, data, handler)** – Attach an event handler function for one or more events to the selected elements ([Events | jQuery API Documentation](https://api.jquery.com/category/events/#:~:text=)). This is the *primary* way to bind events in jQuery. You can specify multiple events like `"click dblclick"` or use a space-separated list. Optionally, provide a *selector* for **delegation** (see below) and a `data` object to pass initial data to the handler.  
- **.one(event, handler)** – Like `.on`, but the handler runs at most once for each element ([Events | jQuery API Documentation](https://api.jquery.com/category/events/#:~:text=)) (it will auto-unbind after the first trigger).  
- **.off(event, handler)** – Remove event handler(s) that were bound with `.on()` (or older methods). Call `.off()` with no arguments to remove *all* handlers from the elements.  
- **Event shorthand methods:** jQuery provides shortcut methods for common events: e.g. `.click(handler)`, `.dblclick()`, `.mouseenter()`, `.mouseleave()`, `.submit()`, `.change()`, `.keyup()`, etc. These are essentially shortcuts to `.on("event", handler)`. For example, `$("#btn").click(fn)` is the same as `$("#btn").on("click", fn)`. You can also **trigger** these events by calling the method without a handler: `$("#btn").click()` programmatically triggers a click. However, using `.on()` for binding and **.trigger()** for triggering events is more flexible.  
- **.trigger(eventType)** – Manually trigger all handlers attached to the elements for the given event type ([Events | jQuery API Documentation](https://api.jquery.com/category/events/#:~:text=Also%20in%3A%20Events%20%20,32)). For example, `$("form").trigger("submit")` will cause the submit handlers to run.  
- **.triggerHandler(eventType)** – Similar to `.trigger`, but only triggers handlers **on the first element** in the selection and does not bubble the event up the DOM ([Events | jQuery API Documentation](https://api.jquery.com/category/events/#:~:text=)). It also doesn’t trigger the default browser action (like clicking a form submit button won’t actually submit the form).  
- **.preventDefault()** (on event object) – Inside an event handler, call `event.preventDefault()` to stop the default browser action (e.g. prevent a link click from navigating or a form from submitting).  
- **.stopPropagation()** – Call on event object to stop the event from bubbling up to parent elements (useful if you don’t want parent handlers to fire). Also, `.stopImmediatePropagation()` to stop **all** other handlers from executing on that element for that event.

When an event handler is called, `this` refers to the DOM element that triggered the event, and the handler function receives the **event object** as an argument (often named `e` or `event`). You can use `$(this)` to get a jQuery object for the element. Within the handler, you can access `e.target` (the actual element that triggered the event), `e.currentTarget` (the element the handler is bound to if different due to delegation), and other properties like `e.pageX`, `e.pageY` for mouse position, `e.which` for key or mouse button, etc.

```js
// Binding and triggering events
$("#myButton").on("click", function(e) {
    e.preventDefault();         // prevent default action (if any, e.g. if button is a submit)
    console.log("Button clicked!", this, $(this).text());
});
$("#myButton").trigger("click");  // programmatically trigger the click event

// Using .one to auto-unbind after first use
$(window).one("resize", () => console.log("Window resized (first time)"));
```

**Tip:** It’s recommended to use `.on()` for all event binding in new code – it supersedes older methods like `.bind()`, `.unbind()`, `.delegate()`, and `.live()`. Those older methods are deprecated/removed in modern jQuery versions in favor of the flexibility of `.on()`.

### Event Delegation
**Event delegation** allows you to handle events on many elements by attaching a single handler to a parent element. jQuery's `.on()` method supports delegation by specifying a *selector* parameter. This means the handler is bound to a parent, but will fire whenever an event originates from a matching child element (even if those child elements are added later dynamically).

- **.on(eventType, **selector**, handler)** – When a selector is provided in `.on()`, the event handler will fire for any descendant element that matches the selector, whenever the event bubbles up to the parent. For example: 

  ```js
  // Delegate clicks on any <li> inside #menu (even if <li> are added later)
  $("#menu").on("click", "li", function() {
      console.log("Clicked menu item:", $(this).text());
  });
  ```

  In this case, the handler is attached to `#menu`. When any `<li>` inside `#menu` is clicked, the event bubbles up to `#menu` and jQuery checks if the target matches `"li"`. If so, the handler runs with `this` set to the `<li>` element. Delegation is useful for handling events on dynamic content or lots of similar elements efficiently (attach to a single ancestor instead of each item).

- **Why delegate?** It improves performance and memory by reducing the number of event listeners, and handles future elements automatically. Common use-case: attach one handler on a container (like a table or list) to catch events on its child items.

### Common Events and Patterns
- **Document Ready:** Use `$(document).ready(handler)` or the shorter `$(handler)` to run code when the DOM is fully loaded. This ensures your code runs after the HTML is parsed (so elements exist). Example: 

  ```js
  $(function() {
    // This code runs when DOM is ready
  });
  ``` 

  Experienced developers often use the shorthand `$()` for `$(document).ready(...)`, but using the full form can be clearer to newcomers ([$( document ).ready() | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/document-ready/#:~:text=Experienced%20developers%20sometimes%20use%20the,to%20use%20the%20long%20form)). Code in a ready handler will execute as soon as the DOM is ready, **before** images or other resources are fully loaded (if you need to wait for all assets, use `$(window).on("load", handler)`). ([$( document ).ready() | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/document-ready/#:~:text=A%20page%20can%27t%20be%20manipulated,just%20the%20DOM%2C%20is%20ready))

- **Form Events:** `.submit()` on forms, `.change()` on inputs/select, `.focus()` / `.blur()`, etc., work similarly to other event binds. For example, `$("form").on("submit", function(e){ ... })` to handle form submission (remember to `e.preventDefault()` if you want to stop actual submission).  

- **Keyboard Events:** `.keydown`, `.keyup`, `.keypress` for key events. You can check `e.which` for key code (or use newer `KeyboardEvent.key`).  

- **Hover Intent:** There’s no direct "hover" event, but jQuery has a convenience method `.hover(handlerIn, handlerOut)` which attaches `mouseenter` and `mouseleave` handlers. Alternatively, use CSS `:hover` for purely style changes, or separate `.on("mouseenter")` and `.on("mouseleave")` for custom JS behavior.

- **Custom Events:** You can trigger and listen for your own event names not built into the browser. For instance, `$(".box").on("myCustomEvent", function(e, data){ ... })` and later `$(".box").trigger("myCustomEvent", [someData])`. This is a way to decouple logic by emitting events.

**Note:** jQuery event handlers automatically use event bubbling (except some like focus/blur which jQuery fixes to bubble). If needed, you can specify capturing manually with vanilla JS, but jQuery’s API doesn’t expose capture in `.on()`. Also, jQuery fixes `this` and event for you in delegated events (so `this` is always the element that matches the selector, not the container).

## Effects and Animations 
One of jQuery’s most famous features is easy **visual effects**. The library provides many methods to show/hide elements with animations. These include simple predefined effects and the ability to create custom animations ([Effects | jQuery API Documentation](https://api.jquery.com/category/effects/#:~:text=The%20jQuery%20library%20provides%20several,to%20craft%20sophisticated%20custom%20effects)). All effect methods return the jQuery object, allowing for chaining and even synchronizing via promises.

### Showing/Hiding 
- **.show(duration)** – Display hidden elements, optionally with an animation over `duration` (in milliseconds or `"fast"/"slow"` presets). Without a duration, it simply removes `display:none` immediately.  
- **.hide(duration)** – Hide elements, optionally with an animation.  
- **.toggle(duration)** – Toggle visibility: show hidden elements or hide visible ones. If a duration is given, it animates the transition.

```js
$("#box").hide();            // immediately hide
$("#box").show("slow");      // show slowly (built-in slow speed ~600ms)
$(".item").toggle(200);      // toggle visibility over 200ms
```

### Fading 
- **.fadeIn(speed)** – Fade in to full opacity (from transparent) ([Effects | jQuery API Documentation](https://api.jquery.com/category/effects/#:~:text=)).  
- **.fadeOut(speed)** – Fade out to transparent (and then sets display to none) ([Effects | jQuery API Documentation](https://api.jquery.com/category/effects/#:~:text=Also%20in%3A%20Effects%20%20,37)).  
- **.fadeToggle(speed)** – Toggle between fading in or out, depending on current state.  
- **.fadeTo(speed, opacity)** – Fade to a specific opacity (0 to 1) without toggling display ([Effects | jQuery API Documentation](https://api.jquery.com/category/effects/#:~:text=)). Allows partial fade (e.g. fade to 50% opacity).

```js
$("#msg").fadeOut(1000);           // fade out over 1 second
$("#msg").fadeIn("fast");         // fade in quickly
$("#msg").fadeTo(500, 0.5);       // fade to 50% opacity over 0.5s (element stays visible at 50%)
```

### Sliding 
- **.slideDown(speed)** – Reveal element by sliding it down (from height 0 to full height).  
- **.slideUp(speed)** – Hide element by sliding it up (collapsing).  
- **.slideToggle(speed)** – Toggle slide up/down.

These are great for accordion-style UI or toggling sections.

```js
$(".panel").slideUp();         // collapse panel
$(".panel").slideDown(400);    // expand panel with animation
$("#menu").slideToggle();      // toggle slide (open/close menu)
```

### Custom Animations 
- **.animate(props, duration, easing, complete)** – Perform a custom animation by changing CSS properties over time ([Effects | jQuery API Documentation](https://api.jquery.com/category/effects/#:~:text=)). You provide an object of CSS properties to animate (numeric values only, e.g. height, width, opacity, left/top for position, etc.), and an optional duration, easing (`"swing"` (default) or `"linear"`, or external easing functions), and a completion callback.

```js
// Animate a box to 200px width and 50% opacity over 0.5s, then alert when done:
$("#box").animate(
  { width: "200px", opacity: 0.5 },
  500,
  "swing",
  function() { alert("Animation complete!"); }
);
```

During an animation, jQuery will repeatedly update the specified CSS properties until the target values are reached. You can queue multiple animations or actions on an element — by default jQuery queues animations, so they run one after the other.

**Other Effects Utilities:**
- **.stop(clearQueue, jumpToEnd)** – Stop the animation queue for the elements. By default, running animation halts, and queued ones may start. You can pass `true` to `clearQueue` to throw away remaining animations, and `jumpToEnd` to immediately finish the current animation. Useful to prevent animations piling up (for example, on rapid hover in/out).  
- **.delay(ms)** – Insert a delay in the animation queue ([Effects | jQuery API Documentation](https://api.jquery.com/category/effects/#:~:text=)). For example, `$("#box").fadeIn().delay(1000).fadeOut();` shows, waits 1s, then hides.  
- **Easing:** jQuery comes with `"swing"` (default, slightly decelerating) and `"linear"`. For more easing functions (bounce, elastic, etc.), you can include the jQuery UI library or other plugins.  
- **.queue()** – Low-level queue control: you can inspect or manipulate the queue of functions for an element. Rarely needed unless doing complex sequencing.

**Chaining and Synchronization:** Because these methods return the jQuery object, you can chain them. Additionally, jQuery’s animation methods return a special **promise-like** object (the jqXHR or a Promise) which you can use with `.done()` for when animation completes. Typically, you’ll use the callback parameter or chain another animation after a previous one to sequence them. For more complex sync, you could use `$.when()` and `.promise()`, but that’s advanced usage.

## AJAX 
jQuery’s AJAX methods let you load data from a server without refreshing the page. jQuery has a full suite of Ajax capabilities ([Ajax | jQuery API Documentation](https://api.jquery.com/category/ajax/#:~:text=The%20jQuery%20library%20has%20a,without%20a%20browser%20page%20refresh)), from simple one-liners to advanced configuration, and it abstracts away browser differences in XMLHttpRequest.

### Shorthand AJAX Methods
These are high-level convenience methods for common HTTP GET/POST requests:

- **$.get(url, data, success, dataType)** – Issue a GET request. `data` (optional) can be an object or query string to send as query params. `success` is a callback to run on successful response. `dataType` can specify the expected response format (`"json"`, `"text"`, `"html"`, `"script"`, `"xml"`, etc.). Returns a **jqXHR** object (which implements Promise interface). ([jQuery.get() | jQuery API Documentation](https://api.jquery.com/jQuery.get/#:~:text=))  
- **$.post(url, data, success, dataType)** – Issue a POST request. Similar usage to $.get.  
- **$.getJSON(url, data, success)** – Shortcut for GET request expecting JSON data (automatically sets dataType to `"json"`). The response will already be parsed into an object.  
- **$(selector).load(url, data, complete)** – Load HTML from a URL and inject it into the selected elements. For example: `$("#result").load("/snippet.html")` will fetch the contents of "snippet.html" and put it inside `#result`. You can include a fragment identifier like `$("div").load("page.html #section")` to load only the part of the page inside `#section`. Optionally, provide data and a callback. This is a quick way to do AJAX content loading with one line.

Example using $.get and $.post:
```js
$.get("/api/user", { id: 42 }, function(data) {
    console.log("User data:", data);
}, "json");

// Posting JSON data to server:
$.post("/api/save", { name: "John", score: 100 })
  .done(function(response) {
    alert("Save successful!");
  })
  .fail(function(xhr) {
    alert("Error: " + xhr.status);
  });
```

In the $.post example above, we use `.done()` and `.fail()` instead of the `success` callback. **Why?** Because as of jQuery 1.5, AJAX methods return a **jqXHR** object that implements the Promise interface, allowing use of `.done()`, `.fail()`, and `.always()` for better flow control ([jQuery.get() | jQuery API Documentation](https://api.jquery.com/jQuery.get/#:~:text=As%20of%20jQuery%201,documentation)). You can attach multiple callbacks and even attach them after the request has completed (they will run immediately if the request is already done).

### Low-Level AJAX & Configuration
- **$.ajax(settings)** – The underlying method used by all shortcuts. It takes a settings object with many options (URL, type, data, dataType, success, error, etc.). It also returns a jqXHR (Promise). Use this when you need to customize the request beyond what $.get/$.post offer, such as setting custom headers, handling different dataTypes, etc.

Key $.ajax options include:
  - `url` – The URL to request.  
  - `type` (or `method`) – HTTP method (`"GET"`, `"POST"`, `"PUT"`, etc.).  
  - `data` – Data to send (will be query string for GET, request body for POST). jQuery will handle encoding an object into query string or JSON if appropriate (for AJAX requests with contentType JSON you might need to JSON.stringify manually unless you set `processData:false`).  
  - `dataType` – Expected response type (`"json"`, `"xml"`, `"text"`, `"script"`, etc.). jQuery will try to parse based on this. If `"json"`, it will parse JSON or call error on parse fail.  
  - `contentType` – The content type of data being sent (defaults `"application/x-www-form-urlencoded; charset=UTF-8"` for POST forms; use `"application/json"` if sending JSON).  
  - `success: function(data, textStatus, jqXHR)` – Callback on 200 OK (or any successful HTTP code).  
  - `error: function(jqXHR, textStatus, errorThrown)` – Callback on failure (network or HTTP error).  
  - `complete: function(jqXHR, textStatus)` – Always called after success or error.  
  - `timeout` – Milliseconds to wait before aborting.  
  - `headers` – Object of custom request headers.  
  - There are many more (see docs), like `xhrFields` for low-level XHR settings, `beforeSend` callback to modify the XHR or abort request, etc.

Example:
```js
$.ajax({
  url: "/api/submit",
  type: "POST",
  data: JSON.stringify({ name: "Alice" }),
  contentType: "application/json; charset=UTF-8",
  dataType: "json",
  success: function(response) { console.log("Server said:", response); },
  error: function(xhr, status, err) { console.error("Ajax error:", status, err); }
});
```

- **$.ajaxSetup(settings)** – Set default values for AJAX requests. For example, you can set a default `$.ajaxSetup({ cache: false })` to force-disable caching for all future requests, or set a default `timeout` for all AJAX calls. This affects all subsequent $.ajax (and related shorthand) calls unless overridden in a specific call. Use with caution (it’s global).  

- **Global AJAX events:** jQuery triggers global events on `document` for all AJAX calls, which you can use to show/hide a global loader, etc. For example, you can do:
  ```js
  $(document).on("ajaxStart", () => { $("#spinner").show(); });
  $(document).on("ajaxStop", () => { $("#spinner").hide(); });
  ```
  Other events include `ajaxSend`, `ajaxError`, `ajaxSuccess`, `ajaxComplete`. These are useful for global feedback. (They won't fire if you set `global:false` in an $.ajax call.)

### Promises and Deferreds
As mentioned, jQuery’s AJAX methods return jqXHR objects that implement the **Promise** interface ([jQuery.get() | jQuery API Documentation](https://api.jquery.com/jQuery.get/#:~:text=As%20of%20jQuery%201,documentation)). This means you can do:
```js
let request = $.get("/data");
request.done(handleSuccess)
       .fail(handleError)
       .always(() => console.log("Request completed"));
```
You can also use `request.then(successFn, errorFn)` which is similar to done/fail.

- **$.Deferred()** – This creates a jQuery Deferred object, which is essentially a promise-controller that you can resolve or reject. It's an advanced feature for synchronizing custom async operations. For example:
  ```js
  function asyncTask() {
    let dfd = $.Deferred();
    setTimeout(() => {
       // do some async work
       if(/* success */) dfd.resolve(result);
       else dfd.reject(error);
    }, 1000);
    return dfd.promise(); // return a promise to the caller
  }

  // Using the custom Deferred
  asyncTask().then(
    data => console.log("Success:", data),
    err => console.log("Failed:", err)
  );
  ```
  In most cases, you won't need to create Deferreds manually unless integrating non-jQuery async code. But it's good to know jQuery’s Ajax and animation methods already use Deferreds internally, so you can chain and coordinate them.

**Note:** jQuery’s promises are based on its own Deferred implementation. Since jQuery 3+, these mostly follow the A+ Promises spec, but be aware older versions had some differences (e.g., jQuery <3’s promise could be immediately resolved if attached after completion). In modern jQuery, you can also use them with `async/await` by wrapping in `Promise.resolve( $.get(...))`, although they are not native ES6 Promise instances (but thenables).

For more info, see jQuery’s documentation on the Deferred object and promise methods ([jQuery.get() | jQuery API Documentation](https://api.jquery.com/jQuery.get/#:~:text=As%20of%20jQuery%201,documentation)).

## Utilities 
jQuery provides a number of utility functions and objects in the `$` namespace for tasks like type checking, array manipulation, etc. These are general helpers not tied to DOM elements ([Utility Methods | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/utility-methods/#:~:text=Utility%20Methods)). Here are some useful ones:

### Type Checking and General Utilities
- **$.type(obj)** – Returns a string indicating the type of the object (e.g. `"string"`, `"object"`, `"array"`, `"function"`, etc.). More reliable than `typeof` for certain things (e.g. `$.type(null)` returns `"null"`).  
- **$.isArray(obj)** – Returns true if the object is an array. (Note: in modern JS, use `Array.isArray`, and jQuery may just alias that).  
- **$.isFunction(obj)** – True if the object is a function. (Deprecated in latest jQuery in favor of native `typeof obj === "function"`).  
- **$.isNumeric(value)** – True if value is numeric (can be converted to a finite number).  
- **$.isEmptyObject(obj)** – True if the object has no enumerable properties (i.e. is an empty `{}`).  
- **$.isPlainObject(obj)** – True if the object is a plain JS object (i.e., `{}` or created via `new Object`, not a custom prototype). Useful to distinguish plain objects from host objects or class instances.

- **$.trim(str)** – Remove leading/trailing whitespace from a string. (Note: This was deprecated as of jQuery 3.5 in favor of native `String.prototype.trim()` ([jQuery.trim() is deprecated · Issue #11 · backdrop-contrib/google_tag](https://github.com/backdrop-contrib/google_tag/issues/11#:~:text=contrib%2Fgoogle_tag%20github,trim))).  
- **$.noop** – An empty function that does nothing (`function(){}`). Can be used as a placeholder callback.

- **$.extend(target, obj1, obj2, ...)** – Merge the contents of two or more objects into the target object and return the target. This is often used for copying or cloning objects, or defining default options for plugins. If target is `true` (first arg), it performs a *deep copy*. Example: `$.extend({}, defaults, options)` creates a new object by merging `options` into `defaults` (without modifying defaults).  
- **$.each(collection, function(index, value){ ... })** – Iterate over arrays or objects ([Utility Methods | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/utility-methods/#:~:text=link%20)). If collection is an array or array-like, `index` is the numeric index and `value` is the item. If collection is an object, `index` is the key and `value` is the value. You can `return false` inside the callback to break out of the loop (like a `break`), or `return true` to skip to next (like `continue`).  
  - **$(selector).each(function(index, element){ ... })** – Similar iteration but for a jQuery selection (set of DOM elements) ([Utility Methods | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/utility-methods/#:~:text=)). Here `this` is the DOM element and the arguments are index and element (same as `this`). This is used to execute code on each element in a selection. Note: `$.each()` and `.each()` are not the same – use `$.each` for generic arrays/objects, and use the jQuery object’s `.each` for a set of elements ([Utility Methods | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/utility-methods/#:~:text=)).
- **$.map(array, function(value, index){ ... })** – Translate all items in an array (or array-like) to a new array of values. The callback returns the new value for each item (or `null/undefined` to skip that item). For example: `$.map([1,2,3], x => x * 2)` gives `[2,4,6]`. There’s also `$(selector).map(fn)` that works on a jQuery collection (and returns a new jQuery collection of mapped elements).
- **$.grep(array, function(value, index){}, invert)** – Filter an array based on a test function. Returns a new array of items where the function returns true (or false if `invert` is true). This is like the equivalent of `Array.prototype.filter` but for older browsers or convenience.

### Data Storage
jQuery allows associating arbitrary data with DOM elements:

- **.data(key, value)** – Store data in the element’s data cache. E.g. `$("#item").data("id", 123)` attaches the value 123 under the key "id" to that element.  
- **.data(key)** (no value) – Retrieve the stored data value for that key. If no key is given, returns an object of all data for the element.  
- **.removeData(key)** – Remove a stored data entry.  
These are useful for attaching information to elements (beyond what’s in the DOM attributes) that your scripts can use. Note that jQuery will also automatically read **HTML5 data-* attributes** and make them available via `.data()`. For example, `<div id="d1" data-role="page"></div>` can be accessed with `$("#d1").data("role")` which yields `"page"`. jQuery handles the mapping of attribute names (dash-case to camelCase, etc.) and also tries to parse JSON or number types in data attributes.

```js
// Using .data() to store and retrieve data on elements
$("#user1").data("age", 25);
console.log( $("#user1").data("age") ); // 25

// HTML5 data- attributes
// <div id="user1" data-name="Alice" data-scores='[10, 20]'></div>
let name = $("#user1").data("name");    // "Alice"
let scores = $("#user1").data("scores"); // [10, 20]  (parsed as array from JSON string)
```

### Miscellaneous Utilities
- **$.now()** – Returns current timestamp (ms since epoch). (`Date.now()` is the native equivalent).  
- **$.param(obj)** – Serialize an object or array into a URL-encoded query string. Useful for sending data in GET or POST. For example, `$.param({a:1, b:2})` returns `"a=1&b=2"`. If obj has array values, jQuery formats by default like `name[]=val1&name[]=val2` (traditional PHP style can be toggled with `$.param(obj, true)`).  
- **$.proxy(func, context)** – Take a function and return a new one that will always run in the provided `context` (`this` value). e.g. `$.proxy(obj.method, obj)` ensures `obj.method` sees `this=obj` when called. (Alternatively in modern JS, use `func.bind(context)`).  
- **$.holdReady(true/false)** – Advanced: hold or release the DOM ready event. If for some reason you need to delay execution of ready handlers (e.g., waiting for another script), you can call `$.holdReady(true)` to pause ready, and later `$.holdReady(false)` to resume it. Rarely used.  

For a full list of utility methods, see the official **jQuery Utilities** documentation ([Utility Methods | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/utility-methods/#:~:text=Utility%20Methods)). Many utilities are for internal use or edge cases, but the ones above are commonly helpful.

## Plugins 
jQuery’s design encourages a vibrant ecosystem of plugins. A **jQuery plugin** is basically a method that extends `$.fn` (the prototype for jQuery objects) so that it can be called on any jQuery selection. There are thousands of plugins available, but you can also write your own to encapsulate reusable functionality.

### Using Plugins
To use a jQuery plugin, include its script file *after* the jQuery library script on your page (so that the `$` is defined). Typically, the plugin will add one or more methods to the jQuery prototype, which you then call on a jQuery selection. For example, if using a plugin `$.fn.slider`, you might do: `$("#myDiv").slider(options)` to initialize it. Always follow the plugin’s documentation for usage.

Many plugins have an initialization call and then provide additional methods or events. Some plugins might require additional CSS or other assets. Including a plugin is as simple as `<script src="jquery.pluginname.js"></script>` (after jQuery). Then you use `$("selector").pluginName(...)` in your code. There’s no special registry in modern jQuery (the old plugins.jquery.com is deprecated); many are distributed via npm or just as JS files.

### Creating a Basic Plugin
Writing a jQuery plugin involves adding a new function to `$.fn`. The pattern is straightforward:

```js
(function($) {              // start a closure to isolate $ 
  $.fn.greenify = function(options) {
    // `this` is the jQuery collection
    this.css("color", "green");
    return this;            // returning this preserves chainability
  };
})(jQuery);                // pass jQuery into the closure
```

In the example above, we added a function `greenify` that turns the text green. We immediately return `this` to allow chaining (so one can do `$("p").greenify().addClass("done")`). The IIFE (immediately-invoked function expression) with `(function($){ ... })(jQuery)` is a **common pattern** to ensure `$` refers to jQuery inside the plugin and to avoid conflicts if `$` is mapped to something else outside (in case `$.noConflict()` is used) ([How to Create a Basic Plugin | jQuery Learning Center](https://learn.jquery.com/plugins/basic-plugin-creation/#:~:text=The%20,and%20name%20the%20parameter)).

Most real plugins accept an `options` object to customize behavior. They often define defaults and merge user options with defaults via **$.extend**. For example:

```js
(function($) {
  $.fn.highlight = function(options) {
    // Default settings:
    let settings = $.extend({
      color: "#ffff00"
    }, options );

    return this.each(function() {  // maintain chainability and work on each element
      $(this).css("background-color", settings.color);
    });
  };
})(jQuery);
```

This plugin `highlight()` will set the background color to yellow by default, but the user can pass a different color. We use `this.each()` to iterate over each element in the jQuery selection ([How to Create a Basic Plugin | jQuery Learning Center](https://learn.jquery.com/plugins/basic-plugin-creation/#:~:text=%24.fn.myNewPlugin%20%3D%20function%28%29%20)) ([How to Create a Basic Plugin | jQuery Learning Center](https://learn.jquery.com/plugins/basic-plugin-creation/#:~:text=Notice%20that%20we%20return%20the,we%27ve%20been%20doing%20so%20far)), applying the effect, and still return the jQuery object (via the return of `.each()` which is `this`). Using `.each()` inside a plugin is a best practice when you need to operate on each element separately (especially if your plugin logic isn't naturally vectorized for multiple elements). By returning `this.each(...)` we ensure the plugin is chainable and each element in the collection gets processed ([How to Create a Basic Plugin | jQuery Learning Center](https://learn.jquery.com/plugins/basic-plugin-creation/#:~:text=)).

**Best Practices for Plugins:** 
- Only take up **one slot** on `$.fn` per plugin ([How to Create a Basic Plugin | jQuery Learning Center](https://learn.jquery.com/plugins/basic-plugin-creation/#:~:text=It%27s%20good%20practice%20when%20writing,other%20words%2C%20this%20is%20bad)). In other words, don't define many separate functions for one conceptual plugin (e.g., avoid `$.fn.openPopup` and `$.fn.closePopup`; instead have a single `$.fn.popup('open')` that takes an action parameter) ([How to Create a Basic Plugin | jQuery Learning Center](https://learn.jquery.com/plugins/basic-plugin-creation/#:~:text=)). This minimizes the chance of collisions with other plugins and keeps the namespace clean.  
- Always return `this` (the jQuery object) from your plugin (unless it's a getter function that extracts a value). This allows users to chain further jQuery calls after your plugin call, which is expected behavior. If your plugin needs to return a computed value (like `.width()` returns a number when no argument given), you typically handle that by returning a value only when appropriate and otherwise returning `this`.  
- Use `this.each(...)` when you need to manipulate each element individually, and do heavy calculations outside loops if possible to avoid repetition.  
- Support options: merge user options with defaults as shown (using `$.extend` or the spread operator to clone). This allows your plugin to be configurable.  
- Protect the `$` alias: wrap your plugin in an IIFE and pass in jQuery so that even if `$.noConflict()` is in effect, your plugin code can still use `$` internally safely ([How to Create a Basic Plugin | jQuery Learning Center](https://learn.jquery.com/plugins/basic-plugin-creation/#:~:text=The%20,and%20name%20the%20parameter)). For example: 
  ```js
  (function($){
    // plugin definition
  })(jQuery);
  ```
- If your plugin adds significant markup or styling, consider providing a cleanup method (e.g., user can call `$(elem).pluginName('destroy')` to remove what was added). Many plugins use a pattern where the plugin function can handle different actions via string arguments (like `"destroy"`, `"disable"`, etc.). Typically you check if `typeof options === "string"` then perform that action.  
- Store plugin state/data using `.data()` on the element if needed (for example, store defaults or an instance reference). This allows your plugin to keep track of whether an element is already initialized, or store intermediate values.
- Try not to conflict with built-in jQuery method names. Choose unique names for your plugin methods (a common prefix or a descriptive name helps).

By following these guidelines, your plugin will play nicely with others and be easier to maintain.

**Example Usage of a Plugin:** Suppose we wrote the `highlight` plugin above. One would use it as:
```js
// Make all paragraphs highlighted in pink:
$("p").highlight({ color: "pink" });
```
This would apply our plugin to all `<p>` elements on the page, turning their background pink.

### Finding and Evaluating Plugins
When looking for existing plugins, check official resources or community sites. The jQuery website previously had a plugins directory; nowadays, you might find plugins on npm, GitHub, or websites like **cdnjs**. Evaluate plugins for recency and compatibility with your jQuery version (many old plugins might not have been updated for jQuery 3.x or 4.x). Also ensure the plugin’s license is suitable for your project.

## Common Patterns and Idioms 
Finally, here are some widely used jQuery patterns and idiomatic practices that will help you write clean and efficient code:

- **Document Ready Pattern:** Always wrap your initialization code in `$(document).ready(...)` (or the shorthand `$()` as shown earlier) so that the DOM is loaded. This is the jQuery equivalent of listening for the DOMContentLoaded event. It protects you from running scripts too early ([$( document ).ready() | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/document-ready/#:~:text=A%20page%20can%27t%20be%20manipulated,just%20the%20DOM%2C%20is%20ready)). If you have multiple script files, you can have multiple `$(document).ready` calls – they will all execute once the DOM is ready. Avoid putting script tags in the `<head>` without a ready handler unless you explicitly want them to run before DOM ready (rare for jQuery code).

- **Chaining:** jQuery encourages chaining of methods for compact code and performance. Since most jQuery methods return the jQuery object, you can do:
  ```js
  $("#title").addClass("active").css("font-size", "18px").text("Hello");
  ```
  This calls three methods on `#title` in one statement. Chaining not only makes code concise but can be more efficient since you avoid creating extra jQuery objects for the same selector repeatedly. Under the hood, it's the same element(s) being operated on sequentially. **How does it work?** Most jQuery methods return `this` (the jQuery object), enabling the next call on the same set ([What is chaining in jQuery ? | GeeksforGeeks](https://www.geeksforgeeks.org/what-is-chaining-in-jquery/#:~:text=In%20jQuery%2C%20chaining%20allows%20us,element%20in%20a%20single%20statement)). This pattern allows multiple operations on an element set in one go. It also improves performance because the browser doesn't have to find the same element again for each operation ([What is chaining in jQuery ? | GeeksforGeeks](https://www.geeksforgeeks.org/what-is-chaining-in-jquery/#:~:text=first%20method%20completes%20its%20execution%2C,be%20called%20on%20this%20object)). Remember that not all methods return the same object – a few (like `.filter()` or `.map()`) return new subsets, but you can still chain further from those. Use `.end()` as noted earlier to go back in the chain if needed.

- **Caching jQuery Objects:** If you need to use the same element multiple times, it's efficient to cache it in a variable:
  ```js
  let $menuItems = $("#menu li");
  $menuItems.addClass("highlight");
  // ... some code ...
  $menuItems.hide();
  ```
  This way, `$("#menu li")` only runs once (which queries the DOM once). The `$` prefix in `$menuItems` is a common convention to indicate the variable holds a jQuery object (collection). This makes your code clearer and avoids redundant DOM lookups.

- **Event Delegation for Dynamic Content:** As mentioned, delegation is a key pattern especially when adding elements on the fly (AJAX-loaded content, or templating). For example, if you have a list that items will be added to dynamically, attach one event handler on the parent and delegate to the child items. This pattern saves you from having to re-bind events after every content update. jQuery’s `.on()` with selector makes this easy and is preferred over repeatedly calling .click() on new elements.

- **Avoiding Conflicts:** If you include multiple libraries that use the `$` (like Prototype.js, or even another version of jQuery), use **jQuery.noConflict()** to relinquish the `$`. You can assign jQuery to another variable if needed:
  ```js
  $.noConflict();
  jQuery("#id").hide();      // now we must use jQuery instead of $
  // Or assign:
  let $jq = jQuery.noConflict();
  $jq("#id").hide();
  ```
  After calling `$.noConflict()`, the global `$` reverts to whatever it was before jQuery (possibly another library). Using noConflict mode, you either use the `jQuery` name fully or store it in another alias variable ([Avoiding Conflicts with Other Libraries | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/avoid-conflicts-other-libraries/#:~:text=That%20said%2C%20there%20is%20one,use%20jQuery%20in%20your%20page)) ([Avoiding Conflicts with Other Libraries | jQuery Learning Center](https://learn.jquery.com/using-jquery-core/avoid-conflicts-other-libraries/#:~:text=%3Cscript%20src%3D)). This pattern is often seen in WordPress themes where Prototype might also be loaded; they wrap jQuery code in `(function($){ ... })(jQuery);` to use `$` safely inside that closure without polluting global `$`. If you write a plugin or module, wrapping in an IIFE with `jQuery` passed in as `$` is a recommended practice (as shown in plugin section).

- **Separation of Concerns:** Keep your JS logic separate from HTML. Avoid inline `onclick=""` or other event attributes in HTML. Instead, use jQuery to select elements and bind events. This makes your HTML cleaner and your JS more maintainable. For example, rather than `<button onclick="doSomething()">`, give the button an ID or class and in your script do `$("#theButton").on("click", doSomething);`. This pattern is fundamental to unobtrusive JavaScript.

- **Selecting Efficiently:** jQuery internally uses the browser's querySelectorAll or Sizzle engine. Use ID selectors or tag+ID where possible for speed (IDs are fastest). Minimize overly broad selectors (like `$("div")` on a huge page). If you already have a DOM element (like `let elem = document.getElementById('x')` or from `this` in a vanilla handler), you can wrap it with jQuery via `$(elem)` instead of reselecting by string, to avoid the lookup. Also, if you need to find relative to a known element, use context: `$("span", $parent)` is equivalent to `$parent.find("span")`. Both restrict search to a subset of the DOM which is more efficient than searching the whole document.

- **Performance Gotcha – DOM Insertion in Loops:** If adding a lot of elements or building HTML, minimize touching the DOM repeatedly (as it causes reflows). For example, rather than doing in a loop: `for {...} $("#list").append("<li>item</li>")`, build a string of HTML or use an array, then append once. Or use a document fragment via vanilla JS or detach the parent, update, then reattach. jQuery itself is fairly optimized, but heavy DOM ops can be slow if not batched.

- **CSS vs JS for animations:** While jQuery animations are easy, consider using CSS transitions/animations for simpler effects (they can be more performant using GPU acceleration). You can use jQuery to add a class that triggers a CSS animation, rather than using .animate. However, jQuery is still very handy for complex sequence control or when you need to support older browsers without CSS animations.

- **Debugging Tip:** Use `console.log()` generously to inspect your selections and data. If a selector isn’t picking up what you expect, `console.log( $(selector) )` to see how many elements it matched. jQuery objects print nicely in Chrome’s console showing the elements. Also, remember that most jQuery collections are array-like, so you can inspect `collection[0]` to see the first DOM element.

**Further Reading:** Check the official jQuery documentation for each method used above for deeper details and edge cases. The jQuery Learning Center (learn.jquery.com) has tutorials on best practices and patterns. As jQuery evolves (jQuery 3.x and upcoming 4.x), be mindful of deprecated features (like `.size()` which was removed in favor of `.length`, or any removed event aliases). This cheatsheet covers the core concepts that remain consistent ([Events | jQuery API Documentation](https://api.jquery.com/category/events/#:~:text=These%20methods%20are%20used%20to,further%20manipulate%20those%20registered%20behaviors)).

jQuery remains useful for quick scripting and legacy projects, and knowing these patterns will help you use it effectively. Happy querying! 

