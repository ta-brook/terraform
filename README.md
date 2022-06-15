## Table of contents

- [Terraform](#terraform)
    - [Terraform Basic](#terraform-basics)
        - [Terraform Blocks](#terraform-blocks)
            - [Provider](#provider)
            - [Resource](#resource)
            - [Additional blocks](#additional-blocks)
                - [Input variables](#input-variables)
                - [local values](#local-values)
    - [Installation](#installation)
        - [Window](#window)
    - [Usage](#usage)
        - [GCP](#gcp-infrastructure)
        - [Run your first terraform](#run-your-first-terraform)
    - [Main terraform command](#main-commands)

# Terraform

[HashiCorp Terraform](https://www.terraform.io/intro) is an open source [infrastructure as code (IaC)](https://en.wikipedia.org/wiki/Infrastructure_as_code) software tool that allows Developer to provision infrastructure resources as a code.

![terraform workflow](/assets/terraform_assets.png)

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

### Terraform Blocks

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

#### Provider

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

#### Resource

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

#### Additional blocks

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

## Usage

### GCP infrastructure

- Create `main.tf` file that contain terraform blocks as below. For other blocks you can specified how many service you want and what it's gonna look like. Example: my [main.tf](terraform/main.tf)
```
terraform {
  required_version = ">= 1.0"
  backend "local" {}
  required_providers {
    google = {
      source  = "hashicorp/google"
    }
  }
}
```
- Set all your `variable.tf` the way you want. Example: my [variable.tf](terraform/variable.tf)

### Run your first terraform!

- Running this command will download other plugin to connect with GCP to `/terraform`
```
terraform init
```

- Running this command will activate `main.tf` and create example of how your infrastructure look like!
```
terraform plan
```

- Running this command will apply everything that `terraform plan` tell you to your GCP project
```
terraform apply
```

- Running this command will destroy everything that `terraform plan` described and `terraform apply` created. So, please make sure to destroy it if you not gonna use it yet.
```
terraform destroy
```

*note: If you follow my code in `variable.tf` project_id didn't defined. So you have to define it yourself in terminal when run something like `terraform apply`*

## Main commands:
- `init`          Prepare your working directory for other commands
- `validate`      Check whether the configuration is valid
- `plan`          Show changes required by the current configuration
- `apply`         Create or update infrastructure
- `destroy`       Destroy previously-created infrastructure

