# Play.Catalog 
Play Economy Catalog microservice.

## Create and publish package Contracts
```powershell
$version="1.0.2"
$owner="DotNetMicroService-Organization"
$ph_pat="[PAT HERE]"

dotnet pack src\Play.Catalog.Contracts\ --configuration Release -p:PackageVersion=$version -p:RepositoryUrl=https://github.com/$owner/play.catalog -o ..\packages

dotnet nuget push ..\packages\Play.Catalog.Contracts.$version.nupkg --api-key $ph_pat --source "github"
```

## Build the docker image
```powershell
$env:GH_OWNER="DotNetMicroService-Organization"
$env:GH_PAT="[PAT HERE]"
docker build --secret id=GH_OWNER --secret id=GH_PAT -t play.catalog:$version .
```

## run the docker image
```powershell
docker run -it --rm -p 5000:5000 --name catalog -e MongoDbSettings__Host=mongo -e RabbitMqSettings__Host=rabbitmq --network playinfra_default play.catalog:$version
```