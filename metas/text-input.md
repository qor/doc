# Text input / Text area

## Text input

It is rendered by the attributes type by default.

```
type User struct {
  Name string
}
```

The `Name` will be rendered as text input.

## Text area

Simply set the `Type` as "text".

```go
  product.Meta(&admin.Meta{Name: "Description", Type: "text"})
```
