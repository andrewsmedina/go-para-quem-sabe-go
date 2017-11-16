# Go generate

`go generate` foi adicionado na versão 1.4 do Go.
Para automatizar a geração de código antes da compilação.

## Hello world

```
package projeto

//go:generate echo Ola mundo!

func Soma(x, y int) int {
	return x + y
}
```

## Usando `go:generate` para rodar um programa em Go

```
package main

import "fmt"

//go:generate go run gen.go

func main() {
	for _, number := range magicNumbers {
        fmt.Println("magic number - ", number)
    }
}
```


