# Comandos úteis

```zsh
# Limpa dependências não utilizadas e instala dependências nao listadas, mas necessárias 
go mod tidy
```

```zsh
# baixa a dependência ou atualiza para a versão mais recente (Atualiza o arquivo go.mod)
go get
```

```zsh
# compila e instala o pacote do projeto baixado no diretório binário do sistema
go install
```

```zsh
# Baixa os módulos necessários do projeto e salva em cache (já é executado automáticamente no comando go build)
go mod download
```

```zsh
# Compila o código fonte do projeto e já o executa
go run
```

```zsh
# Verifica erros de sintaxe e váriaves não usadas no código fonte
go vet
```

```zsh
# Formata o código fonte
go fmt
```

```zsh
# Apresenta no terminal as variáveis definidas no ambiente go
go env
```

```zsh
# Compila o código fonte do projeto em um arquivo executável
go build
```

```zsh
# Roda os testes do projeto (procura por arquivos que terminam com _test.go)
go test ./...
```

