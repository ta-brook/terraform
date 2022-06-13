# Terraform

HashiCorp Terraform is an infrastructure as code tool that lets you define both cloud and on-prem resources in human-readable configuration files that you can version, reuse, and share. You can then use a consistent workflow to provision and manage all of your infrastructure throughout its lifecycle. Terraform can manage low-level components like compute, storage, and networking resources, as well as high-level components like DNS entries and SaaS features.

![terraform workflow](/terraform_assets/terraform_assets.png)

## Terraform Basics

Terraform file is end with `.tf` such as `main.tf` below

```
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "3.5.0"
    }
  }
}

provider "google" {
  credentials = file("<NAME>.json")

  project = "<PROJECT_ID>"
  region  = "us-central1"
  zone    = "us-central1-c"
}

resource "google_compute_network" "vpc_network" {
  name = "terraform-network"
}
```

### Terraform Block

Terraform is consist of `terraform {}` that contain terraform setting required to provision your infrastructure.

```
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "3.5.0"
    }
  }
}
```

- For each, provider need the `source` variable to defines an optional hostname, a namespace, and the provider type which came from this [Terraform Registry](https://registry.terraform.io/) 
- `version` is optional but you can also specify the version you satisfied with.

### Provider

```
provider "google" {
  credentials = file("<NAME>.json")

  project = "<PROJECT_ID>"
  region  = "us-central1"
  zone    = "us-central1-c"
}
```

- The provider block configures the specified provider, in this repo we talked about `gcp`.
- you can also have multiple provider in one terraform such as `aws`, `azure`.

### Resource

Use resource blocks to define components of your infrastructure. A resource might be a physical component such as a server, or it can be a logical resource such as a Heroku application.

```
resource "google_compute_network" "vpc_network" {
  name = "terraform-network"
}
```

In the code block we have:
- resource type: `google_compute_network`
- resource name: `vpc_network`
- The ID of network: `google_compute_network.vpc_network`

### Additional blocks

this would be in `variable.tf`

#### Input variables

The variable block configures a single input variable for a Terraform module. Each block declares a single variable.

The name given in the block header is used to assign a value to the variable via the CLI and to reference the variable elsewhere in the configuration.

```
variable "region" {
    description = "Region for GCP resources. Choose as per your location: https://cloud.google.com/about/locations"
    default = "asia-southeast1"
    type = string
}

variable "images" {
  type = "map"

  default = {
    us-east-1 = "image-1234"
    us-west-2 = "image-4567"
  }
}
```
- `type` (Optional): can be defines to validate value
- `default` (Optional): the value must be provided by this field or on CLI while terraform is running.
- `description` (Optional): provide information about this variable

We can called these variable in `main.tf` with:

```
region = var.region
images = var.images
```


#### local values

A local value assigns a name to an expression, so you can use the name multiple times within a module instead of repeating the expression.

```
local {
    region= "asia-southeast1"
    zone = "asia-southeast1-a"
}
```
We can called these variable in `main.tf` with:

```
region = local.region
zone = local.zone
```

## Installation

### Window

1. you can download terraform from [here](https://www.terraform.io/downloads)
2. unzip and place somewhere in your system. let's say `C:/terraform/`
3. add `C:/terraform/` to your path environment
4. open terminal
5. type `terraform -version`