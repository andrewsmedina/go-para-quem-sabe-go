# Goroutine leaks

Como gerar leaks?

* Receiving from channel without a writer.
* Writing into a channel without Receiver.

```
package main
import (
    "fmt"
    "math/rand"
    "runtime"
    "time"
)
func query() int {
    n := rand.Intn(100)
    time.Sleep(time.Duration(n) * time.Millisecond)
    return n
}
func queryAll() int {
    ch := make(chan int)
    go func() { ch <- query() }()
    go func() { ch <- query() }()
    go func() { ch <- query() }()
    return <-ch
}
func main() {
    for i := 0; i < 4; i++ {
        queryAll()
        fmt.Printf("#goroutines: %d\n", runtime.NumGoroutine())
    }
}
```

```
package main

import (
        "fmt"
        "log"
        "net/http"
        "strconv"
)

// function to add an array of numbers.
func sum(s []int, c chan int) {
        sum := 0
        for _, v := range s {
                sum += v
        }
        // writes the sum to the go routines.
        c <- sum // send sum to c
}

// HTTP handler for /sum
func sumConcurrent(w http.ResponseWriter, r *http.Request) {
        s := []int{7, 2, 8, -9, 4, 0}

        c1 := make(chan int)
        c2 := make(chan int)
        // spin up a goroutine.
        go sum(s[:len(s)/2], c1)
        // spin up a goroutine.
        go sum(s[len(s)/2:], c2)
        // not reading from c2.
        // go routine writing to c2 will be blocked.
        x := <-c1
        // write the response.
        fmt.Fprintf(w, strconv.Itoa(x))
}

func main() {
        http.HandleFunc("/sum", sumConcurrent)   // set router
        err := http.ListenAndServe(":8001", nil) // set listen port
        if err != nil {
                log.Fatal("ListenAndServe: ", err)
        }
}
```
Uma forma de detectar leaks é adicionar métricas.

```
// get the count of number of go routines in the system.
func countGoRoutines() int {
        return runtime.NumGoroutine()
}      

func getGoroutinesCountHandler(w http.ResponseWriter, r *http.Request) {
        // Get the count of number of go routines running.
        count := countGoRoutines()
        w.Write([]byte(strconv.Itoa(count)))
}
func main()
   http.HandleFunc("/_count", getGoroutinesCountHandler)
}
```

## Como identificar a origem do leak

```
import (
  "runtime/debug"
  "runtime/pprof"
)
func getStackTraceHandler(w http.ResponseWriter, r *http.Request) {
       stack := debug.Stack()
       w.Write(stack)
       pprof.Lookup("goroutine").WriteTo(w, 2)
}
func main() {
http.HandleFunc("/_stack", getStackTraceHandler)
}
```

```
// Copyright 2016 tsuru authors. All rights reserved.
// Use of this source code is governed by a BSD-style
// license that can be found in the LICENSE file.

// +build !windows

package main

import (
	"os"
	"os/signal"
	"runtime/pprof"
	"syscall"

	"github.com/tsuru/config"
)

func listenSignals() {
	ch := make(chan os.Signal, 2)
	go func() {
		for sig := range ch {
			switch sig {
			case syscall.SIGUSR1:
				pprof.Lookup("goroutine").WriteTo(os.Stdout, 2)
			case syscall.SIGHUP:
				config.ReadConfigFile(configPath)
			}
		}
	}()
	signal.Notify(ch, syscall.SIGHUP, syscall.SIGUSR1)
}
```

## Detectando leaks nos testes

```
github.com/fortytw2/leaktest
```
