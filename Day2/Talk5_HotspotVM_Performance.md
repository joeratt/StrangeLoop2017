# JVM Hotspot Performance
Speaker: Monica Beckwith - [@mon_bkeck](https://www.twitter.com/mon_bkeck)
Talk: https://thestrangeloop.com/2017/the-performance-engineers-guide-to-java-hotspot-virtual-machines-execution-engine.html

# The VM
- Java Application runs on the VM
  - VM runs on the OS + Hardware
  - Virtual Machine + Java API are the JRE

## The helpers

Execution Engine
- Heap Management / GC
  - Reclamation of heap
  - Allocation
- Compilation (JIT)
  - Static
  - pre-compile (ahead of time)
    - for HotSpot JVM, Java 9 is the first JVM to have "Ahead of Time compilation"
  - Profile-guided
    - Whatever is "hot" for your application, JVM can adapt to optimize
  - Adaptive Optimization

Hotspot VM
- Runtime
  - VM CLass Load-Balancing
  - Interpreter
  - other things (bytecode, error handling, synchronization,...)
  - Goal: Convert bytecode to native code


# What Drives Innovation in performance?

1. Responsiveness
  - How much time until I get a response?
2. Footprint
  - How much memory is consumed?
  - Can I fit in more? Can I compact anything?
3. Throughput
  - How can I maximize usage?
  - Want JVM to optimize code & execute optimally
  - Don't want GC to be a pain


# Responsiveness, the Engine and the Runtime

## Responsiveness and GC
- Not only about reclamation, but also Allocation
- More work done concurrently
  - GC can do some of the work concurrently with application threads

## Responsiveness and Compilation
- Ahead-Of-Time improves startup time
- Adaptive Optimizer
  - Lots done with Hotspot
- Locking Improvements

## Responsiveness and the Runtime
- Locks
  - Uncontended Locks
  - Deflated locks "lightweight"
- Contended Locks
  - Inflated Locks

### Other locks Improvements
- [http://openjdk.java.net/jeps/143](http://openjdk.java.net/jeps/143)
- Biased Locking
  - If the JVM can isolate a lock to a single thread, it will
- Lock Elison
  - Escape Analysis
    - Do we need the lock at all?
- Lock Coarsening
- Conteded Locking - Quick path
  - "Quick enter"
  -


# Footprint, the Execution Engine and the Runtime

## Footprint and GC
- Compaction
  - if you don't have space to allocate enough memory
  - live objects can be moved to reduce memory fragmentation
- Algo Refinements in Reducing Allocations
- Compressed Headers
  - All Java Objects have "headers" and "bodies"

## Footprint and Compilation
- Inlining
  - one of most important benefits of adaptive compiler
- Escape Analysis
  - allocated object doesn't escape the compiled method + allocated object no passed as a param = remove allocation and lock (lock elision) and keep field values in registers
    - If it was passed as a param, there are still optimizers
  - [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/performance-enhancements-7.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/performance-enhancements-7.html)
- Zombie Code (Deoptimiztion)
  - There are 5 levels of Optimization
  - when compiler is going through levels of optimization, older renditions of compilation can stick around in "code cache"
  - Zombie optimization means faster

## Footprint and Runtime
- Compressed OOPs and Class pointers
  - what if you could fit your allocations
- Code Caches
  - compiled code sits in code Caches
  - you can reuse it (in cache)
- Class Data Sharing

### Compressed OOPs & Class pointers
- Headers
  - Mark word
  - klass pointer
  - array length (if array only)

#### 3-bit shift
- All Java objects are 8-byte aligned
  - If we shift left by 3, we can have 2^35 = 32GB of addressable space
```
<wide-oop> = <narrow-oop-base> + (<narrow-oop> << 3) + <field-offset>
```
- With Java 8, JVM can shift alignment to 16-byte
  - Instead of 3 bits, we have 4 bits to shift
  - Addressable space is less than 64
- Important:
  - if less than 4 GB, objects aligned on 8-bytes, with no offset or shifting

#### Compressed Class pointers
- JDK 8 -> PermGen Removal -> ClassData outside of Heap
- Compressed class pointer space
  - \_klass
- [https://community.oracle.com/docs/DOC-992145](https://community.oracle.com/docs/DOC-992145)

# Throughput

## THroughput and GC
- Multi-threaded work stealing
  - You can multi-thread GC
- Multi-generational Heap
- Multi-phase marking
- Compaction
  - G1GC
  - not monolithic


## JIT Intrinsic Optimization
- JIT will call hand-optimized assembly code

## Java String Object
- Strings -> Object -> char[] (backing array)
- Java 9 -- backing array is a byte array
  - saves space!
- Java Interned Strings
- G1 GC automatically does String deduplication (Java 8)
