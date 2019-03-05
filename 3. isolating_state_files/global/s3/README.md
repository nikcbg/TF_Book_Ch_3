# Creating S3 bucket.

### Why you need S3 bucket and its purpose.
S3 bucket is needed to store Terraform state file for the module and MySQL database which talks to webservers cluster.

--------------------------------------------------------------------------------------------------------------
### List of files in the repository:
- __main.tf__ - terraform configuration files to create S3 bucket.
- __variables.tf__ - terraform configuration file with variables parameters.
- __output.tf__ - terraform configuration file with output parameters.

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
- go into the subfolder which is this example `cd 3. isolating_state_files/global/s3`.

------------------------------------------------------------------------------------------------------------------
### Commands needed to create the S3 bucket. 

- execute `terraform init` - to initialize the provider and download the neccesery plugins.
  
- execute `terraform apply` - to apply the desired changes, the output should diplay the following:

```
aws_s3_bucket.terraform_state: Creation complete after 9s (ID: name-of-your-S3-bucket)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:

s3_bucket_arn = arn:aws:s3:::name-of-your-S3-bucket

```

- execute `terraform destroy` - to destroy the resource that you just created.
   - you will need to remove or comment out the below part of `main.tf` code in case to destroy the S3 bucket since it has prevent destroy enabled:
 ```
 lifecycle {
    prevent_destroy = true
  }
 ```
