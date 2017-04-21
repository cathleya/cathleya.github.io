---
layout: post
title: "Concurrency program in GCD"
modified:
categories: blog
excerpt:
tags: [swift,GCD]
image:
  feature:
date: 2017-04-01T08:08:50-04:00
---


In iOS, Apple provides two ways to do multitasking: The Grand Central Dispatch (GCD) and NSOperationQueue frameworks. Both of them work perfectly when it’s time to assign tasks to different threads, or different queues other than the main one. Which one should be use is a subjective matter, but in this trem we’ll focus on the first one, the GCD.

### 1.Runloop &DispatchQueue

```
 DispatchQueue.global().async {
            let time = Timer(timeInterval: 1, repeats: false, block: {(timer) in
                print("\(timer) -- hello")
            })
            RunLoop.current.add(time, forMode: RunLoopMode.commonModes)
            RunLoop.current.run()
        }

```

### 2.dispatch_apply in swift 3 (it will block current queue)

```
DispatchQueue.concurrentPerform(iterations: 10, execute: {(ab)in
            print("hhhhhh----\(ab)")
        })
```


### 3.DispatchGroup &notify

```
  let group = DispatchGroup()
        group.enter()
        DispatchQueue.main.async {
            print("i'm running")
            group.leave()
        }
        
        group.enter()
        DispatchQueue.main.async {
            sleep(10)
            print("i'm stopping")
            group.leave()
        }
        
        
        group.enter()
        DispatchQueue.main.async {
            print("i'm running")
            group.leave()
        }
        
        group.notify(queue: DispatchQueue.main, execute: {
            print("all over")
        
        })
        
```

### 4.comprehension

```

 let quene = DispatchQueue(label: "goodevening", qos: .background, attributes: .concurrent, autoreleaseFrequency: .never, target: nil)
        quene.async {
            
            DispatchQueue.concurrentPerform(iterations: 10, execute: {(ab)in
                print("hhhhhh----\(ab)")
            })
            
            print("hello  >>>>")
            let time = Timer(timeInterval: 1, repeats: true, block: {(timer) in
                print("\(timer) -- hello")
            })
            RunLoop.current.add(time, forMode: RunLoopMode.commonModes)
            RunLoop.current.run()
        }

```

### 5.DispatchWorkItem object

```

        var value = 10
        
        let workitem = DispatchWorkItem{
            value += 5
        }
        
        workitem.perform()
        
```

### 6.DispatchWorkItem notify other queue

```
DispatchQueue.global(qos: .utility).async(execute: workitem)
        
        workitem.notify(queue: DispatchQueue.main, execute: {
            print("value == \(value)")
        })

```

### 7.GCD Quality of Service (QoS) VS concurrent and activate (userInteractive 、userInitiated 、default 、utility 、background 、unspecified)

```
        let queue1 = DispatchQueue(label: "happyevveryday", qos: .userInitiated, attributes: [.concurrent,.initiallyInactive],autoreleaseFrequency: .never, target: nil)
        queue1.activate()
        
        queue1.async {
            for i in 0...10 {
                print("🆚 \(i)")
            }
        }
        
        
        queue1.async {
            for i in 90...100 {
                print("🔥 \(i)")
            }
        }
        
```

### 8.delay doing somthing in queue

```
DispatchQueue.main.asyncAfter(deadline: 15.0) {
            print("after welome  ")
        }
        
extension DispatchTime: ExpressibleByFloatLiteral {
    public init(floatLiteral value: Double) {
        self = DispatchTime.now() + .milliseconds(Int(value * 1000))
    }
}


```






-------

[TOC]








