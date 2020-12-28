# Section-7
# Section-7-Deploying-Sample-App-on-AWS-(With-CI_CD)

# CBBuild-lab

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

# CBBuild-S3-lab

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


# CBProject-Lab

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


