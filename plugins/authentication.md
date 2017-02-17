# Authentication

[QOR Admin](../chapter2/setup.md) allows you to integrate your current authentication methods by providing an interface for common Authentication related tasks. For instance, you could integrate [AuthBoss](https://github.com/go-authboss/authboss) with [QOR Admin](../chapter2/setup.md).

Note: to solve your Authorization needs, please refer to [Roles](roles.md).

What you need to do is implement an `Auth` interface like below, and set it in the [QOR Admin](../chapter2/setup.md) value.

```go
type Auth interface {
  GetCurrentUser(*Context) qor.CurrentUser // get current user, if don't have permission, then return nil
  LoginURL(*Context) string // get login url, if don't have permission, will redirect to this url
  LogoutURL(*Context) string // get logout url, if click logout link from admin interface, will visit this page
}
```

Here is an example:

```go
type Auth struct{}

func (Auth) LoginURL(c *admin.Context) string {
  return "/login"
}

func (Auth) LogoutURL(c *admin.Context) string {
  return "/logout"
}

func (Auth) GetCurrentUser(c *admin.Context) qor.CurrentUser {
  if userid, err := c.Request.Cookie("userid"); err == nil {
    var user User
    if !DB.First(&user, "id = ?", userid.Value).RecordNotFound() {
      return &user
    }
  }
  return nil
}

func (u User) DisplayName() string {
  return u.Name
}

// Register Auth for QOR Admin
Admin.SetAuth(&Auth{})
```
