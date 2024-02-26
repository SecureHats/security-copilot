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

The OAS can contain many differnt attributes, but only a few are required for a custom plugin
In our example we will use the following attributes:
- Info Object
- Server Object
- Paths Object

### The Info object

In the **info object** is like a summary of key details about the API. It usually includes things like the title, a brief description, the version number, and links to stuff like licenses and terms of service.  
The only things that are **required** in there are the `title` and `version`. It's a good idea to include a description too. You can use markdown to format the description if you want.  

```yaml
info:
    title: User Request
    version: "0.2.0"
```

### The Server object

In the **servers object**, we can list one or more main paths used in requests to the API.  
This section is in array format, so each path needs to be specified separately, unlike the values in the info section.  
The basepath is basically the part of the web address that comes before the specific endpoint. The only **required** property is the `url`

```yaml
servers:
    - url: https://httpbin.org/
      description: development server
```

### The Paths object

The **paths object** can be seen as a roadmap for the endpoints available within the API.  
It proved details on how to access the endpoints and what actions you can perform.  
Each endpoint is represented by a unique `path` within the **paths object**

To break this section a bit more down into digestable chucks, I will describe the most important parts.  
- path
- HTTP methods
- operation object
...

### path

The `path` is the relative URL of the API endpoint. It should start with a forward slash `/` and ends with a double collon `:`

```yaml
paths:
    /users:
```

### HTTP Methods

Each `path` should specify one or more HTTP methods that are allowed to be used with the endpoint.  
Each HTTP method represents a different action that can be performed on the endpoint.  
Common HTTP methods are `GET`, `POST`, `PUT`, `DELETE`  

```yaml
paths:
  /users:
    get:
```

### Operation Object

For each HTTP method specified, there must be an associated operation object containing additional information about the operation.  
Examples of these operation objects are parameters, request body, responses, etc. The only required values are `summary` and the operation object `responses`  
The `summary` field provides a brief, human-readable description of the endpoint.  
In the `responses` section we define the possible responses that the API can return when that endpoint is accessed. It contains the HTTP status codes and their corresponding descriptions.  
Optionally, the `responses` section includes details about the response payload, like the format and schema definition.  

```yaml
paths:
  /users:
    get:
      summary: Retrieves a list of users
      responses:
        '200':
          description: A list of users
```

> - ref: [OpenAPI Specification](https://swagger.io/specification/)<br>
> - ref: [Microsoft Learn Build Plugins](https://learn.microsoft.com/en-us/copilot-plugins/get-started)
> - ref: [Security CoPilot Plugins](https://learn.microsoft.com/en-us/security-copilot/plugin_api)