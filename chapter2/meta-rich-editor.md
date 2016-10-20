# Rich Editor

Set the type of field as "rich_editor"

```
product.Meta(&admin.Meta{Name: "Description", Type: "rich_editor"})
```

## Configurations

TODO: document this

```
type RichEditorConfig struct {
  AssetManager *Resource
  Plugins      []RedactorPlugin
  Settings     map[string]interface{}
  metaConfig
}
```
