Configure Jenkins Slaves on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.

Prerequisites
Jenkins Master Running Get help here
EC2 Ubuntu 18.x Instance - _for Slave Node
With Internet Access
Security Group with Port 8080 open for internet
Java v1.8.x
Install Java
apt-get install java-1.8*
#apt-get -y install java-1.8.0-openjdk
Setup Jenkins Slave
# Create user and add the user to wheel group
adduser jenkins-slave-01
# Create SSH Keys
sudo su - jenkins-slave-01
ssh-keygen 
# The private and public keys will be created at these locations `/home/jenkins-slave-01/.ssh/id_rsa` and `/home/jenkins-slave-01/.ssh/id_rsa.pub`
cd .ssh
cat id_rsa.pub > authorized_keys
chmod 700 authorized_keys

#create workspace
like > /home/jenkins-slave-01/
Configuration on Master
Copy the slave node's public key[id_rsa.pub] to Master Node's known_hosts file

mkdir -p /var/lib/jenkins/.ssh
cd /var/lib/jenkins/.ssh
ssh-keyscan -H SLAVE-NODE-IP-OR-HOSTNAME >>/var/lib/jenkins/.ssh/known_hosts
# ssh-keyscan -H 172.31.38.42 >>/var/lib/jenkins/.ssh/known_hosts
chown jenkins:jenkins known_hosts
chmod 700 known_hosts
Configure the Slave using Manage Jenkins
Configure the node as shown here Manage Jenkins > Manage Nodes > New Node

Test Jenkins Jobs
Create “new item”
Enter an item name – My-First-Project
Chose Freestyle project
Under General Section
Choose Restrict where this project can be run
Update your jenkins slave label Java
Under Build section Execute shell
echo "welcome to jenkins slave"
Save your job
Build job
Check "console output"
