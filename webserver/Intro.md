### Nomenclaturas

controller -> Responsável pelas validações dos dados que chegaram na aplicação

service -> Responsável pela validação das regrás de negócio da aplicação

repository -> Responsável pela manipulação dos dados da aplicação 

ordem de chamada

controler <-> service <-> repository

### Estrutura de pastas

cmd
  server
    handler
      products.go  -> controller
    main.go
internal
  domain
    product.go -> entity
  products
    repository.go  -> repository
    service.go     -> service
pkg

go.mod
go.sum