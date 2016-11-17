# WildcardRouter

[WildcardRouter](https://github.com/qor/wildcard_router) is component that designed to handle dynamic routes.

[![GoDoc](https://godoc.org/github.com/qor/wildcard_router?status.svg)](https://godoc.org/github.com/qor/wildcard_router)

## Usage

Suppose you have a model `Page` that handle requests by `ServeHTTP` function based on URL in database.

```go
import (
  "github.com/qor/wildcard_router"
)

type PageHandler struct {}

type Page struct {
  URL string
  Body string
}

// Page's records in database:

// Record1(URL: /page1, Content: "Page1")
// Record2(URL: /page2, Content: "Page2")

func (PageHandler) ServeHTTP(w http.ResponseWriter, req *http.Request) {
  var page Page
  if !db.First(&page, "url = ?", req.URL.Path).RecordNotFound() {
    w.Write([]byte(page.Body))
  }
}
```

And you have another model `FAQ` also handle requests by `ServeHTTP` function based on URL in database too.

```go
type FAQHandler struct {}

type FAQ struct {
  URL      string
  Question string
  Answer   string
}

// FAQ's records in database:

// Record1(URL: /faq1, Question: "FAQ1", Answer: "Answer1")
// Record2(URL: /faq2, Question: "FAQ2", Answer: "Answer2")


func (FAQHandler) ServeHTTP(w http.ResponseWriter, req *http.Request) {
  var faq FAQ
  if !db.First(&faq, "url = ?", req.URL.Path).RecordNotFound() {
    w.Write([]byte(fmt.Sprintf("%v: %v", faq.Question, faq.Answer)))
  }
}
```

Those URLs could be anything, with no rule. Let's initialize [WildcardRouter](https://github.com/qor/wildcard_router) and mount it.

```go
func main() {
  mux := http.NewServeMux()

  wildcardRouter := wildcard_router.New()
  wildcardRouter.MountTo("/", mux)
}
```

`AddHandler` to [WildcardRouter](https://github.com/qor/wildcard_router). Any model that implemented method `ServeHTTP` could be a handler.

```go
  wildcardRouter.AddHandler(PageHandler{})
  wildcardRouter.AddHandler(FAQHandler{})
```

The behavior will be:

```go
  // Visit "/page1"   will return "Page1"
  // Visit "/page2"   will return "Page2"
  // Visit "/faq1"    will return "FAQ1: Answer1"
  // Visit "/faq2"    will return "FAQ2: Answer2"
  // Visit "/unknown" will return "404 page not found" with statu code 404
```
