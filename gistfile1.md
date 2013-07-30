# Deltamethod Code Style Guide

## JavaScript

### Indentation

#### Whitespace
Use 4 spaces for indentation.

#### Comments
Prefer one-line comments for explanations:
```(javascript)
i++;  // increment the "i" variable so it becomes bigger and better
```

Multiline comments should be used for JSDoc.

Use this trick to comment out parts of your code that you need to re-enable quickly, it only requires you to change 1 character:

```(javascript)

/**/
this.code().is('working');
/**/

/**
this.code().is('commented out!');
/**/

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
