## QOR Admin

QOR Admin is a Golang framework allow you instantly create a beautiful, cross platform, configurable Admin Interface and API for managing your data in minutes.

It is not a Web framework like [Chi](https://github.com/go-chi/chi), [Beego](https://github.com/astaxie/beego) or [Gin](https://github.com/gin-gonic/gin), and it could be [used with them](../admin/integration.md).

### Features

* Generate Admin Interface for managing data
* RESTFul JSON API
* Association handling
* Search and filtering
* Actions/Batch Actions
* Authentication and Authorization
* Extendability

### Quick Start

```go
package main

import (
  "fmt"
  "net/http"
  "github.com/qor/qor"
  "github.com/qor/admin"
  "github.com/jinzhu/gorm"
  _ "github.com/jinzhu/gorm/dialects/sqlite"
)

// Define a GORM-backend model
type User struct {
  gorm.Model
  Name string
}

// Define another GORM-backend model
type Product struct {
  gorm.Model
  Name        string
  Description string
}

func main() {
  // Set up the database
  DB, _ := gorm.Open("sqlite3", "demo.db")
  DB.AutoMigrate(&User{}, &Product{})

  // Initalize
  Admin := admin.New(&admin.AdminConfig{DB: DB})

  // Create resources from GORM-backend model
  Admin.AddResource(&User{})
  Admin.AddResource(&Product{})

  // initalize an HTTP request multiplexer
  mux := http.NewServeMux()

  // Mount admin interface to mux
  Admin.MountTo("/admin", mux)

  fmt.Println("Listening on: 9000")
  http.ListenAndServe(":9000", mux)
}
```

Execute `go get -u ./...` to install the dependencies, then run `go run main.go` and visit [localhost:9000/admin](localhost:9000/admin) to see the site.
