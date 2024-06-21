# play.inventory
Play Economy Inventory microservice

## Create and publish inventory package
```powershell
$version="1.0.1"
$owner="ISoftt"
$github_personal_access_token="specify here"

dotnet pack src\Play.Inventory.Contract\ --configuration Release -p:PackageVersion=$version -p:RepositoryUrl=https://github.com/$owner/play.inventory -o ..\packages

dotnet nuget push ..\packages\Play.Inventory.Contract.$version.nupkg --api-key $github_personal_access_token --source "github"
```

## Build the docker image
```powershell
$env:GH_OWNER="ISoftt"
$env:GH_PAT="[PAT HERE]"
docker build --secret id=GH_OWNER --secret id=GH_PAT -t play.inventory:$version .
```

## Run the docker image
```powershell
docker run -it --rm -p 5004:5004 --name inventory -e MongoDbSettings__Host=mongo -e RabbitMQSettings__Host=rabbitmq --network playinfra_default play.inventory:$version
```