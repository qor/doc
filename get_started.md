# Get Started

Although [QOR Admin](admin/README.md) is just a component of [QOR](https://github.com/qor/qor) but we believe that a visible UI could help you getting a sense of [QOR](https://github.com/qor/qor) easily. So let's set up a simple CMS by [QOR Admin](admin/README.md).

First, create a `main.go` with

```go
package main

import (
	"fmt"
	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/sqlite"
	"github.com/qor/admin"
	"net/http"
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

	// Initialize
	Admin := admin.New(&admin.AdminConfig{DB: DB})

	// Create resources from GORM-backend model
	Admin.AddResource(&User{})
	Admin.AddResource(&Product{})

	// Initialize an HTTP request multiplexer
	mux := http.NewServeMux()

	// Mount admin to the mux
	Admin.MountTo("/admin", mux)

	fmt.Println("Listening on: 9000")
	err := http.ListenAndServe(":9000", mux)
	if err != nil {
		panic("Error start serve")
	}
}
```

Now, execute `go get -u ./...` to install the dependencies, then run `go run main.go` and visit [localhost:9000/admin](localhost:9000/admin) to see the admin site.

## Live Demo

* Live Demo http://demo.getqor.com/admin
* Source Code of Live Demo https://github.com/qor/qor-example

## Next Steps

Now you have a basic understanding of QOR Admin, you can dig it deeper in [General Configuration](/admin/general.md) or check other components of [QOR](https://github.com/qor/qor).
