# Oracle WebLogic 12c using Ansible

This setup will do several things=
```
1. Create VirtualBox with Oracle Enterprise Linux 7 using Vagrant
2. Install Oracle WebLogic 12c in Cluster
3. Create CDB and PDB
```
Note: Please modify all necessary configuration files based on your own environment.For Example, one can modify dbca response file to install DB, create CDB and PDB. Please also make sure the installer file already downloaded and placed in the right directory, in this case Oracle WebLogic 12c installer (fmw_12.2.1.4.0_wls.jar) and JDK (jdk-8u221-linux-x64.tar.gz) in roles -> weblogic-install -> files.

Summary commands: 

1. Clone this repository:
```
git clone https://github.com/tazlambert/weblogic-19c-ansible.git
```   
2. Install Ansible in the Control Node
```
sudo yum install git python python-pip
sudo pip install virtualenv
sudo pip install ansible
```
3. Install Vagrant in the Control Node
```
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum update
sudo yum install vagrant
vagrant plugin install vagrant-hostmanager
vagrant plugin install vagrant-disksize
```
4. Install VirtualBox in the Control Node 
```
sudo yum-config-manager --add-repo http://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo
sudo yum update
sudo yum install VirtualBox-6.1
```
5. Start the process
```
ssh-keygen –t rsa
cd weblogic-19c-ansible
export ANSIBLE_CONFIG=ansible.cfg
vagrant up
```
6. Check if installation success
```
vagrant ssh
sudo su - oracle
sqlplus sys as sysdba
```
7. To scrap the installation
```
vagrant destroy
```
