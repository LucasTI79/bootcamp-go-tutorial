# Instalação

### Web server (GIN)

Versão do GO necessária 1.13+

```zsh
go get -u "github.com/gin-gonic/gin"
```

**Iniciando servidor:**

Instanciando o web server do gin

```go
server := gin.Default() // Servidor com middlewares de logs e recuperação de panics
```

ou

```go
server := gin.New() // Essa opção cria um servidor sem as configurações padrões do gin 
```

**Criando uma rota no gin**

```go
server.GET("/", func(c *gin.Context) {
  c.JSON(200, gin.H{
    "message": "Hello World!"
  })
})
```

**Agrupando rotas no gin**

```go
gproducts = server.Group("/products")
{
  gproducts.GET("/", func())        // -> equivalente a /products/
  gproducts.GET("/:id", func())     // -> equivalente a /products/:id
  gproducts.POST("/", func())       // -> equivalente a /products
  gproducts.PUT("/:id", func())     // -> equivalente a /products/:id
  gproducts.DELETE("/:id", func())  // -> equivalente a /products/:id
}
```

**Iniciando o web server com o gin**

```go
server.Run(":8080") // o padrão já é a porta 8080
```

**Capturando a request com o gin**

```go
body := c.Request.Body
header := c.Request.Header
method := c.Request.Method
```

**Populando a referência de uma variável com a request**

```go
type request struct{
  Name string
  Price float64
}

var req request

// duas formas
c.Bind(&req) // se houver um erro na serialização, para a requisição e retorna o status 400
c.ShouldBindJSON(&req) // se houver um erro na serialização apenas retorna o erro e nos deixa tratá-lo
```

**Retornando um JSON como resposta**

```go
// obs: o gin.H na verdade é um mapa representado dessa forma map[string]interface{}
statusCode := http.StatusOk
responseData :=  gin.H{
  "key": "value"
}

c.JSON(statusCode, responseData)
```

**Capturando headers**

```go
header := c.GetHeader("nome_header")
```

**Middlewares**

Fluxo de um middleware

request <-> middleware <-> handler <-> middleware <-> response

![Middleware](/assets/middleware.png "Middleware")

Criando um middleware

```go
// Exemplo de middleware que pode ser usado para verificar se o usuário está mandando um token válido na requisição
func TokenAuthMiddleware() gin.HandlerFunc {
	requiredToken := os.Getenv("TOKEN")

	// We want to make sure the token is set, bail if not
	if requiredToken == "" {
		log.Fatal("Please set API_TOKEN environment variable")
	}

	return func(c *gin.Context) {
		token := c.GetHeader("TOKEN")

		if token == "" {
			respondWithError(c, 401, "API token required")
			return
		}

		if token != requiredToken {
			respondWithError(c, 401, "Invalid API token")
			return
		}

		c.Next()
	}
}
```

Usando um middleware a nível global (Todas as rotas abaixo vão ser impactadas)

```go
  func main(){
    server := gin.Default()
    server.Use(TokenAuthMiddleware)
    server.GET("/", func(c *gin.Context){
      c.JSON(200, gin.H{
        "message": "ok"
      })
    })
    server.Run()
  }
```

Usando um middleware a nível de rota (Apenas essa rota será impactada)

```go
  func main(){
    server := gin.Default()
    server.GET("/", TokenAuthMiddleware, func(c *gin.Context){
      c.JSON(200, gin.H{
        "message": "ok"
      })
    })
    server.Run()
  }
```