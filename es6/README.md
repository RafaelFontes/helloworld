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