# go-para-quem-sabe-go

Go é uma linguagem tipada, compilada, tem tratamento 
nativo a programação concorrente, ferramentas que ajudam
no dia-a-dia, e uma biblioteca rica e moderna.

Nesse workshop vamos aprender como explorar mais essas características de Go entendendo como funciona o scheduler, garbage colector, runtime, profile, tracer, conditional builds, gcflags, go generate, como lidar com race conditions e dead locks.

## Compilação

* [conditional builds](conditional.md)
* [gcflags](gcflags.md)
* [go generate](gogenerate.md) // WIP

## Tipagem estática

* [fuzzy](fuzzy.md) // WIP
* oracle

## Concorrência

* scheduler
* [goroutine leaks](goroutine-leaks.md) // wip
* [race conditions](race.md)
* [dead locks](deadlock.md)

## Tooling

* stack tracing
* tracer
* [profile](profile.md) // wip

## Runtime 

* setfinalizer
* called by 