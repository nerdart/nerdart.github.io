---
layout: post
title:  "Learning react"
date:   2017-06-27 14:34:25
categories: react feature
tags: featured
image: /assets/article_images/train.jpg
---


# Learning React

Random notes 

That said, Redux works especially well with libraries like React and Deku because they let you describe UI as a function of state, and Redux emits state updates in response to actions.

React is fast. Apps made in React can handle complex updates and still feel quick and responsive.
React is modular. Instead of writing large, dense files of code, you can write many smaller, reusable files. React's modularity can be a beautiful solution to JavaScript's maintainability problems.
React is scalable. Large programs that display a lot of changing data are where React performs best.
React is flexible. You can use React for interesting projects that have nothing to do with making a web app. People are still figuring out React's potential. There's room to explore.
React is popular. While this reason has admittedly little to do with React's quality, the truth is that understanding React will make you more employable.
JSX is a syntax extension for JavaScript. It was written to be used with React. JSX code looks a lot like HTML.

What does "syntax extension" mean?

In this case, it means that JSX is not valid JavaScript. Web browsers can't read it!

If a JavaScript file contains JSX code, then that file will have to be compiled. That means that before the file reaches a web browser, a JSX compiler will translate any JSX into regular JavaScript.

JSX elements are treated as JavaScript expressions. They can go anywhere that JavaScript expressions can go.

That means that a JSX element can be saved in a variable, passed to a function, stored in an object or array...you name it.

There's a rule that we haven't mentioned: a JSX expression must have exactly one outermost element.

ReactDOM is the name of a JavaScript library. This library contains several React-specific methods, all of which deal with the DOM in some way or another.

This is the first argument being passed to ReactDOM.render.ReactDOM.render's first argument should be a JSX expression, and it will be rendered to the screen.
One special thing about ReactDOM.render is that it only updates DOM elements that have changed.

That means that if you render the exact same thing twice in a row, the second render will do nothing:

This is significant! Only updating the necessary DOM elements is a large part of what makes React so successful.

React accomplishes this thanks to something called the virtual DOM. Before moving on to the end of the lesson, read this article about the Virtual DOM.

Here's a rule that you need to know: you can not inject an if statement
into a JSX expression.


A key is a JSX attribute. The attribute's name is key. The attribute's value should be something unique, similar to an id attribute.

If you don't use keys when you're supposed to, React might accidentally scramble your list-items into the wrong order.
Not all lists need to have keys. A list needs keysif either of the following are true:

The list-items have memory from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
A list's order might be shuffled. For instance, a list of search results might be shuffled from one render to the next.




React Components
What's a component?
A component is a small, reusable chunk of code that is responsible for one job. That job is often to render some HTML.


{% highlight javascript %}
var React = require('react');
var ReactDOM = require('react-dom');
var MyComponentClass=React.createClass({
render: function(){
     return <h1>Hello World</h1>;

}

)};
ReactDOM.render(<myComponentClass />, document.getElementById('app'));

{% endhighlight %}

The methods returned by require('react-dom')are for interacting with the DOM. You'are already familiar with one of them: ReactDOM.render. The methods returned by require('react')don't deal with the DOM at all. They don't engage directly with anything that isn't part of React. They are for pure React purposes, such as creating components or writing JSX elements..

Here's another fact about components: every component must come from a component class.

Component class variable names must begin with capital letters!

There is only one property that you have to include in this object: a render function.
A render function is a property whose name is render, and whose value is a function. The term "render function" can refer to the entire property, or to just the function part.


A render function must have a return statement. Usually, this return statement returns a JSX expression:
{% highlight javascript %}
var React=require('react');
var ReactDOM=require('react-dom');


var owl = {
  title: "Excellent Owl",
  src: "https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-owl.jpg"
};

// Component class starts here:
var Owl=React.createClass({
  render: function(){
  return (
  <div>
  <h1>{owl.title}</h1>
  <img src={owl.src} alt={owl.title} />
  </div>
  );
  }
});
ReactDOM.render(<Owl />, document.getElementById('app'));
{% endhighlight %}


RENDER

A render function must have a returnstatement. However, that isn't all that it can have.
A render function can also be a fine place to put simple calculations that need to happen right before a component renders.
{% highlight javascript %}
var React = require('react');
var ReactDOM = require('react-dom');

var fiftyFifty = Math.random() < 0.5;

// React.createClass call begins here:
var TonightsPlan=React.createClass({
  render: function(){
  var task;
  if fiftyFifty==true ? task="Tonight I'm going out WOOO" : task="Tonight I'm going to bed WOOO";
  return <h1>{task}</h1>;
  }

});
ReactDOM.render(<TonightsPlan />, document.getElementById('app'));
{% endhighlight %}

A Component in a render function
     
Component render component
 React's specific module system comes from Node.js. More on all of that later.
ProfilePage
{% highlight javascript %}
var React = require('react');
var ReactDOM = require('react-dom');
var NavBar=require('./NavBar');

var ProfilePage = React.createClass({
render: function () {
return (
<div>
<NavBar />
<h1>All About Me!</h1>
<p>I like movies and blah blah blah blah blah</p>
<img src="https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-monkeyselfie.jpg" />
</div>
);
}
});
ReactDOM.render(<ProfilePage />, document.getElementById('app'));
{% endhighlight %}

navbar
{% highlight javascript %}
var React = require('react');

var NavBar = React.createClass({
render: function () {
var pages = ['home', 'blog', 'pics', 'bio', 'art', 'shop', 'about', 'contact'];
var navLinks = pages.map(function(page){
return (
<a href={'/' + page}>
{page}
</a>
);
});

return <nav>{navLinks}</nav>;
}
});
module.exports = NavBar;
{% endhighlight %}

component return s another component

{% highlight javascript %}
var OMG=React.createClass({
render: function(){
return <h1></h1>;

}
)};
var Crazy=React.createClass({
render: function(){

return <OMG />

}
});
{% endhighlight %}
Props
Every component has something called props.

A component's props is an object. It holds information about that component.

To see a component's props object, you use the expression this.props. Here's an example of this.props being used inside of a render function:

Here's how to make a component display passed-in information:

1 - Find the component class that is going to receive that information.
2 - Include this.props.name-of-information in that component class's render function's return statement.


{% highlight javascript %}
Var React = require('react');
var ReactDOM = require('react-dom');

var Greeting = React.createClass({
render: function () {
return <h1>Hi there, {this.props.firstName}</h1>;
}
});

// ReactDOM.render goes here:
ReactDOM.render(
<Greeting firstName='Uma' />,
document.getElementById('app')
);
{% endhighlight %}
 You may have noticed some loose usage of the words prop and props Unfortunately this is pretty inevitable.
props is the name of the object that stores passed-in information.this.props refers to that storage object. At the same time, each piece of passed-in information is also called a prop. props could refer to two pieces of passed-in information, or it could refer to the object that stores those pieces of information. 

The most common use of props is to pass information to a component, from a different component. 

{% highlight javascript %}
var React = require('react');

var Welcome = React.createClass({
  render: function () {
  if (this.props.name == 'Wolfgang Amadeus Mozart') {
  return (
  <h2>
  hello sir it is truly great to meet you here on the web
  </h2>
  );
  } else {
  return (
  <h2>
  WELCOME "2" MY WEB SITE BABYYY!!!!!
  </h2>
  );
  }
  }
});

module.exports = Welcome;
{% endhighlight %}
{% highlight javascript %}
var React = require('react');
var ReactDOM = require('react-dom');
var Welcome = require('./Welcome');

var Home = React.createClass({
  render: function () {
  return <Welcome name='Ludwig van Beethoven' />;
  }
});

ReactDOM.render(
  <Home />,
  document.getElementById('app')
);

var React = require('react');
var ReactDOM = require('react-dom');

var Greeting = React.createClass({
  render: function () {
  if (this.props.signedIn == false) {
  return <h1>GO AWAY</h1>;
  } else {
  return <h1>Hi there, {this.props.name}!</h1>;
  }
  }
});

module.exports = Greeting;
{% endhighlight %}
{% highlight javascript %}
var React = require('react');
var ReactDOM = require('react-dom');
var Greeting = require('./Greeting');

var App = React.createClass({
  render: function () {
  return (
  <div>
  <h1>
  Hullo and, "Welcome to The Newzz," "On Line!"
  </h1>
  <Greeting name="Alison" signedIn={true} />
  <article>
  Latest: where is my phone?
  </article>
  </div>
  );
  }
});

ReactDOM.render(
  <App />,
  document.getElementById('app')
);

{% endhighlight %}




































































































































































