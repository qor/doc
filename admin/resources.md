# Resources

Resources are something that can be administered through the QOR Admin's user interface, often a GORM backend model.

## Add a resource to QOR Admin

```go
// GORM-backend model
type User struct {
  gorm.Model
  Email     string
  Password  string
  Name      sql.NullString
  Gender    string
  Role      string
  Addresses []Address
}

// Add it to Admin
user := Admin.AddResource(&User{}, &admin.Config{Menu: []string{"User Management"}})
```

## Resource Configuration

Available options when customize a Resource inside `admin.Config` are:

| Name       | Type              | Default | Description                                                                                         |
| ---        | ---               | ---     | ---                                                                                                 |
| Name       | string            |         | Display name of the resource                                                                        |
| Menu       | []string          |         | Menu setting of the resource, check [Menus](../admin/theming_and_customization.md#menus) for detail |
| Permission | *roles.Permission |         | Control the authority of the resource, check [Roles](../admin/authentication.md) for detail         |
| Themes     | []ThemeInterface  |         | Set [customized theme](../admin/theming_and_customization.md#themes) for the resource               |
| Priority   | int               |         | Control the display sequence in menu, ordered by ASC                                                |
| Singleton  | bool              | false   | Set the resource is a single object or multiple objects. e.g. "SEO setting" vs "Users"              |
| Invisible  | bool              | false   | Set whether the resource is visible in menu                                                         |
| PageCount  | int               | 20      | Pagination setting, Set how many records shall be shown per page                                    |
