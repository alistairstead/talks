footer: \#inviqaDevDay
slidenumbers: false

## _**D**_ocumentation _**D**_riven
## _**D**_evelopment
#### _**Examples in an alternate form**_

---

### _BDD_
### Uses examples to deepen understanding

---

## Ubiquitous language

^We use a common language developed with business domain. We keep implementation detail away from our definition by example.

---

## Continuous conversation

^This conversation is described as input and output. Question and answer from a human perspective. We are driving the understanding of a business domain where the highest level consumer of the result is a human being.

---

## What if?

---

## What if the consumer of our software is a machine?

---

## What if your software has an API?

^Yes this will include business logic and that logic would be defined and model with conventional BDD examples. After all your domain layer is not your API.

---

## Your API is presentation / UI

---

## Your API is a representation

---

## It is an implementation detail

---

## But we try to keep this detail out of examples?

^And we keep our examples free from implementation detail, right? Because that makes our examples brittle.

---

## But...

---

### Here implementation detail is important!
#### Especially for the poor soul who has to integrate with it

---

## API have lots of important implementation detail

---

## Lots of out of band detail that is actually very important

* Headers
* HTTP Satus
* Body Structure

---

## Is it because it is technical that we treat it differently?

---

## The API is an interface to the application

^ It is the entry point, it is a system boundary that is very important

---

## Documentation for FREE...
### Thats a statment you have heard, right?

^ That sounds useful for creating documentation for how the API works...

---

## Regression suite for FREE...

^ That sounds useful to ensure our API works...

---

## Should we use Gherkin?

---

## Use Mink + Goutte?

---

## Anyone done that?

---

## What is the ubiquitous language to specify an API?

---

## HTTP

---

## What is the ubiquitous language of your business

---

## Its NOT HTTP!

---

## HTTP is a text based protocol
### So lets use that to our advantage
^Quote from the RFC

---

> "Model the problem domain and the solution will take care of itself"
-- David West, Object Thinking

---

## Apiary

---

## Blueprint
### Markdown format designed for _API_ definition

---

```markdown
FORMAT: 1A
# API Name
API description
# First Resource
Resource description
## Action [GET]
+ Response 200 (application/hal+json; charset=utf-8)
    + Headers
        ```
        Link: </>; rel="self", </tokens>; rel="tokens", </ping>; rel="ping"
        ```
    + Body
        ```
        {
            ...
        }
        ```
```

^ Fenced code blocks define the plain text representation of request and response

---

## Request & Response Pairs

```markdown
## API Entry Point [GET]
+ Request (application/hal+json)
+ Response 200 (application/hal+json; charset=utf-8)
    + Body
        ```
        {
            "_links": {
                "self": { "href": "/" },
                "ht:ping": { "href": "/ping" },
                "curie": {
                  "name": "ht",
                  "href": "/docs/{rel}",
                  "templated": true
                }
            },
            "name" : "example"
        }
        ```
```

---

```markdown
## API Entry Point [GET]
+ Request (application/json)
+ Response 200 (application/json; charset=utf-8)
    + Body
        ...
+ Request (application/json)
+ Response 200 (application/json; charset=utf-8)
    + Body
        ...
+ Request (text/html)
+ Response 200 (text/html; charset=utf-8)
    + Body
        ...
```

---

## Defined resource Models

```markdown
# User
+ Model (application/hal+json; charset=utf-8)
    + Body
        ```
        {
            "_links" : {
              ...
            },
            "name" : "Peter Parker",
            ...
        }
        ```
```

---

## Using Models
```markdown
+ Request (application/hal+json)
+ Response 201 (application/hal+json; charset=utf-8)
    [Ping][]
```

---

## _Automate_ the conversation

---

![inline](https://camo.githubusercontent.com/7def91db14bf263182d76f428506b7a2fe181a22/68747470733a2f2f7261772e6769746875622e636f6d2f617069617279696f2f64726564642f6d61737465722f696d672f44726564642e706e67)

---

![inline](https://raw.github.com/apiaryio/gavel/master/img/gavel.png)

---
## Build our first resource
```markdown
# Group Ping
## Ping [/ping]
Simply return the request back to you so that you
+ Model (application/hal+json; charset=utf-8)
    + Body
        ```
        {
            "_links": {"parent": {"href": "/"}, "self": {"href": "/ping"}},
            "name": "pong",
            "data": {
              "object": {
                "string": "value",
                "array": ["one", "2", "three"]
              }
        }
        ```
```

---

## HTTP GET `/ping` response

```markdown
### Validate a GET request [GET]
+ Response 200 (application/hal+json; charset=utf-8)
    + Body
        ```
        {
            "_links": {"parent": {"href": "/"}, "self": {"href": "/ping"}},
            "name": "pong"
        }
        ```
```

---

## HTTP POST `/ping` request

```markdown
### Validate a POST request body [POST]
+ Request (application/json)
      ```
      {
          "param_object": {
            "param_string": "value one",
            "param_array": ["one", "2", "three"]
          }
      }
      ```
```

---

## HTTP POST `/ping` response

```markdown
+ Response 201 (application/hal+json; charset=utf-8)
    [Ping][]
```

---

## HTTP POST `/ping` response

```markdown
+ Response 201 (application/hal+json; charset=utf-8)
    ```
    {
        "_links": {"parent": {"href": "/"}, "self": {"href": "/ping"}},
        "name": "pong",
        "data": {
          "object": {
            "string": "value",
            "array": ["one", "2", "three"]
          }
    }
    ```
```

---

## Great, we can now describe our API

^ Using the ubiquitous language of an API...

---

## And we can use this to drive testing the UI...

---

## Wait...

---

## We have very brittle validation tools

---

## The format and structure is important

---

## The data is not important!

---

## The data is the _**implementation detail!**_

^ The implementation detail that earlier we said we did not want to leak into our testing and specification

---

## How can we decouple our structure from the data?

---

## JSON Scheme can help
#### It can, honest!

---

## Add schema to our model

```markdown
+ Model (application/hal+json; charset=utf-8)
    + Body
        ```
        {
            ...
        }
        ```
    + Schema
        ```
        {
            ...
        }
        ```
```

---

## Example schema

```javascript
{
    "type": "object",
    "required": false,
    "properties": {
        "_links": {
            "type": "object",
            "required": true,
            "properties": {
                "parent": {
                    ...
                },
                ...
            }
        }
        ...
    }
```

---

### Finally, we now have stable validation of our API

---

## Problems...

* The parser currently does not provide helpful error output
* Query parameters are not particularly flexible
* Duplication becomes a problem in the format

---

## _**So what now?**_

---

## Im not saying don't use Gherkin!

---

## I'm saying use it in the right place

^The right layer of your application or the correct context

---

## Drive your core domain with BDD scenarios

---

## Drive your interface with the appropriate tool

---

### For an API this may be `Dredd`...
### _for right now anyway..._

---

### Simply ask yourself:
## _**do your tools help or hinder?**_
