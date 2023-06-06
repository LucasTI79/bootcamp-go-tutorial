# Testes

**Exemplo de teste**

```go
// arquivo a ser testado
package main

import "fmt"

func Ola() string {
    return "Olá, mundo"
}

func main() {
    fmt.Println(Ola())
}

```

```go
// Teste
package main

// módulo de testes nativo do go
import "testing"

func TestOla(t *testing.T) {
    resultado := Ola()
    esperado := "Olá, mundo"

    if resultado != esperado {
        t.Errorf("resultado '%s', esperado '%s'", resultado, esperado)
    }
}
```

**Recursos**

```go
// Podemos testar vários "cenários" num único caso de teste
func TestOla(t *testing.T) {

    // Podemos criar funções que auxiliam na execução dos testes
    verificaMensagemCorreta := func(t *testing.T, resultado, esperado string) {
        // Para que essa função não seja considerada no stack trace, é necessário sinalizar que é uma função "Helper" dessa forma
        t.Helper()
        if resultado != esperado {
            t.Errorf("resultado '%s', esperado '%s'", resultado, esperado)
        }
    }


    t.Run("diz olá para as pessoas", func(t *testing.T) {
        resultado := Ola("Chris")
        esperado := "Olá, Chris"

        if resultado != esperado {
            t.Errorf("resultado '%s', esperado '%s'", resultado, esperado)
        }
    })

    t.Run("diz 'Olá, mundo' quando uma string vazia for passada", func(t *testing.T) {
        resultado := Ola("")
        esperado := "Olá, mundo"

        if resultado != esperado {
            t.Errorf("resultado '%s', esperado '%s'", resultado, esperado)
        }
    })
}
```

**Pontos de atenção**

O go não permite comparar slices, para isso usamos o pacote reflect

```go
// exemplo de função que soma dois slices diferentes
import "reflect"

func TestSomaTudo(t *testing.T) {

    recebido := SomaTudo([]int{1,2}, []int{0,9})
    esperado := []int{3, 9}

    if !reflect.DeepEqual(recebido, esperado) {
        t.Errorf("recebido %v esperado %v", recebido, esperado)
    }
}
```

### Uso

**Rodando os testes**

```zsh
# procura por todos os arquivos com final _test.go dentro do diretório m q foi executado o comando
go test ./...
```

**Pacotes de mock**

# Instalação

### 1° Opção

**go-sqlmock** 
<!-- Não abre uma conexão real com o banco de dados -->
```zsh
go get -u "github.com/DATA-DOG/go-sqlmock"
```

**Uso**

```go
func Test_sqlRepository_Store_Mock(t *testing.T) {
  db, mock, err := sqlmock.New()
  assert.NoError(t, err)
  defer db.Close()
  mock.ExpectPrepare("INSERT INTO users")
  mock.ExpectExec("INSERT INTO users").WillReturnResult(sqlmock.NewResult(1, 1))
  columns := []string{"uuid", "firstname", "lastname", "username", "password", "email", "ip", "macAddress", "website", "image"}
  rows := sqlmock.NewRows(columns)
  userId := uuid.New()
  rows.AddRow(userId, "", "", "", "", "", "", "", "", "")
  mock.ExpectQuery("SELECT .* FROM users").WithArgs(userId).WillReturnRows(rows)
  repository := NewSqlRepository(db)
  ctx := context.TODO()
  user := models.User{
     UUID: userId,
  }
  getResult, err := repository.GetOne(ctx, userId)
  assert.NoError(t, err)
  assert.Nil(t, getResult)
  err = repository.Store(ctx, &user)
  assert.NoError(t, err)
  getResult, err = repository.GetOne(ctx, userId)
  assert.NoError(t, err)
  assert.NotNil(t, getResult)
  assert.Equal(t, user.UUID, getResult.UUID)
  assert.NoError(t, mock.ExpectationsWereMet())
}

```

### 2° Opção

**go-txdb**
<!-- Abre uma conexão real com o banco de dados e faz um "rollback" -->
```zsh
go get -u "github.com/DATA-DOG/go-txdb"
```

**Uso**

```go
package util

import (
   "database/sql"
   "github.com/DATA-DOG/go-txdb"
   _ "github.com/go-sql-driver/mysql"
   "github.com/google/uuid"
)

func init() {
   txdb.Register("txdb", "mysql", "root:mariadb@/mydb")
}

func InitDb() (*sql.DB, error) {
   db, err := sql.Open("txdb", uuid.New().String())

   if err == nil {
       return db, db.Ping()
   }

   return db, err
}

func Test_sqlRepository_Store(t *testing.T) {
   db, err := util.InitDb()
   assert.NoError(t, err)
   repository := NewSqlRepository(db)
   ctx := context.TODO()
   userId := uuid.New()
   user := models.User{
       UUID: userId,
   }
   err = repository.Store(ctx, &user)
   assert.NoError(t, err)
   getResult, err := repository.GetOne(ctx, uuid.New())
   assert.NoError(t, err)
   assert.Nil(t, getResult)
   getResult, err = repository.GetOne(ctx, userId)
   assert.NoError(t, err)
   assert.NotNil(t, getResult)
   assert.Equal(t, user.UUID, getResult.UUID)
}

```

