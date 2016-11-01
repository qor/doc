## Getting started

In this document, we will provide example code based on a simple, hypothetical project: a content management system based on [QOR Admin](../chapter2/setup.md). The end goal is to have a User model and a Product model, which would allow an administrator to use [QOR Admin](../chapter2/setup.md) to manage data within a `User` table and `Product` table within a [SQLite3 database](https://sqlite.org/ "SQLite3 database"). We hope that this simple project code will establish the foundations of [QOR](https://github.com/qor/qor) for you as you read through this document. So the first aspects we will touch on are:

- configuring menus for [QOR Admin](../chapter2/setup.md),
- try out some [QOR](https://github.com/qor/qor) supported form fields.

It will really help if you type out this code and run it - you'll learn a lot in the process!

Let's begin, define a package and import dependencies...

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

Then, we define models...

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

In the `main()` function, set up the database...

```
DB, _ := gorm.Open("sqlite3", "demo.db")
DB.AutoMigrate(&User{}, &Product{})
```

Now initialise [QOR Admin](../chapter2/setup.md)...

```
Admin := admin.New(&qor.Config{DB: DB})

// Create resources from GORM-backend model
Admin.AddResource(&User{})
Admin.AddResource(&Product{})
```

Register a router and mount [QOR Admin](../chapter2/setup.md) to `/admin`...

```
// Register route
mux := http.NewServeMux()
// amount to /admin, so visit `/admin` to view the admin interface
Admin.MountTo("/admin", mux)

fmt.Println("Listening on: 9000")
http.ListenAndServe(":9000", mux)
```

Finally, The file should looks like this:

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

Execute `go get -u ./...` to install the dependencies, then run `go run main.go` and visit [localhost:9000/admin](localhost:9000/admin) to see the site.

Next step: have a look at the [QOR example](https://github.com/qor/qor-example) code which solves the needs of a more realistic example project. The project is considerably larger and includes: Authentication, Authority, i18n, SEO, Widget, Worker, Publish, product, store, order, and user management.
