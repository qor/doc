# Data Processing & Validation

## Validations

### Application Level Validations

  If you want to have some global validations for your application, you could use [QOR Validation](https://github.com/qor/validations), it is a GORM extension, it could be used to validate models when creating, updating.

### Admin Level Validations

  If you only want to validate data from QOR Admin, `QOR Admin Validator` is for you, it will check data before decode form/JSON data to the struct.

```go
store := Admin.AddResource(&Store{})

store.AddValidator(&resource.Validator{
  Name: "check_has_name" // register another validator with same name will overwirte previous one
  Handler: func(record interface{}, metaValues *resource.MetaValues, context *qor.Context) error {
    // Get meta's value from metaValues, metaValues is a struct that hold all post data
    if meta := metaValues.Get("Name"); meta != nil {
        if name := utils.ToString(meta.Value); strings.TrimSpace(name) == "" {
            return validations.NewError(record, "Name", "Name can't be blank")
        }
    }
    return nil
  },
})
```

## Process data before saving to database

### Application Level Processor

  If you want to process some data before saving it into the database, and have it globally, [GORM callbacks](http://jinzhu.me/gorm/callbacks.html) is perfect for your case.

### Admin Level Processor

  But when you only want to process data that from QOR Admin, you can use `QOR Admin Processor`, it can process data after decoding them into the struct, but before saving them into the database, use it like:

```go
store.AddProcessor(&resource.Processor{
  Name: "process_store_data", // register another processor with same name will overwirte previous one
    Handler: func(value interface{}, metaValues *resource.MetaValues, context *qor.Context) error {
        if store, ok := value.(*Store); ok {
      // do something...
        }
        return nil
    },
})
```
