footer: © 2015-2017 Reginald Braithwaite. [Some rights reserved](http://creativecommons.org/licenses/by-sa/4.0/).
slidenumbers: true

![original](images/homage-to-oud-and-von-doesburg.jpg)

^ https://www.flickr.com/photos/faceme/7759225036

---

# JavaScript Combinators
### "306th prime edition"

![](images/homage-to-oud-and-von-doesburg.jpg)

^ https://www.flickr.com/photos/faceme/7759225036

---

![original](images/combinators/exhibition.jpg)

^ https://www.flickr.com/photos/mariannebevis/8636118515

---

## we'll talk about
# Using combinators for decomposition and composition

![](images/combinators/exhibition.jpg)

^ https://www.flickr.com/photos/mariannebevis/8636118515

---

![original](images/combinators/avond.jpg)

^ http://www.wikiart.org/en/piet-mondrian/avond-evening-the-red-tree-1910

^ "Making responsibilities and relationships explicit"

---

## and we'll think about
# Making responsibilities and relationships explicit

![](images/combinators/avond.jpg)

^ http://www.wikiart.org/en/piet-mondrian/avond-evening-the-red-tree-1910

---

![original](images/engine-start.jpg)

^ https://www.flickr.com/photos/npobre/2601582256

---

![original](images/hanging-lamp.jpg)

^ https://www.flickr.com/photos/westher/7709055816

---

# Decomposition

![](images/hanging-lamp.jpg)

^ https://www.flickr.com/photos/westher/7709055816

---

> We *decompose* entities to make discreet responsibilities explicit

---

## a monolith

```javascript
Parse.User.logIn("user", "pass", {
  success: function (user) {
    query.find({
      success: function (users) {
        users[0].save({ key: value }, {
          success: function (user) {
            currentUser = user;
          }
        });
      }
    });
  }
});
```

^ Adapted from http://blog.parse.com/learn/engineering/whats-so-great-about-javascript-promises/

---

## decomposition by extracting functions

```javascript
let assignCurrentUser = (user) => { currentUser = user; };

let saveFirstUser = (users) =>
  users[0].save({ key: value }, {
    success: assignCurrentUser
  });

let logUserIn = (user) =>
  query.find({
    success: saveFirstUser
  });

Parse.User.logIn("user", "pass", { success: logUserIn });
```

^ Adapted from http://blog.parse.com/learn/engineering/whats-so-great-about-javascript-promises/

---

![original](images/rietveld-schroederhuis.jpg)

^ https://www.flickr.com/photos/mariannebevis/8646014265

---

# Composition

![](images/rietveld-schroederhuis.jpg)

^ https://www.flickr.com/photos/mariannebevis/8646014265

---

> We *compose* entities to make the relationships between them explicit

---

## promises explicitly compose asynchronous functions

```javascript
let findUser = (user) => query.find();

let saveFirstUser = (user) => users[0].save({ key: value });

let assignCurrentUser = (user) => { currentUser = user; };

Parse.User.logIn("user", "pass")
  .then(findUser)
  .then(saveFirstUser)
  .then(assignCurrentUser);
```

^ Adapted from http://blog.parse.com/learn/engineering/whats-so-great-about-javascript-promises/

---

![original](images/red-blue-chair.jpg)

^ https://www.flickr.com/photos/jforth/4467184640

---

# Decomposition is about entities

![](images/red-blue-chair.jpg)

^ https://www.flickr.com/photos/jforth/4467184640

---

# Composition is about relationships

![](images/red-blue-chair.jpg)

^ https://www.flickr.com/photos/jforth/4467184640

---

> Back to decomposition

---

## extracting named functions

# The most obvious form of decomposition

---

![original](images/watch-movement.jpg)

^ https://www.flickr.com/photos/guysie/3325635275

---

# Extracting functions works from the *inside-out*

![](images/watch-movement.jpg)

^ https://www.flickr.com/photos/guysie/3325635275

---

## extracting named functions

# Decomposition of *Implementation*

---

> Let's look at something else

---

![original](https://c2.staticflickr.com/8/7743/17185509447_1166acf54b_h.jpg)

^ https://www.flickr.com/photos/130973380@N08/17185509447

^ "pluck"

---

## `pluck`: "A convenient version of what is perhaps the most common use-case for `map`, extracting a list of property values."

![](https://c2.staticflickr.com/8/7743/17185509447_1166acf54b_h.jpg)

^ https://www.flickr.com/photos/130973380@N08/17185509447

^ underscorejs.org

---

```javascript
let pluck = (collection, property) =>
  collection.map( (obj) => obj[property] );

var deStijl = [
  {name: 'Theo van Doesburg', occupation: 'theorist'},
  {name: 'Piet Mondriaan', occupation: 'painter'},
  {name: 'Gerrit Rietveld', occupation: 'architect'}];

pluck(deStijl, 'name')
  //=> ["Theo van Doesburg", "Piet Mondriaan", "Gerrit Rietveld"]
```

---

## functions have interfaces

# `pluck`'s *interface* has two parts: The collection, and the property

---

## manually decomposing `pluck`'s interface

```javascript
var deStijl = [
  {name: 'Theo van Doesburg', occupation: 'theorist'},
  {name: 'Piet Mondriaan', occupation: 'painter'},
  {name: 'Gerrit Rietveld', occupation: 'architect'}];

let pluckFrom = (collection) =>
  (property) => pluck(collection, property);

pluckFrom(deStijl)('name')
  //=> ["Theo van Doesburg", "Piet Mondriaan", "Gerrit Rietveld"]
```

---

## manually decomposing `pluck`'s interface

```javascript
let pluckWith = (property) =>
  (collection) => pluck(collection, property);

pluckWith('name')(deStijl)
  //=> ["Theo van Doesburg", "Piet Mondriaan", "Gerrit Rietveld"]
```

---

> `pluckFrom` and `pluckWith` partially apply `pluck`

---

![origina](images/two-blue-doors.jpg)

^ https://www.flickr.com/photos/16210667@N02/18067981830

---

# Partial application decomposes functions from the *outside-in*

![](images/two-blue-doors.jpg)

^ https://www.flickr.com/photos/16210667@N02/18067981830

---

## partial application

# Decomposition of *Interface*

---

![original](images/lenses.jpg)

^ https://www.flickr.com/photos/vauvau/23067022785

---

# Decorators

![](images/lenses.jpg)

^ https://www.flickr.com/photos/vauvau/23067022785

---

> A *decorator* is a higher-order function that takes a function, and returns another function that adds to or modifies its argument's behaviour.

---

## partial application decomposes a function from the outside-in

```javascript
let pluck = (collection, property) =>
  collection.map( (obj) => obj[property] );

// decomposes into:

let pluckFrom = (collection) =>
  (property) => pluck(collection, property);
```

^ http://raganwald.com/2013/03/07/currying-and-partial-application.html

---

## extract closed-over binding

```javascript
let pluckFrom = (collection) =>
  (property) => pluck(collection, property);

// extract `pluck`:

let ____ = (pluck, collection) =>
  (property) => pluck(collection, property);

// rename:

let leftApply = (fn, a) =>
  (b) => fn(a, b);
```

---

## using `leftApply`

```javascript
let pluckFrom = (collection) =>
  leftApply(pluck, collection);

// again:

let pluckFrom = leftApply(leftApply, pluck);

// again

let pluckFrom = leftApply(leftApply, leftApply)(pluck);
```

---

![](https://farm9.staticflickr.com/8490/8170948596_372d1960a5_o_d.jpg)

^ https://www.flickr.com/photos/saramarlowe/8170948596

---

# hmmmm

## What is `leftApply(leftApply, leftApply)`?

![](https://farm9.staticflickr.com/8490/8170948596_372d1960a5_o_d.jpg)

^ https://www.flickr.com/photos/saramarlowe/8170948596

---

## back to partial application

```javascript
let rightApply = (fn, b) =>
  (a) => fn(a, b);

let pluckWith = (property) => (pluck, property);

// again:

let pluckWith = leftApply(rightApply, pluck);

// again:

let pluckWith = leftApply(leftApply, rightApply)(pluck);
```

---

## combinators that decompose functions

```javascript
// leftApply(leftApply, leftApply)

let Istarstar = (a) => (b) => (c) => a(b, c);

let pluckFrom = Istarstar(pluck);

// leftApply(leftApply, rightApply)

let C = (a) => (b) => (c) => a(c, b)

let pluckWith = C(pluck);
```

^ "cardinal"

---

## more decomposition with combinators

```javascript
let get = (object, property) =>
  object[property];

get({name: 'Gerrit Rietveld'}, 'name')
  //=> Gerrit Rietveld

let getWith = C(get);

let nameOf = getWith('name');

nameOf({name: 'Gerrit Rietveld'})
  //=> Gerrit Rietveld
```

---

![original](images/combinators/cafe-de-unie.jpg)

^ http://www.panoramio.com/photo/82281274

---

# `get` is a function taking two arguments

# `getWith` is a decomposition of `get` that names one part

# `nameOf` is a decomposition of `get` that names and specifies one part

![](images/combinators/cafe-de-unie.jpg)

^ http://www.panoramio.com/photo/82281274

---

## last one

```javascript
let map = (collection, fn) =>
  collection.map(fn);

let mapWith = C(map);

let namesOf = mapWith(nameOf);
```

---

![original](images/combinators/dalbéra.jpg)

^ https://www.flickr.com/photos/dalbera/20059027984

---

# `map` is a higher-order function

# `mapWith` is a decomposition of `map` that names one part

# `namesOf` is a decomposition of `map` that names and specifies one part

![](images/combinators/dalbéra.jpg)

^ https://www.flickr.com/photos/dalbera/20059027984

---

> Back to composition

^ before we go, now you know what to say when somebody asks "What good is currying?" or "What is the practical purpose of partial application?" It's to decompose functions by responsibility.

---

![](https://farm9.staticflickr.com/8376/8487304182_01e9c118c8_o_d.jpg)

^ https://www.flickr.com/photos/ctaweb/8487304182

^ "Simple Composition"

---

# Simple Composition

![](https://farm9.staticflickr.com/8376/8487304182_01e9c118c8_o_d.jpg)

^ https://www.flickr.com/photos/ctaweb/8487304182

---

> `let compose = (a, b) =>`
> `  (c) => a(b(c));`

^ 'compose'. `B = (a) => (b) => (c) => a(b(c))`

---

## `compose` in action

```javascript
var deStijl = [
  {name: 'Theo van Doesburg', occupation: 'theorist'},
  {name: 'Piet Mondriaan', occupation: 'painter'},
  {name: 'Gerrit Rietveld', occupation: 'architect'}];

let pluckWith = compose(mapWith, getWith);

let namesOf = pluckWith('name');

namesOf(deStijl)
  //=> ["Theo van Doesburg","Piet Mondriaan","Gerrit Rietveld"]
```

---

>  `pluckWith = compose(mapWith, getWith);`

^ `compose` wires the output of one function into the input of another

---

![original](images/ode-to-mondriaan.jpg)

^ https://www.flickr.com/photos/72283508@N00/2444938101

^ "We compose entities to make the relationships between them explicit"

^ and now we know how to answer that question, too!

---

## reminder:
# *We compose entities to make the relationships between them explicit*

![](images/ode-to-mondriaan.jpg)

^ https://www.flickr.com/photos/72283508@N00/2444938101

---

> More Composition

---

```javascript
let mix = (...ingredients) =>
  console.log('mixing', ...ingredients);

let bake = () => console.log('baking');

let cool = () => console.log('cooling');

let makeBread = (...ingredients) => {
  mix(...ingredients);
  bake();
  cool();
}
```

---

## composition with `before`

```javascript
let before = (fn, decoration) =>
  (...args) => {
    decoration(...args);
    return fn(...args);
  };

let bakeBread = before(bake, mix);

let makeBread = (...ingredients) => {
  bakeBread();
  cool();
}
```

---

> `before` makes the time relationship between two functions explicit

---

## composition with `after`

```javascript
let after = (fn, decoration) =>
  (...args) => {
    let returnValue = fn(...args);
    decoration(...args);
    return returnValue;
  };

let bakeBread = before(bake, mix);

let makeBread = after(bakeBread, cool);
```

---

> `after` also makes the time relationship between two functions explicit

---

# decomposing `before`

```javascript
let beforeWith = (decoration) =>
  rightApply(before, decoration);

let mixBefore = beforeWith(mix);

let bakeBread = mixBefore(bake);
```

---

# decomposing `after`

```javascript
let afterWith = (decoration) =>
  rightApply(after, decoration);

let coolAfter = afterWith(after);

let makeBread = coolAfter(bakeBread);
```

---

> `beforeWith` and `afterWith` are combinators that turn functions into decorators that compose behaviour

---

![original](images/hoover-dam-bypass.jpg)

^ "These are not the whole story, by far."

---

![original](images/colours.jpg)

^ https://www.flickr.com/photos/lightplay/4727131891

---

# JavaScript invocations are coloured

![](images/colours.jpg)

^ https://www.flickr.com/photos/lightplay/4727131891

^ http://raganwald.com/2015/03/12/symmetry.html

---
## coloured decorators
```javascript
let before = (fn, decoration) =>
  function (...args) {
    decoration.apply(this, args);
    return fn.apply(this, args);
  };

let after = (fn, decoration) =>
  function (...args) {
    let returnValue = fn.apply(this, args);
    decoration.apply(this, args);
    return returnValue;
  };
```

---

> Why coloured decorators matter

---

## bread, revisited
```javascript
class Bread {
  constructor (...ingredients) {
    this.ingredients = ingredients;
  }

  mix () {
    console.log('mixing', ...this.ingredients);
  };

  bake () { console.log('baking'); }

  cool () { console.log('cooling'); }
}
```

---

## bread, revisited
```javascript
class Bread {

  // ...

  make () {
    this.mix();
    this.bake();
    this.cool();
  }
}
```

---

> Classes can be decorated too

---

## `decorateMethodWith`

```javascript
const decorateMethodWith = (decorator, ...methodNames) =>
  (clazz) => {
    for (let methodName of methodNames) {
      const method = clazz.prototype[methodName];

      Object.defineProperty(clazz.prototype, methodName, {
        value: decorator(method),
        writable: true
      });
    }
    return clazz;
  };
```

---

## `beforeAll` and `afterAll`

```javascript
const beforeAll = (decorator, ...methodNames) =>
  decorateMethodWith((method) => before(method, decorator), ...methodNames),

      afterAll = (decorator, ...methodNames) =>
  decorateMethodWith((method) => after(method, decorator), ...methodNames);
```

---

## better bread

```javascript
let invoke = (methodName) => function (...args) {
  return this[methodName](...args);
}

let BetterBread =
  beforeAll(invoke('mix'), 'make')(
    afterAll(invoke('cool'), 'make')(
      class {
        // ...

        make () {
          this.bake();
        }
      }
    )
  );
```

---

![original](https://farm7.staticflickr.com/6211/6210847796_ab54ea2b0b_o_d.jpg)

^ https://www.flickr.com/photos/68112440@N07/6210847796

^ "Looking Forward"

---

## Looking Forward

![](https://farm7.staticflickr.com/6211/6210847796_ab54ea2b0b_o_d.jpg)

^ https://www.flickr.com/photos/68112440@N07/6210847796

---

> ES.who-knows-when

---

## better bread with class decorator sugar

```javascript
let invoke = (methodName) => function (...args) {
  return this[methodName](...args);
}

@beforeAll(invoke('mix'), 'make')
@afterAll(invoke('cool'), 'make')
class AwesomeBread {

  // ...

  make () {
    this.bake();
  }
}
```

---

## method decorators
```javascript
let methodDecorator = (decorator) =>
  function (target, name, descriptor) {
    descriptor.value = decorator(descriptor.value);
  }


let invokeBefore = (methodName) =>
  methodDecorator( (methodBody) =>
    before(methodBody, invoke(methodName))
  );

let invokeAfter = (methodName) =>
  methodDecorator( (methodBody) =>
    after(methodBody, invoke(methodName))
  );
```

---

## better make

```javascript
class Bread {

  // ...

  @invokeBefore('mix')
  @invokeAfter('cool')
  make () {
    this.bake();
  }
}
```

---

![original](images/victory-boogie-woogie.jpg)

^ https://www.flickr.com/photos/hogeslag/3404068564

---

# What have we seen so far?

![](images/victory-boogie-woogie.jpg)

^ https://www.flickr.com/photos/hogeslag/3404068564

---

# What have we seen so far?

- Extract function
- Promise interface
- Partial application
- Extract closed-over binding

(more!)

---

# What have we seen so far?

- Simple composition
- Composition decorators
- Class decorators
- Method decorators

---

> Don't worry about the details!

---

![original](images/combinators/Simultaneous-Counter-Composition-Theo-Van-Doesburg-1800x2880.jpg)

^ http://wallpaperswiki.com/wp-content/uploads/2012/10/Simultaneous-Counter-Composition.-Theo-Van-Doesburg-728x546.jpg

^ "Decorators declutter secondary concerns"

---

## it's all the same idea
# Decomposition makes responsibilities explicit

![](images/combinators/Simultaneous-Counter-Composition-Theo-Van-Doesburg-1800x2880.jpg)

^ http://wallpaperswiki.com/wp-content/uploads/2012/10/Simultaneous-Counter-Composition.-Theo-Van-Doesburg-728x546.jpg

---

![original](images/combinators/GUGG_Composition_décentralisée.jpg)

^ https://en.wikipedia.org/wiki/Theo_van_Doesburg

^ "Composition makes relationships explicit"

---

## and it's all the same idea
# Composition makes relationships explicit

![](images/combinators/GUGG_Composition_décentralisée.jpg)

^ https://en.wikipedia.org/wiki/Theo_van_Doesburg

---

> These ideas matter

---

> There are only two hard problems in Computer Science: Cache invalidation, and naming things.

![](images/phil-karlton.jpg)

^ Phil Karlton

---

![original](https://farm4.staticflickr.com/3260/2744016741_d3ca684983_o_d.jpg)

^ https://www.flickr.com/photos/michale/2744016741

^ "Naming entities is hard because you have to figure out which entities need to be named"

---

# Naming entities is hard because you have to figure out which entities need to be named

![](https://farm4.staticflickr.com/3260/2744016741_d3ca684983_o_d.jpg)

^ https://www.flickr.com/photos/michale/2744016741

---

![original](https://farm6.staticflickr.com/5302/5572576057_86fc9b29cc_o_d.jpg)

^ https://www.flickr.com/photos/stretta/5572576057

^ "Naming relationships is hard because you have to figure out which relationships need to be named"

---

# Naming relationships is hard because you have to figure out which relationships need to be named

![](https://farm6.staticflickr.com/5302/5572576057_86fc9b29cc_o_d.jpg)

^ https://www.flickr.com/photos/stretta/5572576057

---

![original](images/constructive-lives.jpg)

^ https://www.flickr.com/photos/cristiano_betta/2970086666

---

# Combinators do not make naming easy

![](images/constructive-lives.jpg)

^ https://www.flickr.com/photos/cristiano_betta/2970086666

---

![original](https://farm3.staticflickr.com/2693/4362414729_32f57b4f6d_o_d.jpg)

^ https://www.flickr.com/photos/joao_trindade/4362414729

^ "Combinators give us a language for naming things"

---

# Combinators give us a language for naming things in code

![](https://farm3.staticflickr.com/2693/4362414729_32f57b4f6d_o_d.jpg)

^ https://www.flickr.com/photos/joao_trindade/4362414729

---

![original](https://farm2.staticflickr.com/1406/723665503_48c3a82af8_o_d.jpg)

^ https://www.flickr.com/photos/suburbanbloke/723665503

^ "Do not follow in the footsteps of the sages: Seek what they sought"

---

# Do not follow<br>in the footsteps of the sages.

![](https://farm2.staticflickr.com/1406/723665503_48c3a82af8_o_d.jpg)

^ https://www.flickr.com/photos/suburbanbloke/723665503

---

# Seek what they sought.

![](https://farm2.staticflickr.com/1406/723665503_48c3a82af8_o_d.jpg)

^ https://www.flickr.com/photos/suburbanbloke/723665503

---

# Reg Braithwaite<br>PagerDuty, Inc.
### [raganwald.com](http://raganwald.com)<br>[@raganwald](https://twitter.com/raganwald)

![right, fit](images/allonge-six.jpg)

^ https://leanpub.com/javascriptallongesix
