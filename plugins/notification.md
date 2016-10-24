# QOR Notification

QOR Notification is built for send notification to the users. Like order, delivery notices.

## Usage

```go
Notification := notification.New(&notification.Config{})

// Add to Admin
Admin.NewResource(Notification)

// Register Database Channel
Notification.RegisterChannel(database.New(&database.Config{DB: db.DB}))

// Send Notification
Notification.Send(message *Message, context *qor.Context)

// Get Notificatio
Notification.GetNotification(user interface{}, messageID string, context *qor.Context) *QorNotification

// Get Notifications
Notification.GetNotifications(user interface{}, context *qor.Context)

// Get Unresolved Notifications Count
Notification.GetUnresolvedNotificationsCount(user interface{}, context *qor.Context)
```

## Register Actions for Notification

```go
Notification.Action(&notification.Action{
        Name:         "Dismiss",
        MessageTypes: []string{"info", "order_processed", "order_returned"},
        Visible: func(data *notification.QorNotification, context *admin.Context) bool {
                return data.ResolvedAt == nil
        },
        Handle: func(argument *notification.ActionArgument) error {
                return argument.Context.GetDB().Model(argument.Message).Update("resolved_at", time.Now()).Error
        },
})
```
