---
layout: post
title: Generating an API client with NSwag Studio
published: true
---

![Manhattan]({{site.baseurl}}/images/2020-02-15-Generating-an-API-client-with-NSwag-Studio/Manhattan.jpg)



## Sumary:

Do you need to consume an API with lots of endpoints and complex requests and responses? You could write the code yourself, or you could use a Type Script and C# code generator to get all the classes for the requests, responses, and the code to call each endpoint.

<!--more-->

## What is NSwag Studio?

NSwag Studio is a Windows app to generate API client code in C# or TypeScript.

## Download and install:

Get the Windows installer from the [NSwag Studio repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio), or download the installer directly from [this link](http://rsuter.com/Projects/NSwagStudio/installer.php).

Execute the installer and follow the on screen instructions (next, next…)

## Quick start:

This is how NSwag Studio looks when you open it:

![NSwag Studio main window]({{site.baseurl}}/images/2020-02-15-Generating-an-API-client-with-NSwag-Studio/NSwagStudioWindow.png)

To quickly generate some code:

1.Select the runtime that you want to use:

![Select runtime]({{site.baseurl}}/images/2020-02-15-Generating-an-API-client-with-NSwag-Studio/SelectRuntime.png)

2.Paste the URL of the specification for the API that you will consume and click the button **Create local Copy**:

![Specification URL]({{site.baseurl}}/images/2020-02-15-Generating-an-API-client-with-NSwag-Studio/SpecificationUrl.png)

3.Select the type of code that you want to generate:

![Select output]({{site.baseurl}}/images/2020-02-15-Generating-an-API-client-with-NSwag-Studio/SelectOutput.png)

4.Click the **Generate Output** button in the lower right part of the window.

5.You can see the generated code in the tab for the language selected, in the Output lateral tab:

![Generated code]({{site.baseurl}}/images/2020-02-15-Generating-an-API-client-with-NSwag-Studio/GeneratedCode.png)
 
6.You can get the code in files in a path that you specify. For that you need to set the path for the generated code (see the section Tweaking the settings below) and then click the button **Generate files**.

## Tweaking the settings:

There are a lot of settings that you can change to modify the generated code. Click the sub-tab Settings to change them:

![Settings]({{site.baseurl}}/images/2020-02-15-Generating-an-API-client-with-NSwag-Studio/Settings.png)

These are some of the settings that I have modified:

- **Generate contracts output:** Check this to get the contracts generated in a separated file.

- **Contracts namespace:-- Use this to specify the namespace that your contracts should use.

- **Use the base URL for the request:** This is checked by default, but we unchecked it because we want to set the base URL in the Startup file of our app (more on that below)

- **Generate interfaces for Client classes:** We checked this because we want to use dependency injection to supply the client to whatever code will be using it (the code that use the client will not have a direct dependency on the client).

- **Generic Array Type:** We changed this from the default `System.Collections.Generic.ICollection` to `System.Collections.Generic.IList` because we need the items in our response lists to have index-based access.

- **Date Type and Date Time Type:** We use `System.DateTime` instead of the default `System.DateTimeOffset`.

- **Output file path (empty: no file output)** and **Contracts output file path (empty: single output file output):** Use this to specify the path were the files with the generated code should be saved.

## Other ways to extend the generated code:

**Run custom code:** There are some partial methods that you can implement. For instance **PrepareRequest** is called before the generated client code sends the request to the API. We used it to get a token and add it to the request if it's not already set. There are several other partial methods. They are all declared in the top part of the  generated client code. 

**If the return of the API doesn't match the Swagger specification:** We extended the generated code in another way: The responses that we get from the API didn't match completely the Swagger specification used to generate the code, so the part that process the response was not behaving as expected (that is not a problem from NSwag Studio, but from the provider or our API).

The method that process the response **ReadObjectResponseAsync** is virtual, we override it in another class that inherits from our generated client. In the overridden version, we made the modifications to fit our needs.

## Avoid modifying the generated code directly:

If you modify the generated code directly, you are depriving yourself of the possibility of quickly generate that code again. Maybe the API changed, or you want to change the NSwag Studio parameters in another way. In any case, it's very important to be able to generated the code, without having to reproduce loads of modifications inside that generated code.

## Best practices in the generated code:

The generated code follows several best practices that we like. You can read about them in a future entry.



