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
