# Goroutine leaks

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

## Detectando leaks nos testes

```
github.com/fortytw2/leaktest
```
