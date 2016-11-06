# Location

[Location](https://github.com/qor/location) picks up location details from an interactive Google Map widget when creating/editing any applicable Resource in [Qor Admin](http://github.com/qor/qor).

# Usage

```go
import (
  "github.com/jinzhu/gorm"
  "github.com/qor/location"
)

type Store struct {
  gorm.Model
  Name string
  location.Location
}

type Shop struct {
  gorm.Model
  Name string
  location.Location `location:"name:Address"`
}
```

Screenshot:

![location](location.png)

## QOR Integration

Although [Location](https://github.com/qor/location) could be used alone, it integrates nicely with [QOR](https://github.com/qor/qor).

To use [Location](https://github.com/qor/location) with [QOR Admin](../chapter2/setup.md), simply embed `location.Location` in a model then behold the map picker in the admin interface.

[Location Demo:  http://demo.getqor.com/admin/setting](http://demo.getqor.com/admin/setting)
