# Deploying Distributed / Microservice Solutions

The ABP Framework is designed to consider distributed and microservice systems, where you have multiple applications and/or services communicating internally. All of its features are compatible with distributed scenarios. This document highlights some points you should care when you deploy your distributed or microservice solution.

## Application Name & Instance Id

ABP provides `IApplicationInfoAccessor` service that provides the following properties:

* `ApplicationName`: A human-readable name for an application. It is a unique value for an application.
* `InstanceId`: A random (GUID) value generated by the ABP Framework each time you start the application.

These values are used by the ABP Framework for several places to distinguish the application and the application instance (process) in the system. For example, [audit logging](../Audit-Logging.md) system saves the `ApplicationName` in each audit log record written by the related application, so you can understand which application has created the audit log entry. So, if your system consists of multiple applications saving audit logs to a single point, you should be sure that each application has a different `ApplicationName`.

The `ApplicationName` property's value is set automatically from the **entry assembly's name** (generally, the project name in a .NET solution) by default, which is proper for most cases, since each application typically has a unique entry assembly name.

There are two ways to set the application name to a different value. In this first approach, you can set the `ApplicationName` property in your application's [configuration](../Configuration.md). The easiest way is to add an `ApplicationName` field to your `appsettings.json` file:

````json
{
    "ApplicationName": "Services.Ordering"
}
````

Alternatively, you can set `AbpApplicationCreationOptions.ApplicationName` while creating the ABP application. You can find the `AddApplication` or `AddApplicationAsync` call in your solution (typically in the `Program.cs` file), and set the `ApplicationName` option as shown below:

````csharp
await builder.AddApplicationAsync<OrderingServiceHttpApiHostModule>(options =>
{
    options.ApplicationName = "Services.Ordering";
});
````



s
