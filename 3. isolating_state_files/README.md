# Isolating Terraform state file

### Why isolating Terraform state file
It is a good practice to have a separate Terraform state file for each environment(development, staging, production, etc) and not have one single Terraform state file. With separated state files you can only break the state file in the environment you are working on and not break the state files in the other environments. 

--------------------------------------------------------------------------------------------------------------
### List of files in the repository:
- __global/s3/main.tf, output.tf, variables.tf__ - terraform configuration files to create S3 bucket.
- __stage/data-stores/mysql/main.tf, output.tf, variables.tf__ - terraform configuration file files to create MySQL dataabse.
- __stage/services/webserver_cluster/main.tf, output.tf, variables.tf__ - terraform configuration files with bash script to create webserver cluster and load balancer.

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
- go into the subfolder which is this example `cd 3. isolating_state_files/`.

------------------------------------------------------------------------------------------------------------------
### Commands needed to test the Terraform code and create separate state file in each folder.

- execute `terraform init` - to initialize the provider and download the neccesery plugins.
  
- execute `terraform apply` - to apply the desired changes.

- execute `terraform destroy` - to destroy the resource that you just created.
