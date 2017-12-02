# Scheduler

https://golang.org/src/runtime/proc.go?s=8739:8753#L248

G -> goroutine
M -> worker
P -> processor 

x goroutines.

n goroutine por workers, 1 worker por processor.

## Preemptivo ou cooperativo ???

Preemptivo -> interrupção temporária feita pelo scheduler
Cooperativo -> a decisão da interrupção é feita pelo "programa"

`Gosched` ???

```
package main

import "fmt"
import "runtime"
import "time"

func cpuIntensive(p *int) {
  for i := 1; i <= 100000000000; i++ {
    *p = i
  }
}

func main() {
  runtime.GOMAXPROCS(1)

  x := 0
  go cpuIntensive(&x)

  time.Sleep(100 * time.Millisecond)

  // printed only after cpuIntensive is completely finished
  fmt.Printf("x = %d.\n", x)
}
```