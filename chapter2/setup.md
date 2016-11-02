## Set up QOR admin

[QOR Admin](https://github.com/qor/admin) is the foundation of [QOR](https://github.com/qor/qor). All [Resources](../chapter2/resource-intro.md) are created based on it (note: in the next chapter, we will introduce the `Resource`).

### 1. Prepare database

The official ORM that [QOR](https://github.com/qor/qor) uses is [GORM](http://jinzhu.me/gorm/). We use it in the demo code too.

Define models and setup a database...

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

### 2. Initialise [QOR Admin](https://github.com/qor/admin)

The `qor.Config` takes the database as its only parameter.

```
import {
  "github.com/qor/admin"
  "github.com/qor/qor"
}

Admin := admin.New(&qor.Config{DB: DB})
```

### 3. General settings

You can customize the [Dashboard](../chapter2/dashboard.md#h1), Set [Site name](../chapter2/site_name.md#h1), Configure [Menus](../chapter2/menus.md#h1) and [Router](../chapter2/router.md#h1).
