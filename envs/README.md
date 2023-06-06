# Instalação

**GO Dotenv**

```zsh
go get -u "github.com/joho/godotenv"
```

**Uso**

Crie um arquivo ".env" na raíz do seu projeto

```zsh
 touch .env
```

Define suas variáveis nesse arquivo no formato key=value dessa forma

```
API_PORT=3000
```

```go
package main

import (
  "github.com/joho/godotenv"
  "log"
  "os"
)

func main(){
  // Carregando as variáveis de ambiente no programa
  // Importante que esteja na primeira linha do programa
  err := godotenv.Load()
  if err != nil {
    log.Fatal("Arquivo .env não encontrado")
  }

  // Capturando o valor da variavel de ambiente "TOKEN"
  token := os.Getenv("TOKEN")
}
```

