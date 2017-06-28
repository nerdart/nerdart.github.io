---
layout: post
title:  "React Notes"
date:   2014-08-29 14:34:25
categories: jekyll update
tags: featured
image: /assets/article_images/table.jpg
---
React Components

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object. We call this object "props".


For example, this code renders "Hello, Sara" on the page:


Composing Components

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. A button, a form, a dialog, a screen: in React apps, all those are commonly expressed as components.
Props are Read-Only

Whether you declare a component as a function or a class, it must never modify its own props. Consider this sum function:


Such functions are called "pure" because they do not attempt to change their inputs, and always return the same result for the same inputs.
{% highlight javascript %}
function add(a,b){
return a+b;
}
{% endhighlight %}
React is pretty flexible but it has a single strict rule:
All React components must act like pure functions with respect to their props.

Of course, application UIs are dynamic and change over time. In the next section, we will introduce a new concept of "state". State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.


State and Lifecycle
Consider the ticking clock example from one of the previous sections.

So far we have only learned one way to update the UI.


We call ReactDOM.render() to change the rendered output:

State is similar to props, but it is private and fully controlled by the component.

We mentioned before that components defined as classes have some additional features. Local state is exactly that: a feature available only to classes.


Adding Local State to a Class
Class components should always call the base constructor with props.
{% highlight javascript %}
class Clock extends React.Component {
render() {
return (
<div>
<h1>Hello, world!</h1>
<h2>It is {this.props.date.toLocaleTimeString()}.</h2>
</div>
);
}
}
function tick() {
ReactDOM.render(
<Clock date={new Date()} />,
document.getElementById('root')
);
}
setInterval(tick, 1000);
{% endhighlight %}
Adding Lifecycle Methods to a Class

In applications with many components, it's very important to free up resources taken by the components when they are destroyed.

We want to set up a timer whenever the Clock is rendered to the DOM for the first time. This is called "mounting" in React.

We also want to clear that timer whenever the DOM produced by the Clock is removed. This is called "unmounting" in React.


We can declare special methods on the component class to run some code when a component mounts and unmounts:
lifecycle hooks
componentDidMount()
ComponentWillMount()
The componentDidMount() hook runs after the component output has been rendered to the DOM. This is a good place to set up a timer
{% highlight javascript %}
componentDidMount(){
this.timerId=setInterval()=>this.tick(),1000);
}
{% endhighlight %}
While this.props is set up by React itself and this.state has a special meaning, you are free to add additional fields to the class manually if you need to store something that is not used for the visual output.
If you don't use something in render(), it shouldn't be in the state.

We will tear down the timer in the componentWillUnmount() lifecycle hook
{% highlight javascript %}
componentWillMount(){
clearInterval(this.timerId);
}
{% endhighlight %}
{% highlight javascript %}
class Clock extends React.Component {
constructor(props) {
super(props);
this.state = {date: new Date()};
}

componentDidMount() {
this.timerID = setInterval(
() => this.tick(),
1000
);
}

componentWillUnmount() {
clearInterval(this.timerID);
}

tick() {
this.setState({
date: new Date()
});
}

render() {
return (
<div>
<h1>Hello, world!</h1>
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
</div>
);
}
}

ReactDOM.render(
<Clock />,
document.getElementById('root')
);
{% endhighlight %}
this.state.comment="hello"; // will not work
this.setState({comment:'Hello'});
The only place where you can assign this.state is the constructor.

State Updates May Be Asynchronous
React may batch multiple setState() calls into a single update for performance.


Because this.props and this.state may be updated asynchronously, you should not rely on their values for calculating the next state

this.setState({
counter:this.state.counter + this.props.increment;
}); // wrong

this.setState(function(prevState, props){
return {
counter:prevState.counter + props.increment;
}
})

State Updates are Merged
When you call setState(), React merges the object you provide into the current state.
{% highlight javascript %}
constructor(props){
super(props);
this.state={
post=[],
comment=[];
};
}
componentDidMount(){
fetchPosts.then(response=>{
this.setState({
post:response.posts;
)};
)};

fetchComments.then(response => {
this.setState({
comments:response.comments;
)};
)};
}
{% endhighlight %}
Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn't care whether it is defined as a function or a class.


This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.
This is commonly called a "top-down" or "unidirectional" data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components "below" them in the tree.


Handling Events

Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:

React events are named using camelCase, rather than lowercase.
With JSX you pass a function as the event handler, rather than a string.

{% highlight javascript %}
class Toggle extends React.Component {
constructor(props){
super(props);
this.state={isToggleOn:true};
this.handleClick=this.handleClick.bind(this);
}
handleClick(){
this.setState(prevState =>({
isToggleOn:!prevState.isToggleOn

}));

}
render(){
return(

<button onClick = {this.handleClick} >{this.state.isToggleOn ? 'ON' : 'OFF' }</button>
);
}

}
ReactDOM.render(<Toggle />, document.getElementById('root'));
{% endhighlight %}

You have to be careful about the meaning of this in JSX callbacks. In JavaScript, class methods are not bound by default. If you forget to bind this.handleClick and pass it to onClick, this will be undefined when the function is actually called.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

{% highlight js %}

<footer class="site-footer">
 <a class="subscribe" href="{{ "/feed.xml" | prepend: site.baseurl }}"> <span class="tooltip"> <i class="fa fa-rss"></i> Subscribe!</span></a>
  <div class="inner">a
   <section class="copyright">All content copyright <a href="mailto:{{ site.email}}">{{ site.name }}</a> &copy; {{ site.time | date: '%Y' }} &bull; All rights reserved.</section>
   <section class="poweredby">Made with <a href="http://jekyllrb.com"> Jekyll</a></section>
  </div>
</footer>
{% endhighlight %}


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
