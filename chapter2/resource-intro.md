## Resource

#### Add a resource

```
  user := Admin.AddResource(&User{}, &admin.Config{Menu: []string{"User Management"}})
```

TODO: introduce other available configurations

#### Set field type

This set how the field of resource being managed in the QOR admin interface.

```
  user.Meta(&admin.Meta{Name: "Gender", Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female", "Unknown"}}})
```

TODO: common configuration of meta, like Label name, sequence

Here's the meta we supported

- [text input]()
- [select one]()
- [select many]()
- [file upload]()
- [rich text editor]()
- ...

#### Set CRUD attributes

This set what attributes of resource shall be shown in the index, new, edit, show page.

```
  // Attributes in index page
  user.IndexAttrs("Email", "Name", "Gender")

  // Attributes in new page
  user.NewAttrs("Email", "Name", "Gender")

  // Attributes in edit page
  user.EditAttrs("Email", "Name", "Gender")

  // Attributes in show page
  user.ShowAttrs("Email", "Name", "Gender")
```

#### Set search-able attributes

```
  user.SearchAttrs("User.Name", "User.Email")
```

This allow users to be searched by its name and email.

#### Set filters

Make resource filter-able by given setting

```
  user.Filter(&admin.Filter{
    Name: "Gender",
    Config: &admin.SelectOneConfig{
      Collection: []string{"Male", "Female", "Unknown"},
    },
  })
```

TODO: Check more detail about filter [here]()

#### Set action
#### Set resouce-specific theme
