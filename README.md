# F* SDK

This is MSBuild SDK for F*.


## Use templates

```shell
dotnet new --install FStarLang.DotNet.Common.ProjectTemplates.1.0::0.1.0 --nuget-source https://codevision.pkgs.visualstudio.com/FStarLang/_packaging/fstarlang/nuget/v3/index.json
```

Then you can create project using
```shell
dotnet new fstarconsole -o helloworld
```

## Manually convert F# project

Create new Nuget.config using `dotnet new nugetconfig`

Paste in that file following configuration file.
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
    <add key="fstar-experimental" value="https://codevision.pkgs.visualstudio.com/FStarLang/_packaging/fstarlang/nuget/v3/index.json" />
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
dotnet build -c Release src/sdk/FStarLang.Sdk.csproj /p:ReleaseBuild=true
```
- publish Nuget file located at `src\sdk\bin\Release`
```
dotnet nuget push --source fstarlang --api-key $ApiKey artifacts/package/release/FStarLang.Sdk.0.2.0.nupkg
```
- Go back to root of the project
- bump version in the `src/runtime/FStarLang.Runtime.csproj`
- Change `FStarCompilerVersion` to latest F* compiler release in the `src/runtime/FStarLang.Runtime.csproj`
- run
```sh
dotnet build -r win-x64 -c Release src/runtime/FStarLang.Runtime.csproj /p:ReleaseBuild=true
dotnet build -r linux-x64 -c Release src/runtime/FStarLang.Runtime.csproj /p:ReleaseBuild=true
```
- publish Nuget file located at `src\sdk\bin\Release`
```
dotnet restore src/sdk/FStarLang.Sdk.csproj --interactive
dotnet nuget push --source fstarlang --api-key $ApiKey artifacts/package/release/FStarLang.Compiler.runtime.win-x64.0.0.7.nupkg
dotnet nuget push --source fstarlang --api-key $ApiKey artifacts/package/release/FStarLang.Compiler.runtime.linux-x64.0.0.7.nupkg
```
- Go back to root of the project
- bump version in the `src/templates/FStarLang.DotNet.Common.ProjectTemplates.1.0.csproj`
- run
```sh
dotnet pack -c Release src/templates/FStarLang.DotNet.Common.ProjectTemplates.1.0.csproj /p:ReleaseBuild=true
```
- publish Nuget file located at `src\sdk\bin\Release`
```
dotnet nuget push --source fstarlang --api-key $ApiKey artifacts/package/release/FStarLang.DotNet.Common.ProjectTemplates.1.0.0.1.2.nupkg
```
