---
title: Swift-自定义结构体遵循Sequence协议，实现for-in循环
date: 2020-03-05 15:14:24
tags: Swift学习
---



因为疫情滞留在湖北老家回不了深圳，公司也没啥事，为节省成本，就给停薪留职了啊啊啊～！

借此时机，好好学习。毕竟咱弱鸡，不学不行 :)



* 定义一个Queue的结构体，实现队列先入先出的效果

```swift
struct Queue<T>: Sequence {
    
    private var elements = [T]()
    
    var count: Int {
        elements.count
    }
    
    var  isEmpty: Bool {
        elements.isEmpty
    }
    
    mutating func enqueue(_ element: T) {
        elements.append(element)
    }
    
    mutating func dequeue() -> T? {
        return isEmpty ? nil : elements.removeFirst()
    }
}

var queue = Queue<String>()
queue.enqueue("1")
queue.enqueue("3")
queue.enqueue("5")
queue.enqueue("8")

print(queue.dequeue() ?? 0)
print(queue.dequeue() ?? 0)
print(queue.dequeue() ?? 0)
print(queue.dequeue() ?? 0)


-----以下下为Log输入
1
3
5
8

```



* 既然通过Array实现先进先出的结构体成功了，那咱们for-in一下吧！！！   why？ 

```swift
error: MyPlayground1.playground:254:8: error: type 'Queue<T>' does not conform to protocol 'Sequence'
  struct Queue<T>: Sequence {
         ^
```



* 根据报错信息能够知道，是因为咱自定义的结构体没有遵循Sequence协议， 

  想要实现for-in，则需要遵循Sequence协议，并通过 `func makeIterator() -> Self.Iterator`方法给定一个对应的迭代器，那么咱们先实现一个迭代器(代码如下)

```swift
struct QueueIterator: IteratorProtocol {
    let elements: Array<Any>
    var position: Int = 0
    
    init(_ elements: Array<Any>) {
        self.elements = elements
    }

    mutating func next() -> Any? {
        if position == elements.endIndex { return nil }
        let element = elements[position]
        elements.formIndex(after: &position)
        return element
    }
}
```

  

* 创建好迭代器，那么接下来就给Queue结构体添加Sequence协议了

```swift
struct Queue<T>: Sequence {
    
    private var elements = [T]()
    
    var count: Int {
        elements.count
    }
    
    var  isEmpty: Bool {
        elements.isEmpty
    }
    
    mutating func enqueue(_ elemnt: T) {
        elements.append(elemnt)
    }
    
    mutating func dequeue() -> T? {
        return isEmpty ? nil : elements.removeFirst()
    }
    
    typealias Iterator = QueueIterator
    
    func makeIterator() -> Queue.Iterator {
        return QueueIterator(elements)
    }
    
}
```




* 到此大功告成，再试试for-in，则能顺利打印出来了

```swift
for q in queue {
      print("for-in \(q)")
} 
```

  