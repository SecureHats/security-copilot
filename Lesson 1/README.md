# Microsoft CoPilot for Security

## Lesson 1

In this first lesson, we'll cover the basics of creating a plugin for Microsoft Security CoPilot. By the end, you'll understand the essential parts needed to make a plugin and be able to use the template we provide.

Microsoft Security CoPilot comes with some pre-made plugins that users or organizations can turn on. But you can also make your own plugin, which is what we'll focus on.

Creating a plugin for an AI platform might seem daunting at first. I felt the same way, but breaking it down into smaller steps made it much more manageable. Let's dive in and make it easier to understand.


## Developing a Plugin

A plugin for Microsoft Security CoPilot consists of two main components:

OpenAPI Specification: This component describes how the API of the plugin works. It defines the endpoints, request and response formats, authentication mechanisms, and any other relevant details about the plugin's API. The OpenAPI specification provides a standardized way to document and communicate the functionality of the plugin.  

>- More information about the [OpenAPI Schema](https://github.com/OAI/OpenAPI-Specification/) (OAS) can be found here.

Plugin Manifest: The plugin manifest file is used to explain to CoPilot how to use the plugin. It provides metadata and configuration information about the plugin, such as its name, version, author, dependencies, and any other necessary details. The manifest file helps CoPilot understand how to integrate and interact with the plugin.

In our first example we will use a public OpenAPI Spec and focus on creating a Plugin manifest

### Plugin manifest
The plugin manifest can be written in `JSON` or `YAML` format, but we'll stick to using YAML for the plugin manifest.  
YAML is a human-readable languafe that is often used for writing configuration files. It is a clear and easy-to-read format.  
So, for consistency and simplicity, we'll use YAML instead of JSON throughout the lessons.

The YAML manifest starts with a `Descriptor` key, which is a mapping that contains three keys: `Name`, `DisplayName`, and `Description`.  
These keys are used to provide metadata about the plugin it's associated with.  

- `Name` is the identifier for the application or service.
- `DisplayName` is a more human-friendly name that can be used in user interfaces.
- `Description` provides more detailed information about the application or service.

The next key in the document is `SkillGroups`, which is a sequence (or list). Each item in the sequence is a mapping that describes a skill group. In this case, there's only one skill group defined.

- `Format` specifies the format of the skill group. Here, it's set to `API`, indicating that this skill group is related to an API.
- `Settings` is another mapping that contains configuration settings for the skill group. Here, it includes `OpenApiSpecUrl`, which is the URL of the `OpenAPI specification` for the API.  


```yaml
Descriptor:
  Name: 
  DisplayName: 
  Description: 

SkillGroups:
  - Format: API
    Settings:
      OpenApiSpecUrl: https://<path-to-url>/openapi.yaml
```

Now that we understand what the minimum required properties are for a manifest file, we can continue to configure the manifest for our first Microsoft Security CoPilot plugin.

```yaml
Descriptor:
  Name: BasicRequest
  DisplayName: Lesson 1 - My First Plugin (BasicRequest)
  Description: Lesson 1 - Plugin for requesting http headers from the incoming request.

SkillGroups:
  - Format: API
    Settings:
      OpenApiSpecUrl: https://github.com/SecureHats/security-copilot/blob/da70fb9b970d5b95faff462dff8491d46ac7d71a/Lesson%201/openapi.yaml
```