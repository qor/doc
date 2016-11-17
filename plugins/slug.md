# Qor Slug

[Slug](https://github.com/qor/slug) provides an easy way to create a pretty URL for your model.

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

In the [QOR Admin](./chapter2/setup.md), You can see:

![slug](slug.png)

Suppose you generate user profile page URL by `NameWithSlug`. The URL shall be `/users/the-plant` which is prettier and safer than `/users/1`.
