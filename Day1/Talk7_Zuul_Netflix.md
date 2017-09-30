# Zuul - Netflix
Talk Page: https://thestrangeloop.com/2017/zuuls-journey-to-non-blocking.html
Speaker: Arthur Gonigberg - [@agonigberg](https://twitter.com/agonigberg)
Blog: https://medium.com/netflix-techblog/zuul-2-the-netflix-journey-to-asynchronous-non-blocking-systems-45947377fb5c
Github: https://github.com/Netflix/zuul


Zuul
- Routing
- Montoring
- Security
- Resiliency
- Flexibility

# Background
Originally, Built on Tomcat, with Servlets, with Blocking Requests
- Required new instances to be set up to scale

# Industry at the time
- RXJava
- Hystrix


# Zuul 1 vs. Zuul 2
- Zuul 2 - non-blocking
- performance was the same as


# Zuul
proxies all trafic coming in to AWS
Traffic from almost every country
tens of billions of requests per day
small team to support


Hystrix
- [https://github.com/Netflix/Hystrix](https://github.com/Netflix/Hystrix)
- Open-source Netfilx framework
- Used in Zuul for Circuit breaking
  - If something takes too long, kill it and start it over


# What is non-blocking
- Need to know if a socket is ready for reads or writes without having to wait for it
  - OS provides this
  - a.k.a., Non-blocking i/o primitives
- Async programming
- event-loop pattern
  - Netty:


  Why non-blocking?
  - Performance -> Better CPU Utilizaiton
  - REsiliency -> Connection scaling

  ![https://cdn-images-1.medium.com/max/1200/0*jrG2ldEVRRJcgpkj.png](https://cdn-images-1.medium.com/max/1200/0*jrG2ldEVRRJcgpkj.png)

# Zuul 1.5
- switched Tomcat to APR connector
- created async filter chains

## New Filter Interface

Had to rewrite all filters to be async

```
Observable.of(request)
  .flatMap(msg -> applyFilterChaing(msg))
  .flatMap(msg -> writeResponse(msg))
  .doOnError(e -> writeErrorResponse(e))
  .onNextDo(...);
```

# Underlying Assumptions
- Some dependencies were Blocking
  - In Netty, you cannot block (for concurrency to work properly)
- Shared State kills you in multithreaded workflows
  - Manual digging through code

# How to choose blocking vs. non-blocking?
- Do you really need the concurrency?
  - Lots of concurrent requests?
  - Are you I/O bound like Netflix?
- Do you really need it or just trying to do new, shiny?
- Netflix saw big gains, but with high cost

# Notes
- Used `Observable` everywhere: [http://reactivex.io/documentation/observable.html](http://reactivex.io/documentation/observable.html)
-
