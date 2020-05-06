---
layout: post
title:  "Rust’s Futures Explained"
date:   2020-05-07 00:27:03 +0800
categories: rust
---
Imaging we are going to write an application for a call center to answer some emails and phone calls daily. For sake of simplicity we only answer 5 emails and phone calls daily. Let’s start.

<h3>Using threads</h3>

To run our call center we can hire two persons to answer the phones and responding the email **parallelly**. In this case our call center application would be something like this:

{% highlight rust %}
use std::{thread, time::Duration};

fn responding_emails() {
    for i in 1..6 {
        println!("{:?}: Responding email: {}", thread::current().id(), i);
        // sleep for 10 milisecons
        thread::sleep(Duration::from_millis(10));
    }
}

fn answering_phones() {
    for i in 1..6 {
        println!("{:?}: Answering phone: {}", thread::current().id(), i);
        // sleep for 20 milisecons. People talks more!
        thread::sleep(Duration::from_millis(20));
    }
}

fn main() {
    println!("{:?}: Our call center is running...", thread::current().id());

    let handle_1 = thread::spawn(|| { responding_emails(); });
    let handle_2 = thread::spawn(|| { answering_phones(); });

    // Waits for the associated threads to finish.
    handle_1.join().unwrap();
    handle_2.join().unwrap();
}
{% endhighlight %}


If you run this code you might see something like this:

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

Awesome! We have implemented our call center application. As you can see two jobs are done simultaneously. With hiring two persons our call center is running well.

In this scenario, each person is similar to an **OS thread**. When you run the application two worker threads will run in background and start running their **jobs**. As you can see we have three threads in our application. *ThreadId(1)* is main thread, *ThreadId(2)* is for Responding emails and *ThreadId(3)* for answering phones.However using OS threads has some disadvantages:

- They are **expensive**: You need to hire two persons and pay them daily.
- They are **Costly**: You need to allocate two desks, phone lines, … .


<h3>Using Futures</h3>

You might think about doing small **tasks** by yourself instead of hiring two persons. In this case you need to handle the task **concurrently** to not-block you. If you are answering an email, you can put your phone call on **wait**. After sending an email you can back to the pending phone call and answer that. As you can see you are an handling a **state machines**: each task has state of done or pending.

Let’s implement it with Rust’s Futures. Future in rust is a task which is going to be done in future. something similar with Promise in JavaScript, but it’s not! We discuss about it later. Here is our code with Future:

{% highlight rust %}
use std::{thread, time::Duration};

async fn responding_emails() {
    for i in 1..6 {
        println!("{:?}: Responding email: {}", thread::current().id(), i);
        tokio::time::delay_for(Duration::from_millis(10)).await;
    }
}

async fn answering_phones() {
    for i in 1..6 {
        println!("{:?}: Answering phone: {}", thread::current().id(), i);
        tokio::time::delay_for(Duration::from_millis(20)).await;
    }
}

#[tokio::main]
async fn main() {
    println!("{:?}: Our call center is running...", thread::current().id());

    let future_1 = responding_emails();
    let future_2 = answering_phones();

    futures::join!(future_1, future_2);
}
{% endhighlight %}

`async` keyword emphasizes that the function is an asynchronous function. The value returned by `async fn` is a Future. Futures are *lazy*: they do nothing until they are run. The most common way to run a Future is to `.await` it. `join!` is like `.await` but can wait for multiple futures concurrently. We changed sleep to `delay_for` which is an asynchronous analog to `std::thread::sleep`.
If you run this code you might see something like this:

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

Awesome! We have implemented our call center application in asynchronous mode. We only allocated one thread for our call center application. Now we are running our call center without hiring anyone. First we respond to an email, then later we yields or give up responding emails and response to a waited phone call. later we back to respond another email and so on.

<h3>Green threads</h3>

In Rust tasks are asynchronous **green threads**. Green thread is not an OS thread. Green thread are controlled by **runtime** rather than OS:

> Green threads are threads that are scheduled by a virtual machine (VM) instead of natively by the underlying operating system.

The definition from Wikipedia raises several points. First green threads are threads: they can be seen as lightweight processes executed concurrently and sharing the same address space. Second, they are provided by a user mode program, meaning that we do not need to hack the kernel in order to implement them!

The definition mentions a virtual machine, this is not exactly accurate . I would rather say an execution environment or more simply, a runtime.
Green thread can be executed into same OS thread or different OS thread, i.e. if you use `tokio::task::spawn_local` the code will be executed in the same OS thread, however by using `tokio::task::spawn` the code will be executed in different OS thread and this is why Futures need to implement Send trait to work with spawn function.

Calling and awaiting this function will cause the current task to yield to the Tokio runtime's scheduler, allowing other tasks to be scheduled. Eventually, the yielding task will be polled again, allowing it to execute.

<h3>Rust’s Futures vs JavaScript’s Promises</h3>

Futures in Rust are similar to Promises in JavaScripts. However there is a basic difference between them. In JavaScrip Promises are based on callback. It’s Push based model. In Rust, Futures are is Pull based. There is big advantage of using poll base model in Rust. In JavaScripts, promises automatically starts whenever you define them (JavaScript has built-in runtime), however in Rust Futures are lazy, they do not run until they are polled. you need to define a runtime and manually execute a task which can gives you more control on tasks.

<h4>References:</h4>
- [Rust’s Journey to Async/Await](https://www.youtube.com/watch?v=lJ3NC-R3gSI)
- [Rust book](https://doc.rust-lang.org/book/)
- [Asynchronous Programming in Rust](https://rust-lang.github.io/async-book/)
- [Tokio documentation](https://tokio.rs/docs/overview/Green Threads)
- [Green threads Explained](https://c9x.me/articles/gthreads/intro.html)
- [Designing futures for Rust](https://aturon.github.io/blog/2016/09/07/futures-design/)
