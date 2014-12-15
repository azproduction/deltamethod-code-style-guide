# Deltamethod Code Style Guide

## JavaScript

### Indentation and Whitespace

#### Tabs or Spaces?

Until we hire Lea Verou, it's all spaces, bro.

Use 4 spaces for indentation.

#### Quotes
Use single quotes, unless you're writing JSON:

```javascript
var lord = 'Sauron';
```

#### Comments

Prefer one-line comments for explanations:
  * Same-line comments are: 2 spaces, slash, slash, space, comment text
  * Previous-line comments use the same indentation as the line they precede

```javascript
i++;  // increment 'i' so it becomes bigger and better

if (i === 42) {
    // notify all the good people
    sendMessage('got it!');
}
```

Multiline comments should be used for JSDoc mostly, or for this trick when you need to comment and un-comment parts of your code frequently (they differ by 1 character):

```javascript
/**/
this.code().is('working');
/**/

/**
this.code().is('commented out!');
/**/
```

If you comment out a piece of code instead of deleting/replacing it, and then commit the result, there must be a reason for that comment staying in the codebase.

Possible **valid** reasons to keep some old code in comments:

  * small changes likely to cause regressions;
  * code line/block that is often being used for debugging;

Some **non-valid** reasons:

  * obvious bugfix in the old code;
  * big refactoring that changes a significant part of the code

#### Use FIXME and TODO

In your comments, use two special keywords:
  * ``TODO: description`` to describe what should be done later
  * ``FIXME`` to mark bugs you cannot fix right now

#### Good Practices

**Always comment complex parts of your code**.
**If the code is complicated enough, a pull request can be rejected based on that.**

#### Brackets

Whitespace around brackets **collapses**:

```javascript
// no!
banana ( grape ( apple( 12 ), 'red' ) ), quantum );

// yes
banana(grape(apple(12), 'red'), quantum);
```

#### Operations
Add spaces around arithmetical operations and logical conditions:

```javascript
// bad
var q=a?12:'none';

// good
var q = a ? 12 : 'none';
```

#### Line Breaks
Lines should break at about 80 symbols after the first non-whitespace character.

There are obvious exceptions:
  * long string literals
  * code that should stay on 1 line for technical reasons
  
However, using long lines for complex conditions is bad practice; we're not in a Perl one-liner competition.

### Naming Things

Use lowercase for ordinary short variables, camelCase for names that consist of more that one word.
**Do not use** underscore_as_separator_for_variable_names.

Module and component names are capitalized:

```javascript
var Flyout = require('mixins/flyout/mixin');
```

### Scope

  * Use .bind(this) or closures whenever appropriate;
  * Prefer closures for small inline functions (closures are a bit faster as well)
  * Use ``var that = this;`` to keep a reference to ``this``.

### Private/Public Methods

Since we don't really use inheritance, the difference is really subtle.
For private methods and properties, use names starting with underscore, like ``_myPrivateMethod``.

### Objects and Constructors

#### Constructors

Constructor names should begin with a capital letter:

```javascript
function MyConstructor() {
   // some magic
}
```

#### Singletons
Whenever you need a singleton, use anonymous function execution as it allows for some private variables:

```javascript
var single = (function() {
    var privateVar = 42;
    
    return {
        publicMethod: function() {
            alert('Answer to life and everything is: ' + privateVar);
        }
    }
})();
```

### ECMAScript5 Methods
If they are supported by browser, use ES5 instead of jQuery methods.

```Array.prototype.forEach()``` instead of ```$.each()```.


### Functions

#### Declaration and Call
```javascript
// Bad, too loose
function foo ( nomen, count, domElement ) {
    // magic here...
}

foo( 'aleph', 12, new Image );

// Bad, feels like suffocation:
function foo(nomen,count,domElement){/*magic here*/};
foo('aleph',12,new Image);

// Good and readable
function foo(nomen, count, domElement) {
    // magic here...
}

foo('aleph', 12, new Image);
```

#### Anonymous Functions To Be Called In-Place

```javascript
(function($, undefined) {
    // deliver happiness!
})(jQuery);
```

#### var Statement

Always use var statements.

Use single var declaration:

```javascript
function myFunc() {
    var i = 42,
        foo = 'bar',
        emptyObj = {},
        tmp;

    // do some good
}
```

Individual variables still can be introduced later in the function body, especially when it comes to loop control vars.
Variables should be always declared in the beginning of a function.

### Conditons And Loops

Add one space after keywords like ``if``, ``for``, ``while``.

```javascript
if (weekDay === 'Friday') {
    // do party!
    var money = 1000;
    while (money > 0) {
        money--;
    }
} else if (weekDay === 'Sunday') {
    relax();
} else {
    doHardWork();
}
```

#### Indenting Complex Conditions

Use additional indentation of 2 spaces:

```javascript
if (condA
      && condB === 'foo'
      && condC > bar) {
    
    // finally we get here...
}
```

#### Ternary Operator

Use it with short statements, in variable declarations, object literals.
Do not use extra brackets if they are not needed.

Do not use nested ternaries.

```javascript
var properties = {
    happiness: origin === 'Deltamethod' ? 'yes' : 'not really'
};
```

### console.log

Use of ``console.log()`` is only allowed inside error handlers (such as try/catch or Ajax error handlers).
This is only possible if there is a shim in the code that prevents browser from firing 
In normal code, no console.log should be used.


### jQuery

Avoid global selectors:

```javascript
// bad, starts from the document node
$('.button').hide();

// good, provides context
this.$node.find('.button');

// good as well
$(e.target).find('.button');
```

Avoid too general selectors even as parts of your selection chain:

```javascript
// bad, too general
$node.find('div').find('.section');
```
#### Naming

Use names starting with ``$`` for:
  * variables that hold references to jQuery collections
  * functions that return jQuery collections

#### Good Practices

Cache your collections:

```javascript
var $this = $(this);

// reuse $this here
```

Chain your code!
Use meaningful indentation.

You can use ``.end()`` to get back to a previous selection.

```javascript
// Good:
this.$node
    .find(className)
        .on('click', handler)
    .end()
    .find(somethingElse)
        .addClass(mod)
    .end()
    .trigger('allDone');
```

#### jQuery Built-In Helpers

See the full list:
http://api.jquery.com/category/utilities/

Those are there for a reason.
Use them, unless there is a better alternative available because of our nice browser requirements.

### Twitter Flight

Use key names that start with ``$`` for all ``defaultAttr`` properties that represent selectors.
Align them nicely, too.

```javascript
this.defaultAttrs({
    $container: '.dm-page-edit-table__container',
    $table:     '.dm-page-edit-table__table',
    $row:       '.dm-page-edit-table__row',

    flag: true
});
```

Try to use only single-level methods in your components — unless you absolutely have to do otherwise.

Be careful playing with ``this`` inside components; rely on default bindings provided by the framework unless you really undestand the difference between constructor and instance properties in your specific case.


### Modularization

Simplified CommonJS wrapping. Full path should be used in most cases:

```javascript
define(function(require) {
    require('css!pages/generalSettings/generalSettings.css');
    
    var defineComponent = require('defineComponent'),
        dynamicPopup = require('ui/dynamicPopup/dynamicPopup'),
        SampleTable = require('pages/createNewTable/blocks/sampleTable/sampleTable'),
        generalSettingsTemplate = require('text!pages/generalSettings/generalSettings.html');
});
```

Relative require is also possible within the same component or its blocks like this:

```javascript
require('css!./generalSettings.css');
```

### MVC

MVC is not used, but component-based approach. Components may have models. Those have to be defined in a special ```models``` folder within the component.

## HTML

### Doctype
Use HTML5 doctype for all your documents.

### HTML5 elements
Use of HTML5 new tags is **not recommended**.
CSS classnames is how you add semantics to your markup.

### Indentation and Multiline

Use 4 spaces to indent each nested block.
Use 2 spaces for second-level indentation.
Use extra line breaks for tags with many attributes.
For long class attributes, use line breaks to separate class names, this is supported by all browsers.

```html
<!-- This is actually OK -->
<div data-id="my"
  class="dm-section
    dm-section_size_big
    dm-page__section">
    text
</div>
```

### Additional requirements:

  * Lowercase tag and attribute names
  * Use double quotes for attribute values
  * Encode HTML entities in attribute values
  * Do not use **attribute minimization** (attributes without values)

Always close your tags even if they are **void elements**; this does not affect rendering but improves readability.

```html
<!-- Don't do this: -->
<p>Paragraph 1
<p>Paragraph 2 <img src="http://coolcats.com/cat23646">

<!-- Do this instead: -->
<input type="button" disabled="disabled" value="" />
<br />
```

### LESS


We use LESS to create CSS for most components, but the styleguide rules usually apply to both.

every LESS file should import ```global-variables.less``` in the beginning:

```less
@import 'global-variables.less';
```

This is the only global LESS file in the project. It is a library for variables and mixins that are used through the whole project (colors, default font specifications and other). Since it doesn't contain any styles, when compiled, ```global-variables.less``` doesn't output anything in its CSS file.

In ```@block``` variable should be defined BEM block specified for that LESS file

```less
@block: ~".dm-modal";

@{block} {
    // some styles here
}
```

Block's elements should be nested inside of block with an ```& ```. Same rule applies for modifiers. They should be nested inside of an element

```less
@{block} {
    &__element
        // some styles for element here
        &_modifier  {
           // some styles for modifier here
        }
    }
}
```

```&``` is a shortcut for BEM block or element entity, not just a string, so this is bad:

```less
@{block} {
    &__element
        &-name {
            // some styles here
        }
    }
}
```

This is how ```&``` replacing should be done:

```less
@{block} {
    &__element
        // some styles here
    }

    &__element-name {
        // some styles here
    }
}
```
Pseudoclasses are nested inside of an element or a block:

```less
&__drag {
    cursor: move;
    
    &:hover {
        color: @colourOrange1; 
    }
}
```

Component specific modifiers are included in the bottom of a LESS file as a .less-part separate file.

```less
@import (less) "../rec/rec-table.less-part";
```

#### X-responsable grid

```x-responsable-grid.less``` is a grid system with mixins used for aligning content in components and pages.  
It creates responsive designed rows and columns with ```.row()``` and ```.column()``` mixins. 
Every row has 24 columns.
See [x-responsable-grid](https://github.com/ingdir/x-responsable-grid) for more details

### CSS

##### Prefix use

If necessary for a CSS property, use a prefix:

```css
 -webkit-box-shadow: 3px 3px 5px 6px #ccc;
 -moz-box-shadow: 3px 3px 5px 6px #ccc;
 box-shadow: 3px 3px 5px 6px #ccc;
 ```
 
 However, some properties don't need prefixes with newer versions of browsers. 

#### Indentation

4 spaces indentation inside a rule block, curly brackets similar to JS:

```css
.dm-block {
    position: relative;
    top: 5px;
    border: solid 1px #000;
}
```

#### Inline CSS
We do not use ``style="..."`` attribute with our HTML tags and avoid working it from JavaScript.
If you need a small CSS adjustment, create a BEM modifier for your block/element.

There are some legitimate cases when you need that property; you almost never see them in real life, and you don't want to.

#### Naming

We follow BEM methodology and naming conventions.
Because of that, 90% of our CSS should use a single class or a combination of 2 classes as a selector.

See [BEM Cheatsheet](https://gist.github.com/ingdir/0b211b9253c376f9cfa5) for more info on BEM and CSS naming conventions we use.

Non-BEM class names are allowed only in 3rd party code.

#### Order Of Properties Inside A Rule

We try to group all properties into 7 categories, in this order:

  * font
  * positioning
  * display
  * sizes
  * table/list specific
  * text-related
  * color, backgrounds, effects
  
Inside each rule, groups of properties are separated by 1 blank line.
Full list of properties grouped in this order:

```css
.dm-block {
    font: bold 1em/1.4em Arial, Helvetica, sans-serif;
    font-family: Arial, sans-serif;
    font-size: 1em;
    font-weight: bold;
    font-style: italic;
    font-variant: small-caps;
    line-height: 1;

    position: static;
    z-index: 0;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    display: block;
    visibility: hidden;
    float: none;
    clear: none;
    overflow: hidden;
    clip: rect(0 0 0 0);
    zoom: 1;

    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: content-box;
    width: auto;
    min-width: 0;
    max-width: 0;
    height: auto;
    min-height: 0;
    max-height: 0;
    margin: 0;
    padding: 0;

    table-layout: fixed;
    empty-cells: show;
    border-spacing: 0;
    border-collapse: collapse;
    list-style: none;

    content: "";
    counter-reset: chapter 0;
    counter-increment: chapter;
    cursor: default;
    text-align: left;
    vertical-align: top;
    white-space: normal;
    text-decoration: none;
    text-indent: 1;
    text-transform: uppercase;
    letter-spacing: 1;
    word-spacing: normal;

    opacity: 1;
    color: red;
    border: 1px solid red;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    border-radius: 3px;
    outline: 1px solid red;
    background: #fff no-repeat 0 0 url(../i/bg.png);
    box-shadow: ...;
    text-shadow: ...;

    -webkit-transition: -webkit-transform 150ms linear;
    transition: transform 150ms linear;
}
```

#### Support

Styles should support version of Firefox that is one year old and last two versions of Chrome. IE and Safari are NOT supported.
Yandex browser and Opera are also supported.

## File Structure

### Pages file structure

Every ```pages``` folder has its name camel cased. ```tableContent```
Inside of every folder there is one LESS, one HTML and one JavaScript file for the page.

### Blocks file structure
#### Local blocks
Every ```blocks``` folder contains one LESS, one Javascript and one or more HTML files. 
Naming for ```blocks``` files is camel casing, except in case if there is more than one HTML file. 
In that case, first HTML should be camel cased (as a block name) and every additional one has an additional BEM element suffix:
```dynamicRule.html``` is the first HTML, the second one is ```dynamicRule__condition.html```.
Additionally, it may have a ```blocks``` folder that contains local subblocks that are not used anywhere else except on that page.
If there's a requirement for a page's block to be used on multiple pages, it can be converted to a reusable block. 

#### Reusable blocks
These blocks can't contain subblocks. Name should be a BEM block name for that component, but without the prefix, no camel casing.

### Models file structure

Models are located inside of ```models``` folder. 




