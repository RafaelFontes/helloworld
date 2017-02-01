# EcmaScript 6

You may not know but JavaScript is standardized in the [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) language specification.

As of 02/01/2017, browser support for EcmaScript 6, officially known as ECMAScript 2015, is still incomplete.

I will try to concentrate here most of the features I have to know to understand upcoming libraries, modules and frameworks. 


## Scoped variable

We tend to understand that when a variable is created in JavaScript, it will last forever
```
var x = 0;
```

We dont really have to care about that, but it can be a bad thing if we don't know how it works...

![scoped-var-1.gif](https://github.com/poste9/helloworld/raw/master/recs/scoped-var-1.gif)

### Why is it showing undefined instead of the actual value ?

Well... think about it. 
You make an async call to the anonymous function.
In other words, the anonymous function will be executed after the for loop is finished.

So... the value of _i_ would be always the value of the length of the list, as you can see on the console.log's first argument.

### How would we solve that?

Basically we have to make the value of _i_ be scoped to the anonymous function.

```
var list = [ 0, 1, 2 ];

for( var i = 0; i < list.length; ++i )
{
    (function(i){
        setTimeout( 
            function(){ 
                console.log( i, list[i] );
            }, 
            i * 1000
        );
    })( i );
}
```
That way, the value of _i_ on the anonymous function comes from the parameter of his parent function instead of the one declared on the _for loop_.

EcmaScript 2015 solved this problem with a new keyword called [***let***](http://es6-features.org/#BlockScopedVariables)

Variables created with the ***let*** keyword will be automatically scoped appropriately.

The same block of code would look something like:

```
var list = [ 0, 1, 2 ];

for( let i = 0; i < list.length; ++i )
{
    setTimeout( 
        function() { 
            console.log( i, list[i] );
        }, 
        i * 1000
    );
}
```
Better, right?

## Arrow function () => 

At first, it's just another way to express a function


#### Structure:

> parameters => body

arrow function with a single parameter doesn't need to be around brackets
```
x => 
```

with more than one parameter would be

```
( x, y ) => 
```

arrow function with just a return value, the body doesn't need to be around brackets and there's no need for the return statement.

```
x => x
```

the last statement would be _translated_ to 

```
function ( x ) {
    return x;
}
```

if you need more than an expression on your body, you need to surround it with brackets and place a return function, if needed.

```
 x => { 
     if ( x > 0 ) {
        x += 2;
     }
     return x * x; 
}

``` 
the last statement would be _translated_ to 

```
function ( x ) {
    if ( x > 0 ) {
        x += 2;
    }

    return x * x;
}
```

Before es6:

```
function sum ( a, b )
{
    return a + b;
} 

function square( x )
{
    return x * x;
}

function pythagoras( cat1, cat2 )
{
    var a = square( cat1 );
    var b = square( cat2 );

    return Math.sqrt( sum( a, b ) );
}
```

After es6:

```
sum = ( a, b ) => a + b;

square = x => x * x;

pythagoras = ( cat1, cat2 ) => {
    let a = square( cat1 );
    let b = square( cat2 );

    return Math.sqrt( sum( a,b ) );
}
``` 

And now, my favorite change...

### Lexical this

Every experienced JavaScript programmer had to deal with the fact that `this` is not always what you think it is.

Let me show you an example.

```
function MyButton()
{
    this.render = function() {
        this.tooltip = "Just Click The Button";

        this.element = document.createElement( "button" );
        this.element.innerText = "Click me";

        this.element.onclick = function() {
            console.log( this.tooltip );
        }

        document.body.appendChild( this.element );
    }

    return this;
}

var button = new MyButton();
    button.render();
```

This peace of code will add a button to a body of a html document.

The button element created will wait for a click event to trigger and will log to the console `this.tooltip`

The problem is... we get `undefined` logged. Why?

Well.. when the function assigned to the event onclick of the element is called, the variable `this`, is pointing to the element that triggered the event. 

And obviously it **was not** our `MyButton` instance, it was the `HTMLButtonElement` itself. 


We usually resolve this issue saving what we need to a variable, like so:

```
var _this = this;
this.element.onclick = function() {
    console.log( _this.tooltip );
}
```

Another way would be to bind the function, like so:

```
this.element.onclick = function() {
    console.log( _this.tooltip );
}.bind( this );
```
_A bit weird for beginners I think. Anyway...._ 

ES6 brought to us another way to solve this with arrow functions.

The same block of code could look like:

```
this.element.onclick = () => { 
    console.log( this.tooltip ) 
}
```

Just like that.


### Extended Parameter Handling

Every now and then we like to use the special variable inside functions named `arguments`.

It contains an object containing all the parameters used to call the function.

And looks like some people was not liking this approach and decided to remove it from the arrow functions.

Yes, there's no arguments inside an arrow function. If you try to use it, you will even get an exception thrown.

#### What now?

Well... Now, you can actually name your special arguments variable, and even make it better, using the **Rest Parameter**.

```
let sumAll = (...args) => {
    return args.reduce( (prev, current) => 
        { 
            return prev + current; 
        } 
    );
}
```

As I understand the `reduce` function is not well known, I will place a link to the 2011's specification of Ecma about it.

[Array.prototype.reduce(callbackfn[,initialValue])](http://www.ecma-international.org/ecma-262/5.1/#sec-15.4.4.21)

You can specify the `Rest Parameter` even after other parameters.

```
let putOrTake = ( type, ... what ) =>
{
    return what.reduce( (prev, curr) => 
    {
        return (type == "put") ? prev + curr : prev - curr;
    });
}
```

You just need to be aware that a Rest Parameter must be the last of a function.

#### Spread Operator

This one is kind of hard to figure out, but it is such a usefull thing to have.

Anytime you need to spread a iterable collection with JavaScript was such a pain... but now we have this thing called `Spread Operator` that makes it very easy for us to make this kind of job.

Let me show you a simple example:

```
var str = "foo";
console.log( str.split("") ); // [ "f", "o", "o" ]
```
becomes
```
var str = "foo";
console.log( [...foo] ); // [ "f", "o", "o" ]
```

Or just even to concat arrays...

```
var params = [ 1, 2, "name" ];
var params2 = [ 4, 5, "other" ];

console.log( params.concat( params2 ) ) // [1, 2, "name", 4, 5, "other"]
```

becomes

```
var params = [ 1, 2, "name" ];
var params2 = [ 4, 5, "other" ];

console.log( [...params, ...params2] ) // [1, 2, "name", 4, 5, "other"]
```

## String Interpolation

This one came to give us quality of life... so good!

Every time we need to format a string with several parameters just to display a message to the user is such an ugly messy peace of code... 

Something like:

```
var userData = { name: "Mary", gender : "F" };
var greetings = { "F" : "Ms.", "M" : "Mr." }; 

console.log( "Hello " + greetings[userData.gender] + " " + 
        userData.name + ",\n" +
        "how is your day?" );
```

> Just... blergh...

And now ...

```
var userData = { name: "John", gender : "M" };
var greetings = { "F" : "Ms.", "M" : "Mr." }; 

console.log(
`Hello ${greetings[userData.gender]},
how is your day?`);
```

> Do I even have to say anything?

## Assignments and Object Creation

* Property Shorthand

before:

```
var x = 10;
var y = 15;

obj = { "x" : x, "y" : y };
```

now:

```
var x = 10;
var y = 15;

obj = { x, y }
```

Basically if the name of an attribute of the object is identical of the name of the variable, you can just place the variable and everything will be ok.

* Computed Property Names

before:

```
var dynamicPropName = "Index";
var dynamicPropValue = "booya";

var router = {
    "foo" : "bar"
};

router["page" + dynamicPropName] = dynamicPropValue;

console.log( router["page" + dynamicPropName] );
```

now:
```
var dynamicPropName = "Index";
var dynamicPropValue = "booya";

var router = {
    "foo" : "bar",
    [`page${dynamicPropName}`] : dynamicPropValue
};
console.log( router[`page${dynamicPropName}`] );
```

* Method properties

before:

```
obj = {
    foo : function (param)
    {
        return "bar" + param;
    }
}
console.log(obj.foo("man"));
```
now:

```
obj = {
    foo (param)
    {
        return "bar" + param;
    }
}
console.log(obj.foo("man"));
```

* Object Variable Matching

before:

```
rect = {
    x : 10,
    y : 10,
    w : 100,
    h : 100
}

console.log( "Rightmost x position:" + (rect.x + rect.w)  );
```

now:

```
rect = {
    x : 10,
    y : 20,
    w : 100,
    h : 200
}

var { x, w } = rect;

console.log( `Rightmost x position: ${x+w}` );
```


## Promises

Promises finally became available natively. 

The purpose is to retrieve a object or just execute something after an async call.

> How we did it before:

```
function sendMessageAfterTimeout ( msg, who, timeout, onDone, onFail )
{
    if ( !msg ) 
    {
        onFail("Message can't be empty");
        return;
    }

    setTimeout( function(){
        onDone( msg + " to " + who + "!");
    }, timeout );
}

sendMessageAfterTimeout( "Hello", "Rafael", 1000, function(message){
    console.log("messageSent: " + message);
}, function(error)
{
    console.log("Error: " + error);
});
```

And how we do it now:

```
sendMessageAfterTimeout = ( msg, who, timeout ) =>
{
    return new Promise( (resolve,reject) => {

        if ( !msg ) reject("Message can't be empty");
        else
        {
            setTimeout( () => {
                resolve( `${msg} to ${who}!` );
            }, timeout );
        }
    });
}

sendMessageAfterTimeout( "Hello", "Rafael", 1000 ).then( (message)=>{
    console.log(`messageSent: ${message}`);
}).catch( (error) => { console.log(`Error:  ${error}` ) } );
```

Also we can combine several promises and wait until everysingle one is finished.

```
sendMessageAfterTimeout = ( msg, who, timeout ) =>
{
    return new Promise( (resolve,reject) => {

        if ( !msg ) reject("Message can't be empty");
        else
        {
            setTimeout( () => {
                resolve( `${msg} to ${who}!` );
            }, timeout );
        }
    });
}

Promise.all([
    sendMessageAfterTimeout("Hello", "Fallen", 1000 ),
    sendMessageAfterTimeout("Hello", "TACO", 2000 ),
    sendMessageAfterTimeout("Hello", "Fer", 3000 )
]).then( (messages) => {
    console.log("Messages sent:\n"); 
    messages.forEach( message => {
        console.log( message );
    });
 })
```

> output after 3 seconds delay: 

```
Messages sent:

Hello to Fallen!

Hello to TACO!

Hello to Fer!
```