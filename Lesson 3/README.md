![logo](/images/sh-banner.png)
=========
[![Maintenance](https://img.shields.io/maintenance/yes/2024.svg?style=flat-square)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)</br>

# Microsoft Copilot for Security

## Lesson 3 (DRAFT)

>- NOTE: This next lesson is still in development.

In the [2nd lesson](/Lesson%202/README.md), we learned the fundamentals of making an OpenAPI Specification (swagger) for Microsoft Security Copilot.  
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

## Different Manifests

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
<br>
If we compare the manifest files for plugins, it reveals significant differences between the files.  
When uploading an OpenAPI plugin manifest to Microsoft Copilot for Security, the file undergoes conversion to the Microsoft Copilot format in the background, as shown in the image.  

<br>  

![alt text](/images/payload-openapi-manifest.png)

### Here's another cool thing we noticed when checking out the payload.  

The API functions described in the linked OpenAPI Specification are also being processed and added.  
You can find this in the 'skills' section of the payload, and we'll show you in the next two code snippets.  

The first snippet gives a peek at the information sent from the portal when you upload the plugin.  

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

This next snippet displays the openapi.yaml specification mentioned in the manifest file, which is identified by the OpenApiSpecUrl parameter.  

<br>

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
[full OpenApi Spec](https://hacktrack.routum.io/openapi.yaml)  

Based on this observation it would mean that once the plugin's manifest is uploaded, there's no ongoing connection between the plugin and the API documentation file (OpenAPI Spec).  
So, if the OpenAPI spec changes after the plugin is deployed, those updates won't automatically show up in the plugin's output.  

> - NOTE: This is an assumpion based on inspecting the output the previous payload.

## Mapping values

I have created a table to map the values from the OpenAPI manifest to the Copilot manifest as a reference.  

| **OpenAPI manifest** | **Copilot manifest** |
| --- | --- |
| name_for_human | displayName |
| name_for_model | name |
| description_for_human | description |
| description_for_model | description |
| auth | supportedAuthTypes |
| logo_url | icon |

## Summary

Lesson 3 covers the differences between the Security Copilot plugin and the OpenAI plugin. It explains that plugins act as a bridge between powerful language models like ChatGPT and third-party applications, allowing seamless interaction with APIs.  
The lesson explores the role of manifests in defining plugin behavior and capabilities, highlighting the distinctions between OpenAPI and Microsoft Copilot manifests.  

We have demonstrated how an OpenAPI manifests is converted to Copilot format upon upload, and discusses how API functions are processed and added to the plugin's output.  
Additionally, we noticed that once a plugin's manifest is uploaded, there is no ongoing connection with the API documentation, meaning that changes to the OpenAPI spec won't automatically reflect in the plugin's output.  

Finally, a table mapping values from the OpenAPI manifest to the Copilot manifest is provided as a reference.


# Stay tuned for more information
