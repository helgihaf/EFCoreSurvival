# EF Core Survival Guide

[Source](https://docs.microsoft.com/en-us/aspnet/core/data/ef-mvc/migrations)

# Setup
## Step 1
Edit .csproj _where you want the migrations to reside_ and make sure you have this in place:
```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
</ItemGroup>
```
## Step 2
### Either...
If the project has an appsettings.json file, put the target connection string in there:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity2;Trusted_Connection=True;MultipleActiveResultSets=true"
},
```
### or:
Put this in your DbContext class:
```c#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    if (!optionsBuilder.IsConfigured)
    {
        optionsBuilder.UseMySql("Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity2;Trusted_Connection=True;MultipleActiveResultSets=true ");
    }
}
```
## Step 3
Open cmd and cd into the same folder where your migration .csproj is and enter:
```powershell
dotnet ef migrations add InitialCreate
```
Note: This will NOT create the database!

# Migrations

For the migrations commands to work you must have the DbContext set up with a connection string and the database up and running. The simplest way to do that is by overriding the OnConfiguring method (see Setup section).

## Remove Last Migration
```powershell
dotnet ef migrations remove
```

## Create New Migration
```powershell
dotnet ef migrations add MyInitialMigrationYeah
```

