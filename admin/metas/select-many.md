# Select Many

## Configurations

```go
type SelectManyConfig struct {
	Collection               interface{} // []string, [][]string, func(interface{}, *qor.Context) [][]string, func(interface{}, *admin.Context) [][]string
	DefaultCreating          bool
	Placeholder              string
	SelectionTemplate        string
	SelectMode               string // select, select_async, bottom_sheet
	Select2ResultTemplate    template.JS
	Select2SelectionTemplate template.JS
	RemoteDataResource       *Resource
	PrimaryField             string
}
```

### Collection

Specify the options of the *select*, accepts type:

* `[]string`
* `[][]string`
* `func(interface{}, *qor.Context) [][]string`
* `func(interface{}, *admin.Context) [][]string`

### Placeholder

Select option's placeholder

### DefaultCreating

by default, *select* will pop up a list for user to select, if this set to `true`, it will pop up a new form for creating

### SelectionTemplate

`SelectionTemplate` accept a file path to overwrite default *select* template, which usually used when writing plugins to customize select one's template.

Refer [sortable select many](https://github.com/qor/sorting/blob/master/sortable_collection.go#L111) as an example.

### SelectMode

Set the data source of the options:

* select

  Select options with prepare options when loading the page, if you configured `Collection`, this is the only allowed option

* select_async

  Select options with remote data from `RemoteDataResource`

* bottom_sheet

  Select options in a popup with remote data from `RemoteDataResource`, with the popup, you are allowed to do some advanced work, like creating data

  `DefaultCreating` only can works with this mode

### Select2ResultTemplate

Same as [select2](https://select2.github.io)'s option [templateResult](https://select2.github.io/options.html#can-i-change-how-the-placeholder-looks) |

### Select2SelectionTemplate

Same as [select2](https://select2.github.io)'s option [templateSelection](https://select2.github.io/options.html#templateSelection)

### RemoteDataResource

Works in conjunction with the`SelectMode` configuration, when set to `select_async` or `bottom_sheet` the value represents the data resource

*select* will request remote data as JSON from `RemoteDataResource`, and use its `ID` as *select option* key, use field `Name` or `Title` or `Code` or the first value of JSON as *select option* value

### PrimaryField

If `ID` is not your primary key for `RemoteDataResource`, you could customize it with `PrimaryField`

## Examples

### Set option manually

Assuming, in a hypothetical project, a User has many favorite brands, like:

```go
type User struct {
  gorm.Model
  Name              string
  Gender            string
  FavouriteBrands []string
}
```

...So add two hypothetical brands "AwesomeStuff" and "ExcellentStuff" as candidates.

```go
user.Meta(&admin.Meta{Name: "FavouriteBrands", Type: "select_many", Config: &admin.SelectManyConfig{Collection: []string{"AwesomeStuff", "ExcellentStuff"}}})
```

### Use association data

You can use an other resource's data as options. Assuming, in a hypothetical project, a User has some favorite products, and there is a Product called "Product a" existing in the database: QOR can associate these recources automatically, with very little configuration.

```go
type User struct {
  gorm.Model
  Name              string
  Gender            string
  FavouriteProducts []Product
}

type Product struct {
  gorm.Model
  Name        string
  Description string
}

user.Meta(&admin.Meta{Name: "FavouriteProducts", Type: "select_many"})
```

Screenshot:

![select many: nested form](select_many_with_other_objects.png)


### Overwrite default SelectionTemplate

```go
u.Meta(&admin.Meta{
  Name: "FavouriteProducts",
  Type: "select_many",
  Config: &admin.SelectManyConfig{
    Collection: []string{"ASICS", "Lacoste"},
    SelectionTemplate: "metas/form/customised_select_many.tmpl",
  },
})
```

{% include "/admin/common_meta_types_with_title.md" %}
