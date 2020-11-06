# Azure DNS Basics

## Azure DNS Zones

@see [Overview of DNS zones and records](https://docs.microsoft.com/en-us/azure/dns/dns-zones-records)

@see [Terraform azurerm_dns_zone](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_zone)

## Azure DNS Records

@see [Overview of DNS zones and records](https://docs.microsoft.com/en-us/azure/dns/dns-zones-records)

@see [Terraform azurerm_dns_a_record](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_a_record)

@see [Terraform azurerm_dns_ns_record](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_ns_record)

!!! danger "Gotcha for Terraform users coming from AWS"
    If you want to add DNS records to DNS zones in Azure DNS you must keep in mind that the name of the DNS record
    must be specified without any trailing subdomain.
   
    Example:
    You want to create an DNS A record pointing `web.mydomain.azure.msgoat.eu` to a public IP address
    within a DNS zone `mydomain.azure.msgoat.eu`. Instead of using the fully qualified DNS name you simply have
    to specify `web` as the DNS record name.
    
## CXP Root DNS Zone

The root DNS zone `azure.msgoat.eu` for the Cloud Expert Program is already present in our current
Azure subscription.

All child DNS zones managing subdomain of azure.msgoat.eu must be linked to this root DNS zone by copying 
the DNS NS record of the child DNS zone to the root DNS zone.

The top level domain `msgoat.eu` is actually managed on AWS using Route 53.
    
## Resources 

@see [Azure DNS documentation](https://docs.microsoft.com/en-us/azure/dns/)