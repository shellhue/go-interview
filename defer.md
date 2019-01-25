## 问题
写出下面代码输出内容。

#### 问题1
```go
package main

import (
	"fmt"
)

func main() {
	defer_call()
}

func defer_call() {
	defer func() { fmt.Println("打印前") }()
	defer func() { fmt.Println("打印中") }()
	defer func() { fmt.Println("打印后") }()

	panic("触发异常")
}     
```

#### 问题2
```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println(doubleScore(0)) 
	fmt.Println(doubleScore(20.0))
	fmt.Println(doubleScore(50.0))
}
func doubleScore(source float32) (score float32) {
	defer func() {
		if score < 1 || score >= 100 {
			score = source
		}
	}()
	return source * 2
}
```

# 答案


考察对`defer`的理解，`defer`函数属延迟执行，延迟到调用者函数执行 `return` 命令前被执行。多个`defer`之间按`LIFO`先进后出顺序执行。

#### 问题1
在`Panic`触发时结束函数运行，在`return`前先依次执行`defer`内容 。最后由`runtime`运行时抛出打印`panic`异常信息。所以输出为：
```
打印后
打印中
打印前
触发异常
```

#### 问题2
函数的`return value` 不是原子操作.而是在编译器中分解为两部分：返回值赋值 和 `return` 。而`defer`刚好被插入到末尾的`return`前执行。故可以在`derfer`函数中修改返回值。
```
0
40
50
```
