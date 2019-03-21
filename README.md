# Show SdkResolver differences between csproj and vcxproj files
This example shows the different behaviour of the MSBuild SdkResolver from VisualStudio.

## Steps to reproduce
- Open Main\Main.sln in Visual Studio
- Build the Parent project (csproj)
  - Result: succesfull build
- Build the CppParent project (vcxproj)
  - Result: fails with errors:
    ```
    Severity	Code	Description	Project	File	Line	Suppression State
    Error	MSB4236	The SDK 'Microsoft.Build.CentralPackageVersions' specified could not be found.	CppParent	D:\GitHub\ShowSdkResolverBehaviour\OutOfTree\MyOtherLib\MyOtherLib.csproj	1	
    Error		C:\Program Files\dotnet\sdk\2.2.104\Sdks\Microsoft.Build.CentralPackageVersions\Sdk not found. Check that a recent enough .NET Core SDK is installed and/or increase the version specified in global.json.	CppParent		1	
    ```

## Background
Both Parent and CppParent projects will import the rootlevel BuildOtherSolution.targets
This file will actually use the buildin MSBuild task to build the solution in the "OutOfTree" folder.
The "OutOfTree" folder contains a csproj that has an Sdk reference to "Microsoft.Build.CentralPackageVersions",
however the version is specified in the (MainFolder) global.json.

Apparently this works when the 'parent' project is run through the dotnet project build system,
but does not when the 'parent' project is the c++ project build system.