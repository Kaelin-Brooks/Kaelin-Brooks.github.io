I. Introduction
For my course project, I selected Option C, which involves following the instructions from Project A to install and use Ansible and Vagrant.
II. Ansible
What is Ansible?
“Ansible is an open source, command-line IT automation software application written in Python. It can configure systems, deploy software, and orchestrate advances workflows to support application deployment, system updates, and more (Red Hat, n.d.).”

What is the purpose of Ansible?
“Before Ansible, managing hundreds of servers meant either writing complex shell scripts or dealing with heavyweight configuration management tools that required significant setup (Rubenfiszel, 2024).”

How does Ansible accomplish its purpose?
Ansible operates using a control node, which is the location where Ansible is installed and from which all tasks are managed and executed. From this central node, users can run Ansible commands and playbooks. The systems managed by Ansible, called managed nodes, are remote machines that do not require any agents to be installed; communication with these nodes is established securely via SSH. Ansible maintains an inventory, which is a list of all managed nodes it can interact with. Playbooks, written in YAML, define a series of tasks that Ansible carries out on the managed nodes. These tasks are performed using modules—small, reusable programs designed to execute specific functions. When a playbook is run, Ansible transfers the necessary modules to the managed nodes, executes them and then removes them once the tasks are complete.

Use Cases
**Scenario #1: You are a Developer that needs to push code updates and restart services on all web servers simultaneously without any downtime.**

**Scenario #2: You are a System Administrator that needs to run scheduled updates and install security patches across your server fleet to keep the system compliant.**

III. Vagrant
What is Vagrant?
Vagrant is a command-line tool for building and managing virtual machine environments through a unified workflow. It can automatically create lightweight disposable virtual machines for development and testing purposes.

What is the purpose of Vagrant?
Vagrant was started in 2009 with the goal of streamlining the development of infrastructure that mirrors production environments. Before Vagrant, development environments were primarily set up manually, making them prone to errors and time consuming to manage. Additionally, testing infrastructure automation was a slow process, as each test required creating and destroying an entire virtual machine.

How does Vagrant accomplish its purpose?
With Vagrant, there’s no need to manually install an operating system. Instead, virtual machines are set up using images known as Vagrant boxes, which are available through the Vagrant Cloud. VM configuration and management are handled through a single file called a Vagrantfile.

Uses Cases
**Scenario #1: You need to deploy multiple virtual machines in Oracle VirtualBox. Manually spinning up each VM would be time-consuming and cumbersome. Instead, create a Vagrantfile with the configuration instructions for the VMs, then run the vagrant up command to automatically provision the sandbox environment.**

**Scenario #2: You are part of a Software Developer Team that is working on a complex web application that requires a web server, database, and caching server with specific versions and configurations.**







### Objective #1: Prepare VM for installation
Check the version of Ubuntu currently in use: lsb_release -a

![](image.png)

Check which packages on the system have available upgrades and install upgraded versions of those packages without removing any: sudo apt upgrade -y

![](image-1.png)

### Objective #2: Install Ansible

In the UBU-DB-JJ terminal run: curl  -o  to download a Python script that installs pip, the Python package manager

![](image-2.png)

In you the terminal run: python 3  –user to run the .py script and install pip for the current user

![](image-3.png)

Now we can install ansible by running: sudo apt install ansible

![](image-4.png)

Install Ansible version 9.0.1 using pip3 run: pip3 install ansible==9.0.1

![](image-5.png)

Edit the /etc/ansible/hosts file to tell Ansible which machines to manage and how to connect to them: vim /etc/ansible/hosts

![](image-6.png)

An ad hoc ansible command is a command that is run from the terminal to perform a quick task on one or more remote machines, without writing a playbook. Run ad hoc ansible command: ansible all -m ping -u student

![](image-7.png)

Here is another ad hoc ansible command we can run: ansible -m ping appServers

![](image-8.png)

### This output: "localhost | SUCCESS => {" indicates that Ansible successfully connected to the target host, executed the specified module, and completed the task without error.



### Objective #3: Install Vagrant on Windows
### Use PowerShell to add the Ubuntu 20.04 (Focal) image to your box list by running the following commands:
vagrant box add ubuntu/focal64 #Adds a new box to local Vagrant environment
vagrant plugin install vagrant-vbguest #Installs the plugin for Vagrant
mkdir ansible-vagrant-test #Create project folder
cd ansible-vagrant-test #Navigate to the test environment
vagrant init #Creates your vagrant file
vim .\Vagrantfile #replace configuration file with the following contents
vagrant up #Bring up your virtual machine and make the VM ready for use
vagrant ssh ansible_host #Open a secure shell connection to running Vagrant VM

![](image-9.png)

![](image-10.png)

![](image-11.png)

![](image-23.png)

![](image-13.png)

![Copy and Paste: Vagrant.configure(“2”) do |config|
    config.vm.define “ansible_hosts” do |ansible_hosts|
          ansible_hosts.vm.box = “ubuntu/focal64”
          ansible_hosts.vm.hostname = “ansible-host”
          ansible_host.vm.network “private_network”, ip: “192.168.56.10”
          ansible_hosts.vm.provision “shell”, inline: <<-SHELL
sudo apt update
sudo apt install -y ansible
SHELL
	end
end
](image-15.png)

![](image-20.png)

![When you look in VirtualBox Manager, you should see the Vagrant VM running  ](image-21.png)

![After establishing connection the terminal prompt will change to reflect that you’re inside the VM.](image-22.png)


### Objective #4: Configure Ansible with Vagrant & Create and run a network Ansible playbook

Test Ansible with Vagrant: ansible –version

![](image-24.png)

Create Inventory file: vim inventory.ini and write the following contents to the file

![Copy and Paste: localhost ansible_connection=local ](image-25.png)


Create Playbook file: vim network_playbook.yml and write the following contents to the file

![-	name: Check network connectivity
hosts: local
tasks: 
	- name: Ping localhost
	   ansible.builtin.ping:
](image-26.png)

Run the playbook: ansible-playbook -i inventory.ini network_playbook.yml

![](image-27.png)

Write a simple Hello World message. Create a file called hello.yml: vim hello.yml and write the following contents to the file

![---
-	name: Hello World Playbook
hosts: local
tasks: 
	- name: Print Hello World
	   ansible.builtin.debug:
	        msg: “Hello, World!”
](image-28.png)

Run hello.yml: ansible-playbook -i inventory.ini hello.yml

![](image-29.png)

Automate a repeatable Task – Update Linux
Create update.yml vim update.yml and write the following contents to the file

![](image-30.png)

Run update.yml to automate updating all packages on your Linux system: ansible-playbook -i inventory.ini update.yml

![](image-31.png)


### Conclusion:
After completing this project, I have a clearer understanding of the purpose and practical applications of both Vagrant and Ansible, as well as the synergistic relationship between them. I developed a foundational knowledge of how to download, install, and use these command-line tools effectively. All project requirements were met successfully.
Moving forward, I can build on this foundation to perform security-related tasks such as automating user creation, generating SSH keys, or implementing firewall rules. I could also scale the environment by adding multiple virtual machines with Vagrant and managing them all with Ansible. Overall, Vagrant and Ansible can be combined to create a powerful and efficient workflow for developing, testing and managing infrastructure.


















### References:
Cherry Servers. (n.d.). How to install Ansible on Ubuntu 22.04. https://www.cherryservers.com/blog/how-to-install-ansible-on-ubuntu-22-04
HashiCorp. (n.d.). Documentation | Vagrant. https://developer.hashicorp.com/vagrant/docs
Kavilkar, A. (n.d.). A guide to Ansible with Vagrant. Medium. https://medium.com/@alokkavilkar/a-guide-to-ansible-with-vagrant-6a4e8a4a4e0f
Microsoft Bing. (n.d.). How exactly Ansible works: A step-by-step guide for beginners [Video]. Bing Videos. https://www.bing.com/videos
TechGuide. (2024). How to download and install Vagrant 2024 (Windows, Mac, Linux) [Video]. YouTube.
Tutorials Point. (n.d.). Ansible tutorial for beginners: Playbook & examples. https://www.tutorialspoint.com/ansible/index.htm
VimHelp. (n.d.). How to install VIM in Windows. https://www.vim.org/download.php
freeCodeCamp. (n.d.). Vim Windows install guide – How to run the Vim text editor in PowerShell on your PC. https://www.freecodecamp.org/news/vim-windows-install-guide/
Windmill Engineering. (n.d.). What is Ansible? A brief history. https://www.windmill.dev/blog/what-is-ansible-a-brief-history
YouTube – Simplilearn. (n.d.). #1 What is Vagrant? Introduction to Vagrant | Vagrant tutorial for beginners | DevOps training [Video].
Ansible.com. (n.d.). Ansible collaborative - How Ansible works. https://www.ansible.com/resources/get-started






Appendix I: Troubleshooting Guide
Troubleshooting: PowerShell did not recognize the vim command
### Solution install vim so that it can be used in PowerShell with the following steps:
### Download vim for Windows:
Select x86.exe, if downloading to Windows
Open program file and run, allow changes
From the drop-down menu select full

![](image-32.png)

Keep selecting Next, don’t make any other changes to the default setting
Install
Open PowerShell
Navigate the directory where Vim is located (C:\Program Files (x86)\Vim\vim91)
Modify properties to allow use of Vim in all directories
Open Control Panel
Open Advanced System Settings
Select Path under System Variables
Select New
Add path to vim (C:\Program Files (x86)\Vim\vim91), select OK
Close and reopen PowerShell
Test vim in the current directory, you should be able to create a file

### Troubleshooting: To install Ansible in Ubuntu, I had to run the following commands first:
Run: sudo apt-add-repository ppa:ansible/ansible to add the official Ansible Personal Package Archive (PPA) to your system’s APT sources list, so I can install Ansible using apt

![](image-33.png)

Now that Ansible PPA has been added run: sudo apt update so the system can refresh its list of available packages from all configured sources

![](image-34.png)
