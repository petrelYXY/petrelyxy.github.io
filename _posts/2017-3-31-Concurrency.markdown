---
layout: post
title: "并发"
date:  2017-03-31 09:53:20 +0800
desc: "Always be fimiliar with these basic skills"
keywords: "Job"
categories: [Java]
tags: [Job]
icon: icon-html
---
java并发编程：  
原子变量类， 加锁机制， 内置锁机制来支持原子性：同步代码块(synchronized block)；
1. 安全性问题， 2. 活跃性问题， 3. 性能问题
竞态条件(race condition)：指设备或系统出现不恰当的执行时序，而得到不正确的结果。，可见性， 重入。
volatile， 不变性条件。  

synchronized:同步代码块在执行完成后自动释放锁。  
lock:需要手动释放锁，所以为了保证锁最终被释放，需要将互斥区放在try内，将释放锁放在finally内。 可以在一个方法中加锁，而在另一个方法中释放锁。  
读写锁：读写互斥，写写互斥，读读不互斥

  


