# Sample app for demonstrating continuous integration and deployment of a multi-container Docker app to Azure Container Service
This repository contains a sample Azure multi-container Docker application.

* service-a: Angular.js sample application with Node.js backend 
* service-b: ASP .NET Core sample service

## Run application locally
First, compile the ASP .NET Core application code. This uses a container to isolate build dependencies that is also used by VSTS for continuous integration:

```
docker-compose -f docker-compose.ci.build.yml run ci-build
```

(On Windows, you currently need to pass the -d flag to docker-compose run and poll the container to determine when it has completed).

Now build Docker images and run the services:

```
docker-compose up --build
```

The frontend service (service-a) will be available at http://localhost:8080.

## Integration with Azure Container Service 
https://docs.microsoft.com/en-us/azure/container-service/container-service-setup-ci-cd

```

az login

az account set --subscription <subscription id>

az acs list   (use this to see the acs name)

az container release create ^
--target-name containerservice-ACSTrials ^
--target-resource-group ACSTrials ^
--registry-name jomitregistry ^
--vsts-account-name jomitv ^
--vsts-project-name ACSTrials ^
--remote-access-token <Git Token>

az container release list ^
--target-name containerservice-ACSTrials ^
--target-resource-group ACSTrials

```