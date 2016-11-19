# SizedWaitGroup

SizedWaitGroup has the same role and API as sync.WaitGroup but it adds a limit of the amount of goroutines started concurrently.

`SizedWaitGroup` adds the feature of limiting the maximum number of concurrently started routines. It could for example be used to start multiples routines querying a database but without sending too much queries in order to not overload the given database.

# Example

```
package main

import (
        "fmt"
        "math/rand"
        "time"
)

func main() {
        rand.Seed(time.Now().UnixNano())

        // Typical use-case:
        // 50 'things' should be started as quick as possible
        // but without overloading the database, so only
        // 8 routines should be started concurrently.
        swg := New(8)
        for i := 0; i < 50; i++ {
                swg.Add(1)
                go func(i int) {
                        fmt.Println(i) 
                        sleep()
                        swg.Done()
                }(i)
        }

        swg.Wait()
}

func sleep() {
        ms := 500 + rand.Intn(500)
        time.Sleep(time.Duration(ms) * time.Millisecond)
}
```

# License

MIT

# Copyright

Rémy Mathieu © 2016
