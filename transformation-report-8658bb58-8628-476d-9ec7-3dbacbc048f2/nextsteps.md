# Next Steps

## Summary

The transformation appears to have completed successfully. No build errors were detected across any of the projects in the solution, including `Bookstore.Web`.

## Validation Steps

### 1. Restore Dependencies

Run the following command from the solution root to ensure all NuGet packages are restored correctly:

```bash
dotnet restore
```

Review the output for any warnings related to missing packages or version conflicts.

### 2. Build the Solution

Perform a full build to confirm there are no compilation issues:

```bash
dotnet build --configuration Release
```

Ensure the output reports zero errors and review any warnings that may indicate deprecated APIs or compatibility concerns.

### 3. Run Unit Tests

If the solution contains test projects, execute them to verify that existing functionality has not regressed:

```bash
dotnet test --configuration Release --logger "console;verbosity=detailed"
```

Review test results and address any failing tests before proceeding.

### 4. Run the Application Locally

Start the web application locally to verify it runs as expected on the new .NET runtime:

```bash
dotnet run --project Bookstore.Web/Bookstore.Web.csproj --configuration Release
```

Navigate to the application in a browser and manually verify core functionality such as page rendering, navigation, and any data access operations.

### 5. Review Target Framework

Open `Bookstore.Web.csproj` and confirm the `<TargetFramework>` element targets the intended modern .NET version, for example:

```xml
<TargetFramework>net8.0</TargetFramework>
```

If the target framework needs to be updated, modify this value and re-run the build and test steps above.

### 6. Check for Removed or Changed APIs

Review the codebase for any usage of APIs that were available in .NET Framework but have changed or been removed in modern .NET. Key areas to check include:

- `System.Web` namespace usages (not available in modern .NET)
- `HttpContext` and related types (now accessed via `IHttpContextAccessor`)
- Configuration APIs (now use `Microsoft.Extensions.Configuration`)
- Any Windows-specific APIs if cross-platform support is required

Use the [.NET Upgrade Assistant compatibility analyzer](https://learn.microsoft.com/en-us/dotnet/core/porting/upgrade-assistant-overview) or the `Microsoft.DotNet.UpgradeAssistant` tool to surface any remaining compatibility issues.

### 7. Verify Database Connectivity

If the application uses a database, confirm that the connection strings in `appsettings.json` are correctly configured for the new environment and that the application can successfully connect and perform queries at runtime.

### 8. Review Middleware and Startup Configuration

If the project was migrated from ASP.NET MVC (.NET Framework) to ASP.NET Core, verify that:

- `Program.cs` and/or `Startup.cs` correctly register all required services
- Middleware is configured in the correct order (authentication, authorization, routing, etc.)
- Static files, routing, and error handling are all functioning as expected

### 9. Publish the Application

Once validation is complete, publish the application to confirm the output is correct:

```bash
dotnet publish --configuration Release --output ./publish
```

Review the contents of the `./publish` directory and confirm all required files are present before deploying to the target environment.