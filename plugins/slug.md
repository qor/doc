# Qor Slug

[Slug](https://github.com/qor/slug) package is an extension for [QOR](https://github.com/qor/qor). It provides an easy way to create a pretty URL for your model.

[![GoDoc](https://godoc.org/github.com/qor/slug?status.svg)](https://godoc.org/github.com/qor/slug)

## Usage

Use `slug.Slug` as your field type, then this field could be used as slug field

```go
import (
  "github.com/jinzhu/gorm"
  "github.com/qor/slug"
)

type User struct {
  gorm.Model
  Name            string
  NameWithSlug    slug.Slug
}
```

In the admin:

![slug](slug.png)
