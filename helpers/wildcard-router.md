# WildcardRouter

[WildcardRouter](https://github.com/qor/wildcard_router) is component that used to handle dynamic routing.

You could use wildcard_router to satisfy below scenario:

* You have a Module `Page` that handle requests based on saved page URL
* You have another Module `FAQ` also handle requests based on saved page URL
* Those URLs could be any things, with no rule

[![GoDoc](https://godoc.org/github.com/qor/wildcard_router?status.svg)](https://godoc.org/github.com/qor/wildcard_router)

## Usage

```go
import (
  "github.com/qor/wildcard_router"
)

type PageHandler struct {}

func (PageHandler) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    // Page Model definition:
    // type Page struct {
    //   URL string
    //   Body string
    // }
    // Page has below records:
    //   Record1(URL: /page1, Content: "Page1")
    //   Record2(URL: /page2, Content: "Page2")
  var page Page
  if !db.First(&page, "url = ?", req.URL.Path).RecordNotFound() {
    w.Write([]byte(page.Body))
  }
}

type FAQHandler struct {}

func (FAQHandler) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    // FAQ Model definition:
    // type FAQ struct {
    //   URL      string
    //   Question string
    //   Answer   string
    // }
    // FAQ has below records:
    //   Record1(URL: /faq1, Question: "FAQ1", Answer: "Answer1")
    //   Record2(URL: /faq2, Question: "FAQ2", Answer: "Answer2")
  var faq FAQ
  if !db.First(&faq, "url = ?", req.URL.Path).RecordNotFound() {
    w.Write([]byte(fmt.Sprintf("%v: %v", faq.Question, faq.Answer)))
  }
}

func main() {
  mux := http.NewServeMux()

  wildcardRouter := wildcard_router.New()
  wildcardRouter.MountTo("/", mux)

  // Any module that implemented method ServeHTTP could be Handler
  wildcardRouter.AddHandler(PageHandler{})
  wildcardRouter.AddHandler(FAQHandler{})

  // Visit "/page1"   will return "Page1"
  // Visit "/page2"   will return "Page2"
  // Visit "/faq1"    will return "FAQ1: Answer1"
  // Visit "/faq2"    will return "FAQ2: Answer2"
  // Visit "/unknown" will return "404 page not found" with statu code 404
}
```
