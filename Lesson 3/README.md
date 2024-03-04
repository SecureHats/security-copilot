![logo](/images/sh-banner.png)
=========
[![Maintenance](https://img.shields.io/maintenance/yes/2024.svg?style=flat-square)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)</br>

# Microsoft Copilot for Security

## Lesson 3 (DRAFT)

>- NOTE: This next lesson is still in development.

In the 2nd lesson, we learned the fundamentals of making an OpenAPI Specification (swagger) for Microsoft Security Copilot.  
We have created our own OpenAPI Specification that is used to query a public API containing intel on Harry Potter.  
<br>
In this next lesson, we will take a look into the difference between the `Security Copilot plugin` and the `OpenAI plugin`.  
At the end of this lesson, you will be able to explain the difference and you will be able to create both plugin types.  
<br>

![alt text](/images/plugins.png)

## Plugins

 let's quickly refresh our memory on what exactly a plugin does. Essentially, plugins are a bridge between a powerful Large Language Model (LLM) service like ChatGPT and third-party applications.  
 They enable seamless interaction between these applications and the APIs defined by developers.  

 The manifests play a crucial role in defining the behavior and capabilities of plugins within the ecosystem.  
 They outline key parameters such as authentication methods, supported API endpoints, and data formats as we have seen in the previous lesson.  

As we delve deeper into the plugin manifests, it becomes clear that they are the building blocks to create a relationship between AI and external APIs  

The OpenAPI plugin uses JSON format and has a different setup compared to the manifest file used in a Microsoft CoPilot plugin.  
This isn't surprising because the OpenAPI plugin follows the guidelines outlined in the ChatGPT documentation.  
Microsoft has created a helpful table that lists all the necessary properties in the OpenAPI plugin manifest file. You can check it out [here](https://learn.microsoft.com/en-us/copilot-plugins/overview#plugin-manifest-fields)  

Let's take a look at an example of an openapi manifest file. This file is setup differently from the Microsoft Copilot manifest as discussed earlier.

```json
{
    "schema_version": "v1",
    "name_for_human": "HackTrack",
    "name_for_model": "hacktrack",
    "description_for_human": "This tool checks if credentials linked to an email have been exposed in data breaches or hacks.",
    "description_for_model": "This tool checks if credentials linked to an email have been exposed in data breaches or hacks.",
    "auth": {
        "type": "none"
    },
    "api": {
        "type": "openapi",
        "url": "https://hacktrack.routum.io/openapi.yaml",
        "is_user_authenticated": false
    },
    "logo_url": "https://hacktrack.routum.io/logo.png",
    "contact_email": "x@votkon.com",
    "legal_info_url": "https://hacktrack.routum.io/terms-of-use.html"
}
```
[source](https://hacktrack.routum.io/.well-known/ai-plugin.json)

Comparing the manifest file for Microsoft Copilot, as discussed in Lesson 1, but in `JSON` format reveals significant differences between the files.  
When uploading an OpenAPI plugin manifest to Microsoft Copilot for Security, the file undergoes conversion to the Microsoft Copilot format in the background, as shown in the following image.

![alt text](/images/payload-openapi-manifest.png)

The output displayed below showcases the redacted payload of the previous OpenAPI manifest file once it's uploaded to Microsoft Copilot portal.  
This transformation highlights how the OpenAPI manifest file converts to the Copilot format, streamlining integration and enhancing functionality.  

```json
{
    "descriptor": {
        "name": "hacktrack",
        "description": "This tool checks if credentials linked to an email have been exposed in data breaches or hacks.",
        "displayName": "HackTrack",
        "icon": "https://hacktrack.routum.io/logo.png",
        "authorization": null,
        "supportedAuthTypes": [
            "None"
        ]
    },
    "skillGroups": [
        {
            "format": "API",
            "settings": {
                "OpenApiSpecUrl": "https://hacktrack.routum.io/openapi.yaml"
            }
        }
    ]
}
```

Another observation from inspecting the payload is that API functions as described in the linked OpenAPI Specification are also processed an populated.  
This can be seen under the `skills` section in the payload and shown in the next 2 code snippet(s).  
<br>
The first snippet shows the payload from the portal when uploading the plugin

```json
"skills": [
                {
                    ...
                    "name": "getTrack",
                    "inputs": [
                        {
                            "required": true,
                            "name": "email",
                            "type": {
                                "name": "String"
                            },
                            "description": "The email of the user."
                        }
                    ],
                    "settings": {
                        "PathInputs:email:Type:Name": "String",
                        "PathInputs:email:Required": "True",
                        "PathInputs:email:Name": "email",
                        "PathInputs:email:Description": "The email of the user.",
                        "OperationType": "Get",
                        "OperationPath": "/track/{email}",
                        "EndpointUrl": "https://hacktrack.routum.io",
                        ...
                    }
                }
            ],
```
<br>
This second snippet shows the openapi.yaml specification that is referenced in the manifest file under the `OpenApiSpecUrl` parameter.  


```yaml
...
servers:
  - url: https://hacktrack.routum.io
paths:
  /track/{email}:
    get:
      operationId: getTrack
      parameters:
      - in: path
        name: email
        schema:
            type: string
        required: true
        description: The email of the user.
...
```

# Stay tuned for more information
