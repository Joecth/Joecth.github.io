---
layout: post
categories: Concurrency
tag: [Concurrency] 
date: 2023-05-25
---





# High Concurrency

High concurrency refers to a situation where a system encounters a large number of operation requests in a short period of time, mainly occurring in web systems with concentrated access and receiving a large number of requests (e.g., ticket grabbing on 12306; Second-Kill event). This situation leads to the system performing a large number of operations during a pretty short time, such as requests on services, apps, and database operations.

### Performance Metrics 

including: response time, throughput, queries per second (QPS), and concurrent user count.

- `Response Time`
   The time it takes for the system to respond to a request. For example, if it takes 200ms for the system to process an HTTP request, that 200ms is the system's response time. 
- `Throughput`
   The number of requests processed per unit of time. 
- `Queries Per Second (QPS)` The # of requests responded to per second. In the Internet domain, this metric is not as clearly distinguished from throughput. 
- `Concurrent User Count` The number of users who can simultaneously use the system's features normally. E.g., in an instant messaging system, the number of users online at the same time to some extent represents the system's concurrent user count.



### High Concurrency Solutions

- Use CDN for static resources to handle image file access.
- Distributed caching: Redis, Memcached, etc.
- Message queue middleware: ActiveMQ, etc., to solve the asynchronous processing capability of a large number of messages.
- Application splitting: A single project is split into multiple projects for deployment, using Dubbo to solve communication between multiple projects.
- Vertical and horizontal database splitting (sharding).
- Database read-write separation to solve large data query problems.
- Use NoSQL, such as MongoDB, in combination with MySQL.
- Establish service degradation and flow control mechanisms under high data access conditions.



In sum, high concurrency refers to a system's ability to handle a large number of concurrent requests or transactions simultaneously. It refers to the system's capability to process multiple user requests or transactions at the same time. High concurrency performance is an important measure of a system's load capacity and responsiveness. Multithreading is often used as a common technique for achieving high concurrency. By using multithreading, concurrent requests or transactions can be assigned to different threads for processing, thus enhancing the system's concurrent processing capability.



# Multithreading

A program can have multiple threads running in parallel internally. The execution of a thread can be considered as a CPU executing the program. When a program runs with multiple threads, it's as if multiple CPUs are executing the program simultaneously.

In summary, multithreading can be understood as follows: <u>Multithreading is a programming method for handling high concurrency, meaning that concurrency needs to be implemented using multiple threads,</u> e.g. 1) Stress-tested Aliyun's audio generation API with C++'s multithread library
2) Adopted multithreading & coroutine to build "Automatic Video Generation System" and "Async Web Service for Adobe Application", from my experiences.



In sum, multi-threading refers to the execution of multiple threads simultaneously within a program, where each thread can run independently and perform different tasks. The main purpose of multithreading is to improve program performance and resource utilization. By dividing tasks into multiple threads, the parallel computing capabilities of multi-core processors can be fully utilized, enabling tasks to be executed in parallel. Multithreading is commonly used in scenarios involving simultaneous execution of multiple tasks, I/O-intensive operations, and parallel computing.



# Relationship btw The 2 Terms

Though they are usually mentioned together and are related, giving the impression that they are equal, but in fact, `high concurrency ≠ multithreading` ,



Therefore, it can be said that multithreading is one of the commonly used techniques for achieving high concurrency. <u>However</u>, high concurrency performance is also influenced by other factors such as system architecture, resource management, locking mechanisms, and algorithm design. Hence, multithreading and high concurrency are related but not exactly the same concepts..



### Scenarios

1. High concurrency scenarios --  When we have a Java web project, and after it goes live, a large number of users log in to the system, which is considered high concurrency. For the system, there will be <u>a large number of requests coming in</u>, each request corresponding to a thread, and each thread is independent (Tomcat supports 200-300). Although it is the same code, it is executed in different threads without affecting each other.
2. Multithreading scenarios -- Multithreading is used either for <u>asynchronous tasks</u> or for running <u>subtasks</u>. Each user thread is the main thread. Asynchronous tasks involve opening another thread. The purpose of running subtasks is to handle large data sets. Single-threaded processing would be slow, so the task is split into multiple subtasks, and multiple threads run them, making it faster.





ref: 

[怎么理解分布式、高并发、多线程？（含面试题和答案解析）-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1477818)

[美团面试官：哟！你对高并发与多线程的解决思路了解的还挺深！ - 掘金 (juejin.cn)](https://juejin.cn/post/6964748689382326280)

[【多线程与高并发】告诉你二者的区别 - 掘金 (juejin.cn)](https://juejin.cn/post/6947966434748301326#_35)