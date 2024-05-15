# Setting up GitHub Builds
## Overview
Setting up GitHub builds is very easy once you get your source code for your microservice checked in. 
The only thing you need to be cautious of is unit tests and integration tests.

In your dotnet.yml file, you are going to make sure that you skip all integration tests. 
This is done by adding the --filter command line option and specifying the namespace.

### Code Coverage
We found a simple tool that helps you display your code coverage on your repo, [DotNetCodeCoverageBadge](https://github.com/simon-k/dotnet-code-coverage-badge).
You create a gist and it pushes the code coverage percentage as json to it. Then in your readme file, you specify the gist and it renders a badge. I found it pretty simple to setup.

### Setting up the .yml File


Lastly, in your Xunit test, change the namespace for your InMemory providers to NOT BE ServiceBricks.Xunit.Integration. Then they can be easily unit tested and covered in the build without having to deploy all your providers.
```csharp
    - name: Test
      run: dotnet test --no-build --verbosity normal --filter "FullyQualifiedName!~ServiceBricks.Xunit.Integration" -p:CollectCoverage=true -p:CoverletOutput=TestResults/ -p:CoverletOutputFormat=opencover
```

You can view the [ServiceBricks YML File](https://github.com/holomodular/ServiceBricks/blob/main/.github/workflows/dotnet.yml) as a reference.

