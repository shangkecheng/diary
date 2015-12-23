---
layout: post
title: "Thread"
date: "2015-12-23 16:55"
---
# Thread
-------------------------
## **handle interrupt**
Thread.interrupted() 不仅仅是返回标识变量的值，而且会将标识变量的值设置为 false。因此，一旦抛出 InterruptedException 异常，标志变量将会重置。线程不再收到任何拥有者发送的中断请求。线程的所有者要求停止线程，Thread.sleep() 监测到该请求并将其删除，再抛出 InterruptedException。如果你再次调用 Thread.sleep()，就不会响应任何中断请求，也不会抛出任何异常。

```java
public static void sleep(long millis)
  throws InterruptedException {
  while (/* You still need to wait */) {
    if (Thread.interrupted()) {
      throw new InterruptedException();
    }
    // Keep waiting
  }
}
```
不能只抛出运行时异常，这种行为太不负责了。当一个线程接收一个中断请求时，不能只是将其转换成为一个 RuntimeException。我们不能将这种严峻的情况如此轻松地对待。
```java

try {
  Thread.sleep(100);
} catch (InterruptedException ex) {
  Thread.currentThread().interrupt(); // Here!
  throw new RuntimeException(ex);
}
```
