# Tracer

* Problemas com latência
* Paralelismo

```
package main

import (
	"fmt"
	"os"
	"runtime/trace"
)

func main() {
	trace.Start(os.Stderr)
	defer trace.Stop()

	fmt.Println("hello worl")
}
```