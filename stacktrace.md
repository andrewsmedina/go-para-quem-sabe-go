# Stack trace

Usando os pacotes `runtime/debug` e `runtime/pprof` é
possível imprimir toda stack trace de um programa em Go.

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

Com stack trace é possível ver todas as goroutines que estão
sendo executadas. Possibilitando identificar goroutine leaks
e deadlocks.