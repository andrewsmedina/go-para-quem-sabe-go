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

```

// +build ignore

// This program generates contributors.go. It can be invoked by running
// go generate
package main

import (
	"os"
	"text/template"
)

func main() {
	f, _ := os.Create("magic.go")
	defer f.Close()

	packageTemplate.Execute(f, struct {
		Numbers []int
	}{
		Numbers: []int{1, 9, 2, 8, 3},
	})
}

var packageTemplate = template.Must(template.New("").Parse(`// Code generated by go generate; DO NOT EDIT.
// This file was generated by robots at
// {{ .Timestamp }}
// using data from
// {{ .URL }}
package main

var template = []{
{{- range .Numbers}}
	{{ printf "%q" . }},
{{- end }}
}
`))
```


