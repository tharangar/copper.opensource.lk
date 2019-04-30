# Prerequisites


## Host Requirment

Following requirments must be satisfied by the Email solution host, vertual machine or cloud environment.

RAM 2 GB

HDD 50 GB

Internet connectivity.


## Check Open Ports

Make sure any other application does not use ports that we are going to listen to

```
$ netstat -tulpn | grep -E -w '25|80|110|143|443|465|587|993|995|4190|11334'

//check each port

$ sudo lsof -i :25
```


Unblock following ports

| Service | Software | Protocol | Port |
| ------- | -------- | -------- | ---- |
| SMTP | Postfix | TCP | 25 |
| IMAP | Dovecot | TCP | 143 |
| SMTPS | Postfix | TCP | 465 |
| Submission | Postfix | TCP | 587 |
| IMAPS | Dovecot | TCP | 993 |


Then check external firewall also .


## Docker

It is must to have docker installed in your host machine. So first check whether docker is already available and docker version should be version 18.0 or greater.

``` 
docker -v
```

If docker has not already installed then it must be installed. 

### Docker Installation
[Docker installation](https://www.linux.com/learn/intro-to-linux/2017/11/how-install-and-use-docker-linux)

<p align="justify">
Since Ubuntu Server 16.04 is sans GUI, the installation and usage of Docker will be handled entirely through the command line. Before you run the installation command, make sure to update apt and then run any necessary upgrades. Do note, if your server’s kernel upgrades, you’ll need to reboot the system. Thus, you might want to plan to do this during a time when a server reboot is acceptable.
</p>

To update apt, issue the command:

```sudo apt update```

Once that completes, upgrade with the command:

```sudo apt upgrade```

If the kernel upgrades, you’ll want to reboot the server with the command:

```sudo reboot```

If the kernel doesn’t upgrade, you’re good to install Docker (without having to reboot). The Docker installation command is:

```sudo apt install docker.io```

<p align="justify">
If you’re using a different Linux distribution, and you attempt to install (using your distribution’s package manager of choice), only to find out docker.io isn’t available, the package you want to install is called docker. For instance, the installation on Fedora would be:
</p>

```sudo dnf install docker```
<p align="justify">
If your distribution of choice is CentOS 7, installing docker is best handled via an installation script. First update the platform with the command sudo yum check-update. Once that completes, issue the following command to download and run the necessary script:
</p>

```curl -fsSL https://get.docker.com/ | sh```
<p align="justify">
Out of the box, the docker command can only be run with admin privileges. Because of security issues, you won’t want to be working with Docker either from the root user or with the help of sudo. To get around this, you need to add your user to the docker group. This is done with the command:
</p>

```sudo usermod -a -G docker $USER```
<p align="justify">
Once you’ve taken care of that, log out and back in, and you should be good to go. That is, unless your platform is Fedora. When adding a user to the docker group to this distribution, you’ll find the group doesn’t exist. What do you do? You create it first. Here are the commands to take care of this:
</p>

```sudo groupadd docker && sudo gpasswd -a ${USER} docker && sudo systemctl restart docker```

```newgrp docker```

Log out and log back in. You should be ready to use Docker.

Starting, stopping, and enabling Docker

Once installed, you will want to enable the Docker daemon at boot. To do this, issue the following two commands:

```sudo systemctl start docker```

```sudo systemctl enable docker```

Should you need to stop or restart the Docker daemon, the commands are:

```sudo systemctl stop docker```

```sudo systemctl restart docker```

Docker is now ready to deploy containers.

## Kubernetes

To deploy copper email solution we should have kubernetes version greater than  1.10 .

### Check kubernetes version

``` kubectl version```

Now your system is ready for copper installation.

### kubernetes Installation

[Install kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/)


Install kubectl on Linux

Install kubectl binary with curl on Linux

Download the latest release with the command:

```curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl```

<p align="justify">
To download a specific version, replace the $(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt) portion of the command with the specific version.
For example, to download version v1.14.0 on Linux, type:
</p>

```curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.14.0/bin/linux/amd64/kubectl```

Make the kubectl binary executable.

```chmod +x ./kubectl```

Move the binary in to your PATH.

```sudo mv ./kubectl /usr/local/bin/kubectl```

