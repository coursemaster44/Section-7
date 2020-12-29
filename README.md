# Section-7
# Section-7-Deploying-Sample-App-on-AWS-(With-CI_CD)

# CBBuild-lab-1

**Step 1. Goto AWS Management Console>Services>Developers Tools>CodeBuild>first-cd-project>Start new build**
- Build configuration 
  - Timeout - 0 Hours 5 Minutes
Click on Start Build

**Step 2. Open Terminal in Visual Studio Code**
```sh
$ git status 
$ git add .
$ git commit -m "changed node version to 12"
$ git push
```
**Step 3. Goto AWS Management Console>Services>Developers Tools>CodeBuild>first-cd-project>Start Build**
- Build configuration 
  - Timeout - 0 Hours 5 Minutes
- Source version shows node version to 12
- Click on Start Build

**Step 4. See Build Status>Phase details**

**Step 5. Now Goto S3>sample-node-app-amit>devbuild/>first-cd-project**

**Step 6.Goto Developers Tools>CodeBuild>Build projects>first-cd-project>Edit>Artifacts**
- See semantic versioning
Click on Cancel & than click on Discard

**Step 7.Goto AWS Management Console>CloudWatch>CloudWatch>CloudWatch Logs>Log groups>first-cd-project**
- Click on current time logstream
- see the log events

# End of Lab

# CBBuild-S3-lab-2

**Step 1.Zip the Sample-Node_App folder**
```
$ zip -r Sample-Node_App.zip . -x "_MACOSX"**
```
**Step 2.Goto AWS Management Console>Services>S3>Buckets>teacheramitk>Add files**
- Select Sample-Node_App.zip and Open
- Click on Upload

**Step 3.Click on Sample-Node_App.zip**
- Copy S3 URI

**Step 4. Goto AWS Management Console>Services>Developers Tools>CodeBuild>Build Projects>Create Build Project**
- Provide Project Configuration
  - project name - cb-project-s3

- In Source Tab
  - Source provider - Amazon S3
  - Bucket - teacheramitk
  - S3-object-key - Sample-Node_App.zip
  
- In Environment Tab
  - Environment image - Managed image
  - Operating system - Amazon Linux 2
  - Runtime(s) - Standard
  - Image - aws/codebuild/amazonlinux2-x86_64-standard:3.0
  - Image version - Always use the latest image for this version
  - Service role - Existing service role
  - Role ARN - arn:aws:iam::xxxxrole/sevice role/codebuild-first-cd-project-service-role
  
- In Buildspec section: 
  - Build specifications - select “Use a buildspec file”

- Artifacts Section:
  - Type - Amazon S3
  - Bucket name - Sample-Node_App-amit
  - Path - devbuild/
  - Artifacts packaging - Select Zip
  - Select “Disable artifacts encryption”
- Logs section
  - CloudWatch - Select Cloudwatch logs
  - Group name - “cb-project-s3”
  - Stream name - "cb-project-s3"  

Click on Create Build Project

**Step 5.Project is successfully created**
- Click on Start build

**Step 6.Provide Build configuration:**
  - Project - cb-project-s3
  - Build type - Single build  
  - Timeout - 0 Hours 5 Minutes

Click on Start Build
 
**Step 7. Check the status of every step of all phases in phase details-**
```sh
SUBMITTED
QUEUED
PROVISIONING
DOWNLOAD_SOURCE
INSTALL
PRE_BUILD
BUILD
POST_BUILD
FINALIZING
COMPLETED
```
**Step 8.Goto AWS Management Console>CloudWatch>CloudWatch>CloudWatch Logs>Log groups>cb-project-s3**
- Click on current time Log stream
- See the log events

**Step 9. Now Goto S3>sample-node-app-amit>devbuild/>cb-project-s3**

# End of Lab


# CBProject-Lab-3

**Step 1.Goto AWS Console>Services>S3>Create bucket**
- Give Bucket name - sample-node-app-amit
- Unblock all public access & acknowledge
**Step 2. Open sample-node-app-amit bucket>Create Folder**
- Folder name - devbuild
- Encryption - Disable

Click on Create Folder

**Step 3.Goto AWS Console>Developers Tools>CodeBuild>Build projects>Create build project**
- Project name - first-cd-project
- In Source Tab
  - Source provider - AWS CodeCommit
  - Repository - Sample-Node_App
  - Reference Type select “Branch” and Branch as “Master”

- In Environment Tab
  - Environment image - Managed image
  - Operating system - Amazon Linux 2
  - Runtime(s) - Standard
  - Image - aws/codebuild/amazonlinux2-x86_64-standard:3.0
  - Image version - Always use the latest image for this version
  - Service role - New service role
  
- In Buildspec section:
  - Build specifications - select “Use a buildspec file”

- Artifacts Section:
  - Type - Amazon S3
  - Bucket name - Sample-Node_App-amit
  - Path - devbuilds/
  - Artifacts packaging - Select Zip
  - Select “Disable artifacts encryption”

- Logs section
  - CloudWatch - Select Cloudwatch logs
  - Group name - “cb-project-s3”
  - Stream name - "cb-project-s3"

Click on Create Build Project

**Step 4.Project is successfully created**

Click on Start build

**Step 5.Provide Build configuration:**
- Project - cb-project-s3
- Build type - Single build
- Timeout - 0 Hours 5 Minutes

Click on Start Build


**Step 6. Check the status of every step of all phases in phase details-**
```sh
SUBMITTED - succeed
QUEUED    - succeed
PROVISIONING - succeed
DOWNLOAD_SOURCE - Failed----Due to no Buildspec.yml
FINALIZING      - succeed
COMPLETED       - succeed
```


**Step 7. Goto IAM>Roles>codebuild-first-cd-project-service-role**
- Click on Permissions
  - There are two policies 
  - See and compare both policies.

**Step 8.AWS Management Console>CloudWatch>CloudWatch>CloudWatch Logs>Log groups>first-cb-project**
- Click on current time Log stream
  - See the log events
  
- Goto Metrics and validate 

# End of Lab

# CD-ALB-Lab-4

**Step 1. Go to AWS Console>All Services>EC2>Load Balancing>Target Groups>Create Target Group**

**Step 2. Specify group details as following**

- Select Instances in target type
- Target group name - tg-cd
- Protocol-HTTP,Port-3000
- VPC-Default
- Protocol version
- Health Checks
  - Health check protocol- HTTP
  - Health check path- /

Now Click on Next

**Step 3. Register targets**
- Select the available instances
- Ports for the selected instances-3000
- Click on "Include as pending below"

Now Click on Create target group

**Step 4. "tg-cd" Target Group has been created**

**Step 5. Go to AWS Console>All Services>EC2>Load Balancing>Load Balancers>Create load balancer**

**Step 6. Click on Create in Application Load Balancer**

**Step 7. Configure Load Balancer as following**
In Basic Configuration
- Name- alb-cd
- Scheme- internet-facing
- Ip address type- ipv4
- Listeners-Protocol-HTTP,Port-80
- Availability Zones - VPC-Default - Availability Zones-Select all Zones

Click Next:Configure Security Settings

**Step 8.Configure Security Groups**

- Select security group

Click on Next:Configure Routing

**Step 9.Configure Routing**
- Target group- Select Existing target group Name - tg-cd

Click on Register Targets

**Step 10.Click on Next:Review>Click on Create>Click on Close**

**Step 11.EC2>Target groups >tg-cd>edit Attributes**
- change Deregistration delay - 120 seconds

**Step 12.Open Terminal in Visual Studio Code**
```sh
$ git status
$ git commit -a
$ git commit -m "changed index page's color"
$ git push
```
**Step 13.Developers Tools>CodeBuild>Build projects>first-cd-project**
- Click on Retry build

**Step 14.Copy the DNS of Load Balancer and paste it in browser**
- See it is running

**Step 15. Goto S3>sample-node-app-amit>devbuild/>first-cd-project**
- Click on Object actions>Make Public>make public>Exit
- Copy S3 URI

**Step 16. Goto AWS Console>Developers Tools>CodeDeploy>Applications>cd-app>cd-app-asg**
- Click on Edit
- Provide the details:
  - Deployment group name - cd-app-asg-alb
  - Deployment type - In-place
  - Environment configuration - keep Selected Auto Scaling groups 
  - Deployment settings - CodeDeployDefault:AllAtOnce
  - Load Balancer - Enable Load Balancing with Application load Balancer
    - Choose target group
	
Click on Save changes

**Step 17. Click on Create Deployment**
- keep Deployment group as it is
- Revision type - My application is stored in Amazon S3
- Revision location - Paste the S3 URI

Click on Create Deployment

**Step 18.Monitor Life Cycle Events**
```sh
BeforeBlockTraffic
BlockTraffic
AfterBlockTraffic
ApplicationStop
DownloadBundle
BeforeInstall
Install
AfterInstall
ApplicationStart
Validateservice
BeforeAllowTraffic
AllowTraffic
AfterAllowTraffic
```
# End of Lab

# CD-ASG-Lab-5

**Step1-AWS Console>All Services>EC2>Auto Scaling>Launch Configuration>Create Launch Configuration**

**Step2- Provide following details:**
- Give name as “lc-cd” 
- select AMI(Copy it from Ec2 Launch Instance)
- Select instance type t2.micro 
- IAM Instance profile - Ec2S3FullAccess
- In Advanced details>as text>Provide User data:
```sh
#!/bin/bash
$ yum update -y
$ yum install ruby -y
$ yum install wget -y
$ yum install nmap-ncat -y
$ cd /home/ec2-user
$ wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install
$ chmod +x ./install
$ ./install auto
$ service codedeploy-agent status
```
- keep storage as default
- Select security group open to All all ip addresses
- Choose existing key pair 

Click on Create launch configuration

**Step 3. Launch configuration is created**

**Step 4. Select launch configuration “lc-cd ”** 
- Click Actions>create auto scaling group

**Step 5. Give auto scaling group name - asg-cd**
- See launch-configuration and click on Next

**Step 6. Select default vpc and subnets all three subnets**
- Click Next

**Step 7. Select No Load Balancer and Health checks with ELB**
- Click Next

**Step 8 Keep Group size as following**
- Desired - 1 
- Minimum - 1
- Maximum - 1

**Step 9 Select Scaling policies-None**
Click on Next

**Step 10. In Add Notifications Click Next**

**Step 11. In Add tags Click Next**

**Step 12. See Review and Click on Create Auto Scaling Group**

**Step 13. Goto EC2 dashboard and see the newly created instance by ASG**

**Step 14. AWS Console>Developers Tools>CodeDeploy>Applications>cd-app>Create Deployment Group**

**Step 15.Provide details:**
- Deployment group name - cd-app-asg
- Deployment Type - In-Place
- Environment configuration - Select Auto Scaling group asg-cd
- Deployment settings - CodeDeployDefault:AllAtOnce
- Load Balancer - No Load Balancer 
 
Click Create deployment group
	
**Step 16. Click on Create Deployment**
- keep Deployment group as it is
- Revision type - My application is stored in Amazon S3
- Revision location - Paste the S3 URI
- Revision file type - .zip

Click on Create Deployment

**Step 17.Monitor Deployment LifeCycle Events**
```sh
ApplicationStop
DownloadBundle
BeforeInstall
Install
AfterInstall
ApplicationStart
Validateservice
```
**Step 18. Copy the Instance Ip address and paste it in browser**
- See the application running

**Step 19.AWS Console>Developers>CodeDeploy>Applications>cd-app>cd-app-asg>Edit**
- click on cancel and discard

**Step 20.Ec2>Auto Scaling group>Instance Management**
- Select codedeploy hook>Actions>Edit lifecycle hook

**Step 20.Ec2>Auto Scaling groups>asg-cd>Edit Group size**
- Desired - 2
- Minimum - 2
- Maximum - 2

Click on Update

**Step 21.Ec2>Auto Scaling Group>asg-cd>Instances**
- See the lifecycle behaviour pending>pending-wait>pending-wait:proceed>INservice

**Step 22.Click the new Instance and copy & paste the Public IP address in browser to see it running**
- e.g.-13.222.125.52:3000

# End of lab


























