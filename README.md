# F* SDK

This is MSBuild SDK for F*.


## How to use

Create new Nuget.config using `dotnet new nugetconfig`

Paste in that file following configuration file.
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
    <add key="fstar-experimental" value="https://www.myget.org/F/fstar/api/v3/index.json" />
    <add key="nuget" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
```

Create new F# project and replace it with following content.

```xml
<Project Sdk="FStarLang.Sdk/0.0.2">
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <Compile Include="Program.fst" />
  </ItemGroup>
</Project>
```
Also rename Program.fs to Program.fst and now you can run your code using `dotnet run`. That's it folks!

## Development

For creation of new version of SDK
- bump version in the `src\sdk\FStarLang.Sdk.csproj`
- run 
```sh
dotnet build -c Release src/sdk/FStarLang.Sdk.csproj
```
- publish Nuget file located at `src\sdk\bin\Release`
```
dotnet nuget push --source fstarlang --api-key az src/sdk/bin/Release/FStarLang.Sdk.0.0.4.nupkg
```
- Go back to root of the project
- run
```sh
dotnet build -r win-x64 -c Release /p:FStarLangVersion=2022.07.13 src/runtime/FStarLang.Runtime.csproj
```
- publish Nuget file located at `src\sdk\bin\Release`
```
dotnet nuget push --source fstarlang --api-key az --interactive src/runtime/bin/Release/runtime.win-x64.FStarLang.Compiler.0.0.2.nupkg
```
