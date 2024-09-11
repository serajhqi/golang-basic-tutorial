## WaitGroup

```go
func main(){
  var wg sync.WaitGroup

  wg.Add(2) // this is the number of go routines
  go func(){
    log.Println("1")
    wg.Done()
  }()

  go func(){
    log.Println("2")
    wg.Done()
  }()

  wg.Done()
}
```

Without WaitGroup the main function gets executed and finishes and nothing gets printed.


## runtime.GOMAXPROCS()
Defines how many cpu cors works.
example:
```go
runtime.GOMAXPROCS(4)
```
