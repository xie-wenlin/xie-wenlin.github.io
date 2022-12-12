# 懒汉模式


# go懒汉模式

"懒汉模式"（lazy instantiation）是指在第一次使用时才会创建对象的方法。这与饿汉模式（eager instantiation）不同，饿汉模式会在程序启动时就创建对象。

Go 语言中的懒汉模式指的是在需要的时候才创建对象的方式。这种方式的好处是可以节省内存，因为对象只有在真正需要的时候才会被创建。不过，使用懒汉模式时需要注意线程安全问题。Go语言没有直接支持懒汉模式的语法，但是你可以通过编写一个函数来实现这个模式。例如：

```go
var instance *Singleton

func GetInstance() *Singleton {
    if instance == nil {
        instance = &Singleton{}
    }
    return instance
}

```

在这个函数中，我们首先检查实例是否为空，如果是，我们创建一个新的实例并将其保存到变量中。如果实例已经存在，我们直接返回它。

需要注意的是，由于Go语言没有类型，所以我们需要使用一个全局变量来保存实例。这可能会导致一些问题，例如多个单例实例之间的冲突，因此在实际开发中应该谨慎使用。

在 go 语言中，可以使用 sync.Once 类型来实现懒汉模式,解决多个单例之间的冲突。例如：

```go
import "sync"

var once sync.Once
var instance *MyType

func GetInstance() *MyType {
    once.Do(func() {
        instance = &MyType{}
    })
    return instance
}

```

在上面的代码中，GetInstance 函数使用 sync.Once 类型来确保 instance 变量只会被创建一次。

不过，在 go 语言中，懒汉模式并不是很常用。因为 go 语言的并发模型和其他语言不太一样，所以在 go 中很少有需要用到懒汉模式的场景。

