# Azure DNS records

## Overview

!!! danger "Gotcha for Terraform users coming from AWS"
    If you want to add DNS records to DNS zones in Azure DNS you must keep in mind that the name of the DNS record
    must be specified without any trailing subdomain.
   
    Example:
    You want to create an DNS A record pointing `web.mydomain.azure.msgoat.eu` to a public IP address
    within a DNS zone `mydomain.azure.msgoat.eu`. Instead of using the fully qualified DNS name you simply have
    to specify `web` as the DNS record name.