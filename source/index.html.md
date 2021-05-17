---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - go

toc_footers:
  - <a href='https://access.invopop.dev/'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the Invopop API! You can use our API to create invoices, sign them, create PDFs for them and much more...

Through the whole documentation you will have access to examples in Go and directly using the command line utility CURL.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "Authorization: Bearer your.api.token"
```

```go
import (
    "net/http"
)

httpClient := &http.Client{}
req, _ := http.NewRequest("GET", "api_endpoint_here", nil)
req.Header.Set("Authorization", "Bearer your.api.token")
req, err := client.Do(req)
```

> Make sure to replace `your.api.token` with your API key.

The Invopop API uses API keys to authenticate requests. You can create and manage your keys in [your profile page](https://access.invopop.dev/profile).

Authentication is performed using [HTTP bearer auth](https://stackoverflow.com/questions/25838183/what-is-the-oauth-2-0-bearer-token-exactly/25843058) and you will never have to provide a password.

All API requests must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTP_Secure). Calls made over plain HTTP will fail. API requests without authentication will also fail.

<aside class="notice">
Your API keys carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.
</aside>

# Versions

```shell
curl "https://api.invopop.dev/auth/version" \
  -H "Authorization: Bearer your.api.key"
```

```go
import (
    "encoding/json"
    "net/http"
)

httpClient := &http.Client{}
req, _ := http.NewRequest("GET", "https://api.invopop.dev/auth/version", nil)
req.Header.Set("Authorization", "Bearer your.api.token")

res, err := client.Do(req)
if err != nil {
    // Deal with possible errors here...
}

var version string
decodingErr := json.NewDecoder(res.Body).decode(&version)
// Here version will contain the last version of the API
```

Right now we only have one version of the API that is continuously updated, but that could change in the future.

