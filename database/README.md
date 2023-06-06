# Instalação

### Banco de dados 

**Instalando o driver do mysql**

<!-- Consulte os drivers disponíveis https://www.google.com/url?q=https://github.com/golang/go/wiki/SQLDrivers&sa=D&source=editors&ust=1682648569189599&usg=AOvVaw30ivicKMDm8_syNMuZ1bNG -->

```zsh
go get -u "github.com/go-sql-driver/mysql"
```

**Uso**

```go
package db

import (
	"database/sql"
	"log"

	_ "github.com/go-sql-driver/mysql"
)

var (
	StorageDB *sql.DB
)

func init() {
	dataSource := "root:root@tcp(localhost:3306)/storage"
	var err error
	StorageDB, err = sql.Open("mysql", dataSource)
	if err != nil {
		panic(err)
	}
	if err = StorageDB.Ping(); err != nil {
		panic(err)
	}
	log.Println("database Configured")
}

```