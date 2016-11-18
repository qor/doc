# Media Library

[Media Library](https://github.com/qor/media_library) is a [Golang](http://golang.org/) library that supports the upload of *files*/*images*/*videos* to a filesystem or cloud storage as well as *linked videos* (i.e. YouTube, Vimeo, etc.). The plugin includes:

- cropping and resizing features for images.
- optional multiple sizes for each media resource.
- Accessibility helpers.

[![GoDoc](https://godoc.org/github.com/qor/media_library?status.svg)](https://godoc.org/github.com/qor/media_library)

###### File Types

[Media Library](https://github.com/qor/media_library) accepts any and every file type, yet it associates certain file types as *images* or *videos* so as to provide helpers supporting those media's specific needs.


    Images: .jpg, .jpeg, .png, .tif, .tiff, .bmp, .gif

    Videos: .mp4, .m4p, .m4v, .m4v, .mov, .mpeg, .webm, .avi, .ogg, .ogv


## Usage

[Media Library](https://github.com/qor/media_library) depends on [GORM](https://github.com/jinzhu/gorm) models as it is using [GORM](https://github.com/jinzhu/gorm)'s callbacks to handle file processing, so you will need to register callbacks first:

```go
import "github.com/jinzhu/gorm"
import "github.com/qor/media_library"

DB, err = gorm.Open("sqlite3", "demo_db") // [gorm](https://github.com/jinzhu/gorm)

media_library.RegisterCallbacks(&DB)
```

Add [Media Library](https://github.com/qor/media_library) support to model.

You can upload file to FileSystem.

```go
import "github.com/qor/media_library"

type Product struct {
  gorm.Model
  Image media_library.FileSystem
}
```

Or upload file to s3

```go
import "github.com/qor/media_library/aws"

type Product struct {
  gorm.Model
  Image aws.S3
}
```

And you're done setting up! You could use it like this:

```go
var product Product

if productImage, err := os.Open("product_image.png"); err == nil {
  product.Image.Scan(productImage)
}

DB.Save(&product)

// Get image's url, will be s3 url if it is uploaded to s3
product.Image.URL()
```

## Advanced usage

You can predefine common image size and fetch it easily.

```go
type ProductIconImageStorage struct{
  media_library.FileSystem
}

func (ProductIconImageStorage) GetSizes() map[string]media_library.Size {
  return map[string]media_library.Size{
    "small":    {Width: 60 * 2, Height: 60 * 2},
    "small@ld": {Width: 60, Height: 60},

    "middle":    {Width: 108 * 2, Height: 108 * 2},
    "middle@ld": {Width: 108, Height: 108},

    "big":    {Width: 144 * 2, Height: 144 * 2},
    "big@ld": {Width: 144, Height: 144},
  }
}

// Get image's url with style
product.Image.URL("small")
product.Image.URL("big@ld")
```

## Accessibility helpers

Media Library has some features aimed at helping achieve Accessibile frontends:

- capture of a textual description for *images*, *videos*, and *linked videos* to aid with Accessibility.
- capture of textual transcript for *videos* and *linked videos* to aid with Accessibility.

The values captured are fed into the sub-templates for each media type to be used if/where necessary. For example, an *image*'s HTML output (an `img` tag) manifests the textual description within an `alt` attribute while a video's HTML (an `iframe` tag) manifests the textual description within a `title` attribute.
