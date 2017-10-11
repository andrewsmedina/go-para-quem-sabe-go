# Profile

## CPU Profile

```
go test -run=^$ -bench=. -cpuprofile=cpu.out
```

```
go tool pprof bench.test cpu.out
```

# Memory Profile

```
go tool pprof --alloc_space bench.test mem.out
```
