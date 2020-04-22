---
layout: post
title: Web Application using Go
gh-repo: rishikeshwar/Go-Basic-WebService
gh-badge: [star, fork, follow]
tags: [Go]
comments: true
---

The Intent of this Application is to host a Basic Web Service, reposponding to a couple of static URL paths and how the requests are handled accordingly.

It is a start to learn how Golang works and how a simple web application can be made to work.

## **Lets get Started**

### The Application itself in a bigger picture

Below is a pictorial representaion of  **Model << >> View << >> Controller**  in action.


![MVC](/img/model-view-controller.png){: .center-block :}


<b>Model: </b> The model can be viewed as pure application level data. There is no logic or processing that is present at this level.

<b>View: </b> This View is the place which is presented to the user. The job of View is to display the content without the understanding of what it means or what the user can do with it.

<b>Controller: </b> The Controller works between the View and the Model. It listens to events and responds by executing certain section of code which are triggered by the View (or any other external event). Since the view and the model are connected through a notification mechanism, the result of this action is then automatically reflected in the view.

 [Reference](https://www.geeksforgeeks.org/mvc-design-pattern/)



Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .center-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
