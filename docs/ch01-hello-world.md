
### go mod init hello_world
* initialise go project
* add simple hello_world code
* build and executed
```
~/projects/learning-go/ch01-hello-world  $ go mod init hello_world
package main
go: creating new go.mod: module hello_world
~/projects/learning-go/ch01-hello-world  $ vi hello.go
~/projects/learning-go/ch01-hello-world  $ cat hello.go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world!")
}
~/projects/learning-go/ch01-hello-world  $ go build
~/projects/learning-go/ch01-hello-world  $ ls -ltr
total 4680
-rw-r--r--@ 1 davidpitts  staff       30 Feb  9 20:02 go.mod
package main
-rw-r--r--@ 1 davidpitts  staff       74 Feb  9 20:03 hello.go
-rwxr-xr-x@ 1 davidpitts  staff  2387874 Feb  9 20:03 hello_world
~/projects/learning-go/ch01-hello-world  $ ./hello_world
Hello, world!
```
### go fmt and go vet
* everything was correctly formatted
* simple demo of `go vet`
```
~/projects/learning-go/ch01-hello-world  $ go fmt hello.go
~/projects/learning-go/ch01-hello-world  $ ls -ltr
total 4680
-rw-r--r--@ 1 davidpitts  staff       30 Feb  9 20:02 go.mod
-rw-r--r--@ 1 davidpitts  staff       74 Feb  9 20:03 hello.go
-rwxr-xr-x@ 1 davidpitts  staff  2387874 Feb  9 20:03 hello_world
~/projects/learning-go/ch01-hello-world  $ go vet hello.go
~/projects/learning-go/ch01-hello-world  $ vi hello2.go
~/projects/learning-go/ch01-hello-world  $ cp hello.go hello2.go
~/projects/learning-go/ch01-hello-world  $ vi hello2.go
~/projects/learning-go/ch01-hello-world  $ cat hello2.go
package main

import "fmt"

func main() {
	fmt.Println("Hello, %s !")
}
~/projects/learning-go/ch01-hello-world  $ go vet hello2.go
# command-line-arguments
package main
# [command-line-arguments]
./hello2.go:6:2: fmt.Println call has possible Printf formatting directive %s
~/projects/learning-go/ch01-hello-world  $ cp hello2.go hello3.go
~/projects/learning-go/ch01-hello-world  $ vi hello3.go
~/projects/learning-go/ch01-hello-world  $ go vet hello3.go
# command-line-arguments
# [command-line-arguments]
./hello3.go:6:2: fmt.Println call has possible Printf formatting directive %s
~/projects/learning-go/ch01-hello-world  $ vi hello3.go
~/projects/learning-go/ch01-hello-world  $ go vet hello3.go
# command-line-arguments
# [command-line-arguments]
./hello3.go:6:2: fmt.Println call has possible Printf formatting directive %s
~/projects/learning-go/ch01-hello-world  $ cat hello3.go
package main

import "fmt"

func main() {
	fmt.Println("Hello, %s !\n", "Madrid")
}
~/projects/learning-go/ch01-hello-world  $ vi hello3.go
~/projects/learning-go/ch01-hello-world  $ go vet hello3.go
~/projects/learning-go/ch01-hello-world  $ cat hello3.go
package main

import "fmt"

func main() {
	fmt.Printf("Hello, %s !\n", "Madrid")
}
~/projects/learning-go/ch01-hello-world  $ go build
# hello_world
./hello2.go:5:6: main redeclared in this block
	./hello.go:5:6: other declaration of main
./hello3.go:5:6: main redeclared in this block
	./hello.go:5:6: other declaration of main
~/projects/learning-go/ch01-hello-world  $ ls -ltr
total 4696
-rw-r--r--@ 1 davidpitts  staff       30 Feb  9 20:02 go.mod
-rw-r--r--@ 1 davidpitts  staff       74 Feb  9 20:03 hello.go
-rwxr-xr-x@ 1 davidpitts  staff  2387874 Feb  9 20:03 hello_world
-rw-r--r--@ 1 davidpitts  staff       72 Feb  9 20:06 hello2.go
-rw-r--r--@ 1 davidpitts  staff       83 Feb  9 20:09 hello3.go
~/projects/learning-go/ch01-hello-world  $ cat go.mod
module hello_world

go 1.25.7
~/projects/learning-go/ch01-hello-world  $ ./hello_world
Hello, world!
~/projects/learning-go/ch01-hello-world  $ mv hello2.go hello2.go.txt
~/projects/learning-go/ch01-hello-world  $ go build
# hello_world
./hello3.go:5:6: main redeclared in this block
	./hello.go:5:6: other declaration of main
~/projects/learning-go/ch01-hello-world  $ cat ./hello3.go
package main

import "fmt"

func main() {
	fmt.Printf("Hello, %s !\n", "Madrid")
}
~/projects/learning-go/ch01-hello-world  $ mv hello.go hello.go.txt
~/projects/learning-go/ch01-hello-world  $ go build
~/projects/learning-go/ch01-hello-world  $ ./hello_world
Hello, Madrid !
```

### Using Makefile
* initial errors are because my code was in hello3.go and not hello.go

```
~/projects/learning-go/ch01-hello-world main $ cat Makefile
# Basic Go Makefile

BINARY_NAME=hello_world

.PHONY: build run clean test

build:
	go build -o ${BINARY_NAME} hello.go

run: build
	./${BINARY_NAME}

test:
	go test ./...

clean:
	go clean
	rm -f ${BINARY_NAME}
~/projects/learning-go/ch01-hello-world main $ make build
go build -o hello_world hello.go
no required module provides package hello.go; to add it:
	go get hello.go
make: *** [build] Error 1
~/projects/learning-go/ch01-hello-world main $ mv hello3.go hello.go
~/projects/learning-go/ch01-hello-world main $ make build
go build -o hello_world hello.go
~/projects/learning-go/ch01-hello-world main $ make run
go build -o hello_world hello.go
./hello_world
Hello, Madrid !
~/projects/learning-go/ch01-hello-world main $ make clean
go clean
rm -f hello_world
~/projects/learning-go/ch01-hello-world main $ ls -ltr
total 40
-rw-r--r--@ 1 davidpitts  staff   30 Feb  9 20:02 go.mod
-rw-r--r--@ 1 davidpitts  staff   74 Feb  9 20:03 hello.go.txt
-rw-r--r--@ 1 davidpitts  staff   72 Feb  9 20:06 hello2.go.txt
-rw-r--r--@ 1 davidpitts  staff   83 Feb  9 20:09 hello.go
-rw-r--r--@ 1 davidpitts  staff  212 Feb  9 20:26 Makefile
```