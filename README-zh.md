# open edX 安装手册  

## 安装前的准备  

1. **知识储备**  
   - 对终端的基本使用有一定的认识  
   - 其他的知识和技能包括但不限于Python, Linux, Ansible等  
2. **需预装的软件**  
   - make 工具  
   - Docker 17.06CE 及其后的版本  
   - docker-compose(Linux环境下需要自行安装)  
3. **计算资源需求**  
   - 为Docker分配的最低资源配置为 2 CPUs 和 6GB 内存  

## Docker 的安装

1. **卸载旧版本Docker**
   旧版本的Docker的名称为`docker`, `docker.io`, 或者`docker-engine`. 使用以下指令来卸载他们:  
   `$ sudo apt-get remove docker docker-engine docker.io containerd runc`  
2. **安装Docker**  
   1. 升级`apt`包  
   `$ sudo apt-get update`  
   2. 安装https依赖包  
   `$ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common`  
   