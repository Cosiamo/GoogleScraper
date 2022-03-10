# Notes
Created GitHub repo

Started with `go mod init github.com/Cosiamo/GoogleScrapper`

In `main.go` I'm importing the following packages:
```go
import (
	"fmt"

	"math/rand"
	// for http requests
	"net/http"
	"net/url"
	"strings"
	"time"

	// help with scrapping from google
	"github.com/PuerkitoBio/goquery"
)
```

Because I'm using an external package I needed to install it: `go get "github.com/PuerkitoBio/goquery"`

- countryCode is like .jp for Japan, .fr for France, .uk for England, .com for global, etc...
	- using countryCode to search in GoogleDomains
```go
// list of all available Google domains
var googleDomains = map[string]string {
	// using "search?q=" to start a search query
	// appending a query at the end of it depending on the countryCode

	// global
	"com":"https://www.google.com/search?q=",
	// Zimbabwe
	"za":"https://www.google.co.za/search?q="
}
```

- fmt.Sprintf is used to format a string

- languageCode is en for English, fr for French, es for Spanish, etc...

- "%s%s&num=%d&hl=%s&start=%d&filter=0"
	- first %s is `googleBase`
	- second %%s is `searchTerm`
	- num=%d is `count` (how many results do you want back)
	- hl=%s is `languageCode`
	- start=%d is `start` (in the for loop in buildGoogleUrls, `start` is `i` multiplied by `count`)

- `proxyString` is there if you want the server or computer to make a proxy request to Google

### GoogleScrape function
- `results` is the slice of `SearchResults`
- since the for loop is going on, we'll receive `SearchResults` one by one from passing that result to `googleResultParsing`
#### Order
- make a request to `googlePages` (the Google URL) 
- receive via `scrapeClientRequest`
- parse it (from the structure of `SearchResult`) with `googleResultParsing`
- then the `data` we receive from `googleResultParsing`, we'll range over it and append that to our `results` slice


# func googleResultParsing
- When we received some response from the request to Google, the document format can query the document to run find functions, string functions, trimming functions, etc... so we can run on a document
	- that's why we convert it into a document
- In that document we want to find "div.g" because that will contain every single request