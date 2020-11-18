# Naming Rules and Naming Conventions for Azure Resources

## Naming Rules

@see [Naming rules and restrictions for Azure resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/resource-name-rules)

## Naming Conventions

@see [Recommended naming and tagging conventions](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)

## CXP Naming Conventions

We are going to use the following tailored naming conventions in our Cloud Expert Programm:

* all names are using lowercase characters only 
* each part is separated by a dash or hyphen character `-`
* first part of each name is the prefix defined in the Azure [Recommended naming and tagging conventions](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)
* second part of each name is the region code of the Azure region hosting the resource (e.g. `westeu` for `Western Europe`)
* third part of each name is the name of the Azure solution (i.e. product name, project name, team name etc.)
* further parts as appropiate
