# open edX localization 概况 

## 服务运行概况  
   本文档在README.md安装配置的基础上进行操作  
   使用docker将devstack服务成功配置并启动之后，应有如下容器运行:  
   <pre> docker  
    |-- edxops/edxapp  学生用户站点  
    |-- edxops/edxapp  教师用户站点  
    |-- edxops/notes  
    |-- edxops/forum  
    |-- edxops/discovery  
    |-- edxops/ecommerce  
    |-- edxops/credentials  
    |-- edxops/chrome  
    |-- edxops/firefox  
    |-- edxops/elasticsearch  
    |-- mongo  
    |-- node  
    |-- mysql  
    |-- edxops/devpi
    |-- mecached  
   </pre>  
   有关运行服务的具体信息可在服务器上通过以下指令查看:  
   `$ sudo docker ps`  
   其中，最上面两个服务为open edX面向用户的主要服务，其他服务为依赖服务  

## 切换中文版配置方法     
   如果需要将站点切换为中文站点，可以依照下列步骤:  
### 切换学生用户站点为中文  
   1. 需要进入到学生用户站点的docker容器中进行操作，使用如下指令在站点容器中使用bash  
   `$ docker exec -it fe4520a0afd2 /bin/bash`   
   其中的`fe4520a0afd2`对应学生用户站点的容器id  
   2. 进入指定目录  
   `$ cd /edx/app/edxapp`  
   3. 使用`ls`指令可以查看当前目录下的文件，可以看到存在`cms.env.json`, `lms.evn.json`两个配置文件  
   可以使用vi等编辑器将两个配置文件中的原本的`"LANGUAGE_CODE": "en",`改为`"LANGUAGE_CODE": "zh-cn",`  
   4. 退出当前容器  
   `$ exit`  
   重启docker中的服务，首先使用如下指令关闭服务  
   `$ cd /root/openEDX/devstack && make down`  
   然后重启启动即可  
   `$ make dev.up`  
### 切换教师用户站点为中文  
   教师用户站点的切换方式与上述过程基本一直，只不过需要在不同的容器中进行操作  
   1. 进入教师用户站点服务容器中使用`bash`    
   `$ docker exec -it ef950a730f37 /bin/bash` 
   2. 接下来的配置方式与上述过程相同  

## 配置superuser  
   edxapp为我们提供了如下几个用户的账号密码:  

   | email | password | is_superuser | is_staff |
   | ----- | -------- | ------------ | -------- |
   | staff@example.com | edx | false | true |
   | verified@example.com | edx | false | false |
   | audit@example.com | edx | false | false |
   | honor@example.com | edx | false | false |  

   可以看到，账号密码对我们可见的几个预装用户都没有superuser权限，无superuser权限也就意味着我们没有在线操作站点的最高权限  
   为此我们需要为原本的员工账户staff添加superuser权限  
   操作如下:  
   1. 启动mysql服务对应容器的bash  
   `$ docker exec -it d5d55b9c0aa3 /bin/bash`  
   2. 登陆mysql  
   `$ mysql -u root -p`  
   其中mysql的root用户密码为空，即无需输入直接回车即可  
   3. 选择数据库  
   `mysql> use edxapp;`  
   4. 更新auth_user数据表中staff用户的is_superuser字段  
   `mysql> UPDATE auth_user SET is_superuser=1 WHERE username=staff`  
   5. 退出重新登陆staff账户，这时候可以看到staff账户已经开启了superuser权限，可以在admin页面对站点进行编辑  
       
## 站点当前localization概况  
   以下url中的ip地址使用host指代, host=47.************(服务器ip)  
   实现等级分为: 实现、基本实现、部分实现、基本未实现、未实现(实现程度递减)  
   接下来分功能展示各站点的localization概况，其中教师用户站点的localization效果最好，学生用户站点的效果其次，管理站点的效果最差  

### 学生用户站点  
   目前学生用户站点运行在18000端口上  

   | 页面描述  |  title | url | 中文化概况 | 未中文化部分 |
   | -------- | ------ | --- | -------- | ---------- |
   | 课程主页面 | Your Platform Name Here | host:18000/ or host:18000/cources | 基本实现 | title等个别部分 |
   | 课程介绍页面 |  edX Demonstration Course | host:18000/courses/{课程信息参数}/about | 基本实现 | title等个别部分、英文课程信息仍显示英文 |
   | 我的课程页面 | 课程面板 | host:18000/dashboard | 基本实现 | title后缀 |
   | 用户资料页面 | 学员资料 | host:18000/u/username | 部分实现 | title后缀、账户设置页面跳转部分 |
   | 账户设置页面 | 账户设置 | host:18000/account/settings | 部分实现 | title后缀、部分设置字段、删除账户信息 |
   | 登出页面 | 已注销 | host:18000/logout | 基本实现 | title后缀 |
   | 登录页面 | 登录或注册 | host:18000/login | 部分实现 | title后缀、页面内标题、控件placeholder |
   | 注册页面 | 登录或注册 | host:18000/register | 部分实现 | title后缀、页面内标题、注册条款 |
   | 课程页面 | (根据课程信息而定) | host:18000/courses/course-{课程信息参数} | 基本实现 | 部分小标题 |  


### 教师用户站点  
   目前教师用户站点运行在18010端口上  

   | 页面描述 | title | url | 中文化概况 | 未中文化部分 |
   | ------- | ----- | --- | -------- | ---------- |
   | 欢迎页面 | 欢迎 | host:18010/ | 实现 | 暂无 |
   | 注册页面 | 注册 | host:18010/signup | 实现 | 暂无 |
   | 登录页面 | 登录 | host:18000/login(该页面调用18000端口服务登录页面，登录成功后重定向，目前源码配置中重定向存在问题) | 部分实现 | 页面内标题、控件placeholder |
   | 个人主页 | Studio主页 | host:18010/home/ | 实现 | 暂无 |
   | 课程维护页面 | 维护面板 | host:18010/maintenance/ | 实现 | 暂无 |
   | 课程页面 | 课程大纲 | host:18010/course/{课程信息参数} | 实现 | 暂无 |
   | 课程更新 | 课程更新 | host:18010/course_info/course-{课程信息参数} | 实现 | 暂无 |
   | 页面 | 页面 | host:18010/tabs/course-{课程信息参数} | 实现 | 暂无 |
   | 文件上传页面 | 文件&上传 | host:18010/assets/course-{课程信息参数} | 实现 | 暂无 |
   | 日程页面 | 日程与细节设置 | host:18010/details/course-{课程信息参数} | 实现 | 暂无 |
   | 评分页面 | 评分设置 | host:18010/grading/course-{课程信息参数} | 部分实现 | 作业类型设计 |
   | 课程团队页面 | 课程团队设置 | host:18010/course_team/course-{课程信息参数} | 实现 | 暂无 |
   | 组配置页面 | 组配置 | host:18010/group_configurations/course-{课程信息参数} | 部分实现 | 添加组部分 |
   | 高级设置 | 高级设置 | host:18010/advanced/course-{课程信息参数} | 实现 | 暂无 |  
   | 证书页面 | 课程证书 | host:18010/certificates/course-{课程信息参数} | 部分实现 | 添加证书部分 |
   | 课程导入 | 课程导入 | host:18010/import/course-{课程信息参数} | 实现 | 暂无 |
   | 导出课程 | 导出课程 | host:18010/export/course-{课程信息参数} | 实现 | 暂无 |
   | 清单页面 | 清单 | host:18010/checklist/course-{课程信息参数} | 部分实现 | 清单部分 |  

### 管理员站点  
   管理员站点也运行在18000端口上  

   | 页面描述 | title | url | 中文化概况 | 未中文化部分 |
   | ------- | ----- | --- | -------- | ---------- |
   | 登出页面 | Logged out | host:18000/admin/logout/ | 基本未实现 | 除页面标题 |
   | 登录页面 | 登录 | host:18000/adming/login/ | 实现 | 暂无 |
   | 管理页面 | Site adminstration | host:18000/admin/ | 基本未实现 | 除页面标题 |
   | 具体项配置页面 | (由不同的配置项决定) | host:18000/admin/{配置项参数} | 基本未实现 | 除页面标题 |  
   
## 进一步localization的分析  

### 目前站点localization存在的问题  
   1. **localization不彻底**. 当前站点已经通过open edX配置项修改了网站语言版本，将其修改为中文版本。配置完成后能够看到一定的localization效果，但是仍然可以非常明显的看到当前存在的首要问题，仍有大部分内容未完成语言切换。  
   2. **部分配置项存在问题**. 当前站点的所有功能目前基本均可投入使用，但是在部分功能和页面配置上存在一些小问题，比如部分静态资源无法加载，重定向存在问题等。  
   3. **站点前端美观度不足**. 当前站点使用open edX默认主题，学生用户界面美观度较为不足。  

### 当前问题的可能解决方案  
   1. **locaization不彻底**. 这是网站当前最主要的问题，不同语言混杂，会在用户使用时带来较差的用户体验。但是解决该问题也是接下来工作中最困难的一部分，目前一种可行的方案是对项目源码进行分析，对于模版中写死的部分进行更改，同样对于代码中写死的部分也需要更改。  
   2. **部分配置项存在问题**. 对于当前网站的存在的诸如部分静态资源无法加载等问题，可能是在源码中的某些配置项存在问题，比如重定向时会将q请求直接打在localhost而造成问题，这些问题只需在项目中定位后修改即可。  
   3. **站点前端美观度不足**. 这个问题可以与解决第一个问题时同步进行，可以在页面中加入一些样式来调整页面外观，但是这样的效果有限。如果需要引入其他较为成熟的前端框架，则可能需要对项目进行较大规模的重构。另外open edX也提供了其他主题供建站者选择使用，也在一定程度上可以在美观度问题上有所改观，但同样可能效果有限。  