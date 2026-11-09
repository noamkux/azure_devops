# Azure DevOps Pipeline - .NET Hello World

A .NET console app built and packaged by an Azure DevOps pipeline (azure-pipelines.yml) running on a clean Microsoft-hosted agent (ubuntu-latest). The source code is hosted on GitHub.

## Pipeline steps

1. Install .NET SDK - installs .NET 10 SDK on the clean agent.
2. Print version - prints the version (build number) to the console.
3. SonarQube Prepare - configures the scanner before the build (master only).
4. Build and pack - builds the project and creates a NuGet package from the DLL.
5. SonarQube Analyze - runs the code analysis and uploads the results (master only).
6. SonarQube Publish - shows the Quality Gate result in the pipeline run (master only).
7. Push to Azure Artifacts - uploads the package to a NuGet feed.

## Conditions

The variable isMaster checks whether the run is on the master branch. The SonarQube steps run only when the previous steps succeeded and isMaster is true.

## Versioning

The version is the pipeline build number, defined in name and incremented automatically on every run. The pack step uses it as the package version.

## SonarQube

SonarQube Community runs on an Azure VM (port 9000) and is connected to Azure DevOps through the sonarqube service connection. The conditioned branch is named master because SonarQube Community does not support branch analysis, and the Azure DevOps extension treats master as the main branch for GitHub repos.

## Azure Artifacts

Packages are pushed to an internal NuGet feed. The project Build Service has the Feed Publisher (Contributor) role on the feed.

## E-mail notification

Configured in Azure DevOps (Project settings > Notifications > A build completes) for this pipeline. An e-mail with the run status (succeeded or failed) is sent after every run.

## Run

The trigger is manual: Pipelines > Run pipeline > choose a branch (main or master).
