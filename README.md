goxx
====

``goxx`` is a *temporary* snapshot of the official ``go`` tool at tip
(926082d) so packages with `C++` files can be seamlessly go-get-installed.

You just need to replace ``go`` with the ``goxx`` command.

## Example

```sh
$ go get github.com/sbinet/goxx
$ cd $GOPATH/src/ttt
$ tree .
.
├── inc
│   └── ttt.h
├── ttt.cxx
├── run.go
└── ttt.go

1 directory, 4 files
```

where ``ttt.go`` contains:

```go
package ttt

// #include "inc/ttt.h"
// #cgo LDFLAGS: -lstdc++
import "C"

import (
	"fmt"
)

func Ttt() {
	fmt.Printf("foo\n")
	C.MyPrint()
}
```

and ``inc/ttt.h``:

```c++
#ifndef TTT_TTT_H
#define TTT_TTT_H 1

#ifdef __cplusplus
extern "C" {
#endif

void MyPrint(void);

#ifdef __cplusplus
}
#endif

#endif /* !TTT_TTT_H */
```

and ``ttt.cxx``

```c++
#include "inc/ttt.h"

#include <iostream>

extern "C" {
void MyPrint() {
  std::cout << "hello from c++" << std::endl;
}
}
```

finally, ``run.go`` contains:

```go
//+build ignore

package main

import (
	"ttt"
)

func main() {
	ttt.Ttt()
}
```

so:

```sh
$ goxx run ./run.go
foo
hello from c++
```
