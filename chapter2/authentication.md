# Authentication

QOR Admin provides a flexible authentication and authorization solution. With it, you could integrate with your current authentication and authorization methods.

The authorization aspect of this interface lies in the `GetCurrentUser` method, while the remaining methods are all about authentication.

What you need to do is implement an `Auth` interface like below, and set it in the Admin value.

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

func (Auth) LogoutURL(*Context) string
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
