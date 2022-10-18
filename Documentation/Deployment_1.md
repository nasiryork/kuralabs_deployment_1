<h1 align=center>Deployment 1 Documentation</h1>

## Deployment goal:
- Set up a deployment pipeline using Jenkins

## Software and Tools Used:
- GitHub, Amazon EC2, Jenkins, and Elastic Beanstalk

## GitHub:
To begin this deployment I needed to start in GitHub. 
- I needed to fork the repository: kura-labs-org/kuralabs_deployment_1. 
- After forking the repo I created a token that I would use later to connect Jenkins with Github and validate my access to the repo. 

## EC2:
I created an EC2 instance that ran on Ubuntu and used the deployment-1 Security Group. 
- Within the security group I opened ports 22 for SSH access, 80 for HTTP access, and port 8080 to access Jenkins. 
- With my EC2 up and running I SSH into it to install Python, Jenkins, and Its main dependency Java. After following the documentation i was able to set up jenkins.
- I was able to start Jenkins after using the command:  
```
sudo systemctl start jenkins
```
## Jenkins:
With Jenkins started I typed in the public ip of my ec2 followed by port :8080 to launch Jenkins. Once in Jenkins I created a new item and started the formation of my multibranch pipeline. 
- After entering the name of the application I needed to link the forked repo where the application was stored. 
- Also contained in the repo was a file called : JenkinsFile, which tells jenkins how to construct the deployment pipeline. 
- The two stages in this pipeline were the build stage and the test stage.
```
stage ('Build') {
      steps {
        sh '''#!/bin/bash
        python3 -m venv test3
        source test3/bin/activate
        pip install pip --upgrade
        pip install -r requirements.txt
        export FLASK_APP=application
        flask run &
        '''
     }
   }
    stage ('test') {
      steps {
        sh '''#!/bin/bash
        source test3/bin/activate
        py.test --verbose --junit-xml test-reports/results.xml
        ''' 
      }
```
![image](https://github.com/nasiryork/kuralabs_deployment_1/blob/main/static/Deployment%201%20Jenkins.png)

## Elastic Beanstalk:
With Jenkins Pipeline working it was time to deploy my application to Elastic Beanstalk.
- Inorder to run my application I needed to log into my virtual machine and compress the deployment 1 repo into a zip file.  
- Once zipped I was able to upload the zip file to elastic beanstalk for the virtual environment to be created for my application. After some trial and error I was able to get my application to run.

## Challenges Faced: 
I faced two main challenges during this deployment. 
- The First of which came during the initial EC2 SSH stage. Due to an incorrect Key Pairing with my EC2 I was unable to SSH into it. I resolved this issue by deleting the old EC2 instance, configuring a new one and creating a new key pair. 
- The second error that I made occurred during the Virtual Machine/ Elastic Beanstalk stage. I was getting a notification from Elastic Beanstalk saying that the health of my application was Degraded/Severe. The issue that caused this problem was a syntax error when zipping the compressed application; instead of zipping just the file I zipped a larger portion of my directory. Once I zipped the application and reuploaded it, elastic beanstalk ran as intended.

## Improvements:
- The biggest improvement that can be made is to simplify the Jenkins installation process. There were alot of different commands entered that could be put into a script to run Jenkins.
