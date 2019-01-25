# 问题
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

