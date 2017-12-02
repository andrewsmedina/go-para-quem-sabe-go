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


```
package main

import (
	"bytes"
	"html/template"
	"log"
	"net/http"
	"strconv"
)

type add struct {
	Sum int
}

func handleStructAdd(w http.ResponseWriter, r *http.Request) {

	var html bytes.Buffer
	first, second := r.FormValue("first"), r.FormValue("second")
	one, err := strconv.Atoi(first)
	if err != nil {
		http.Error(w, err.Error(), 500)
	}
	two, err := strconv.Atoi(second)
	if err != nil {
		http.Error(w, err.Error(), 500)
	}
	m := struct{ a, b int }{one, two}
	structSum := add{Sum: m.a + m.b}

	t, err := template.ParseFiles("template.html")
	if err != nil {
		http.Error(w, err.Error(), 500)
	}
	err = t.Execute(&html, structSum)

	if err != nil {
		http.Error(w, err.Error(), 500)
	}
	w.Header().Set("Content-Type", "text/html; charset=utf-8")
	w.Write([]byte(html.String()))
}
func main() {

	http.HandleFunc("/struct", handleStructAdd)
	log.Fatal(http.ListenAndServe("127.0.0.1:8081", nil))
}
```

```
<h2>Here is the sum {{.Sum -}}</h2>
```

```
package main

import (
	"net/http"
	"net/http/httptest"
	"testing"
)

func TestHandleStructAdd(t *testing.T) {

	r := request(t, "/?first=20&second=30")

	rw := httptest.NewRecorder()

	handleStructAdd(rw, r)
	if rw.Code == 500 {
		t.Fatal("Internal server Error: " + rw.Body.String())
	}
	if rw.Body.String() != "<h2>Here is the sum 50</h2>" {
		t.Fatal("Expected " + rw.Body.String())
	}

}
func BenchmarkHandleStructAdd(b *testing.B) {
	r := request(b, "/?first=20&second=30")
	for i := 0; i < b.N; i++ {
		rw := httptest.NewRecorder()
		handleStructAdd(rw, r)
	}

}

func request(t testing.TB, url string) *http.Request {
	req, err := http.NewRequest("GET", url, nil)
	if err != nil {
		t.Fatal(err)
	}
	return req
}
```
```
$go tool pprof --alloc_space bench.test mem0.out
```

```
$go test -run=^$ -bench=. -memprofile=mem0.out
```

1236