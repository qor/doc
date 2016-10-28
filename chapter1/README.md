## Getting started

Start with building a very simple content management system based on QOR admin. It only have a user model and a product model. By using this as a foundation, You can explore QOR further. Like configure menus for QOR admin, Try QOR supported form fields. Set up authentication/authority etc.

So first, Define package and import dependencies

```
package main

import (
    "fmt"
    "net/http"

    "github.com/jinzhu/gorm"
    _ "github.com/mattn/go-sqlite3"
    "github.com/qor/qor"
    "github.com/qor/admin"
)
```

Then, Define models

```
type User struct {
  gorm.Model
  Name string
}

type Product struct {
  gorm.Model
  Name        string
  Description string
}
```

In the `main()` function

Set up database

```
DB, _ := gorm.Open("sqlite3", "demo.db")
DB.AutoMigrate(&User{}, &Product{})
```

Initialise QOR admin

```
Admin := admin.New(&qor.Config{DB: DB})

// Create resources from GORM-backend model
Admin.AddResource(&User{})
Admin.AddResource(&Product{})
```

Register router and mount qor admin to `/admin`

```
// Register route
mux := http.NewServeMux()
// amount to /admin, so visit `/admin` to view the admin interface
Admin.MountTo("/admin", mux)

fmt.Println("Listening on: 9000")
http.ListenAndServe(":9000", mux)
```

Finally, The file should looks like this

```
package main

import (
    "fmt"
    "net/http"

    "github.com/jinzhu/gorm"
    _ "github.com/mattn/go-sqlite3"
    "github.com/qor/qor"
    "github.com/qor/admin"
)

// Create a GORM-backend model
type User struct {
  gorm.Model
  Name string
}

// Create another GORM-backend model
type Product struct {
  gorm.Model
  Name        string
  Description string
}

func main() {
  DB, _ := gorm.Open("sqlite3", "demo.db")
  DB.AutoMigrate(&User{}, &Product{})

  // Initalize
  Admin := admin.New(&qor.Config{DB: DB})

  // Create resources from GORM-backend model
  Admin.AddResource(&User{})
  Admin.AddResource(&Product{})

  // Register route
  mux := http.NewServeMux()
  // amount to /admin, so visit `/admin` to view the admin interface
  Admin.MountTo("/admin", mux)

  fmt.Println("Listening on: 9000")
  http.ListenAndServe(":9000", mux)
}
```

Execute `go get -u ./...` to install the dependencies, then run `go run main.go` and visit <localhost:9000/admin> to see the site.
