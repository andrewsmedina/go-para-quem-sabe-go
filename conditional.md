# Conditional compilation

Go não tem um pre processador ou um sistema de macros.
Mas tem um sistema baseado em tags e convensão que o `go/build`
permite um pacote definir quando um arquivo deve ou não ser
compilado.

## build tags

```
// +build darwin
```

## Name convension

```
file_darwin.go
```

## Detectando que arquivos serão compilados

```
go list -f '{{.GoFiles}}' os/exec
```