# Set finalizer

Outro método do pacote `runtime`.

`SetFinalizer` associa um `finalizer` a um objeto. Quando
o garbage collector vai limpar um bloco associado a um `finalizer`
ele remove a associação e executa `finalizer(objieto)` em uma
goroutine separada.

### Mas para que o finalizer pode ser útil???

Um dos usos mais comuns é para detectar leaks, conexões que não estão sendo fechadas.

```
package main

import (
	"fmt"
	"os"
	"runtime"
)

func fileFinalizer(file *os.File) {
	// _, err := file.Read([]byte{})
	// if err == nil {
	// 	fmt.Println("LEAK")
	// }
}

func write(message string) {
	file, _ := os.Open("file.txt")
	runtime.SetFinalizer(file, fileFinalizer)
	file.Write([]byte(message))
}

func read() string {
	file, _ := os.Open("file.txt")
	runtime.SetFinalizer(file, fileFinalizer)
	var b []byte
	file.Read(b)
	return string(b)
}

func main() {
	write("hello")
	msg := read()
	fmt.Println("msg 1", msg)
	write("ok")
	msg = read()
	fmt.Println("msg 2", msg)
}
```