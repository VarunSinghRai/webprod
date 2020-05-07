Assignment for Devops Training, Linux World! 
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
2)Source Code Managemet and check Git button, fill in the repository address as above, and the branch as '/*dev'
3)Build Triggers, Check the box "GitHub hook trigger for GITScm polling"
4)Now lastly in Build section, select an Execute Shell and type in command "cp * -rvf /lwweb/dev"
5)Click apply Save

job1 for prod: Create a freestyle Project with name "WebDev-job1" 
in a similar manner create a job for the production site
the only changes would be in step 2) and step 4)
in step2) enter branch details as '/*master'
and in step4) type in the command as  "cp * -rvf /lwweb/prod"

Before we setup job2 to deploy docker containers to host test and prod website, there few homeworks that needs to be done. 
I am using a RedHat Linux 8.0 so depending on the Host OS, you might have to do the steps slightly different. 

Okay, first of all create folder /lwweb and make user jenkins the owner of folder
secondly create subfolders /lwweb/dev and /lwweb/prod

mkdir /lwweb
chown jenkins /lwweb
mkdir -p /lwweb/dev /lwweb/prod

Now, we have to add Jenkins to sudoers and for that make some changes in the /etc/sudoers
please add this two lines in the file
jenkins ALL=(ALL)       NOPASSWD: ALL
%jenkins ALL=(ALL)       NOPASSWD: ALL

Now lets create job2 in Jenkins to deploy dockers containers to host our test and prod websites.





