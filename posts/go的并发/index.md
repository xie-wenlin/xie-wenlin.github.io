# go的并发



# go的并发

在 Go 中，您可以使用 goroutine 来编写并发代码。goroutine 是一种轻量级的线程，它允许您在单个进程中执行多个任务。您可以通过在函数名之前添加 `go` 关键字来创建一个 goroutine。下面是一个简单的例子：

```go
package main

import "fmt"

func main() {
    go fmt.Println("Hello, world!")
}

```

在这个例子中，我们创建了一个 goroutine 来打印 "Hello, world!"。这个 goroutine 会在 `main` 函数返回之前执行，因此您可以使用 goroutine 来执行需要长时间运行的任务。

如果你想写一个优美的 Go 并发程序，你需要遵循一些基本的原则。首先，在 Go 中使用通信来共享内存，而不是通过共享内存来通信。这意味着，在 Go 程序中，你应该尽量使用通道（channels）来在 goroutines 之间传递数据。

其次，你应该尽量避免使用全局变量，因为它们会使程序的并发性变得更加困难。相反，你应该通过函数参数和返回值来传递数据。

此外，你应该注意避免竞争条件（race condition），这是指在多个 goroutines 之间发生的不确定的状态更改。这可以通过 Go 语言提供的 sync 包中的互斥锁（mutex）来解决。你还需要掌握 Go 语言中的并发模型。Go 语言提供了一种简单而有效的并发模型，通过 goroutine 和 channel 来实现。使用 goroutine 可以创建并行的任务，通过 channel 可以在多个 goroutine 之间传递消息。

```go
package main

import "fmt"

func main() {
  // 创建一个通道，用于在 goroutine 之间传递消息。
  messages := make(chan string)

  // 创建 5 个 goroutine。
  for i := 0; i < 5; i++ {
    go func() {
      // 在通道中发送一条消息。
      messages <- "Hello, World!"
    }()
  }

  // 从通道中接收并打印 5 条消息。
  for i := 0; i < 5; i++ {
    fmt.Println(<-messages)
  }
}

```

```go
Hello, World!
Hello, World!
Hello, World!
Hello, World!
Hello, World!

```

```go
package main

import (
	"fmt"
	"sync"
)

// 定义一个函数，用于在并发执行
func printHello(wg *sync.WaitGroup, id int) {
	// 在函数结束时通知WaitGroup
	defer wg.Done()
	
	// 打印Hello
	fmt.Println("Hello from goroutine", id)
}

func main() {
	// 创建WaitGroup
	var wg sync.WaitGroup
	
	// 设置要执行的并发数量
	wg.Add(5)
	
	// 循环创建5个goroutine
	for i := 1; i <= 5; i++ {
		// 开启一个新的goroutine
		go printHello(&wg, i)
	}
	
	// 等待所有goroutine结束
	wg.Wait()
}

```

在这个例子中，我们创建了一个WaitGroup用于等待所有goroutine结束，并在每个goroutine结束时调用`Done`方法，告知WaitGroup已经结束。在主goroutine中，我们调用`Add`方法来设置并发的数量，并调用`Wait`方法来等待所有goroutine结束。
