---
layout: post
title: Terraform getting started
description: Terraform getting started
keywords: 
---

### Installation

Download terraform CLI [here](https://www.terraform.io/downloads.html)

Unzip and move the `terrform` executable file to `/usr/local/bin`

### Example with docker

```hcl
resource "docker_image" "nginx" {
	name = "nginx:latest"
	keep_locally = false
}

resource "docker_container" "nginx" {
	image = docker_image.nginx.latest
	name = "tutorial"
	ports {
		internal = 80
		external = 8000
	}
}
```

**Common Commands**

```bash
terraform init

terraform plan

terraform apply
```

To stop the container

```bash
terraform destroy
```

### Terraform with Google Cloud

1. Create a project on google cloud
2. [Enable Google Cloud Engine API for created project](https://console.developers.google.com/apis/library/compute.googleapis.com?authuser=1&project=tf-test-286005&folder=&organizationId=)
3. Create GCP Service account key by access [this](https://console.cloud.google.com/apis/credentials/serviceaccountkey?authuser=1&project=tf-test-286005) with the Role is `Project-Editor`
4. Download the generated JSON file which contains credentials.

```hcl
provider "google" {
  version = "3.5.0"
  credentials = file("tf-test-2760c44799d7.json")

  project = "tf-test-286005"
  region = "us-central1"
  zone = "us-central1-c"
}

resource "google_compute_network" "vpc_network" {
  name = "terraform-network"
}

resource "google_compute_instance" "vm_instance" {
  name = "terraform-instance"
  machine_type = "f1-micro"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  network_interface {
    network = google_compute_network.vpc_network.name
    access_config {
    }
  }
}
```

**How to re-create instance?**

```bash
terraform taint
```

### Define input variables

- Defining variables in `variables.tf` file

```hcl
variable "project" {}

variable "credentials_file" {}

variable "region" {
  default = "us-central1"
}

variable "zone" {
  default = "us-central1-c"
}
```

Whenever you run `terraform apply`, Terraform will prompt you for the value for `project` and `credentials_file`

- Using variables in configuration

```hcl
provider "google" {
  version = "3.5.0"

  credentials file(var.credentials_file)

  project = var.project
  region = var.region
  zone = var.zone
}
```

- How to assign variable values

1. `-var` flag
2. from `terraform.tfvars` file
3. `-var-file`
4. from ENV `TF_VAR_name` 

