# Nomenclaturas

**Controller/Handler** -> Responsável pelas validações dos dados que chegaram na aplicação

**Service** -> Responsável pela validação das regrás de negócio da aplicação

**Repository** -> Responsável pela manipulação dos dados da aplicação

**Entity/Model/Domain** -> Cada que contém as estruturas da nossa aplicação

ordem de chamada

# Fluxo da aplicação

controller <-> service <-> repository 

![Request](/assets/request.png "Request")

# Estrutura de pastas (Pode variar)

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

go.mod -> arquivo que contém as dependências da nossa aplicação
go.sum -> arquivo que registra as dependências que as dependências tem e suas versões