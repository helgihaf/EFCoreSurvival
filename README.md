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
The exact version for these references varies between releases of .NET Core. For me I simply use the latest version reported on nuget.org.

## Step 2
Open cmd and cd into the same folder where your migration .csproj is and enter:
```powershell
dotnet add package Microsoft.EntityFrameworkCore.Design 
```

## Step 3
Create your Code-first database, for example:
```C#
namespace Marsonsoft.AM.Database
{
    public class Account
    {
        public long Id { get; set; }
        public string Email { get; set; }
        public string AuthenticationType { get; set; }
        public string Token { get; set; }
        public string RefreshToken { get; set; }
    }
}

using Microsoft.EntityFrameworkCore;

namespace Marsonsoft.AM.Database
{
    public class AdventureDbContext : DbContext
    {
        public AdventureDbContext(DbContextOptions<AdventureDbContext> options)
            : base(options)
        {
        }

        public DbSet<Account> Accounts { get; set; }
    }
}    
```
## Step 4
### Option 1: SQL Server and appsettings.json
If the project has an appsettings.json file, put the target connection string in there:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity2;Trusted_Connection=True;MultipleActiveResultSets=true"
},
```
### Option 2: SQL Server and DbContext configuration
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
### Option 3: SQLite and DbContext configuration
Open cmd and cd into the same folder where your migration .csproj is and enter:
```powershell
cd <to where my .csproj is>
dotnet add package Microsoft.EntityFrameworkCore.Sqlite  
dotnet add package Microsoft.EntityFrameworkCore.Sqlite.Design
```
Put this in your DbContext class:
```c#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    if (!optionsBuilder.IsConfigured)
    {
        optionsBuilder.UseSqlite("Data Source=adventure.db");
    }
}
```
## Step 5
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

## Drop Database
Useful for new development when you have already applied your initial migration.
```powershell
dotnet ef database drop
```
