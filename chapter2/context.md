# Context

Context is a set of commonly used informations. You can call it via `admin.Context`. This is useful when you customizing special logic inside [QOR Admin](../chapter2/setup.md). Like [Action with user input](../chapter2/actions.md#action-with-user-input).

## Common options

| Name | Type | Description |
| --- | --- | --- |
| CurrentUser | `type CurrentUser interface { DisplayName() string }` | The current logged in user. |
| DB | `*gorm.DB` | The current database. Please DO NOT use this option directly, use `admin.Context.GetDB()` instead. |
| Errors | `type Errors struct { errors []error }` | Current errors.  |
| Request | `*http.Request` | Current request. |
| ResourceID | `string` | The id of current resource instance. |
| Roles | `[]string` | [Roles](../plugins/roles.md) of current user. |
| Writer | `http.ResponseWriter` | Current response writer. |
