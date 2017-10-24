# Text input / Text area

## Text input

It is rendered by the attribute's type by default, for example given the follwing code:

```
type User struct {
  Name string
}
```

...`Name` will be rendered as a text input.

## Text area

Simply set the `Type` as "text".

```go
  product.Meta(&admin.Meta{Name: "Description", Type: "text"})
```

{% include "/admin/common_meta_types_with_title.md" %}
