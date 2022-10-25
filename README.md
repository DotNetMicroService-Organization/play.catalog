# Play.Catalog 
Play Economy Catalog microservice.

## Create and publish package Contracts
```powershell
$version="1.0.3"
$owner="DotNetMicroService-Organization"
$ph_pat="[PAT HERE]"

dotnet pack src\Play.Catalog.Contracts\ --configuration Release -p:PackageVersion=$version -p:RepositoryUrl=https://github.com/$owner/play.catalog -o ..\packages

dotnet nuget push ..\packages\Play.Catalog.Contracts.$version.nupkg --api-key $ph_pat --source "github"
```

## Build the docker image
```powershell
$env:GH_OWNER="DotNetMicroService-Organization"
$env:GH_PAT="[PAT HERE]"
$appname="dotnetplayeconomy"
docker build --secret id=GH_OWNER --secret id=GH_PAT -t "$appname.azurecr.io/play.catalog:$version" .
```

## run the docker image
```powershell
$cosmosDbConnString="[CONN STRING HERE]"
$serviceBusConnString="[CONN STRING HERE]"
docker run -it --rm -p 5000:5000 --name catalog -e MongoDbSettings__ConnectionString=$cosmosDbConnString -e ServiceBusSettings__ConnectionString=$serviceBusConnString -e ServiceSettings__MessageBroker="SERVICEBUS" play.catalog:$version
```

## Publishing the docker image
```powershell
az acr login --name $appname
docker push "$appname.azurecr.io/play.catalog:$version"
```