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

--------------------------------------------------------------------------------------------------------------
