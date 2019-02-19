# OpenEDX Installation Document

## Installation Prerequisites

1. **Knowledge Prerequisites**  
   - Basic terminal usage.
   - Other technologies and skills includes Python, Linux, Ansible etc.
2. **Softeware Prerequisites**  
   - make tool
   - Docker 17.06CE or later
   - docker-compose
3. **Resources Prerequisites**  
   - Configure Docker with a minimum of 2 CPUs and 6GB memory.

## Docker Installation

1. **Uninstall old versions**  
  Older versions of Docker were called `docker`, `docker.io`, or `docker-engine`. Use following command to uninstall them:  
     `$ sudo apt-get remove docker docker-engine docker.io containerd runc`  
2. **Install Docker CE**  
   1. Update the `apt` package  
      `$ sudo apt-get update`  
   2. Install packages to allow `apt` use repository over https  
      `$ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common`  
   3. Add Docker's official GPG key  
      `$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`  
   4. Set up the `stable` repository  
      `$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`  
   5. Update the `apt` package again  
      `$ sudo apt-get update`  
   6. Install the latest version of Docker CE and containerd  
      `$ sudo apt-get install docker-ce docker-ce-cli containerd.io`  
   7. Verify that Docker CE is installed correctly by running the `hello-world` image  
      `$ sudo docker run hello-world`  
      Docker has been installed correctly if you can see the following output:  
      <pre>
      Hello from Docker!
      This message shows that your installation appears to be working correctly.

      ······

      For more examples and ideas, visit:
      https://docs.docker.com/get-started/
      </pre>  
## Install docker-compose  
To install `docker-compose`, follow these steps.  
1. **Download docker-compose to `/usr/local/bin/`**  
`sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`  
2. **Apply executable permissions to the binary file**  
`sudo chmod +x /usr/local/bin/docker-compose`  
3. **Verify that `docker-compose` is installed correctly**  
`docker-compose --version`  
`docker-compose` has been installed correctly if you can see the following output  
`docker-compose version 1.23.2, build 1110ad01`

## Install Devstack  
To install Devstack, follow these steps.  
1. Get a local copy of `edx/devstack` repository  
   ```
   cd ~
   mkdir openEDX 
   cd openEDX
   git clone https://github.com/edx/devstack
   cd devstack
   ```  
2. Use make to check out the correct branch  
   `make dev.checkout`  
3. Clone the Open edX service repositories  
   `make dev.clone`  
   Use the command will clone several repos into `~/openEDX` directory  
   > You may encounter the following problem when cloning repositories  
   > <pre>error: RPC failed; curl 18 transfer closed with outstanding read data remaining  
   > fatal: The remote end hung up unexpectedly  
   > fatal: early EOF  
   > fatal: index-pack failed</pre>  
   > It is the poor network connection with github.com on Aliyun that cause the error.  
   > To solve the error, we need use a proxy: [proxy settings](https://www.jianshu.com/p/b1f6e6944f94)  
4. Deploy the platform  
   `make dev.provision`  
   > Step 3 and Step 4 may take a long time to process, please be patient.  
   > If error happens during steps, please try again.
5. Start service  
   `make dev.up`  
   Then we could get platform service from http://host:18000/ and http://host:18010/ (host is server public ip)  
   