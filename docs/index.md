# ACMP (Also Cloud Marketplace) Provider

## Authenticating to ACMP

Currently the provider (and the ACMP) only supports one form of login: via Username and Password.

Make sure that you save your credentials somewhere save and dont check it into version control or other repos with public access!

When storing the credentials as Environment Variables, for example:

```bash
$ export ACMP_USERNAME="username@domain.com"
$ export ACMP_PASSWORD="SecretP@ssw0rd"
$ export ACMP_REGION="Germany"
```

It's also possible to configure these variables either in-line or from using variables in Terraform.

~> **NOTE:** We'd recommend not defining these variables in-line since they could easily be checked into Source Control.

```hcl
provider "acmp" {

  username = "username@domain.com"
  password = "SecretP@ssw0rd"
  region = "Germany"
}
```

# Notes

Terraform Provider for the ALSO Cloud Marketplace, the actual URL depends on the country your login into the marketplace.

You need to provide a value to the marketplace your connecting to.
See the section Authenticating to ACMP above.

	Austria     = "https://marketplace.also.at"
	Switzerland = "https://marketplace.also.ch"
	Germany     = "https://marketplace.also.de"
	Denmark     = "https://marketplace.also.dk"
	Estonia     = "https://marketplace.also.ee"
	Finland     = "https://marketplace.also.fi"
	Lithuania   = "https://marketplace.also.lt"
	Netherlands = "https://marketplace.also.nl"
	Norway      = "https://marketplace.also.no"
	Sweden      = "https://marketplace.also.se"
	Slovenia    = "https://marketplace.also.si"
	France      = "https://marketplace.alsofrance.fr"
	Latvia      = "https://marketplace.alsolatvia.lv"
	Poland      = "https://marketplace.alsopolska.pl"
	Debug       = "https://marketplacetest.ccpaas.net"

## Example Usage

```hcl
# We strongly recommend using the required_providers block to set the
# Provider source and version being used
terraform {
  required_providers {
    azurerm = {
      source  = "toowoxx/acmp"
      version = "=1.0.0"
    }
  }
}

provider "acmp" {
}

# Create Company in the ACMP
resource "acmp_company" "Company_1" {
  name     = "Company 1"
  location = "West Europe"
  street = "mystreet"
  city = "mycity"
  country = "mygermany"
  zip = "myzip"
  email = "myemail@mydomain.com"
  currency = "mycurrency"
  customer_id = ""
  domain = ["mydomain1.com","mydomain2.com"]
  marketplaces = ["myMarketplace1","MyMarketplace2"]
}

# Provision Subscription for the created company

resource "acmp_subscription" "Microsoft_Organization_Tenant" {
    name = "141309_MicrosoftOrganizationtenant_54197"
    account_id = acmp_company.Company_1.account_id
}

```

## Argument Reference

* List any arguments for the provider block.

The following arguments are supported:

* `region` - (Required) The cloud marketplace API endpoint your connecting. This can usually found out via the table above and which url your usually using to login to ACMP with your User.

---

When authenticating as a Service Principal using a Client Certificate, the following fields can be set:

* `username` - (Optional) The username which should be used. This can also be sourced from the ACMP_USERNAME Environment Variable.
* `password` - (Optional) The password which should be used. This can also be sourced from the ACMP_PASSWORD Environment Variable.