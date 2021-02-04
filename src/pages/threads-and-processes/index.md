---
title: 'Threads and Processes in Python'
date: '2020-12-03'
spoiler: Parallel processing in python.
---

## Threading

- all threads use the same memory heap space 🧠
- sharing info between threads is easy (a consequence of sharing same memory space)
- multiple threads can write to the same location(a consequence of sharing same memory space. No race conditions because of GIL)
- module isnt killable
- multiple threads live in the same process
- each thread has own code,stack memory and instruction pointer
- if a thread has a memory leak, it can damage other threads and parent process
- cannot be used for parallel cpu computation as python doesnt allow true multithreading
- perfect for I/O, network, data operations as the processor is sitting idle in the duration that it is waiting for a resource(useful when a process is waiting for substantial amount of time)
- for cpu intensive processes there is very little advantage in using threading
- Use Cases:
    - lot of I/O or network usage
    - lot of gui

## Processing

- uses different memory heap space
- sharing information between processes is difficult (a consequence of sharing same memory space)
- child processes are killable
- each process has its own python interpreter and GIL
- data corruption and deadlocks are no more an issue
- The entire memory is copied into all sub processes causing significant overhead
- Use Cases:
    - code is CPU bound