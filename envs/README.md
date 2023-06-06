# Instalação

**GO Dotenv**

```zsh
go get -u "github.com/joho/godotenv"
```

**Uso**

```go
package main

import (
  "github.com/joho/godotenv"
  "log"
  "os"
)

func main(){
  // Importante que esteja na primeira linha do programa
  err := godotenv.Load()
  if err != nil {
    log.Fatal(err)
  }

  token := os.Getenv("TOKEN")
}
```

