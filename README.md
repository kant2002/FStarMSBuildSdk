# F* SDK

This is MSBuild SDK for F*.

Example how to use.

<Project Sdk="FStarLang.Sdk">
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <Compile Include="Program.fst" />
  </ItemGroup>
</Project>