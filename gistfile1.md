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
Lines should wrap at 70..100 symbols.

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

```(javascript)
// Good and readable
function foo(nomen, count, domElement) {
    // magic here...
}

foo("aleph",12,newImage);

// Bad, too loose
function foo ( nomen, count, domElement ) {
    // magic here...
}

foo( "aleph", 12, newImage );

// Bad, feels like suffocating:
function foo(nomen,count,domElement){/*magic here*/};
foo("aleph",12,newImage);

```

