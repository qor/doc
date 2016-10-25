# Rich Editor

The rich editor of QOR is based on [Redactor editor](https://imperavi.com/redactor/docs/).

Set the type of field as "rich_editor", Then it is ready to go.

```
product.Meta(&admin.Meta{Name: "Description", Type: "rich_editor"})
```

## File/Image uploading support

First, Create a asset manager provided by [Media library](plugins/media-library.md). Then configure it to rich editor.

```
  // Add Asset Manager, for rich editor
  assetManager := Admin.AddResource(&media_library.AssetManager{}, &admin.Config{Invisible: true})

  product.Meta(&admin.Meta{Name: "Description", Config: &admin.RichEditorConfig{AssetManager: assetManager}})
```

## Add plugin to rich editor

You can add redactor plugin or [create your own plugin](https://imperavi.com/redactor/docs/how-to-create-plugin/) and use it with rich editor by:

```
  product.Meta(&admin.Meta{Name: "Description", Config: &admin.RichEditorConfig{AssetManager: assetManager, Plugins: []admin.RedactorPlugin{{Name: "medialibrary", Source: "/admin/assets/javascripts/qor_redactor_medialibrary.js"}}, Settings: map[string]interface{}{
    "medialibraryUrl": "/admin/product_images",
  }}})
```
