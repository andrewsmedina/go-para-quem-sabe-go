# Called by

Esse programa compila???

```
package main

import (
	"fmt"
)

func f() {}
func f() {}

func main() {
	fmt.Println("hello world!")
}
```

E esse aqui?

```
package main

import (
    "fmt"
)

func init() {}
func init() {}

func main() {
    fmt.Println("hello world!")
}
```

## Por que isso funciona??

Na compilação o compilador renomeia cada função init com um sufixo único.

É possível visualizar isso com `panic` e `runtime.Callers`.

```
var pcs [1]uintptr
runtime.Callers(1, pcs[:])
fn := runtime.FuncForPC(pcs[0])
fmt.Println(fn.Name())
```
