# Linux-Docker

https://docs.docker.com/engine/install/debian/#set-up-the-repository

Uninstall old versions
Older versions of Docker went by the names of docker, docker.io, or docker-engine. Uninstall any such older versions before attempting to install a new version:

 sudo apt-get remove docker docker-engine docker.io containerd runc


Set up the repository
Update the apt package index and install packages to allow apt to use a repository over HTTPS:

 sudo apt-get update
 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
Add Docker’s official GPG key:

 sudo mkdir -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
Use the following command to set up the repository:

 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
  
  Update the apt package index:

 sudo apt-get update
Receiving a GPG error when running apt-get update?

Your default umask may be incorrectly configured, preventing detection of the repository public key file. Try granting read permission for the Docker public key file before updating the package index:

 sudo chmod a+r /etc/apt/keyrings/docker.gpg
 sudo apt-get update
Install Docker Engine, containerd, and Docker Compose.

Latest
Specific version

To install the latest version, run:

 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
Verify that the Docker Engine installation is successful by running the hello-world image:

 sudo docker run hello-world
 
 Uninstall Docker Engine
Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:

 sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-ce-rootless-extras
Images, containers, volumes, or custom configuration files on your host aren’t automatically removed. To delete all images, containers, and volumes:

 sudo rm -rf /var/lib/docker
 sudo rm -rf /var/lib/containerd
 
 
 https://docs.docker.com/engine/install/linux-postinstall/
 
 To create the docker group and add your user:

Create the docker group.

 sudo groupadd docker
Add your user to the docker group.

 sudo usermod -aG docker $USER
Log out and log back in so that your group membership is re-evaluated.

If you’re running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.

You can also run the following command to activate the changes to groups:

 newgrp docker
Verify that you can run docker commands without sudo.

 docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

If you initially ran Docker CLI commands using sudo before adding your user to the docker group, you may see the following error:

WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
This error indicates that the permission settings for the ~/.docker/ directory are incorrect, due to having used the sudo command earlier.

To fix this problem, either remove the ~/.docker/ directory (it’s recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:

 sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
 sudo chmod g+rwx "$HOME/.docker" -R
 
 
