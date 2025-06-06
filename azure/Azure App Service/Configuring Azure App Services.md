---
tags:
  - azure
  - cloud
  - az-204
  - azure-app-services
---

## Application settings

In Azure App Services, configurations are stored as _environment variables_.

App settings are always encrypted when stored (_encrypted-at-rest_).

If you are using [[Azure App Services Deployment Slots]], you can mark a setting as _swappable_.

Note: **the key format changes for Linux-based containers**. Instead of using `Key:SubKey` you have to use `Key__SubKey`.

## General settings

There are some settings that define the underlying infrastructure of the App Service.

Depending on the values, the application might require to scale up to a higher pricint tier.

Examples are:

- **Stack settings**: language and SDK version;
- **Platform settings**: used to configure the hosting platform.
  - **Bitness**: 32- or 64-bit;
  - **WebSocket protocol**: useful for [[SignalR]];
  - **[[Always On]]**: disabled by default;
  - **Managed pipeline version**: The IIS pipeline mode. Set it to _Classic_ if you have a legacy app that requires an older version of IIS.
  - **HTTP version**: Set to 2.0 to enable support for HTTPS/2 protocol.
  - **[[ARR Affinity]]**: In a multi-instance deployment, ensure that the client is routed to the same instance for the life of the session. You can set this option to _Off_ for stateless applications.
- **Debugging**: Enable remote debugging for ASP.NET, ASP.NET Core, or Node.js apps. This option turns off automatically after 48 hours.
- **Incoming client certificates**: require client certificates in mutual authentication. [[Transport Layer Security]] mutual authentication is used to restrict access to your app by enabling different types of authentication for it.

## Path mappings

You can configure **handler mappings,** and **virtual application** and **directory mappings**.

The Path mappings page displays different options based on the OS type.

### Windows apps

For Windows apps, you can customize the IIS handler mappings and virtual applications and directories.

Handler mappings let you add custom script processors to handle requests for specific file extensions.

**Each app has the default root path (/) mapped to _D:\home\site\wwwroot_**, where your code is deployed by default. If your app root is in a different folder, or if your repository has more than one application, you can edit or add virtual applications and directories.

You can configure virtual applications and directories by specifying each virtual directory and its corresponding physical path relative to the website root (D:\home). To mark a virtual directory as a web application, clear the Directory check box.

### Linux app + containeraized apps

Containerized apps include **all Linux apps and also the Windows and Linux custom containers** running on App Service.

You can use a storage account as a storage mount. The mount can be a [[Azure Files]] or [[Azure Blob Storage]].

## Diagnostic logging

There are some built-in deiagnostics associated with App Services.

### Application logging

Logs messages **generated by your application code**. The messages are generated by the web framework you choose, or from your application code directly using the standard logging pattern of your language.

Each message is assigned one of the following categories: Critical, Error, Warning, Info, Debug, and Trace.

**Available for Windows and Linux** platform.

Logs are **stored in the App Service file system and/or Azure Storage blobs**.

For Windows: the Filesystem option is for temporary debugging purposes, and **turns itself off in 12 hours**. The Blob option is for long-term logging, and needs a blob storage container to write logs to.

For Linux: you can specify the Quota (MB) the application logs. You can also specify the Retention Period, set the number of days the logs should be retained.

### Web server logging

Raw HTTP request data in the W3C extended log file format.

Each log message includes data like the HTTP method, resource URI, client IP, client port, user agent, response code, and so on.

**Available only for Windows** platform.

Logs are **stored in the App Service file system or Azure Storage blobs**.

### Detailed error logging

Copies of the .html error pages that would have been sent to the client browser. For security reasons, detailed error pages shouldn't be sent to clients in production, but App Service can save the error page each time an application error occurs that has HTTP code 400 or greater.

**Available only for Windows** platform.

Logs are **stored in the App Service file system**.

### Failed request tracing

Detailed tracing information on failed requests, **including a trace of the IIS components** used to process the request and the time taken in each component. One folder is generated for each failed request, which contains the XML log file, and the XSL stylesheet to view the log file with.

**Available only for Windows** platform.

Logs are **stored in the App Service file system**.

### Deployment logging

Helps determine why a deployment failed. Deployment logging happens automatically and there are no configurable settings for deployment logging.

**Available for Windows and Linux** platform.

Logs are **stored in the App Service file system**.

### Where to find these logs

If you configure the Azure Storage blobs option for a log type, you need a client tool that works with Azure Storage.

For logs stored in the App Service file system, the easiest way is to download the ZIP file in the browser at:

- Linux/container apps: `https://<app-name>.scm.azurewebsites.net/api/logs/docker/zip`
- Windows apps: `https://<app-name>.scm.azurewebsites.net/api/dump`

For Linux/container apps, the ZIP file contains console output logs for both the docker host and the docker container.

**For a scaled-out app**, the ZIP file contains one set of logs for each instance.

In the App Service file system, these log files are the contents of the /home/LogFiles directory.

## Configuring certificates

Azure App Service has tools that let you create, upload, or import a private certificate or a public [[Certificate]] into App Service.

A certificate uploaded into an app is stored in a deployment unit that is bound to the app service plan's resource group and region combination (internally called a webspace). This makes the certificate accessible to other apps in the same resource group and region combination.

You can either:

- **Create a free App Service managed certificate**: you need a Basic, Standard, Premium or Isolated tier; it's a TLS/SSL server certificate that's fully managed by App Service and **renewed continuously and automatically in six-month increments**, 45 days before expiration. You create the certificate and bind it to a custom domain, and let App Service do the rest.
- Purchase an App Service certificate;
- **Import a certificate from Key Vault**: the certificate is then stored in [[azure-key-vault]];
- Upload a private certificate (it must be stored in a [[PFX file]] and encrypted with [[Triple-DES]])
