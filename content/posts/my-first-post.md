---
title: "My First Post"
date: 2018-10-24T21:20:42+11:00
draft: true
categories: [development, publishing]
tags: [code,hugo,content,static site generator]
share: false
---


In these series, I will show lessons that I noticed during my learning journey of the javaascript language. I'm trying here to highlight on each tip on a practical way rather than devling into the details. Just keep in mind that `Mozilla` documentation should be your friend and you should be familiar with it.

Well, I'd couldn't help it either. So, you can skip to your tip.

* [defineProperty](#defineProperty)
* [~indexOf](#~indexOf)
* [__proto__ and prototype](#__proto__ and prototype)
* [call vs. apply](#callvsapply)
* [Clojure](#Clojure)
* [Hoisting](#Hoisting)
* [Shadowing](#Shadowing)
* [patterns](#patterns)
* [Strict Mode](#strictMode)
* [Pormise](#Prmoise)
* [generator](#generators)
* [iterators](#iterators)
* [callback](#callbacks)
* [currying](#currying)
* [async](#async)
* [this](#this)


### <a id="defineProperty"></a> defineProperty

As the name implies, `defineProperty` defines a new property on an object.

{% highlight javascript %}

/** initialize an empty object literal */
example = {};
// ▹ {}

Object.defineProperty(example, 'saySomething', { 
    get: function() {
      return "saySomething getter!";
    },
    set: function(value) {
      return value;
    }
 });

/** Check whether the 'saySomething' property exists or not. */
example.hasOwnProperty('saySomething')
// ▹ true

example.saySomething
// ▹ 'saySomething getter!'

{% endhighlight %}

It would be really practical to come up with a scenario that all `String` objects should have a `reverse` function which obviously should reverse the each letter in the `String` object. That can simply achieved by using `defineProperty` to add the new property `customReverse` to `String.prototype`.

{% highlight javascript %}

if (!String.prototype.customReverse) {
  Object.defineProperty(String.prototype, 'customReverse', {
    value: function() {
      /** Just note that `reverse()` method here is used to reverse an `Array` */ 
      return this.split('').reverse().join('');
    }
  });
}

'ABCD...'.customReverse()
// ▹ '...DCBA'

{% endhighlight %}

### <a id="~indexOf"></a>~indexOf

`indexOf` is a case-sensitive method that returns the index within the calling `String` of the first matching `String` in its specified value. The return value is:
* `-1` if not found
* Positive integer value (including `0`) of the index in the calling `String`. Well, that makes sense since the `String` index will start from `0`.

{% highlight javascript %}

'ABCD'.indexOf('C')
// ▹ 2
'ABCD'.indexOf('Z')
// ▹ -1
'ABCD'.indexOf('A')
// ▹ 0

{% endhighlight %}

The most important note is that the return value of this method doesn't evaluate to `true` or `false`. So, it's your responsiblity to validate that. A very classical approach to achieve that is to check if the return value of the index is above `-1`.

Let's test that by using the ternary operator `?`:

{% highlight javascript %}

'ABCD'.indexOf('A') >= 0 ? "Found!" : "Oops, not found..";
// ▹ 'Found!'
'ABCD'.indexOf('Z') >= 0 ? "Found!" : "Oops, not found..";
// ▹ 'Oops, not found..'

{% endhighlight %}  

A cleaner way to get around that `>=0` syntax is to use the binary `NOT` operator `~`. This will simply make the code cleaner by returning `true` or `false` for the index values. Therefore:

{% highlight javascript %}

~'ABCD'.indexOf('A') ? "Found!" : "Oops, not found..";
// ▹ 'Found!'
~'ABCD'.indexOf('Z') ? "Found!" : "Oops, not found..";
// ▹ 'Oops, not found..'

{% endhighlight %}  

### <a id="__proto__ and prototype"></a> \__proto__ and prototype
The `prototype` property exists functions. Say for example:

{% highlight javascript %}

function dummyFunc() {
  return 'check my prototype property';
}

{% endhighlight %}


### <a id="callvsapply"></a> call vs. apply

{% highlight javascript %}

speak.apply(fatRabbit, ["Burp!"]);
// ▹ The fat rabbit says 'Burp!'
speak.call({type: "old"}, "Oh my.");
// ▹ The old rabbit says 'Oh my.'

{% endhighlight %}


### <a id="Closure"></a> Closure

Closure can be a daunting concept to get your head around it. The best way to think of it is to remember that a closure should have:
* Outer Function
* Inner Function (which is inside the outer function)
* Variables in the outer function are accessible by the inner function. Even after the outer function has returned.

{% highlight javascript %}
/** Outer Function */
function colorMyCar() {
    /** Variable in the Outer Function */
    var myCarName = "Nissan";
    /** Inner Function */
    function colorIt(colorName) {
        /** Variables in the OF is accessible here (in the IF) */
        return myCarName + " is " + colorName;
    }
    /** Return the IF */
    return colorIt;
}

var c = colorMyCar();
// ▹ undefined
c("Red!")
// ▹ 'Nissan is Red!'
// ▹ 'Nissan is Red!'
// ▹ 'Nissan is Red!'
c("Yellow!")
// ▹ 'Nissan is Yellow!'
c("Blue!")
// ▹ 'Nissan is Blue!'

{% endhighlight %}


### <a id="Hoisting"></a> Hoisting

As its name implies, hoisting is the mechansim of moving the `decleration` and `NOT initialization` of the variables and functions.

{% highlight javascript %}
/** Outer Function */
function tellAStory3() {
      console.log(varBelow); 
      varBelow = "Got hoisted!";
      var varBelow;
};
{% endhighlight %}


### <a id="Shadowing"></a> Shadowing

Shadowing can help in defining the same variable name in a different scope.


### <a id="Iterator"></a> Iterator

Iterator is an object that uses `next` to move to the next item of the sequence. Therefore, it knows its position in the sequence. One important note to remember is that you don't need to maintain the state of an iterator. This is the opposite of a `generator`.


### <a id="generator"></a> Generator

A generator uses `yield` method that can pause or resume the generator. In order to execute the generator, you should use the `next` method which will execute the generator until it reaches the `yield` line.

{% highlight javascript %}

/** Generator syntax function* */
function* getStates() {
  yield 'First';
  yield 'Second';
  yield 'Last';
}

for (let state of getStates()) {
  console.log(state);
}

// ▹ First
// ▹ Second
// ▹ Last

{% endhighlight %}

Now, let's modify the above code with a pause/resume capability.


{% highlight javascript %}

/** Generator syntax function* */
function* getStates() {
  console.log("Before First");
  yield 'First';
  console.log("After First and Before Second");
  yield 'Second';
  console.log("After Second and Before Third");
  yield 'Last';
  console.log("I'm done!");
}

let gs = getStates();

console.log("Calling next for the first time..");
console.log(gs.next());

console.log("Calling next for the second time..");
console.log(gs.next());

console.log("Calling next for the third [last] time..");
console.log(gs.next());

console.log("Calling next for the fourth time..");
console.log(gs.next());

// ▹ Calling next for the first time..
// ▹ Before First
// ▹ { value: 'First', done: false }

// ▹ Calling next for the second time..
// ▹ After First and Before Second
// ▹ { value: 'Second', done: false }

// ▹ Calling next for the third [last] time..
// ▹ After Second and Before Third
// ▹ { value: 'Last', done: false }

// ▹ Calling next for the fourth time..
// ▹ I'm done!
// ▹ { value: undefined, done: true }

{% endhighlight %}


### <a id="vallback"></a> Callback

Simply put, `Callback` is a mechanism of passing a function to another function as an argument.

{% highlight javascript %}

function greetMe() {
  console.log('Callback says: Hello there!');
}

function callingFunc(callbackFunc) {
  console.log("I'm going to call a callback!");
  callbackFunc();
}

callingFunc(greetMe);

// ▹ I'm going to call a callback!
// ▹ Callback says: Hello there!

{% endhighlight %}

I had difficulties working with Mail client in my Macbook Pro 15". While I'm really happy about the Retina high DPI resolution, I'm still can't composing a new email as the font of the new email is very small. Well, this is a trivial task if you consider using Chrome, Terminal or a code editor. However, the Accessiblity Preference comes very handy here.

{{< figure src="/images/accessiblity-zoom-enabled.png" title="Enable Zooming in the Accessiblity Preference" >}}

{{< highlight python >}}
def main():
    return pass
{{< / highlight >}}
