# OpenEDX Installation Document

## Installation Prerequisites

1. **Knowledge Prerequisites**  
   - Basic terminal usage.
   - Other technologies and skills includes Python, Linux, Ansible etc.
2. **Softeware Prerequisites**  
   - make tool
   - Docker 17.06CE or later
3. **Resources Prerequisites**  
   - Configure Docker with a minimum of 2 CPUs and 6GB memory.

## Docker Installation

1. **Uninstall old versions**  
  Older versions of Docker were called `docker`, `docker.io`, or `docker-engine`. Use following command to uninstall them:  
  `$ sudo apt-get remove docker docker-engine docker.io containerd runc`  
2. **Install Docker CE**  
   1. Update the `apt` package  
      ```
      $ sudo apt-get update
      ```  
   2. Install packages to allow `apt` use repository over https  
      ```
      $ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
      ```  
   3. Add Docker's official GPG key  
      ```
      $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      ```  
   4. Set up the `stable` repository  
      ```
      $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
      ```  
   5. Update the `apt` package again  
      ```
      $ sudo apt-get update
      ```  
   6. Install the latest version of Docker CE and containerd  
      ```
      $ sudo apt-get install docker-ce docker-ce-cli containerd.io
      ```  
   7. Verify that Docker CE is installed correctly by running the `hello-world` image  
      ```
      $ sudo docker run hello-world
      ```  
      Docker has been installed correctly if you can see the following output:  
      ```
      Hello from Docker!
      This message shows that your installation appears to be working correctly.

      ······

      For more examples and ideas, visit:
      https://docs.docker.com/get-started/
      ```
## Install Devstack  
To install Devstack, follow these steps.  
1. Get a local copy of `edx/devstack` repository  
   ```
   cd ~
   git clone https://github.com/edx/devstack
   cd devstack
   ```
2. 