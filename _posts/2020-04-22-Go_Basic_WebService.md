---
layout: post
title: Web Application using Go
subtitle: --------------------------
gh-repo: rishikeshwar/Go-Basic-WebService
gh-badge: [star, fork, follow]
tags: [Go]
comments: true
---

The Intent of the Application to host a Basic Web Service, reposponding to a couple of static URL paths and how the requests are handled accordingly.
It is a start to learn how Golang works and handles such Web requests.

**Lets get Started**

## An Introduction to how the whole Application works

Below is a pictorial _representaion_ *of* **how** the  **Model << >> View << >> Controller**  work hand in hand.

![MVC](/img/model-view-controller.png){: .center-block :}

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
