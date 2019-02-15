# Isolating Terraform state file

### Why isolating Terraform state file
It is a good practice to have a separate Terraform state file for each environment(development, staging, production, etc) and not have one single Terraform state file. With separated state files you can only break the state file in the environment you are working on and not break the state files in the other environments. 

----------------------------------------------------------------------------------------------------------------

### Solutions on locking Terraform state file:

#### Terraform Enterprise or Terraform Pro:


---------------------------------------------------------------------------------------------------------------------------

#### Build server(continuous integration CI) solution: 


-------------------------------------------------------------------------------------------------------------------------

#### Terragrunt:

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
- execute `terraform plan` - to create execution plan for changes to be applied, the output should diplay the following:  
```

```
  
- execute `terraform apply` - to apply the desired changes, the output should diplay the following:

```


```

- execute `terraform destroy` - to destroy the resource that we just created, the output should diplay the following:
```

```


