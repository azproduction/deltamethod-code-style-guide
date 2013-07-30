# Deltamethod Code Style Guide

## JavaScript

### Scope

  * Use .bind(this) or closures whenever appropriate;
  * Prefer closures for small inline functions (closures are a bit faster as well)
  * Use ``var that = this;`` to keep a reference to ``this``.

### Private/Public Methods

Since we don't really use inheritance, the difference is really subtle.
For private methods and properties, use names starting with underscore, like ``_myPrivateMethod``.

### Objects and Constructors

Constructor names should begin with a capital letter:
```(javascript)
function MyConstructor() {
   // some magic
}
```

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
