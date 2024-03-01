---
title : "Terraform"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

Terraform profiles a [CLI](https://developer.hashicorp.com/terraform/cli) to help developer run setup cloud infrastructure from terminal. Some of basic cli you need to remember and also the steps to setup a infra:
1. `terraform init`: initialize working directories and download necessary modules, packages.
2. `terraform plan`: create an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure.
3. `terraform apply`: executes the actions proposed in a Terraform plan.
4. `terraform destroy`: destroy all remote objects managed by a particular Terraform configuration.

Install CLI: HashiCorp distributes Terraform as a [binary package](https://developer.hashicorp.com/terraform/install). You can also install Terraform using popular package managers 

Manual Installation:

1. Retrieve the terraform binary by downloading a pre-compiled binary or compiling it from source.

2. Move the Terraform binary to `bin` folder so you can execute command without specifying a path.
```
$ mv ~/Downloads/terraform /usr/local/bin/
```
3. Verify the installation
```
$ terraform -help
Usage: terraform [-version] [-help] <command> [args]

The available commands for execution are listed below.
The most common, useful commands are shown first, followed by
less common or more advanced commands. If you're just getting
started with Terraform, stick with the common commands. For the
other commands, please read the help and docs before usage.
```