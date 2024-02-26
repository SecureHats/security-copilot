![logo](/images/sh-banner.png)
=========
# Microsoft CoPilot for Security

#### 🎓 Level: 100 (Beginner)
#### ⌛ Estimated time to complete this lab: 20 minutes

## Lesson 1

In this first lesson, we will cover the basics of creating a plugin for Microsoft CoPilot for Security. By the end, you'll understand the essential parts needed to make a plugin and be able to use the template we provide.

Microsoft CoPilot for Security comes with some pre-made plugins that users or organizations can turn on. But you can also make your plugin, which we'll focus on.

Creating a plugin for an AI platform might seem daunting at first. I felt the same way, but breaking it down into smaller steps made it much more manageable. Let's dive in and make it easier to understand.


## Developing a Plugin

A plugin for Microsoft CoPilot for Security consists of two main components:

OpenAPI Specification:  
  This component describes how the API of the plugin works. It defines the endpoints, request and response formats, authentication mechanisms, and any other relevant details about the plugin's API.  
The OpenAPI specification provides a standardized way to document and communicate the plugin's functionality.  

>- More information about the [OpenAPI Schema](https://github.com/OAI/OpenAPI-Specification/) (OAS) can be found here.

Plugin Manifest:  
  The plugin manifest file is used to explain to CoPilot how to use the plugin. It provides metadata and configuration information about the plugin, such as its name, version, author, dependencies, and any other necessary details. The manifest file helps CoPilot understand how to integrate and interact with the plugin.  

Microsoft Copilot uses the following process flow when the user asks a question and Microsoft Copilot answers the question by searching for and using a plugin.  

![alt text](/images/data-flow.png)

In our first example, we will use a public OpenAPI Spec and focus on creating a Plugin manifest

### Plugin manifest
The plugin manifest can be written in `JSON` or `YAML` format, but we'll stick to using YAML for the plugin manifest.  
YAML is a human-readable language that is often used for writing configuration files. It is a clear and easy-to-read format.  
So, for consistency and simplicity, we'll use YAML instead of JSON throughout the lessons.

The YAML manifest starts with a `Descriptor` key, a mapping containing three keys: `Name`, `DisplayName`, and `Description`.  
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

### Creating our first manifest

Now that we know what info we need for a manifest file, let's set it up for our first Microsoft CoPilot for Security plugin.  
1. Create a new file named `manifest.yaml`
2. Copy and paste the following example into the manifest.yaml

```yaml
Descriptor:
  Name: BasicRequest
  DisplayName: My First Plugin (BasicRequest)
  Description: Lesson 1 - Plugin for requesting http headers from the incoming request.

SkillGroups:
  - Format: API
    Settings:
      OpenApiSpecUrl: https://raw.githubusercontent.com/SecureHats/security-copilot/da70fb9b970d5b95faff462dff8491d46ac7d71a/Lesson%201/openapi.yaml
```

> NOTE: The `Name` is the command to be used in the CoPilot Prompt to call the Plugin

## Adding a custom plugin

1. Open the [Security CoPilot](https://securitycopilot.microsoft.com) portal and log in
2. click on the **Security CoPilot plugin** button at the bottom right to open the _Manage plugins_ dialog
3. scroll to the bottom section named **custom**
4. select **Add plugin**
5. upload the manifest.yaml file
6. select **add**

Congratulations, you have now added your first **custom plugin**

![custom plugin loaded](/images/custom-plugin.png)

## Testing the custom plugin

Now that we've got our shiny new plugin installed in Microsoft CoPilot for Security, it's time to try it out and see how well it works.  
Let's kick off a new session and try adding the following request.

![prompt-message](/images/prompt-message.png)

Microsoft CoPilot for Security successfully figured out and used the right plugin.  
It did this because we included a specific word, `BasicRequest`, in our question earlier.

Now, if we examine the results more closely from what we asked before, we find these specific details:

![prompt-result](/images/prompt-result.png)

1. In `step 1` we can observe that the `DisplayName` from the manifest is shown **My First Plugin (BasicRequest)**  
2. Step 2 is not showing any details, but this is the step where the API call to the provided `uri` was executed. 
> NOTE: The API endpoint is described in the OpenAPI Specification that is referenced in the manifest [see openapi.yaml](https://github.com/SecureHats/security-copilot/blob/da70fb9b970d5b95faff462dff8491d46ac7d71a/Lesson%201/openapi.yaml)

3. In the last step the response is processed and shows us the header information from the call to the httpbin.org API

## Summary

In this first lesson about Microsoft CoPilot for Security, we've covered the basics of creating a plugin for the platform.  
With our plugin added, we tested it by making a request.  
CoPilot successfully utilized the plugin, thanks to the keyword "BasicRequest" we included earlier.  Examining the results, we see the plugin's display name, details of the API call, and the response's header information.  
</br>
By following these steps, you've successfully created and integrated your first custom plugin into Microsoft CoPilot for Security!
