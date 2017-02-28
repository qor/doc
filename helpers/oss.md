# OSS(Object Storage Service)

[QOR OSS](https://github.com/qor/oss) provides common interface to operate files in cloud storage, ftp, filesystem etc.

## Usage

Currently, [QOR OSS](https://github.com/qor/oss) only support file system and S3. But you can easily implement your own storage strategies by implementing the interface.

```go
// StorageInterface define common API to operate storage
type StorageInterface interface {
  Get(path string) (*os.File, error)
  Put(path string, reader io.Reader) (*Object, error)
  Delete(path string) error
  List(path string) ([]*Object, error)
  GetEndpoint() string
}
```

Here's an example about how to use [QOR OSS](https://github.com/qor/oss) with S3. After initialized the s3 storage, The functions in the interface are available.

```go
import (
    "github.com/oss/s3"
)

func main() {
    storage := s3.New(s3.Config{AccessID: "access_id", AccessKey: "access_key", Region: "region", Bucket: "bucket", Endpoint: "cdn.getqor.com", ACL: aws.BucketCannedACLPublicRead})

    // Save a reader(io.Reader) interface into storage
    storage.Put("/sample.txt", reader)

    // Get file with path
    storage.Get("/sample.txt")

    // Delete file with path
    storage.Delete("/sample.txt")

    // List all objects under path
    storage.List("/")

    // Get the end point of current storage
    storage.GetEndpoint()
}
```
