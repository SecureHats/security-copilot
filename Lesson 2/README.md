![logo](/images/sh-banner.png)
=========

# Microsoft CoPilot for Security

## Lesson 2

In our initial lesson, we've tackled the fundamentals of making a plugin for Microsoft Security CoPilot.  
We started by crafting a manifest file and connecting it to an online OpenAPI Specification.  
<br>
Now, we will dive into the simple setup of an OpenAPI specification file. We will also create a template that we can utilize in upcoming lessons.  
For the next example we will use the previous API endpoint as in [lesson 1](/Lesson%201/README.md) `https://httpbin.org/`  
<br>

## Documenting the API

When developers make an API (a tool for software to talk to other software), they usually provide instructions called documentation.  
This explains what the API can do and how to use it. The documentation follows a standard format set by the OpenAPI initiative, which is built on the Swagger Specification. Nowadays this documentation is called OpenAPI Specification (OAS) Throughout the following lessons we will use the abbriviation OAS.  
<br>
OAS, is like a blueprint or agreement for APIs. It explains what an API does, what information it needs (request parameters), and what it gives back (response objects), but it doesn't include specific code details.  
This means APIs built using OAS can talk to each other even if they're written in different programming languages, because OAS is designed to work with any language and can be easily understood by machines.  
<br>

### OpenAPI Specification

The OAS can contain many attributes, but only a few are required to work properly
In our example we will use the following objects:
- Info Object
- Server Object
- Paths Object

### The Info object

...

### The Server object

...

### The Paths object

...




```yaml
openapi: 3.0.0

info:
    title: httpbin.org
    version: "1.0.0"

servers:
    - url: https://httpbin.org/

paths:
    /basic-auth/{user}/{passwd}:
        get:
            operationId: TestBasicAuth
            summary: Prompts the user for authorization using HTTP Basic
            parameters:
                - in: path
                  name: user
                  schema:
                    type: string
                  required: true
                - in: path
                  name: passwd
                  schema:
                    type: string
                  required: true
            responses:
                200:
                    description: Successful authentication. \
                401:
                    description: Unsuccessful authentication.
```