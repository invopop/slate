# Sequences

A sequence can be defined as an ordered collection of terms, in our case,
without repetitions. The order is based on a deterministic pattern and, a term
is build using the previous one. In the `Sequence` service, the construction of
the terms is described by the `Code` object, being like the specification to
create the next sequence's term. So the `Entry` object would be a term of the
sequence, having a reference to its previous term except for the first term.

## The Code object

```json
{
    "id": "d4cfb49f-035b-46f0-b7b4-ec733aee2d00",
    "name": "Some series",
    "prefix": "XX",
    "padding": 7,
    "suffix": "Y"
}
```

The object has three properties to create the terms of its sequence, `prefix`,
`padding` and `suffix`. All the terms will share the same `prefix` and
`suffix`, `padding` tells how many digits are set (at most) between those fixed
values. Those digits represent a numerical value that always starts with one.
Independently of the actual amount of digits of that number, the `padding`
ensures the size of that portion of the entry's value, filling the gap with
leading zeros.

## The Entry object

```json
{
    "id": "550d89db-2b53-476c-a011-f796e196b053",
    "value": "XX0000186Y",
    "prev_id": "..."
}
```

Each entry has a value built from the code properties, and it's linked to other entry. Except for the first one. Similar to a singly linked list. The following object would be the next entry for the previous object.

```json
{
    "id": "2a8ece9f-7590-4e8b-a4f5-82d1d0ee16f6",
    "value": "XX0000187Y",
    "prev_id": "550d89db-2b53-476c-a011-f796e196b053"
}
```

## Create a code

```shell
curl --request POST "https://api.invopop.dev/sequence/code" \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
        "id":"fb3ad823-65fe-4a15-beff-7dfb186c7b29",
        "name":"Some series",
        "prefix": "XX",
        "suffix": "Y",
        "padding": 7
      }'
```

```go
import (
	sequence "github.com/invopop/client/sequence"
)

newCode := &sequence.CodeParameters{
    ID: "fb3ad823-65fe-4a15-beff-7dfb186c7b29"
    Name: "Some series",
    Preffix: "XX",
    Suffix: "Y",
    Padding: 7
}

client := sequence.New("https://api.invopop.dev", "<API_KEY>")

code, err := client.CreateCode(newCode)
if err != nil {
    // Deal with possible client error here...
}
```

> Response

```json
{
    "id": "fb3ad823-65fe-4a15-beff-7dfb186c7b29",
    "name": "Some series",
    "prefix": "XX",
    "padding": 7,
    "suffix": "Y"
}
```

Creates a new code, that will define the specification for a new sequence.

### HTTP Request

`POST /sequence/code`

### Parameters

Parameter    | Type    | Description
------------ | ------- | -----------
id           | body    | UUID that will identify the code
name         | body    | Descriptive name of the code
prefix       | body    | Sequence of characters that will preffix every entry of the code
suffix       | body    | Sequence of characters that will suffix every entry of the code
padding      | body    | Max number of digits the code entries will have (they will be filled with 0s)

## Retrieve a code

```shell
curl "https://api.invopop.dev/sequence/code/:code_id" \
  -H "Authorization: Bearer <API_KEY>"
```

```go
import (
	sequence "github.com/invopop/client/sequence"
)

codeID := "fb3ad823-65fe-4a15-beff-7dfb186c7b29"
client := sequence.New("https://api.invopop.dev", "<API_KEY>")

code, err := client.FetchCode(codeID)
if err != nil {
    // Deal with possible client error here...
}
```

> Response

```json
{
    "id": "fb3ad823-65fe-4a15-beff-7dfb186c7b29",
    "name": "Some series",
    "prefix": "XX",
    "padding": 7,
    "suffix": "Y"
}
```

Retrieves a specific code object using the given UUID

### HTTP Request

`GET /sequence/code/:code_id`

### Parameters

Parameter    | Type    | Description
------------ | ------- | -----------
code_id      | path    | UUID that identifies the code

## List all codes

```shell
curl "https://api.invopop.dev/sequence/codes" \
  -H "Authorization: Bearer <API_KEY>"
```

```go
import (
	sequence "github.com/invopop/client/sequence"
)

client := sequence.New("https://api.invopop.dev", "<API_KEY>")

codeCollection, err := client.FetchCodeCollection()
if err != nil {
    // Deal with possible client error here...
}
```

> Response

```json
{
    "codes": [
        {
            "id": "fb3ad823-65fe-4a15-beff-7dfb186c7b29",
            "name": "Some series",
            "prefix": "XX",
            "padding": 7,
            "suffix": "Y"
        },
        ...
    ]
}
```

Retrieves all the code objects belonging to the owner.

### HTTP Request

`GET /sequence/codes`

### Parameters

Parameter    | Type    | Description
------------ | ------- | -----------
\-           | \-      | \-

## Create entry for a code

```shell
curl --request POST "https://api.invopop.dev/sequence/code/:code_id/entry" \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
        "id": "175ec7e5-8c14-46fd-90ff-ccb37d71550c",
        "meta": {
          "key": "value"
        }
      }'
```

```go
import (
	sequence "github.com/invopop/client/sequence"
)

codeID := "fb3ad823-65fe-4a15-beff-7dfb186c7b29"
newEntry := &sequence.EntryParameters{
    ID: "175ec7e5-8c14-46fd-90ff-ccb37d71550c"
    Meta: map[string]string{
        "key": "value",
    }
}

client := sequence.New("https://api.invopop.dev", "<API_KEY>")

entry, err := client.CreateEntry(newEntry)
if err != nil {
    // Deal with possible client error here...
}
```

> Response

```json
{
    "id": "175ec7e5-8c14-46fd-90ff-ccb37d71550c",
    "value": "XX0000001Y",
    "meta": {
        "key": "value"
    }
}
```

Creates a new entry object, following the properties and sequence of the specified code.

### HTTP Request

`POST /sequence/code/:code_id/entry`

### Parameters

Parameter    | Type    | Description
------------ | ------- | -----------
code_id      | path    | UUID that identifies the code
id           | body    | UUID that will identify the entry
meta         | body    | Metadata object

## Retrieve entry for a code

```shell
curl "https://api.invopop.dev/sequence/code/:code_id/entry" \
  -H "Authorization: Bearer <API_KEY>"
```

```go
import (
	sequence "github.com/invopop/client/sequence"
)

codeID := "fb3ad823-65fe-4a15-beff-7dfb186c7b29"
entryID := "175ec7e5-8c14-46fd-90ff-ccb37d71550c"
client := sequence.New("https://api.invopop.dev", "<API_KEY>")

entry, err := client.FetchEntry(codeID, entryID)
if err != nil {
    // Deal with possible client error here...
}
```

> Response

```json
{
    "id": "175ec7e5-8c14-46fd-90ff-ccb37d71550c",
    "value": "XX0000001Y",
    "meta": {
        "key": "value"
    }
}
```

Retrieves a specific entry object using the given UUID.

### HTTP Request

`GET /sequence/code/:code_id/entry/:entry_id`

### Parameters

Parameter    | Type    | Description
------------ | ------- | -----------
code_id      | path    | UUID that identifies the code
entry_id     | path    | UUID that identifies the entry


## Retrive code's entries

```shell
curl "https://api.invopop.dev/sequence/code/:code_id/entries" \
  -H "Authorization: Bearer <API_KEY>"
```

```go
import (
	sequence "github.com/invopop/client/sequence"
)

codeID := "fb3ad823-65fe-4a15-beff-7dfb186c7b29"
client := sequence.New("https://api.invopop.dev", "<API_KEY>")

entry, err := client.FetchEntryCollection(codeID)
if err != nil {
    // Deal with possible client error here...
}
```

> Response

```json
{
    "entries": [
        {
            "id": "175ec7e5-8c14-46fd-90ff-ccb37d71550c",
            "value": "XX0000001Y",
            "meta": {
                "key": "value"
            }
        },
        ...
    ]
}
```

Retrieves all the entries objects belonging to the specified code.

### HTTP Request

`GET /sequence/code/:code_id/entries`

### Parameters

Parameter    | Type    | Description
------------ | ------- | -----------
code_id      | path    | UUID that identifies the code
