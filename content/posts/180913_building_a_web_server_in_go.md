+++ 
draft = false
date = 2018-09-13T16:41:00-04:00
title = "Building a Web Server in Go"
slug = "" 
tags = []
categories = ["Go"]
thumbnail = "<no value>"
description = ""
+++

>This post was originally published on https://thenewstack.io/building-a-web-server-in-go/

Go (Golang.org) is the system programming language that provides standard HTTP protocol support in its standard library, which makes it easy for developers to build and get a web server running very quickly. Meanwhile, Go offers developers a lot of flexibility. In this post, we lay out several ways to build an HTTP web server in Go and then offer an analysis about how and why these different approaches all work perfectly in Go.

Before we get started, I assume you have basic knowledge about how to write Go code, you understand HTTP and know what a web server is. Then, we can begin with the famous “hello world” example in an HTTP web server version.

It’s better to see the effect and then explain the detail. Create a file named `http1.go` and copy and paste the following code into the file:

```go
package main

import (
	"io"
	"net/http"
)

func hello(w http.ResponseWriter, r *http.Request) {
	io.WriteString(w, "Hello world!")
}

func main() {
	http.HandleFunc("/", hello)
	http.ListenAndServe(":8000", nil)
}
```

In the terminal, execute the command `go run http1.go`, then open the browser and visit `http://localhost:8000`. You will see`Hello world!` is showing on your screen.

How did that happen? Well, any runnable package must be named `main` in Go, and here we have the main function and a hello function.

In the main function, we called the function `http.HandleFunc` from the package `net/http` to register another function to be the handle function, which is the hello function in this case. This function accepts two arguments. The first one is a `string` type pattern, which is the route you want to match and it’s the root path in the example. The second argument is a function with the signature `func(ResponseWriter, *Request)`(https://gowalker.org/net/http#HandleFunc). As you can see, our hello function has exactly the same signature, therefore we can pass it as the argument. The next line we called the `http.ListenAndServe(“:8000”, nil)` function to listen on localhost with port 8000. Ignore the `nil` argument just for now.

In the hello function, we have two arguments. One with type `http.ResponseWriter` and its corresponding response stream, which is actually an interface type. The second is `*http.Request` and its corresponding HTTP request. We don’t have to use all the arguments. Just like in the hello function, if we want the response string `Hello world!` in the browser, then we only use `http.ResponseWriter`. `io.WriteString` is a helper function to let you write a string into a given writable stream. In Go, we call it the `io.Writer` interface. It’s not the point here, so knowing it works for us is enough.

This example is a little bit more complicated:

```go
package main

import (
	"io"
	"net/http"
)

func hello(w http.ResponseWriter, r *http.Request) {
	io.WriteString(w, "Hello world!")
}

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", hello)
	http.ListenAndServe(":8000", mux)
}
```

In the example above, we don’t use the `nil` in function `http.ListenAndServe` any more. Instead, replace it with a variable with type `*ServeMux`. As you may guess, this example does exactly the same thing as the previous example but we use the variable `mux` to register the handle function instead of directly registering from the `net/http` package. What’s going on underneath?  Well, the reason you can directly register the handle function in the package level is because `net/http` has a default `*ServeMux` inside the package, where now we defined our own.

To make the example even more complicated, see the code below:

```go
package main

import (
	"io"
	"net/http"
)

func hello(w http.ResponseWriter, r *http.Request) {
	io.WriteString(w, "Hello world!")
}

var mux map[string]func(http.ResponseWriter, *http.Request)

func main() {
	server := http.Server{
		Addr:    ":8000",
		Handler: &myHandler{},
	}

	mux = make(map[string]func(http.ResponseWriter, *http.Request))
	mux["/"] = hello

	server.ListenAndServe()
}

type myHandler struct{}

func (*myHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	if h, ok := mux[r.URL.String()]; ok {
		h(w, r)
		return
	}

	io.WriteString(w, "My server: "+r.URL.String())
}
```

To confirm your guess, this time we’re doing the same thing again, which is showing the `Hello world!` string on the screen. However, we not only define the `*ServeMux`, but also the variable server with type `http.Server`. At this point, you should know why we are able to run and serve the HTTP server directly from the `net/http` package. Yes, it has a default server inside the package as well. A new thing we see here is the type `myHandler` we defined and its method `ServeHTTP`. How is it possible to define a custom type and use it in an unmodified function from a standard library in a static programming language? The truth is simple. The `Handler` is an interface and only required to implement one method whose signature is `func(w http.ResponseWriter, r *http.Request)`(https://gowalker.org/net/http#Handler) and must be named as `ServeHTTP`. The type `myHandler` has the method that the interface expects, so we are good.

You can see how our code changed but for the same purpose. Don’t forget to try the three examples yourself, then make some changes and see how the change behaves. Hope you enjoy!
