# Sequences

What is a sequence?

## The Code object

```json
{
    "id": "d4cfb49f-035b-46f0-b7b4-ec733aee2d00",
    "name": "Some series",
    "prefix": "XX",
    "padding": 7,
    "suffix": "Y",
    "ownerId": "db0db654-1694-4535-ae91-4506a206e4c2"
}
```

Information about the code object

## The Entry object

```json
{
    "id": "550d89db-2b53-476c-a011-f796e196b053",
    "codeId": "d4cfb49f-035b-46f0-b7b4-ec733aee2d00",
    "idx": 186,
    "value": "XX0000186Y"
}
```

Information about the entry object

## Create a code

```shell
curl "https://api.invopop.dev/sequence/:owner_id/code" \
  -H "Authorization: Bearer your.api.key"
  -H "Content-Type: application/json"
  -d '{ \
        "id":"175ec7e5-8c14-46fd-90ff-ccb37d71550c", \
        "name":"Some series", \
        "prefix": "XX", \
        "suffix": "Y", \
        "padding": 7 \
      }'
```

```go
import (
    "bytes"
    "encoding/json"
    "net/http"
)

type Code struct {
    ID      string `json:"id"`
    Name    string `json:"name"`
    Preffix string `json:"preffix"`
    Suffix  string `json:"suffix"`
    Padding int    `json:"padding"`
    OwnerID string `json:"ownerId"`
}

newCode := Code{
    ID: "175ec7e5-8c14-46fd-90ff-ccb37d71550c"
    Name: "Some series",
    Preffix: "XX",
    Suffix: "Y",
    Padding: 7
}
newCodeBytes, encodingError := json.Marshall(newCode)
if encodingError != nil {
    // Deal with possible encoding errors
}

httpClient := &http.Client{}
data := bytes.NewBuffer(newCodeBytes)
req, _ := http.NewRequest("POST", "https://api.invopop.dev/sequence/:owner_id/code", data)
req.Header.Set("Authorization", "Bearer your.api.token")
req.Header.Set("Content-Type", "application/json")

res, err := client.Do(req)
if err != nil {
    // Deal with possible network errors here...
}


var code Code
decodingErr := json.NewDecoder(res.Body).decode(&Code)
```

> Response

```json
{
    "id": "d4cfb49f-035b-46f0-b7b4-ec733aee2d00",
    "name": "Some series",
    "prefix": "XX",
    "padding": 7,
    "suffix": "Y",
    "ownerID": "db0db654-1694-4535-ae91-4506a206e4c2"
}
```


Explain what the operation does

### HTTP Request

`POST /sequence/:owner_id/code`

### Parameters

Parameter    | Type    | Description
------------ | ------- | -----------
owner_id     | path    | Id of the company that creates the code
id           | body    | UUID that will identify the code
name         | body    | Descriptive name of the code
prefix       | body    | Sequence of characters that will preffix every entry of the code
suffix       | body    | Sequence of characters that will suffix every entry of the code
padding      | body    | Max number of digits the code entries will have (they will be filled with 0s)

## Retrieve a code

Something here

## List all codes

Something here

## Create entry for a code

Something here

## Retrieve entry for a code

Something here

## Retrive code's entries

Something here
