# Isolating Terraform state file

### Why isolating Terraform state file
It is a good practice to have a separate Terraform state file for each environment(development, staging, production, etc) and not have one single Terraform state file. With separated state files you can only break the state file in the environment you are working on and not break the state files in the other environments. 

--------------------------------------------------------------------------------------------------------------

#### Check the links below for terraform example code and explanation on what the code does in each folder:

----------------------------------------------------------------------------------------------------------------------


1.[3.isolating_state_files/global/s3/](https://github.com/nikcbg/TF_Book_Ch_3/tree/master/3.%20isolating_state_files/global/s3) - Terraform configuration code that creates S3 bucket in AWS that stores terraform state file. 

2.[3.isolating_state_files/stage/data-stores/mysql](https://github.com/nikcbg/TF_Book_Ch_3/tree/master/3.%20isolating_state_files/stage/data-stores/mysql) - Terraform configuration code that creates MySQL database which talks to webservers cluster.

3.[3.isolating_state_files/stage/services/webserver_cluster](https://github.com/nikcbg/TF_Book_Ch_3/tree/master/3.%20isolating_state_files/stage/services/webserver_cluster) - Terraform configuration code that creates webservers cluster and load balancer in AWS.

