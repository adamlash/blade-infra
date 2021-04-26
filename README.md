# Blade Infra 
## Required Installs
Azure CLI: https://docs.microsoft.com/en-us/cli/azure/install-azure-cli

## CLI
### 1. Clone this Repo

### 2. Set some vars on the CLI for use
* Projectname: the overall unique name for your resources
* appreg: the name of the application registration for your hololens app
* username: your UPN in Azure


```
projectname=adamsblade
appreg=adamsbladeappreg
```

### 3. Create App Registration
`az ad sp create-for-rbac --name ${appreg} --skip-assignment`


Save Output for later

### 4. Get ObjectID of App Registation and User and assign to envvar
```
objectid=$(az ad sp list --display-name ${appreg} --query [0].objectId --output tsv)
userid=$(az ad signed-in-user show --query objectId -o tsv)
```
you can echo this `echo $objectid` and `echo $userid` to check it has a value


### 5. Create Resource Group
`az group create --name ${projectname}-rg --location eastus`

### 6. Deploy ARM template to Resource Group
`az deployment group create -f azuredeploy.bicep -g ${projectname}-rg --parameters projectName=${projectname} userId=${userid} appRegObjectId=${objectid}`

### 7. Add values to the Device Sim and Test
