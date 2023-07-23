
# Multi-tier Web-app hosting using AWS resources

A java-based web application is hosted using aws resources in this project.


## Pre-requisite
- Aws Account
- Domain name
- JDK 11
- Maven 3
- MySQL 8

## Aws Resources
- Ec2 instance
- IAM user 
- S3 bucket 
- Load Balancer
- Autoscaling
- Amazon Route 53

## Project Architecture

![Screenshot](https://drive.google.com/uc?export=view&id=1Cb3lmeznR81Sfa2PckcsFoBA-OQ09ODy)
## Project review
 An HTTP request made by a user with the domain name "http://xyz.com" is received on port 80.The http request is then forwarded by the application load balancer to port 8080 on the tomcat instance where the application is operating. The application server uses the Amazon Route 53 service to route requests to backend services operating on various instances inside a Security group in accordance with the user's request. Requesting backend services results in a response that is sent to the user.




## Project Execution Setup.

### 1. Create key-pair.
a. Sign in to your AWS account, then select Create Key Pair under Instances > Network Security 
b. Choose pem file format type for ssh connection to the instances.

### 2. Create Security groups, add inbound rules that apply to the various services.
#### Security group for Load balancer.

![Screenshot](https://drive.google.com/uc?export=view&id=1rEM-LgjkB-rL6Pt2TSbztAUsQPPYEPTy)

#### Security group for tomcat app and Auto Scaling group.

![Screenshot](https://drive.google.com/uc?export=view&id=1e7WPv1rU42BdLGT9HT5h-t_7Zgcz5PP8)

#### Security group for backend services.

![Screenshot](https://drive.google.com/uc?export=view&id=1zDbDvpj-AmaDg0x_WpSTCsk0IMEDZ-Xi)


### 3. Create EC2 instances for Load balancer, tomcat instance and backend Services.

created an ssh connection to each instance after it was launched, then executed the bash script for each instances listed in the project repository in the aws-LiftAndShift branch within the userdata folder.

![Screenshot](https://drive.google.com/uc?export=view&id=13QGn0LyEQydWLPGKqFmN4s2AyVOXhe3_)


### 4. Update ip to name mapping in Amazon Route 53 for backend services.

![Screenshot](https://drive.google.com/uc?export=view&id=1vkCxofgb-vkKpWRkm28mDenZfy5J2ph7)

### Build Application from source code .

Build the source code artifact using remote CLI. Make sure the pom.xml file is available and perform the "mvn install" command. The "target" folder should then appear.

### 5. Upload the Artifact to s3 bucket.

- create IAM user for s3 bucket acess and provide AmazonS3fullAccess privilege
craete Acess key and Secret key for this user and configure these key with amazon using command : "aws configure" and provide Access key And Secret Key with precaution.

- create s3 bucket through CLI using command 

$  aws s3 mb s3://"unique bucket name"

- copy the Artifact to the s3 bucket using command 

$ aws s3 cp target/vprofile-v2.war s3://"unique bucket name"/

![Screenshot](https://drive.google.com/uc?export=view&id=1VbabwWKA_1K2XOS9RVNTn0B5iYOPb2FG)

### 6.Download this Artifact to tomcat ec2 instance 

- create IAM role for s3 with AmazonS3fullAccess privilege 
- attact this IAM role with tomcat ec2 instance 
- ssh to tomcat instanse 
- install aws CLI
- copy the artifact from the s3 bucket to the /tmp folder of tomcat instance using command 

$ aws s3 cp s3:// "unique bucket name"/vprofile-v2.war /tmp/


### 7. Setup ELB with HTTPS [Amazon certificate Manager] and map  ELB endpoint to website name in Godaddy DNS 

- create a certificate from ACM 
- Once the certificate has been issued, update your domain name record with the certificate endpoint.

### 8. Verify
 - use the url to search using your domain name
http://projectapp.sinchan62project.com/


![Screenshot](https://drive.google.com/uc?export=view&id=16z7UPjzBHFxljjsSPEyfgAtcnFWe7JWb)


- login id :  admin_vp
- password : admin_vp

![Screenshot](https://drive.google.com/uc?export=view&id=1YHLOdiPh3-zezulhU9Q9nnQvH2CbOHt8)
