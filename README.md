# Coverlet

[![Build Status](https://dev.azure.com/tonerdo/coverlet/_apis/build/status/coverlet-coverage.coverlet?branchName=master)](https://dev.azure.com/tonerdo/coverlet/_build/latest?definitionId=5&branchName=master) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/tonerdo/coverlet/blob/master/LICENSE)

Coverlet is a cross platform code coverage framework for .NET, with support for line, branch and method coverage. It works with .NET Framework on Windows and .NET Core on all supported platforms.

# Main contents
* [QuickStart](#Quick-Start)
* [How It Works](#How-It-Works)
* [Deterministic build support](#Deterministic-build-support)
* [Known Issues](#Known-Issues)
* [Consume nightly build](#Consume-nightly-build)
* [Feature samples](Documentation/Examples.md)
* [Cake Add-In](#Cake-Add-In)
* [Changelog](Documentation/Changelog.md)
* [Roadmap](Documentation/Roadmap.md)

## Quick Start

Coverlet can be used through three different *drivers* 

* VSTest engine integration
* MSBuild task integration
* As a .NET Global tool  

Coverlet supports only SDK-style projects https://docs.microsoft.com/en-us/visualstudio/msbuild/how-to-use-project-sdk?view=vs-2019  


### VSTest Integration (preferred due to [known issue](https://github.com/tonerdo/coverlet/blob/master/Documentation/KnownIssues.md#1-vstest-stops-process-execution-earlydotnet-test) supports only .NET Core application)

At the moment collectors integration **does not support** .NET Framework application.

### Installation
```bash
dotnet add package coverlet.collector
```
N.B. You **MUST** add package only to test projects 

### Usage
Coverlet is integrated into the Visual Studio Test Platform as a [data collector](https://github.com/Microsoft/vstest-docs/blob/master/docs/extensions/datacollector.md). To get coverage simply run the following command:

```bash
dotnet test --collect:"XPlat Code Coverage"
```

After the above command is run, a `coverage.cobertura.xml` file containing the results will be published to the `TestResults` directory as an attachment.

See [documentation](Documentation/VSTestIntegration.md) for advanced usage.

#### Requirements
* _You need to be running .NET Core SDK v2.2.401 or newer_
* _You need to reference version 16.5.0 and above of Microsoft.NET.Test.Sdk_
```
<PackageReference Include="Microsoft.NET.Test.Sdk" Version="16.5.0" />
```

### MSBuild Integration (suffers of possible [known issue](https://github.com/tonerdo/coverlet/blob/master/Documentation/KnownIssues.md#1-vstest-stops-process-execution-earlydotnet-test))

### Installation
```bash
dotnet add package coverlet.msbuild
```
N.B. You **MUST** add package only to test projects  

### Usage

Coverlet also integrates with the build system to run code coverage after tests. Enabling code coverage is as simple as setting the `CollectCoverage` property to `true`

```bash
dotnet test /p:CollectCoverage=true
```

After the above command is run, a `coverage.json` file containing the results will be generated in the root directory of the test project. A summary of the results will also be displayed in the terminal.

See [documentation](Documentation/MSBuildIntegration.md) for advanced usage.

#### Requirements
Requires a runtime that support _.NET Standard 2.0 and above_

### .NET Global Tool ([guide](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools), suffers of possible [known issue](https://github.com/tonerdo/coverlet/blob/master/Documentation/KnownIssues.md#1-vstest-stops-process-execution-earlydotnet-test))

### Installation

```bash
dotnet tool install --global coverlet.console
```

### Usage

The `coverlet` tool is invoked by specifying the path to the assembly that contains the unit tests. You also need to specify the test runner and the arguments to pass to the test runner using the `--target` and `--targetargs` options respectively. The invocation of the test runner with the supplied arguments **must not** involve a recompilation of the unit test assembly or no coverage result will be generated.

The following example shows how to use the familiar `dotnet test` toolchain:

```bash
coverlet /path/to/test-assembly.dll --target "dotnet" --targetargs "test /path/to/test-project --no-build"
```

_Note: The `--no-build` flag is specified so that the `/path/to/test-assembly.dll` assembly isn't rebuilt_

See [documentation](Documentation/GlobalTool.md) for advanced usage.

#### Requirements
.NET global tools rely on a .NET Core runtime installed on your machine https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools#what-could-go-wrong

.NET Coverlet global tool requires _.NET Core 2.2 and above_ 


## How It Works

Coverlet generates code coverage information by going through the following process:

### Before Tests Run

* Locates the unit test assembly and selects all the referenced assemblies that have PDBs.
* Instruments the selected assemblies by inserting code to record sequence point hits to a temporary file.

### After Tests Run

* Restore the original non-instrumented assembly files.
* Read the recorded hits information from the temporary file.
* Generate the coverage result from the hits information and write it to a file.

## Deterministic build support

Coverlet supports coverage for deterministic builds. The solution at the moment is not optimal and need a workaround.  
Take a look at [documentation](Documentation/DeterministicBuild.md).

## Are you in trouble with some feature? Check on [examples](Documentation/Examples.md)!

## Known Issues

Unfortunately we have some known issues, check it [here](Documentation/KnownIssues.md) 

## Cake Add-In

If you're using [Cake Build](https://cakebuild.net) for your build script you can use the [Cake.Coverlet](https://github.com/Romanx/Cake.Coverlet) add-in to provide you extensions to dotnet test for passing Coverlet arguments in a strongly typed manner.

## Consume nightly build

We offer nightly build of master for all packages.
See the [documentation](Documentation/ConsumeNightlyBuild.md)

## Issues & Contributions

If you find a bug or have a feature request, please report them at this repository's issues section. See the [CONTRIBUTING GUIDE](CONTRIBUTING.md) for details on building and contributing to this project.

## Coverlet Team

Author and owner    
* [Toni Solarin-Sodara](https://github.com/tonerdo)  

Co-maintainers

* [Peter Liljenberg](https://github.com/petli)  
* [Marco Rossignoli](https://github.com/MarcoRossignoli)

## Code of Conduct

This project has adopted the code of conduct defined by the Contributor Covenant
to clarify expected behavior in our community.

For more information, see the [.NET Foundation Code of Conduct](https://dotnetfoundation.org/code-of-conduct).

## License

This project is licensed under the MIT license. See the [LICENSE](LICENSE) file for more info.  
  
## Supported by the [.NET Foundation](https://dotnetfoundation.org/)
