## Set up engine

### 1. Prepare database

The official ORM that QOR used is [GORM](http://jinzhu.me/gorm/). But this doesn't means QOR is binded with GORM. You can use any ORM you want. We would recommend user to use GORM because other ORM may lack of features that QOR needed.

Define models and setup a database.

```
type User struct {
  gorm.Model
  Email     string
  Password  string
  Name      sql.NullString
  Gender    string
  Role      string
  Addresses []Address
}

DB, _ := gorm.Open("sqlite3", "demo.db")
DB.AutoMigrate(&User{})
```

### 2. Initialise QOR admin

```
import {
  "github.com/qor/admin"
}

Admin := admin.New(&qor.Config{DB: DB})

// Set admin page title
Admin.SetSiteName("Qor DEMO")

// Add dashboard
Admin.AddMenu(&admin.Menu{Name: "Dashboard", Link: "/admin"})
```

Check [here](./menus.md) about how to setup menus for QOR admin

TODO: how to set up menus for QOR admin
TODO: add search center resource
TODO: Other qor.Configs

Next, we'll introduce how to manage resource for QOR admin.
