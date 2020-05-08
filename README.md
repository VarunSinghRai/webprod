Assignment for Devops with Assembly Line Training, Linux World!
Automating using Jenkins, Git, Github to deploy dockers containers to host test and production website.
And finally to Merge Branch Dev with Master Branch.
Git is configured to work on two branches Master i.e. the production and the dev i.e. for the Test or developer

Sequence 1(setting up git) 
open Git and clone repository "webprod"

git clone https://github.com/VarunSinghRai/webprod
1) touch index.html linux.html 
2) git add index.html linux.html
3) commit -m "update"
4) git push -u
5) git branch dev 
6) git checkout 
7) vi index.html 
8) git add index.html
9) git commit -m " "
10) git push -f

Sequence 2 ( setting up Jenkins job)

job1 for dev: Create a freestyle Project with name "WebDev-job1"

1)check Github Project and fill in repository address https://github.com/VarunSinghRai/webprod
screenshot :https://github.com/VarunSinghRai/webprod/blob/master/images/Github%20Project%20URL.JPG

2)Source Code Managemet and check Git button, fill in the repository address as above, and the branch as '/*dev'
screenshot :https://github.com/VarunSinghRai/webprod/blob/master/images/Source%20Code%20Management.JPG

3)Build Triggers, Check the box "GitHub hook trigger for GITScm polling"
screenshot :https://github.com/VarunSinghRai/webprod/blob/master/images/Build%20Triggers.JPG

4)Now lastly in Build section, select an Execute Shell and type in command "cp * -rvf /lwweb/dev"
screenshot :https://github.com/VarunSinghRai/webprod/blob/master/images/Build%20Execute.JPG

5)Click Save and Apply

job1 for prod: Create a freestyle Project with name "WebProd-job1" 
in a similar manner create a job for the production site
1) the only changes would be in step 2) and step 4)
2) in step2) enter branch details as '/*master'
3) and in step4) type in the command as  "cp * -rvf /lwweb/prod"

Before we setup job2 to deploy docker containers to host test and prod website, there few homeworks that needs to be done. 
I am using a RedHat Linux 8.0 so depending on the Host OS, you might have to do the steps slightly different. 

1) Okay, first of all create folder /lwweb and make user jenkins is the owner of folders and subfolders
2) secondly create subfolders /lwweb/dev and /lwweb/prod

1) mkdir /lwweb
2) chown jenkins /lwweb
3) mkdir -p /lwweb/dev /lwweb/prod

1) Now, we have to add Jenkins to sudoers and for that make some changes in the /etc/sudoers
please add this two lines in the file
2) jenkins ALL=(ALL)       NOPASSWD: ALL
3) %jenkins ALL=(ALL)       NOPASSWD: ALL

Now lets create job2 in Jenkins to deploy dockers containers to host our test and prod websites.

Job 2 for Dev: Create a freestyle Project with name "WebDev-job2"
1) In Build Trigger section, check "Build after other projects are built", fill "Webdev-job1" as Project to Watch and select buttong for "Trigger only if build is stable"
screenshot : https://github.com/VarunSinghRai/webprod/blob/master/images/Build%20Triggers%20for%20job2.JPG
2) In the Build section, select execute and type in this script. Refer this link https://github.com/VarunSinghRai/webprod/blob/master/images/webdev-job2-execute 
3) Select Save and Apply

Job 2 for Dev: Create a freestyle Project with name "WebDev-job2"
1) In Build Trigger section, check "Build after other projects are built", fill "Webdev-job2" as Project to Watch and select buttong for "Trigger only if build is stable"
2)  In the Build section, select execute and type in this script. Refer this link 
https://github.com/VarunSinghRai/webprod/blob/master/images/webdprod-job2-execute
3) Select Save and Apply

We will also create a third job, this will be to merge branch 'dev' with our master branch  'master' in Github. 

For making this happen, we need make our jenkins accessible to our Github repository. And for this you need to download ngrok, visit link https://ngrok.com/download

1) open terminal in Redhat Linux
2) extract the zip folder containing the .ngrok file
3) run the scrip, ./ngrok http 8080

Now, in the Jenkins configure webhooks using link that you received after running .ngrok script

in my case its http://b7ad53e1.ngrok.io
1) enter this details in Payload URL: http://cfb3a820.ngrok.io/github-webhook/
Make sure entry always ends with / otherwise your webhook will not sync.
screenshot: https://github.com/VarunSinghRai/webprod/blob/master/images/configure%20ngrok.JPG

Finally, the third job to call the merge of branch dev onto master branch in Github.
(to callin the merge first you will have to configure credentials, refer this link )
1) Create a freestyle Project with name "WebDev-Merge"
2)check Github Project and fill in repository address https://github.com/VarunSinghRai/webprod
screenshot :https://github.com/VarunSinghRai/webprod/blob/master/images/Source%20Code%20Management%20with%20github%20creds.JPG
3) Click Add and select the Github credentials 
4) fill in the repository address as the above screenshot, and the branch as '/*dev'
5) Now configure Post build actions to Merge Branch dev with out master branch in github, refer screenshot
https://github.com/VarunSinghRai/webprod/blob/master/images/Post%20build%20actions.JPG
6) Click save and apply

Test your system by updating Dev and Master Branch

user curl to check your hosted docker containers 
1) curl http://hostipaddress:8081
2) curl http://hostipaddress:8082

once satisfactory, run the build of Jenkins third job to call the merge.








