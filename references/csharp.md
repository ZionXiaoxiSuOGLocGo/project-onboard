# C# Project Analysis Rules

## Signature Detection
- `*.csproj` or `*.sln` files exist
- No `Assets/` + `ProjectSettings/` directory (to distinguish from Unity which is already handled)

## Scan Steps

### 1. Read Project File
```bash
read *.csproj (or the main .csproj file)
```
Extract:
- `<TargetFramework>` → .NET version (net6.0, net8.0, etc.)
- `<OutputType>` → Exe, Library, WinExe
- `<PackageReference>` → NuGet dependencies
- `<ProjectReference>` → Internal project references
- `<RootNamespace>` → Default namespace

If `.sln` exists, read it for multi-project structure:
```bash
read *.sln (first 50 lines)
```

### 2. Detect Project Type
| Clues | Type |
|---|---|
| `Microsoft.NET.Sdk.Web` in csproj | ASP.NET Core Web App |
| `Microsoft.NET.Sdk.Worker` | Worker Service / Background Service |
| `UseWPF` or `UseWindowsForms` | Desktop GUI App |
| `Microsoft.NET.Sdk.BlazorWebAssembly` | Blazor WASM |
| `Microsoft.Maui` | .NET MAUI Cross-platform |
| `xunit` / `nunit` / `mstest` in deps | Test project |
| `Microsoft.Orleans` | Orleans distributed app |
| `Microsoft.EntityFrameworkCore` | EF Core data access |

### 3. Map Directory Structure
| Directory | Common Role |
|---|---|
| `src/` | Source projects |
| `tests/` or `test/` | Test projects |
| `Controllers/` | ASP.NET API controllers |
| `Pages/` | Razor Pages or Blazor pages |
| `Views/` | MVC views |
| `Models/` | Data models / DTOs |
| `Services/` | Business logic services |
| `Data/` or `DbContext/` | EF Core DbContext and migrations |
| `Migrations/` | EF Core migrations |
| `wwwroot/` | Static web assets |
| `Properties/` | Launch settings, assembly info |
| `Middleware/` | ASP.NET middleware |
| `Hubs/` | SignalR hubs |
| `Endpoints/` | Minimal API endpoints |
| `Components/` | Razor components |

### 4. Find Entry Points
```bash
grep "static.*Main" *.cs or Program.cs
```
Framework-specific:
- ASP.NET: `Program.cs` with `WebApplication.CreateBuilder`
- Worker: `Program.cs` with `CreateHostBuilder`
- WPF: `App.xaml.cs` with `Main()`
- WinForms: `Program.cs` with `Application.Run`
- MAUI: `App.xaml.cs` + `MauiProgram.cs`

### 5. Read App Configuration
```bash
read appsettings.json
read appsettings.Development.json (if exists)
```
Extract:
- Connection strings
- API keys / service URLs (not secret values)
- Logging configuration
- Feature flags

### 6. Check for Config Files
- `appsettings.json` / `appsettings.*.json` → Application configuration
- `Properties/launchSettings.json` → Debug profiles
- `Dockerfile` → Container deployment
- `docker-compose.yml` → Multi-service setup
- `nuget.config` → NuGet source configuration
- `.editorconfig` → Code style rules
- `Directory.Build.props` → MSBuild shared properties
- `.github/workflows/` → CI/CD
