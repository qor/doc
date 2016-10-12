# Scopes and filters

## Filters

Make resource filter-able by given setting

```
  user.Filter(&admin.Filter{
    Name: "Gender",
    Config: &admin.SelectOneConfig{
      Collection: []string{"Male", "Female", "Unknown"},
    },
  })
```

## Scopes

You can define scopes to filter data with given conditions, for example:

```go
// Only show actived users
user.Scope(&admin.Scope{Name: "Active", Handle: func(db *gorm.DB, context *qor.Context) *gorm.DB {
  return db.Where("active = ?", true)
}})
```

## Group Scopes

```go
order.Scope(&admin.Scope{Name: "Paid", Group: "State", Handle: func(db *gorm.DB, context *qor.Context) *gorm.DB {
  return db.Where("state = ?", "paid")
}})

order.Scope(&admin.Scope{Name: "Shipped", Group: "State", Handle: func(db *gorm.DB, context *qor.Context) *gorm.DB {
  return db.Where("state = ?", "shipped")
}})
```

[Scopes Online Demo](http://demo.getqor.com/admin/products)
