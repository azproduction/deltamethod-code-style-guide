# Deltamethod Code Style Guide

## JavaScript

### Indentation and Whitespace

#### Tabs or Spaces?

Until we hire Lea Verou, it's all spaces, bro.

Use 4 spaces for indentation.

#### Comments

Prefer one-line comments for explanations:
  * Same-line comments are: 2 spaces, slash, slash, space, comment text
  * Previous-line comments use the same indentation as the line they precede

```(javascript)
i++;  // increment "i" so it becomes bigger and better

if (i === 42) {
    // notify all the good people
    sendMessage("We've got it!");
}

```

Multiline comments should be used for JSDoc mostly, or for this trick when you need to comment and un-comment parts of your code frequently (they differ by 1 character):

```(javascript)
/**/
this.code().is('working');
/**/

/**
this.code().is('commented out!');
/**/
```

#### Good Practices

**Always comment complex parts of your code**.
**If the code is complicated enough, a pull request can be rejected based on that.**

#### Brackets

Whitespace around brackets **collapses**:

```(javascript)
// yes
banana(grape(apple(12), "red"), quantum);

// no!
banana ( grape ( apple( 12 ), "red" ) ), quantum );
```

#### Operations
Indent arithmetical operations and logical conditions with spaces:

```(javascript)
// good
var q = a ? 12 : "none";

// bad
var q=a?12:"none";
```

#### Line wraps
Lines should wrap at 80 symbols or less.

### Naming Things

Use lowercase for ordinary variables, camelCase for names that consist of more that one word.
**Do not use** underscore_as_separator_for_variable_names.

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
```(javascript)
function MyConstructor() {
   // some magic
}
```

#### Singletons
Whenever you need a singleton, use anonymous function execution as it allows for some private variables:

```(javascript)
var single = (function() {
    var privateVar = 42;
    
    return {
        publicMethod: function() {
            alert('Answer to life and everything is: ' + privateVar);
        }
    }
})();
```

### Functions

#### Declaration and Call
```(javascript)
// Good and readable
function foo(nomen, count, domElement) {
    // magic here...
}

foo("aleph", 12, new Image);

// Bad, too loose
function foo ( nomen, count, domElement ) {
    // magic here...
}

foo( "aleph", 12, new Image );

// Bad, feels like suffocating:
function foo(nomen,count,domElement){/*magic here*/};
foo("aleph",12,new Image);

```

#### Anonymous Functions To Be Called In-Place

```(javascript)
(function($, undefined) {
    // deliver happiness!
})(jQuery);
```

#### var Statement

Always use var statements.

Use single var declaration:
```(javascript)
function myFunc() {
    var i = 42,
        foo = "bar",
        emptyObj = {},
        tmp;

    // do some good
}
```

Individual variables still can be introduced later in the function body, especially when it comes to loop control vars.

### Conditons And Loops

Add one space after keywords like ``if``, ``for```, ``while``.
```(javascript)
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

```(javascript)
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

```(javascript)
var properties = {
    happiness: origin === 'Deltamethod' ? 'yes' : 'not really'
};

```

### jQuery

Avoid:
  * global selectors
  * overcomplicated selectors

#### Naming

Use names starting with ``$`` for:
  * variables that hold references to jQuery collections
  * functions that return jQuery collections

#### Good Practices

Cache your collections:

```(javascript)
var $this = $(this);

// reuse $this here

```

Chain your code!
Use meaningful indentation.
Use ``.end()`` to get back to a previous selection.

```(javascript)
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

Those are there for a reason.
Use them, unless there is a better alternative available because of our nice browser requirements.

### Twitter Flight

Use key names that start with ``$`` for all defaultAttr properties that represent selectors.
Align them nicely, too.

```(javascript)
this.defaultAttrs({
    $container: '.dm-page-edit-table__container',
    $table:     '.dm-page-edit-table__table',
    $row:       '.dm-page-edit-table__row',

    flag: true
});
```

Try to use only single-level methods in your components — unless you absolutely have to do otherwise.

Be careful playing with ``this`` inside components; rely on default bindings provided by the framework unless you really undestand the difference between constructor and instance properties in your specific case.

