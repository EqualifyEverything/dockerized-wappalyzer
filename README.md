# Wappalyzergo

A high performance port of the Wappalyzer Technology Detection Library to Go. Inspired by [Webanalyze](https://github.com/rverton/webanalyze) and [Project Discovery](https://github.com/projectdiscovery/wappalyzergo)

Uses data from https://github.com/AliasIO/wappalyzer

## Features

- Very simple and easy to use, with clean codebase.
- Normalized regexes + auto-updating database of wappalyzer fingerprints.
- Optimized for performance: parsing HTML manually for best speed.
- Dockerized Endpoint

## Getting Started

### Docker Variables
- `PORT`: the port number to listen on
- `LOG_LEVEL`: log level (`debug`, `info`, `warn`, `error`)
- `HTTP_PROXY`: HTTP proxy address
- `HTTPS_PROXY`: HTTPS proxy address

### API Endpoint
- Send a GET request to `http://your_server_address:port?url=<url_to_analyze>` to analyze a URL.
- Replace `your_server_address` with the IP address or domain name of the server running the application.
- Replace `port` with the value of the `PORT` variable.

### Example
```sh
docker run -p 8087:8087 -e PORT=8087 -e LOG_LEVEL=info -d ghcr.io/equalifyapp/
```

This will start the application and expose port 8087 on the host machine. You can then access the API using the endpoint described above.



### Using *go install*

```sh
go install -v github.com/projectdiscovery/wappalyzergo/cmd/update-fingerprints@latest
```

After this command *wappalyzergo* library source will be in your current go.mod.

## Example
Usage Example:

``` go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"

	wappalyzer "github.com/projectdiscovery/wappalyzergo"
)

func main() {
	resp, err := http.DefaultClient.Get("https://www.hackerone.com")
	if err != nil {
		log.Fatal(err)
	}
	data, _ := ioutil.ReadAll(resp.Body) // Ignoring error for example

	wappalyzerClient, err := wappalyzer.New()
	fingerprints := wappalyzerClient.Fingerprint(resp.Header, data)
	fmt.Printf("%v\n", fingerprints)

	// Output: map[Acquia Cloud Platform:{} Amazon EC2:{} Apache:{} Cloudflare:{} Drupal:{} PHP:{} Percona:{} React:{} Varnish:{}]
}
```
/Volumes/Macintosh Hd/Users/Shared/GitHub/Orgs/TheBoatyMcBoatFace/wappalyzergo/wappalyzergo_test.go