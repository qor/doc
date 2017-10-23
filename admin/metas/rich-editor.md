# Rich Editor

The Rich Editor of QOR is based on [Redactor editor](https://imperavi.com/redactor/docs/).

Set the type of field as `rich_editor`, et voila!

Currently, if the content added/managed via the Rich Editor needs to comply with Accessibility needs, the administrator using QOR Admin should edit the raw HTML after using the Rich Editor, to be sure, to be sure, to be sure.

```
product.Meta(&admin.Meta{Name: "Description", Type: "rich_editor"})
```

## File/Image uploading support

First, create an asset manager provided by [Media library](/plugins/media-library.md), then configure it to be attached to a Rich Editor.

```go
import "github.com/qor/media/asset_manager"

// Add Asset Manager, for rich editor
assetManager := Admin.AddResource(&asset_manager.AssetManager{}, &admin.Config{Invisible: true})

product.Meta(&admin.Meta{Name: "Description", Config: &admin.RichEditorConfig{AssetManager: assetManager}})
```

## Add plugin to rich editor

You can add [Redactor plugins](https://imperavi.com/redactor/plugins/) and/or [create your own Rich Editor plugin](https://imperavi.com/redactor/docs/how-to-create-plugin/) and use either/both with Rich Editor:

```go
product.Meta(&admin.Meta{Name: "Description", Config: &admin.RichEditorConfig{AssetManager: assetManager, Plugins: []admin.RedactorPlugin{{Name: "medialibrary", Source: "/admin/assets/javascripts/qor_redactor_medialibrary.js"}}, Settings: map[string]interface{}{
  "medialibraryUrl": "/admin/product_images",
}}})
```
