# Example of shared storage for Terraform state file

#### This folder contains terrafom code on how to setup shared storage for terraform state file using S3 bucket in AWS.

-----------------------------------------------------------------------------------------------------------------------
### List of files in the repository:
- __main.tf__ - terraform configuration file and bash script.
- __variables.tf__ - terraform configuration file with input variables.
- __output.tf__ - terraform configuration file with output parameters.

---------------------------------------------------------------------------------------------------------------
###  Infrastructure resources to be created:
- __S3__ - simple storage service is AWS file storage service, it is like unlimited FTP server.
-----------------------------------------------------------------------------------------------------------------
### S3 terraform code parameters explanation:
- __bucket__ - name of S3 bucket(like folder), name must be globally unique.
- __versioning__ - versioning on every file that is updated in the S3 bucket.
- __prevent_destroy__ -  prevents accidental deletion of important resources, in this case S3 bucket.  

---------------------------------------------------------------------------------------------------------------
### How to use this repository:
- install `terraform` from [here](https://www.terraform.io/downloads.html).
- setup Amazon Web Services (AWS) account [here](https://aws.amazon.com/).
- configure your AWS access keys [here](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys).
- configure your AWS access keys as environment variables so you can authenticate to your AWS account:

```
export AWS_ACCESS_KEY_ID="your access key id here"
export AWS_SECRET_ACCESS_KEY="your secret access key here"
```
   
- clone the repository to your local computer: `git clone https://github.com/nikcbg/TF_Book_Ch_3.
- go into the cloned repo on your computer: `cd TF_Book_Ch_3`.
- go into the ubfolder which is this example `cd 1. shared_storage_for_state_files`.

------------------------------------------------------------------------------------------------------------------
### Commands needed to build the webserver on the EC2 instance.
- execute `terraform init` - to initialize the provider and download the neccesery plugins.
  
- execute `terraform plan` - to create execution plan for changes to be applied, the output should diplay the following:  
```
Terraform will perform the following actions:

  + aws_s3_bucket.terraform_state
      id:                          <computed>
      bucket:                      "tf-state-file-bucket"
      versioning.#:                "1"
      versioning.0.enabled:        "true"
      versioning.0.mfa_delete:     "false"

Plan: 1 to add, 0 to change, 0 to destroy.
```
  
- execute `terraform apply` - to apply the desired changes, the output should diplay the following:

```
aws_s3_bucket.terraform_state: Creating...
  bucket:                      "" => "tf-state-file-bucket"
  versioning.#:                "" => "1"
  versioning.0.enabled:        "" => "true"
  versioning.0.mfa_delete:     "" => "false"
aws_s3_bucket.terraform_state: Still creating... (10s elapsed)
aws_s3_bucket.terraform_state: Creation complete after 12s (ID: tf-state-file-bucket)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:

s3_bucket_arn = arn:aws:s3:::tf-state-file-bucket
```
- up to this point the `terraform.tfstate` file is still stored on your local computer.
- to store the file in the S3 bucket we just created you need to add the below code to your `main.tf` file:

```
terraform {
  backend "s3" {
    bucket  = "tf-state-file-bucket"
    region  = "us-east-1"
    key     = "terraform.tfstate"
    encrypt = true
  }
}
```

- execute `terraform init` again to copy the local state to the S3 bucket, the output should diplay the following:

```
Initializing the backend...
Do you want to copy existing state to the new backend?
Pre-existing state was found while migrating the previous "local" backend to the
newly configured "s3" backend. No existing state was found in the newly
configured "s3" backend. Do you want to copy this state to the new "s3"
backend? Enter "yes" to copy and "no" to start with an empty state.
```
- execute `terraform apply` again to refresh the state, the output should diplay the following:  
```
aws_s3_bucket.terraform_state: Refreshing state... (ID: tf-state-file-bucket)

Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

s3_bucket_arn = arn:aws:s3:::tf-state-file-bucket
```
- you can go to your S3 bucket in your AWS account:
  - click on your bucket name where you will see `terraform.tfstate` file.
  - next click the "Show" button next to "Versions" to see versions of your `terraform.tfstate` file.
  
- to destroy the S3 bucket make sure to comment out below parameter in `main.tf` file.
- this way you are disabling prevents accidental deletion and allowing S3 bucket to be deleted.
```
/*  lifecycle {
   prevent_destroy = true
  } */
```

- next delete all versions in the bucket and execute `terraform destroy`. 
- in this case, I will not destroy the S3 bucket because I will use it for the next two folders examples.
