Ember.String.interpolate
=================

Adds string interpolation to Ember.String (i.e. no more unreadable getter concatenation)

##Usage
The syntax follows a similar convention to many languages with built-in interpolation. The dollar sign ($) is used to identify a property: `$firstName`, while the dollar sign followed by curly brackets allows evaluation of any JavaScript expression. `${firstName.toUpperCase()}`.

If you want to get really crazy, it even supports variable variables. i.e. `$$propertyName` is roughly equivalent to this.get(this.get('propertyName')).
##Examples

```javascript
App.PersonController = Ember.ObjectController.extend({
    firstName: 'Bilbo',
    lastName: 'Baggins',
    // Since .interpolate() returns a computed property,
    // you can chain any of the other methods like .volatile(), .meta(), etc
    fullName: '$firstName $lastName'.interpolate().readOnly(),
    
    // Old way (many times even worse than this!)
    welcomeMessage: function () {
        return 'Welcome, ' + this.get('fullName') + '! Expressions too: ' + this.get('fullName').toUpperCase();
    }.property('fullName'),
    
    // New way!
    welcomeMessage: 'Welcome, $fullName! Expressions too: ${fullName.toUpperCase()}'.interpolate()
});
```

```handlebars
<script type="text/x-handlebars" data-template-name="person">
    <h1>{{welcomeMessage}}</h1>
 </script>
```

Output:

```html
<div>
    <h1>Welcome, Bilbo Baggins! Expressions too: BILBO BAGGINS</h1>
</div>
```

## Other Usage:

Interpolation works outside of properties as well. Just pass the context you want to use:

```javascript
var Robot = Ember.Object.extend({
    status: 'ready',
    
    alertStatusMessage: function () {
        alert('Robot status is: $status'.interpolate(this));
    }
});

```

##EXTEND_PROTOTYPES = false
If you've told Ember not to extend the native prototypes, this library will honor that as well.

In that case, you need to wrap the string inside an Ember.String:
```javascript
Ember.String('Robot status is: $status').interpolate();
```


##Features
* Inline property getter evaluation and observing/bindings!
* Inline expression evaluation

NOTE: Currently doesn't support mult-level properties, meaning for `$controller.something`, only controller is looked up. This is a bug and a fix is being tested for a release in the next day or two.

##License
MIT Licensed

