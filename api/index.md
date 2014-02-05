---
layout: default
title: "Magic: The Gathering API Documentation"
nav-api: active
---

# API

## Overview

The DeckBrew Magic: The Gathering API is [open
source](https://github.com/kyleconroy/deckbrew-api).  Please report any issues
or bugs you encounter. This API wouldn't have been possible without the amazing
[mtgjson](http://mtgjson.com) and [mtgimage](http://mtgimage.com) resources.

All API access is over HTTPS, and accessed from the `api.deckbrew.com` domain.
All data is sent and received as JSON.

### Current Version

The DeckBrew API is currently in **beta**. Backwards incompatible changes
may be made at any time. 

> Content-Type: application/json

### Pagination

Requests that return multiple items will be paginated to 100 items by default.
You can specify further pages with the `?page` parameter.

    $ curl https://api.deckbrew.com/mtg/cards?page=2

Note that page numbering is 1-based and that omitting the `?page` parameter
will return the first page.

#### Link Header

The pagination info is included in the Link header. It is important to follow
these Link header values instead of constructing your own URLs.

    Link: <https://api.deckbrew.com/mtg/cards?page=3>; rel="next",
      <https://api.deckbrew.com/mtg/cards?page=1>; rel="prev"

The possible `rel` values are:

| Name | Description |
| ---- | ----------- |
| next | Shows the URL of the immediate next page of results.| 
| prev | Shows the URL of the immediate previous page of results. |

### Errors

Any response with a status code greater than or equal to 400 is considered an
error. An error object will be returned with an `errors` key pointing to a list
of errors for a given request.

> GET /mtg/page/not/found

```js
{
  "errors": [
    "Page not found"
  ]
}
```

## Magic Cards

### List all cards

Return a list of all Magic cards. Can be filtered using query string parameters
to narrow the search.

> GET /mtg/cards

```js
[
  {
    "name": "About Face",
    "id": "05194b17e11a7a45c7820c13f3ba8cbc",
    "types": [
      "instant"
    ],
    "colors": [
      "red"
    ],
    "cmc": 1,
    "cost": "{R}",
    "text": "Switch target creature's power and toughness until end of turn.",
    "url": "https://api.deckbrew.com/mtg/cards/05194b17e11a7a45c7820c13f3ba8cbc",
    "editions": [
      {
        "set": "Urza's Legacy",
        "rarity": "common",
        "artist": "Melissa A. Benson",
        "multiverse_id": 12414,
        "flavor": "The overconfident are the most vulnerable.",
        "number": "73",
        "layout": "normal",
        "url": "https://api.deckbrew.com/mtg/editions/12414",
        "image_url": "http://mtgimage.com/multiverseid/12414.jpg",
        "set_url": "https://api.deckbrew.com/mtg/sets/ULG"
      }
    ]
  }
]
```
#### Card Filtering

Cards can be filtering using query string parameters. Parameters with the
**same name** are evaluated as OR statements. For example, the query below will
find all red or blue rare cards in Unhinged.

> GET /mtg/cards?set=UNH&color=red&color=blue&rarity=rare

| Name | Type | Description |
| ---- | ---- | ----------- |
| `type` | `[]string` |  Any valid card type. Possible values include `enchantment` and|`artifact`. |
| `subtype` | `[]string` | Any valid card subtype. Possible values include `zombie` and `tribal`. |
| `supertype` | `[]string` | Any valid card supertype, such as `legendary`|
| `name` | `string` | A fuzzy match on a card's name |
| `set` | `[]string` | A three letter identifier for a Magic set |
| `rarity` | `[]string` | Select cards printed at this rarity |
| `color` | `[]string` | Select cards of the chosen color |
### Get a single card

> /mtg/cards/:id

```js
{
  "name": "About Face",
  "id": "05194b17e11a7a45c7820c13f3ba8cbc",
  "types": [
    "instant"
  ],
  "colors": [
    "red"
  ],
  "cmc": 1,
  "cost": "{R}",
  "text": "Switch target creature's power and toughness until end of turn.",
  "url": "https://api.deckbrew.com/mtg/cards/05194b17e11a7a45c7820c13f3ba8cbc",
  "editions": [
    {
      "set": "Urza's Legacy",
      "rarity": "common",
      "artist": "Melissa A. Benson",
      "multiverse_id": 12414,
      "flavor": "The overconfident are the most vulnerable.",
      "number": "73",
      "layout": "normal",
      "url": "https://api.deckbrew.com/mtg/editions/12414",
      "image_url": "http://mtgimage.com/multiverseid/12414.jpg",
      "set_url": "https://api.deckbrew.com/mtg/sets/ULG"
    }
  ]
}
```

## Magic Card Editions

### Get a single card edition

An edition is a specific print of a card or cards identified by it's
[Multiverse ID](http://gatherer.wizards.com/pages/Help.aspx). This endpoint
always returns an array of editions, as certain prints contain for than one
card, such as the split card [Turn //
Burn](https://api.deckbrew.com/mtg/editions/369080).

> GET /mtg/editions/:multiverseid

```js
[
  {
    "name": "About Face",
    "id": "05194b17e11a7a45c7820c13f3ba8cbc",
    "types": [
      "instant"
    ],
    "colors": [
      "red"
    ],
    "cmc": 1,
    "cost": "{R}",
    "text": "Switch target creature's power and toughness until end of turn.",
    "url": "https://api.deckbrew.com/mtg/cards/05194b17e11a7a45c7820c13f3ba8cbc",
    "editions": [
      {
        "set": "Urza's Legacy",
        "rarity": "common",
        "artist": "Melissa A. Benson",
        "multiverse_id": 12414,
        "flavor": "The overconfident are the most vulnerable.",
        "number": "73",
        "layout": "normal",
        "url": "https://api.deckbrew.com/mtg/editions/12414",
        "image_url": "http://mtgimage.com/multiverseid/12414.jpg",
        "set_url": "https://api.deckbrew.com/mtg/sets/ULG"
      }
    ]
  }
]
```
## Magic Sets

### List all sets

> GET /mtg/sets

```js
[
  {
    "id": "ARB",
    "name": "Alara Reborn",
    "border": "black",
    "type": "expansion",
    "url": "http://localhost:3000/mtg/sets/ARB"
  }
]
```

### Get a single set


> GET /mtg/sets/:id

```js
{
  "id": "ARB",
  "name": "Alara Reborn",
  "border": "black",
  "type": "expansion",
  "url": "http://localhost:3000/mtg/sets/ARB"
}
```

## Magic Types and Colors

These endpoints provide a list of possible values for specific search terms.

### List all types

> GET /mtg/types

```js
[
  "artifact",
  "creature",
  "enchantment",
  "instant",
  "land",
  "phenomenon",
  "plane",
  "planeswalker",
  "scheme",
  "sorcery",
  "tribal",
  "vanguard"
]
```

### List all supertypes

> GET /mtg/supertypes

```js
[
  "basic",
  "legendary",
  "ongoing",
  "snow",
  "world"
]
```

### List all subtypes

> GET /mtg/subtypes

```js
[
  "advisor",
  "ajani",
  "alara",
  "ally",
  "angel",
  "anteater",
]
```
### List all colors

> GET /mtg/colors

```js
[
  "black",
  "blue",
  "green",
  "red",
  "white"
]
```
