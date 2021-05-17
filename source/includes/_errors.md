# Errors

Invopop uses conventional HTTP response codes to indicate the success or failure of an API request. In general: Codes in the 2xx range indicate success. Codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted, etc.). Codes in the 5xx range indicate an error with Invopop's servers.

Error Code | Meaning
---------- | -------
400 | Bad Request -- The request was unacceptable, often due to missing a required parameter.
401 | Unauthorized -- Your API key is wrong.
404 | Not Found -- The request resource does not exists.
405 | Method Not Allowed -- The request HTTP method is not supported.
429 | Too Many Requests -- Too many requests hit the API too quickly. We recommend an exponential backoff of your requests.
5xx | Internal Server Error -- Something went wrong on Invopop's end (this are rare).

When there is an error in a request, the error body contains should contain info about the kind of error that has ocurred.

## Attributes

> Example error response body:

```json
{
    "code": "missing-parameters",
    "humanMessage": "The parameter xxxx is missing",
    "devMessage": "More info about the right parameters for the request could be found in http://docs.invopop.dev#request"
}
```

- **code** For errors that could be handled programmatically, a short string indicating the type of error.

- **humanMessage** A message string that could be shown to the user.

- **devMessage** A message string with more information about the cause an how to solve the error. Could contain a link to a URL with more information.

## Handling Errors

```shell
# No error handling could be done using cURL
```

```go
type InvopopError struct {
    code string
    humanMessage string
    devMessage string
}

res, err := client.Do(req)
if err != nil {
    // Deal with possible connecction errors here...
}

if res.StatusCode >= 400 || res.StatusCode <= 600 {
    var errorInfo InvopopError
    _ := json.NewDecoder(res.Body).decode(&errorInfo)
    
    if res.StatusCode == 400 {
        fmt.Printf("The request failed with cause: %s", errorInfo.humanMessage)
    }
    if res.StatusCode == 429 {
        // Implement an exponential backoff of the request ...
    }
}
```

The right way of handling an error will depend on your application and the implementation language, but some general recomendations could be made.
