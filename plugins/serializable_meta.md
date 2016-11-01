# Serializable Meta

[Serializable Meta](https://github.com/qor/serializable_meta) allows the developer to specify, for a given model, a custom serialization model along with field mappings. This mechanism thus allows one model to act as another model when it comes to serialization.

[![GoDoc](https://godoc.org/github.com/qor/serializable_meta?status.svg)](https://godoc.org/github.com/qor/serializable_meta)

### Serializable Model Definition

```go
type QorJob struct {
  gorm.Model
  Name string
  serializable_meta.SerializableMeta // Embed serializable_meta.SerializableMeta to apply the serializable feature
}

// Needs method GetSerializableArgumentResource, so `Serializable Meta` can know your saving argument's type
func (qorJob QorJob) GetSerializableArgumentResource() *admin.Resource {
  return jobsArgumentsMap[qorJob.Kind]
}

var jobsArgumentsMap = map[string]*admin.Resource{
  "newsletter": admin.NewResource(&sendNewsletterArgument{}),
  "import_products": admin.NewResource(&importProductArgument{}),
}

type sendNewsletterArgument struct {
  Subject      string
  Content      string `sql:"size:65532"`
}

type importProductArgument struct {
  ProductsCSV media_library.FileSystem
}
```

### Usage

```go
// Save QorJob with argument into database
var qorJob QorJob
qorJob.Name = "sending newsletter"
qorJob.Kind = "newsletter"
qorJob.SetSerializableArgumentValue(&sendNewsletterArgument{
  Subject: "subject",
  Content: "content",
})

db.Create(&qorJob) // will Marshal `sendNewsletterArgument` as json, and save it into database column `value`
// INSERT INTO "qor_jobs" (kind, value) VALUES (`newsletter`, `{"Subject":"subject","Content":"content"}`);

// Get QorJob and its argument from database
var result QorJob
db.First(&result, "name = ?", "sending newsletter")

// Use its argument
var argument = result.GetSerializableArgument(result)
argument.(*sendNewsletterArgument).Subject // "subject"
argument.(*sendNewsletterArgument).Content // "content"
```
