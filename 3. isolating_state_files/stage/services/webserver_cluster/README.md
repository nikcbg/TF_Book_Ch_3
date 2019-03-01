# Creating webservers cluster and load balancer in AWS using terraform module

This folder contains terraform code that creates webservers cluster and load balancer in AWS. Make sure to create MySQL databse first because webservers cluster depends on it. 

--------------------------------------------------------------------------------------------------------------
### List of files in this folder:
- main.tf - terraform configuration file that pulls code form module that creates webserver cluster and load balancer.
- variables.tf - terraform configuration file with input variables.
- output.tf - terraform configuration file with output parameters.
----------------------------------------------------------------------------------------------------------------------
### How to use this part of the repository:
- install `terraform` from [here](https://www.terraform.io/downloads.html).
- setup Amazon Web Services (AWS) account [here](https://aws.amazon.com/).
- configure your AWS access keys [here](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys).
- configure your AWS access keys as environment variables so you can authenticate to your AWS account:

```
export AWS_ACCESS_KEY_ID="your access key id here"
export AWS_SECRET_ACCESS_KEY="your secret access key here"
```
   
- clone the repository to your local computer: `git clone https://github.com/nikcbg/TF_Book_Ch_3`.
- go into the cloned repo on your computer: `cd TF_Book_Ch_3`.
- go into the subfolder which is this example `cd 3. isolating_state_files/stage/services/webserver_cluster`.

------------------------------------------------------------------------------------------------------------------

### Configure remote state using S3 bucket:
- terraform state file of this folder will be using the module state file which uses MySQL state file stored in the S3 bucket 
- add the below code to your `variables.tf` file:
   - both `default` parameters don't have to be uploaded to GitHub, they can exist only in your code in your local computer.

```
variable "db_remote_state_bucket" {
  description = "The name of the S3 bucket used for the database's remote state storage"
  default = "name-of-your-S3-bucket"
}

variable "db_remote_state_key" {
  description = "The name of the key in the S3 bucket used for the database's remote state storage"
  default = "path to the state file inside the bucket"
}
```

-------------------------------------------------------------------------------------------------------------------

### Commands needed to build the webservers cluster and laod balancer.
- execute `terraform init` - to initialize the provider and download the neccesery plugins.
  
- execute `terraform plan` - to create execution plan for changes to be applied, the output should diplay the following:

```
Terraform will perform the following actions:


  
Plan: 11 to add, 0 to change, 0 to destroy.
```
  
- execute `terraform apply` - to apply the desired changes, the output should diplay the following:

```
Terraform will perform the following actions:


Apply complete! Resources: 11 added, 0 changed, 0 destroyed.

Outputs:

elb_dns_name = name-of-webserver-cluster
```
- execute `terraform destroy` - to destroy the resources you created, the output should diplay the following:
  
```
Terraform will perform the following actions:

  
  
Plan: 0 to add, 0 to change, 11 to destroy.



Destroy complete! Resources: 11 destroyed.
```
