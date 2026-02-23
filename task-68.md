## Day 68: Set Up Jenkins Server

1- Switch to jenkins server
``` bash
ssh root@jenkins
> Enter password: ********
``` 
---
2 - Install jenkins service
``` bash
sudo yum install wget -y
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/rpm-stable/jenkins.repo
sudo yum upgrade
# Add required dependencies for the jenkins package
sudo yum install fontconfig java-21-openjdk -y
sudo yum install jenkins -y
sudo systemctl daemon-reload
```
---
3- Start jenkins service
``` bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```
---
4- Configure jenkins by clicking on the jenkins button and adding user, password, and email details.