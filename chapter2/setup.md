## Set up QOR admin

[QOR Admin](https://github.com/qor/admin) is the foundation of [QOR](https://github.com/qor/qor). All [Resources](../chapter2/resource-intro.md) are created based on it (note: in the next chapter, we will introduce the `Resource`).

[QOR Admin](https://github.com/qor/admin)'s User Interface (UI) and User Experience (UX) heavily draws on Google's [Material Design](https://material.google.com) language, we have pushed the envelope in some aspects and hope you like the result! That said, when extending QOR Admin for your own needs, you can leverage the [Material Design](https://material.google.com) specification to aid in achieving a seamless UI and UX.

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
