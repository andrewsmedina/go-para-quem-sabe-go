# Testes fuzzy

## Assinatura da função de fuzzy

```
func Fuzz(data []byte) int
```

## Instalando o go-fuzz

```
$ go get github.com/dvyukov/go-fuzz/go-fuzz
$ go get github.com/dvyukov/go-fuzz/go-fuzz-build
```

Executando o build do pacote com o `go-fuzz`

```
$ go-fuzz-build project
```

Executando os testes

```
$ go-fuzz -bin=fuzz.zip -workdir=workdir
```

Exemplo:

```
package fuzzy

func Fuzz(data []byte) int {
	firstLetter(string(data))
	return 0
}

func firstLetter(text string) string {
	return string(text[0])
}
```