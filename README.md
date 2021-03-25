DOTNETCORESELENIUM
=============================================
Changelog:
--------------------
- 24.03.2021 => Added initial version 1.0.0 which is based on Debian 10 (mcr.microsoft.com/dotnet/core/sdk), latest chrome (v.89) and .NET5.0
--------------------

This Dockerfile is able to build a Linux Docker Image based on Debian 10 which contains:
- .NET5.0
- Last stable Google Chrome
- The goal was to have a comprehensive Docker container where we can run all the .NET Core tests including OpenQA Selenium Chrome Headless UI Tests within CI.
---------------------------------------------

* Current docker image on Artifactory: docker.repo.kapschtraffic.com/ebo/build-container/dotnetcoreselenium:1.0.0

## How to use the dotnetcoreselenium Docker Image
1. Execute `docker build . -t dotnetcoreselenium:1.0.0` to build a docker image
2. Prepare `runtest.sh` file the with the corresponding `dotnet test` command, e.g.:
```bash
#!/bin/bash
dotnet build /p:RuntimeIdentifiers=linux-x64 /p:Configuration=Release ./TestAutomation/TestAutomation.sln
dotnet test --logger:"trx;LogFileName=TestAutomation.SpecFlow.trx" ./TestAutomation/SpecFlow/bin/Release/net5.0/TestAutomation.SpecFlow.dll
```
3. You can either run the docker container in `-it` mode and mount the target Test Solution and run `dotnet test`, e.g.
```
docker run -it -p 12001:80 -v C:\TestAutomation:/tmp/qa dotnetcoreselenium:1.0.0 /bin/bash
```
OR start the execution immediately using the Shell script `runtest.sh` from above, e.g.:
```
docker run -p 12001:80 -v C:\TestAutomation:/tmp/qa docker.repo.kapschtraffic.com/ebo/build-container/dotnetcoreselenium:1.0.0 /bin/bash -C /tmp/qa/runtest.sh
```
