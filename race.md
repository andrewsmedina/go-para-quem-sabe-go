# Race conditions

É quando duas goroutines acessam a mesma variável ao mesmo tempo e pelo pelos um desses acessos é uma “escrita”.

```
package main

import (
	"fmt"
)

func main() {
	c := make(chan bool)
	data := map[string]string{}
	go func() {
		data["name"] = "andrews"
		c <- true
	}()
	go func() {
		data["surname"] = "medina"
	}()

	<-c
	for key, value := range data {
		fmt.Println(key, value)
	}
}
```

## Como detectar race conditions

O go tem um detector de race conditions. Basta usar o 
parâmetro `-race` ao compilar ou ao executar os testes.

```
go build -race
```

O detector mostra quais são as goroutines que estão acessando
a variável de forma concorrente:

```
==================
WARNING: DATA RACE
Write at 0x00c420072180 by goroutine 7:
  runtime.mapassign_faststr()
      /usr/local/Cellar/go/1.9.2/libexec/src/runtime/hashmap_fast.go:598 +0x0
  main.main.func2()
      /Users/andrews/projects/opensource/go-para-quem-sabe-go/race/main.go:15 +0x5d

Previous write at 0x00c420072180 by goroutine 6:
  runtime.mapassign_faststr()
      /usr/local/Cellar/go/1.9.2/libexec/src/runtime/hashmap_fast.go:598 +0x0
  main.main.func1()
      /Users/andrews/projects/opensource/go-para-quem-sabe-go/race/main.go:11 +0x5d

Goroutine 7 (running) created at:
  main.main()
      /Users/andrews/projects/opensource/go-para-quem-sabe-go/race/main.go:14 +0xe3

Goroutine 6 (running) created at:
  main.main()
      /Users/andrews/projects/opensource/go-para-quem-sabe-go/race/main.go:10 +0xc1
==================
Found 1 data race(s)
```
 