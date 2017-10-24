# Select one

## Configurations

```go
type SelectOneConfig struct {
	Collection               interface{} // []string, [][]string, func(interface{}, *qor.Context) [][]string, func(interface{}, *admin.Context) [][]string
	Placeholder              string
	AllowBlank               bool
	DefaultCreating          bool
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

### AllowBlank

Allow clear current selection, default `false`

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

In a hypothetical project, to change the Meta type of the `Gender` field in User resource from `string` (default value) to `select_one`, with options `M` | `F`, one might use the following code:

```go
user.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}}})
```

### Add clear icon to clear current selection

```go
u.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}, AllowBlank: true}})
```

### Generate options by data from the database

This is an example of generates options by the languages in the database. The `Collection` option accept a function with `context *admin.Context` parameter. You can get the database by `context.GetDB()` function. The returned value should be a `[][]string` array.

```go
  user.Meta(&admin.Meta{Name: "Language", Label: "Locale", Type: "select_one",
    Config: &admin.SelectOneConfig{
      Collection: func(_ interface{}, context *admin.Context) (options [][]string) {
        var languages []models.Language
        context.GetDB().Find(&languages)

        for _, n := range languages {
          idStr := fmt.Sprintf("%d", n.ID)
          var option = []string{idStr, n.NameWithCountry()}
          options = append(options, option)
        }

        return options
      },
    },
  })
```

### Overwrite default SelectionTemplate

```go
u.Meta(&admin.Meta{
  Name: "Gender",
  Type: "select_one",
  Config: &admin.SelectOneConfig{
    Collection: []string{"M", "F"},
    SelectionTemplate: "metas/form/customised_select_one.tmpl",
  },
})
```
