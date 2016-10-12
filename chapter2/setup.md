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
```

### 3. General settings

- [Dashboard](../chapter2/dashboard.md#h1)
- [Site name](../chapter2/site_name.md#h1)
- [Menus](../chapter2/menus.md#h1)
- [Router](../chapter2/router.md#h1)
- [Authentication](../chapter2/authentication.md#h1)

TODO: Other qor.Configs
