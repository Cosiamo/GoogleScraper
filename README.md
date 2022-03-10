# About
Simple web scrapper that grabs search results from Google. Not meant to be a standalone application or package. Using the logic from this for a different project.

# SetUp
Clone this repo then run the command `go get "github.com/PuerkitoBio/goquery"` for the goquery package. After that run the commands `go build` to build the application and `go run main.go` to run it. Searches for GitHub by default, check next section on how to change that.

# How to use
In the `main` function there's a call to the function `GoogleScrape`. That's what determines what's being searched, as well as the search parameters.
```go
res, err := GoogleScrape("github", "com", "en", nil, 1, 30, 10)
```
If you run this with the default settings the output should look something similar to this:
```
{1 https://github.com/  }
{2 https://github.com/about  }
{3 https://github.com/explore  }
{4 https://github.com/login  }
{5 https://github.com/github  }
{6 https://github.com/features  }
{7 https://desktop.github.com/  }
{8 https://pages.github.com/  }
{9 https://en.wikipedia.org/wiki/GitHub  }
{10 https://docs.github.com/en  }
{11 https://octoverse.github.com/  }
{12 https://twitter.com/github  }
...
...
```

### GoogleScrape Parameters In Order
- Searching for "github" 
- Using Google's global search domain
    - .com
    - can set it to region specific, such as: "uk" for .co.uk
- Language is set to English
- Using the value `nil` for the proxy string
    - this is for VPN or server configuration
- Only calling one page
- 30 results per page
- Waits 10 seconds 

# Future Plans
I want to eventually make an application that aggregates the best results from the various different search engines without the advertisement links that appear at the top. I'm probably going to call it "Sea Urchin" (because it sounds like "searchin"). To run it as a command in the terminal you would type `sea`. I'm also considering making a GUI with NextJS and Electron, however my main focus currently is on the CLI. 

I want to take a few small steps to get to this point, this Google Scrapper being the first step.

My next step is to make a program with the ability to search on Google through the CLI. This *should* be relatively easy.
- Type in search term then print out links and the description of the result
    - so if you're searching for GitHub you would type `sea github`
    - you would receive the links and the description of that site page
- To change the settings you would need to type in certain flags
    - off the top of my head, I'm thinking something like `sea -settings` to view your current settings
    - to change which Google domain you want to access `sea -settings -url`
    - language `sea -settings -lang`
    - proxy `sea -settings -proxy` (might hold off on proxy settings for now)
- For the results per page I might keep it fixed at 30, but give the user the option to receive up to 5 pages of results
    - would press a key to move forwards and backwards through the pages
    - either the left and right arrow keys or `ctrl+{something}`

Then make a web scrapper for Bing. It would essentially be the same thing as this program. I might do the same with DuckDuckGo, however I want to see how the next step goes.

After I make the BingScrapper I want to include it to Sea Urchin. There's two ways I'm thinking about implementing this. Either find the average of both, or because most people like Google more, favor Google's top 3 results and then find the average for the rest. This way Bing's results improve the rest of Google's results.

So if Google's results are weighed heavier and the same term is passed to both search engines, but only 2 of the top 3 results are similar, that additional link that's in Bing's top 3 is likely to be more relevant than whatever Google decided would be it's fourth option.

Let's say each of these letters are the results for the same search term.

|Google|Bing|
|---|---|
|1. a|1. a|
|2. b|2. c|
|3. c|3. e|
|4. d|4. b|
|5. e|5. d|

If I decide to consider Google's results to weigh more than Bing's, it would return the links in the order of: `a b c e d`. Whereas, if I decided to find the average, it would return: `a c b e d`. This is what I mean by using Bing to improve Google's later results.