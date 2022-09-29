---
layout: post
title: "Rust’s Futures Explained"
date: 2020-05-07 00:00:00 +0000
categories: rust
---

Imaging we are going to write an application for a call center to answer some emails and phone calls daily. For the sake of simplicity, we answer 5 emails and phone calls daily. Let’s start.

### Using threads

To run our call center we can hire two people to answer the phones and respond the email **in parallel**. In this case our call center application would look something like this:

{% gist 23c7b4f6f6eae5c454d890d6289757b8 %}

If you <a href="https://play.rust-lang.org/?gist=23c7b4f6f6eae5c454d890d6289757b8&edition=2018" target="_new">run this code</a> you might see something like this:

```
ThreadId(1): Our call center is running...
ThreadId(3): Answering phone: 1
ThreadId(2): Responding email: 1
ThreadId(2): Responding email: 2
ThreadId(3): Answering phone: 2
ThreadId(2): Responding email: 3
ThreadId(2): Responding email: 4
ThreadId(3): Answering phone: 3
ThreadId(2): Responding email: 5
ThreadId(3): Answering phone: 4
ThreadId(3): Answering phone: 5
```

Awesome! We have implemented our call center application. As you can see, both jobs are performed simultaneously. With two people, our call center is running well.

In this scenario, each person is similar to an **OS thread**. When you run the application, two worker threads will run in background and start running their **jobs**. We have three threads in our application: _ThreadId(1)_ is main thread, _ThreadId(2)_ is for responding emails and _ThreadId(3)_ for answering phones. However using OS threads has some disadvantages:

- They are **expensive**: You need to hire two persons and pay them daily.
- They are **Costly**: You need to allocate two desks, phone lines, … .

### Using Futures

You might think about doing small **tasks** by yourself instead of hiring two persons. In this case you need to handle the tasks **concurrently** to ensure everything is done. If you are answering an email, you can put your phone call on **wait**. After sending an email, you can go back to the pending phone call and answer that. Each task becomes a **state machine**, which lets you pause a task, and later take it up again.

Let’s implement it with Rust’s Futures. A Future in Rust is a task that is going to be done in future. It sounds similar to a Promise in JavaScript, but it’s not the same thing! We will get back to that later, so in the mean-time, here is our code with Future:

{% gist 3f1bf444a56b130909c6a2de8fbb8e7d %}

If you <a href="https://play.rust-lang.org/?gist=3f1bf444a56b130909c6a2de8fbb8e7d&edition=2018" target="_new">run this code</a> you might see something like this:

```
ThreadId(1): Our call center is running...
ThreadId(1): Responding email: 1
ThreadId(1): Answering phone: 1
ThreadId(1): Responding email: 2
ThreadId(1): Answering phone: 2
ThreadId(1): Responding email: 3
ThreadId(1): Responding email: 4
ThreadId(1): Answering phone: 3
ThreadId(1): Responding email: 5
ThreadId(1): Answering phone: 4
ThreadId(1): Answering phone: 5
```

The `async` keyword emphasizes that the function is an asynchronous function. The value returned by `async fn` is a Future. Futures are _lazy_: they do nothing until they are executed. The most common way to run a Future is to `.await` it. The `join!` macro is like `.await`, but can wait for multiple futures concurrently. We used `tokio::time::sleep`, which is the asynchronous analogue to `std::thread::sleep`.

Awesome! We have implemented our call center application in asynchronous mode. We only allocated one thread for our call center application. Now we are running our call center without hiring anyone. First we respond to an email, then we yield or give up responding emails and respond to a waiting phone call. Later we go back and respond to another email and so on.

### Green threads

In Rust tasks are asynchronous **green threads**. A green thread is not an OS thread, rather a green thread is controlled by a **runtime** instead of the OS:

> Green threads are threads that are scheduled by a virtual machine (VM) instead of natively by the underlying operating system.

The definition from Wikipedia raises several points. First green threads are threads: they can be seen as lightweight processes executed concurrently and sharing the same address space. Second, they are provided by a user mode program, meaning that we do not need to hack the kernel in order to implement them!

The definition mentions a virtual machine, but this is not exactly accurate. I would rather say an execution environment, or more simply a runtime.
Green threads can be executed on one OS thread or multiple different OS threads. If you use `tokio::task::spawn_local`, the code will be executed on the same OS thread, however by using `tokio::task::spawn` the code might be executed in different OS thread. Futures need to implement the Send trait to work with the `spawn` function.

Calling and awaiting a function will cause the current task to yield to the Tokio runtime's scheduler, which allows other tasks to be scheduled. Eventually, the yielding task will be polled again, allowing it to continue executing.

### Rust’s Futures vs JavaScript’s Promises

Futures in Rust are similar to Promises in JavaScript, but there is a basic difference between them. In JavaScrip Promises are based on callbacks. It is a push based model. In Rust, Futures are poll based.

In JavaScript, promises are automatically started when you define them (JavaScript has a built-in runtime), however Futures in Rust are lazy, which means that they do not run until they are polled. You need to define a runtime and manually execute a task which can give you more control over your tasks. Executing a future in Rust doesn't need to allocate even a single byte in the heap memory which is why the Futures in Rust are so powerful than Promises in Javascript.

---

- [Rust’s Journey to Async/Await](https://www.youtube.com/watch?v=lJ3NC-R3gSI)
- [Rust book](https://doc.rust-lang.org/book/)
- [Asynchronous Programming in Rust](https://rust-lang.github.io/async-book/)
- [Tokio documentation](https://tokio.rs/docs/overview/)
- [Green threads Explained](https://c9x.me/articles/gthreads/intro.html)
- [Designing futures for Rust](https://aturon.github.io/blog/2016/09/07/futures-design/)
