# Locking Terraform state file 

### Why locking the state file

To prevent corruption, data loss, conflicts or race conditions (performing two or more operations at the same time)
when more than one team member is running Terraform at the same time on the same state file.

----------------------------------------------------------------------------------------------------------------

### Solutions on locking Terraform state file:

#### Terraform Enterprise or Terraform Pro:
- both products support locking state file. 
- both products are paid. 
- more information about them can be found [here](https://www.hashicorp.com/products/terraform).

---------------------------------------------------------------------------------------------------------------------------

#### Build server(continuous integration CI) solution: 
- removes the need for locking the state file.
- all deployment changes are applied automatically by build server such as Jenkins or CircleCI.
- server can be configured to never apply more than one change at the same time.
- build server discovers bugs and runs tests before applying any changes.

-------------------------------------------------------------------------------------------------------------------------

#### Terragrunt:
- open source warapper for Terraform.
- configures remote state automatically.
- provides locking of the state file using Amazon DynamoDB(Amazon database service).
- this solution is used in the example below.
--------------------------------------------------------------------------------------------------------------
### List of files in the repository:
- __main.tf__ - terraform configuration file and bash script.
- __variables.tf__ - terraform configuration file with input variables.
- __output.tf__ - terraform configuration file with output parameters.

----------------------------------------------------------------------------------------------------------------------
### How to use this repository:
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
- go into the subfolder which is this example `cd 2. locking_state_file`.

------------------------------------------------------------------------------------------------------------------
### Commands needed to create the S3 bucket and move the terraform state file to S3 bucket using Terragrunt.

  #### Installing and configuring Terragrunt:
    - go to Terragrunt repo in GitHub and follow the [instructions](https://github.com/gruntwork-io/terragrunt#install-terragrunt) how to isntall it.
    - create `.terraform.tfvars` file in `2. locking_state_file` directory and put the below code:
```
    # Configure Terragrunt to use DynamoDB for locking
lock = {
  backend = "dynamodb"

  config {
    state_file_id = "global/s3"
  }
}

# Configure Terragrunt to automatically store tfstate file in S3
terragrunt {
remote_state = {
  backend = "s3"

  config {
    encrypt = true
    bucket = "tf-state-file-bucket"
    key = "global/s3/terraform.tfstate"
    region = "us-east-1"
   }
 }
}

```
- execute `terragrunt plan` - to create execution plan for changes to be applied, the output should diplay the following:  
```
[terragrunt] [/home/nikolay/repos/book_examples/ch3/shared_storage_for_state_files] 2019/02/14 16:06:40 Running command: terraform --version
[terragrunt] 2019/02/14 16:06:40 Reading Terragrunt config file at /home/nikolay/repos/book_examples/ch3/shared_storage_for_state_files/terraform.tfvars
[terragrunt] 2019/02/14 16:06:40 Initializing remote state for the s3 backend
[terragrunt] 2019/02/14 16:06:41 Configuring remote state for the s3 backend
[terragrunt] 2019/02/14 16:06:41 Running command: terraform init -backend-config=bucket=tf-state-file-bucket -backend-config=key=global/s3/terraform.tfstate -backend-config=region=us-east-1 -backend-config=encrypt=true

Terraform has been successfully initialized!

Plan: 1 to add, 0 to change, 0 to destroy.
```
  
- execute `terragrunt apply` - to apply the desired changes, the output should diplay the following:

```
aws_s3_bucket.terraform_state: Creation complete (ID: tf-state-file-bucket)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

The state of your infrastructure has been saved to the path
below. This state is required to modify and destroy your
infrastructure, so keep it safe. To inspect the complete state
use the `terraform show` command.

State path: 

Outputs:

s3_bucket_arn = arn:aws:s3:::tf-state-file-bucket

```

- execute `terraform destroy` - to destroy the resource that we just created, the output should diplay the following:
```
aws_s3_bucket.terraform_state: Refreshing state... (ID: tf-state-file-bucket)
aws_s3_bucket.terraform_state: Destroying... (ID: tf-state-file-bucket)
aws_s3_bucket.terraform_state: Destruction complete

Destroy complete! Resources: 1 destroyed.

```

