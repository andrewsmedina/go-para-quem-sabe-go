# Deadlock

`Deadlock (interbloqueio, blocagem, impasse), no contexto de sistemas operacionais (SO), refere-se a uma situação em que ocorre um impasse, e dois ou mais processos ficam impedidos de continuar suas execuções - ou seja, ficam bloqueados, esperando uns pelos outros. Trata-se de um problema bastante estudado em sistemas operacionais e banco de dados, pois é inerente à própria natureza desses sistemas.` https://pt.wikipedia.org/wiki/Deadlock

Normalmente o `runtime` do `Go` vai levantar um `panic` 
informando que todas as goroutines estão `asleep`.

```
package main

func main() {
	ch1 := make(chan bool)
	ch2 := make(chan bool)
	go func() {
		<-ch1
		ch2 <- true
	}()
	<-ch2
	ch1 <- true
}
```

Resultado:

```
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
main.main()
	/Users/andrews/.go/src/github.com/andrewsmedina/tutorial/deadlock/main.go:25 +0xc5

goroutine 5 [chan receive]:
main.main.func1(0xc420074060)
	/Users/andrews/.go/src/github.com/andrewsmedina/tutorial/deadlock/main.go:17 +0xa5
created by main.main
	/Users/andrews/.go/src/github.com/andrewsmedina/tutorial/deadlock/main.go:10 +0x5c

goroutine 17 [chan receive]:
main.main.func1.1(0xc42008a000, 0xc42008a060)
	/Users/andrews/.go/src/github.com/andrewsmedina/tutorial/deadlock/main.go:14 +0x34
created by main.main.func1
	/Users/andrews/.go/src/github.com/andrewsmedina/tutorial/deadlock/main.go:13 +0x8e
```

Mas se você "encapsular" o código que causa o deadlock em uma
goroutine o `panic` não vai ser propagado e não será fácil
identificar o que está acontecendo:

```
package main

import (
	"fmt"
)

func main() {
	go func() {
		ch1 := make(chan bool)
		ch2 := make(chan bool)
		go func() {
			<-ch1
			ch2 <- true
		}()
		<-ch2
		ch1 <- true
		fmt.Println("waiting!")
	}()

	for {
	}
}
```

Uma forma de identificar o que está acontecendo é através dos
`dumps` de `goroutines`.