# Instalação

### Documentação

**Instalando o swagger**

```zsh
go get -u "github.com/swaggo/swag/cmd/swag"
go get -u "github.com/swaggo/files"
go get -u "github.com/swaggo/gin-swagger"

export PATH=$PATH:$HOME/go/bin
```

**Gerando a documentação**

```zsh
swag init -g cmd/server/main.go # -> ponto de entrada da aplicação
```

**Importando no código**

```go
package main

import (
	"os"

	"github.com/gin-gonic/gin"
	"github.com/joho/godotenv"

	"github.com/lucasti79/web-server/docs"
	swaggerFiles "github.com/swaggo/files"
	ginSwagger "github.com/swaggo/gin-swagger"
)

func main(){
  _ = godotenv.Load()
  r := gin.Default()

  docs.SwaggerInfo.Host = os.Getenv("HOST")
  r.GET("/docs/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))

  r.Run()
}
```

**Documentando**

OBS: consulte as docs da biblioteca para ver as configurações possíveis: https://github.com/swaggo/swag

```go
package main

// Informações referente a documentação

// @title MELI Bootcamp API
// @version 1.0
// @description This API Handle MELI Products.
// @termsOfService https://developers.mercadolibre.com.ar/es_ar/terminos-y-condiciones

// @contact.name API Support
// @contact.url https://developers.mercadolibre.com.ar/support

// @license.name Apache 2.0
// @license.url http://www.apache.org/licenses/LICENSE-2.0.html
func main(){}
```

```go
package handler

// Exemplo de documentação de uma rota PUT de produtos

// @Summary Update products
// @Tags Products
// @Description update products
// @Accept json
// @Produce json
// @Param token header string true "token"
// @Param        id   path      int  true  "Product ID"
// @Param product body request true "Product to update"
// @Success 200 {object} web.Response
// @Router /products/:product_id [put]
func (c *ProductController) GetAll() gin.HandlerFunc {}
```

