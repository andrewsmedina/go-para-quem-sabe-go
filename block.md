# infinito loop

## Empty select

```
select {}
```

## WaitGroup com Wait sem Done

```
import "sync"

func blockForever() {
    wg := sync.WaitGroup{}
    wg.Add(1)
    wg.Wait()
}
```

## Double Locking

```
import "sync"

func blockForever() {
    m := sync.Mutex{}
    m.Lock()
    m.Lock()
}
```

## Lendo de um canal sem um produtor

```
func blockForever() {
    c := make(chan struct{})
    <-c
}
```

## “Busy Blocking”

```
import "runtime"

func blockForever() {
    for {
        runtime.Gosched()
    }
}
```

## Escrevendo em um canal sem um consumidor

```
func blockForever() {
    c := make(chan struct{})
    c <- 1
}
```

## Enviando e recebendo mensangem no mesmo canal em um select

```
func blockForever() {
    c := make(chan struct{}, 1)
    for {
        select {
        case <-c:
        case c <- struct{}{}:
        }
    }
}
```

## Esperando o futuro

```
import (
    "math"
    "time"
)

func blockForever() {
    <-time.After(time.Duration(math.MaxInt64)) # 292
}
```

https://blog.sgmansfield.com/2016/06/how-to-block-forever-in-go/