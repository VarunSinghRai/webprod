Assignment for Devops with Assembly Line Training, Linux World! 
Automating using Jenkins, Git, Github to deploy dockers containers to host test and production website.
Git is configured to work on two branches Master i.e. the production and the dev i.e. for the Test or developer

Sequence 1(setting up git) 
open Git and clone repository "webprod"

git clone https://github.com/VarunSinghRai/webprod

Create a new branch dev
git branch dev

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
the only changes would be in step 2) and step 4)
in step2) enter branch details as '/*master'
and in step4) type in the command as  "cp * -rvf /lwweb/prod"

Before we setup job2 to deploy docker containers to host test and prod website, there few homeworks that needs to be done. 
I am using a RedHat Linux 8.0 so depending on the Host OS, you might have to do the steps slightly different. 

Okay, first of all create folder /lwweb and make user jenkins is the owner of folders and subfolders
secondly create subfolders /lwweb/dev and /lwweb/prod

mkdir /lwweb
chown jenkins /lwweb
mkdir -p /lwweb/dev /lwweb/prod

Now, we have to add Jenkins to sudoers and for that make some changes in the /etc/sudoers
please add this two lines in the file
jenkins ALL=(ALL)       NOPASSWD: ALL
%jenkins ALL=(ALL)       NOPASSWD: ALL

Now lets create job2 in Jenkins to deploy dockers containers to host our test and prod websites.
Job 2 for Dev: Create a freestyle Project with name "WebDev-job2"
step1) In Build Trigger section, check "Build after other projects are built", fill "Webdev-job1" as Project to Watch and select buttong for "Trigger only if build is stable"
screenshot : https://github.com/VarunSinghRai/webprod/blob/master/images/Build%20Triggers%20for%20job2.JPG
Step 2) In the Build section, select execute and type in this script. Refer this link https://github.com/VarunSinghRai/webprod/blob/master/images/webdev-job2-execute 
Step 3) Select Save and Apply

Job 2 for Dev: Create a freestyle Project with name "WebDev-job2"
step 1) In Build Trigger section, check "Build after other projects are built", fill "Webdev-job2" as Project to Watch and select buttong for "Trigger only if build is stable"
Step 2)  In the Build section, select execute and type in this script. Refer this link 
https://github.com/VarunSinghRai/webprod/blob/master/images/webdprod-job2-execute
Step 3) Select Save and Apply





