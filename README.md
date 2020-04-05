Concurrent Programming With Go
==============================


| __Thread__                      | __Goroutine__                       |
|---------------------------------|-------------------------------------|
| Have own execution stack        | Have own execution stack            |
| Fixed stack space (around 1 MB) | Variable stack space (starts @2 KB) |
| Managed by OS                   | Managed by Go runtime               |


- Coordinating tasks   -> [WaitGroups](https://golang.org/pkg/sync/#WaitGroup)
- Shared memory        -> Mutexes
- Goroutine communication -> Channels


Simple Channel/WaitGroup example
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	wg := &sync.WaitGroup{}
	ch := make(chan int)

	wg.Add(2)
	go func(ch chan int, wg *sync.WaitGroup) {
		fmt.Println(<-ch)
		wg.Done()
	}(ch, wg)
	go func(ch chan int, wg *sync.WaitGroup) {
		ch <- 42
		wg.Done()
	}(ch, wg)

	wg.Wait()
}
```
