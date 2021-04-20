# Blade Infra 


## CLI
01. Clone this Repo

0. Set some vars on the CLI for use
Projectname: the overall unique name for your resources
appreg: the name of the application registration for your hololens app
username: your UPN in Azure
```
projectname=adamsblade
appreg=adamsbladeappreg
username=adamlash@microsoft.com
```

1. Create App Registration
`az ad sp create-for-rbac --name ${appreg}`
- Save Output for later

2. Get ObjectID of App Registation and assign to envvar
`objectid=$(az ad sp list --display-name ${appreg} --query [0].objectId --output tsv)`
- you can echo this `echo $objectid` to check it has a value


3. Create Resource Group
`az group create --name ${projectname}-rg --location eastus`

4. Deploy ARM template to Resource Group
`az deployment group create -f azuredeploy.bicep -g ${projectname}-rg --parameters projectName=${projectname} userId=${username} appRegObjectId=${objectid}`

5. Add values to the Device Sim and Test
