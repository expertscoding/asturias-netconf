# Asturias NetConf

This project contains the demos explained in the talk Asturias NetConf.

Every web host is based on the standard template _ASP.NET Core Web Application_ within Visual Studio.

Before running the projects, a couple of steps should be done:

1. Create the database for project Host. Execute the following command from a console within project directory:
```
dotnet ef database update -c ApplicationDbContext
```
2. Seed each the database with users data. The parameter seed will be caught in the Program entry point. Execute the following command from a console within project directory:
```
dotnet run -- seed
```

## Demo 1
### Create our first SSO server
______________________________________________________
This project will act as an identity server implementing the protocol OpenId Connect for our applications. To accomplish this we'll use the library [Identity Server 4](https://github.com/IdentityServer/IdentityServer4) as well as [ASP.NET Core Identity](https://github.com/aspnet/identity) available as nuget packages.

The project, in addition to being based on the standard template, contains a Quickstart template from [IS4 Samples](https://github.com/IdentityServer/IdentityServer4.Samples) repo.

The relevant code for this demo is in Startup class where everything is configured.


## Demo 2
### Connect an App with our SSO server
______________________________________________________
For this demo we have two additional projects within Clients folder. A web application and a web api to demonstrate the multiple purposes of our SSO server.

Relevant parts of code here are:
* Web App (ECApp): The Startup class has all the configuration needed to connect to an SSO using the extension method `AddOpenIdConnect` included in the standard packages for ASP.Net core.<br/>This configuration it's defined in the class `Host.Configuration.Clients` and added in the InMemory repository through the extension method `.AddInMemoryClients(Clients.Get())` as well as the resources needed to startup the SSO server.
* Web API (ECApi): In this case, we'll use the library to [IdentityServer4.AccessTokenValidation](https://github.com/IdentityServer/IdentityServer4.AccessTokenValidation) to parse and validate the tokens receive in every call to our API.<br/>We'll also set up an authorization policy to require a specific scope. You can see the policy in action in the `FilmsController` class: adding the attribute `[Authorize("ECApiPolicy")]` with the policy name.
