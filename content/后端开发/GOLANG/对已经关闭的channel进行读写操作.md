```json
{
  "date": "2022.06.12 13:39",
  "tags": ["channel","golang面试题（中等）"],
  "description": "对已经关闭的channel进行读写操作会发生什么？"
}
```


#### 读已关闭的channel

读已经关闭的channel无影响。

* 如果在关闭前，通道内部有元素，会正确读到元素的值；
* 如果关闭前通道无元素，则会读取到通道内元素类型对应的零值。

```go
// 读一个已经关闭的通道
func main() {
    channel := make(chan int, 10)
    channel <- 2
    close(channel)
    x := <-channel
    fmt.Println(x)
}
/*[Output]: 不会报错，输出2*/
```

#### 读未关闭的空channel

```go
func main() {
    channel := make(chan int, 10)
    channel <- 2
    channel <- 3
    close(channel) 
    fmt.Println(<-channel) // 2
    fmt.Println(<-channel) // 3
    // 如果上面没有close [fatal error: all goroutines are asleep - deadlock!]
    fmt.Println(<-channel)
}
```


#### 写已关闭的channel


```go
func main() {
    channel := make(chan int, 10)
    close(channel)
    channel <- 1
}
/*[Output]: panic: send on closed channel*/
```

#### 关闭已关闭的channel

> 会引发panic: close of closed channel
```go
func main() {
    channel := make(chan int, 10)
    close(channel)
    close(channel)
}
/*[Output]: panic: close of closed channel */
```
