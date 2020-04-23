---
layout: post
title: Web Application using Go
gh-repo: rishikeshwar/Go-Basic-WebService
gh-badge: [star, fork, follow]
tags: [Go, WebApplication]
comments: true
---


The Intent of this Application is to host a Basic Web Service, reposponding to a couple of static URL paths and how the requests are handled accordingly.

It is a start to learn how Golang works and how a simple web application can be made to work.

## **Lets get Started**

### The Big Picture

Below is a pictorial representaion of  **Model << >> View << >> Controller**  in action.


![MVC](/img/model-view-controller.png){: .center-block :}


<b>Model: </b> The model can be viewed as pure application level data. There is no logic or processing that is present at this level.

<b>View: </b> This View is the place which is presented to the user. The job of View is to display the content without the understanding of what it means or what the user can do with it.

<b>Controller: </b> The Controller works between the View and the Model. It listens to events and responds by executing certain section of code which are triggered by the View (or any other external event). Since the view and the model are connected through a notification mechanism, the result of this action is then automatically reflected in the view.

 [Reference](https://www.geeksforgeeks.org/mvc-design-pattern/)


### A Broader understanding of the Application itself

It works based on exposing the port number that is defind in _main.go_ to allow and serve HTTP Requests.

The motive is to create a simple locally stored database of users with basic information such as ID, FirstName and LastName

The Application serves the get, post, put and delete requests that are defnined in the controller section. We do not have a View in this project as the intention is not to create an UI.


**/main.go:** Calling the controlers to register it and exposing the port number 3000 to Listen and Serve
{% highlight go linenos %}
package main

import (
	"net/http"

	"github.com/rishikeshwar/webservice/controllers"
)

func main() {
	controllers.RegisterControllers()
	http.ListenAndServe(":3000", nil)
}
{% endhighlight %}


The Model folder contains 1 file /models/user.go

**/models/user.go:** The below file handles requests when a new user is created, updated, requested or deleted. 

{% highlight go linenos %}
package models

import (
	"errors"
	"fmt"
)

type User struct {
	ID        int
	FirstName string
	LastName  string
}

var (
	users  []*User
	nextID = 1
)

func GetUsers() []*User {
	return users
}

func AddUser(u User) (User, error) {

	if u.ID != 0 {
		return User{}, errors.New("it is an error")
	}

	u.ID = nextID
	nextID++
	users = append(users, &u)
	return u, nil
}

func GetUserByID(id int) (User, error) {
	for _, u := range users {
		if u.ID == id {
			return *u, nil
		}
	}

	return User{}, fmt.Errorf("user with id '%v' not found", id)
}

func UpdateUser(u User) (User, error) {
	for i, candidate := range users {
		if candidate.ID == u.ID {
			users[i] = &u
			return u, nil
		}
	}

	return User{}, fmt.Errorf("user with id '%v' not found", u.ID)
}

func RemoveUserByID(id int) error {
	for i, u := range users {
		if u.ID == id {
			users = append(users[:i], users[i+1:]...)
			return nil
		}
	}

	return fmt.Errorf("user with id '%v' not found", id)
}
{% endhighlight %}


The Controllers folder contains 2 files /controllers/user.go

**/controllers/user.go:** The below file handles the get, post, put and delete requests and calls in the necessary functions defined under models 

{% highlight go linenos %}
package controllers

import (
	"encoding/json"
	"fmt"
	"net/http"
	"regexp"
	"strconv"

	"github.com/rishikeshwar/webservice/models"
)

type userController struct {
	userIDPattern *regexp.Regexp
}

func (uc userController) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path == "/users" {

		switch r.Method {
		case http.MethodGet:
			uc.getAll(w, r)
		case http.MethodPost:
			uc.post(w, r)
		default:
			w.WriteHeader(http.StatusNotImplemented)
		}

	} else {
		matches := uc.userIDPattern.FindStringSubmatch(r.URL.Path)

		if len(matches) == 0 {
			w.WriteHeader(http.StatusNotFound)
		}
		fmt.Println(matches)
		id, err := strconv.Atoi(matches[1])

		if err != nil {
			w.WriteHeader(http.StatusNotFound)
		}

		switch r.Method {
		case http.MethodGet:
			uc.get(id, w)
		case http.MethodPut:
			uc.put(id, w, r)
		case http.MethodDelete:
			uc.delete(id, w)
		default:
			w.WriteHeader(http.StatusNotImplemented)
		}

	}
}

func (uc *userController) getAll(w http.ResponseWriter, r *http.Request) {
	encodeResponseAsJson(models.GetUsers(), w)
}

func (uc *userController) get(id int, w http.ResponseWriter) {
	u, err := models.GetUserByID(id)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		return
	}
	encodeResponseAsJson(u, w)
}

func (uc *userController) post(w http.ResponseWriter, r *http.Request) {
	u, err := uc.parseRequest(r)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("could not parse the User object"))
		return
	}

	u, err = models.AddUser(u)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("not coming"))
		return
	}

	encodeResponseAsJson(u, w)
}

func (uc *userController) put(id int, w http.ResponseWriter, r *http.Request) {
	u, err := uc.parseRequest(r)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("could not parse the User object"))
		return
	}

	if id != u.ID {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("ID of submitted user must watch ID in URL"))
		return
	}

	u, err = models.UpdateUser(u)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte(err.Error()))
		return
	}

	encodeResponseAsJson(u, w)
}

func (uc *userController) delete(id int, w http.ResponseWriter) {
	err := models.RemoveUserByID(id)

	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte(err.Error()))
		return
	}

	w.WriteHeader(http.StatusOK)
}

func (uc *userController) parseRequest(r *http.Request) (models.User, error) {
	dec := json.NewDecoder(r.Body)
	var u models.User

	err := dec.Decode(&u)
	if err != nil {
		return models.User{}, err
	}
	return u, nil
}

func newUserController() *userController {
	return &userController{
		userIDPattern: regexp.MustCompile(`^/users/(\d+)/?`),
	}
}
{% endhighlight %}


**/controllers/front.go:** The below file handles the static paths that are defined and which paths to be honored

{% highlight go linenos %}
package controllers

import (
	"encoding/json"
	"io"
	"net/http"
)

func RegisterControllers() {
	uc := newUserController()

	http.Handle("/users", *uc)
	http.Handle("/users/", *uc)
}

func encodeResponseAsJson(data interface{}, w io.Writer) {
	enc := json.NewEncoder(w)
	enc.Encode(data)
}
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
