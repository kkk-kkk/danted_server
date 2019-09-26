## danted安装部署
### 环境要求
经过测试和修改安装脚本，当前danted支持如下系统版本：

| 系统类型            | 版本       |
| ------------------- | ---------- |
| Centos 6 series     | 6.0        |
| ---                 | 6.2        |
| ---                 | 6.3        |
| ---                 | 6.4 (推荐) |
| ---                 | 6.5        |
| ---                 | 6.6        |
| ---                 | 6.7        |
| ---                 | 6.8        |
| Centos 7 series     | 7.1        |
| ---                 | 7.2        |
| ubuntu series       | 12.04      |
| ---                 | 14.04.2    |
| ---                 | 14.04.3    |
| ---                 | 14.04.5    |
| ---                 | 15.10      |
| ---                 | 16.04      |
| Oracle linux series | 6.5        |
| ---                 | 7.1        |
| ---                 | 7.2        |

### 安装选项
推荐使用rpm包进行安装:
rpm -ivh dante-1.4.1-5.x86_64.rpm

| 选项          | 描述                                        |
| ------------- | ------------------------------------------- |
| --port=socks5 | 端口号码                                    |
| --ip=;;       | 配置的IP地址，默认全部开启，使用;分格       |
| --user=       | pam认证用户名                               |
| --passwd=     | pam认证用户密码                             |
| --master=     | 免认证地址，例如 github.com 或者 8.8.8.8/32 |

使用实例：
>bash danted_install.sh --port="1000" --ip="192.168.199.206" --user="zk" --passwd="123456"

### 功能特点
- 1.采用dante稳定版本 1.3.2 编译安装。
- 2.自动识别系统IP（默认排除192.168.0.*， 10.0.0.*,127.0.0.*）,根据安装命令选择部分Ip或者全部IP安装(多IP环境)。
- 3.采用PAM 用户认证，认证不需要添加系统用户（默认添加进程用户sock），删除、添加用户方便，安全。
- 4.sock5 运行状态查看,系统启动后自动加载。
- 5.完美支持多访问进出口（多IP的环境，支持 使用IP-1，访问网站IP查询为IP-1）。
- 6.认证方式可选： 无用户名密码，系统用户名密码，Pam用户名密码
- 7.完美支持Centos/Ubuntu/Debian,自动识别系统进行安装配置。[注意，经反馈，Centos 5 无法使用。]
- 8.自定义对连接客户端认证方式，支持白名单即支持某些IP/IP段无需认证即可连接。

### 安装步骤
- 1.下载 danted_install.sh脚本
- 2.[可选] 修改 默认参数，DEFAULT_PORT 为默认端口，DEFAULT_USER PAM用户名，DEFAULT_PAWD PAM用户对应密码 MASTER_IP 为免认证白名单（域名，IP可选：如默认的buyvm.info 或者具体Ip 8.8.8.8/32 ）
- 3.修改后，执行 bash danted_install.sh （指使用默认参数）
- 4.若运行结束后显示 Dante Server Install Successfuly! 则表明成功。
  显示 Dante Server Install Failed! 则表明安装失败。

注意：也可动态知道配置参数， 执行命令如下:
>bash danted_install.sh --port="1000" --ip="192.168.199.206" --user="zk" --passwd="123456"

### 安装后使用说明
- 1.命令参数 /etc/init.d/danted {start|stop|restart|status|add|del}
- 2.重启sock5 /etc/init.d/danted restart 或者 service danted restart
- 3.关闭sock5 /etc/init.d/danted stop 或者 service danted stop
- 4.开启sock5 /etc/init.d/danted start 或者 service danted start
- 5.查看sock5状态 /etc/init.d/danted status 或者 service danted status
- 6.添加SOCK5 PAM用户/修改密码 /etc/init.d/danted add 用户名 密码
- 7.删除SOCK5 PAM用户 /etc/init.d/danted del 用户名
- 8.配置文件路径/etc/danted/sockd.conf
- 9.日志记录路径 /var/log/danted.log
- 10.danted 帮助命令 danted --help

### 使用注意事项
- 1.绝大部分浏览器（除了Opera）都不支持带密码认证的Socks5，所以使用电脑需要安装proxifier/proxycap 等软件做验证处理。
- 2.如果是固定IP/Ip 段 可以修改配置文件，设置白名单访问。

        a.进入 /etc/danted/ 找到配置文件
        
        b.修改 第一个pass {} 模块下的 from: Master_IP/32 to: 0.0.0.0/0 . 把 Master_IP/32 修改为需要使用代理的Ip段/IP地址 如 114.114.114.0/24 或者 5.5.5.5/32 . 多个访问源，请复制多个 client pass {} 模块
        
        c.重启Danted 进程 service danted restart
- 3.如需删除danted，请参考以下命令删除程序文件

        service danted stop
        
        rm -rf /etc/danted/
        
        rm -f /etc/init.d/danted

### 遗留问题
- 分析log对连接sock5的用户进行统计。







## danted Installation and Deployment

### Environmental requirements
After testing and modifying the installation script, the current danted supports the following system versions: 

OS Type | Version 
---|---
Centos 6 series | 6.0
---| 6.2
---| 6.3
---| 6.4 (recommended) 
---| 6.5
---| 6.6
---| 6.7
---| 6.8
Centos 7 series | 7.1
---| 7.2
Ubuntu series	| 12.04
---| 14.04.2
---| 14.04.3
---| 14.04.5
---| 15.10
---| 16.04
Oracle linux series	| 6.5
---| 7.1
---| 7.2

### Installation options
It is recommended to use the rpm package to install: 
rpm -ivh dante-1.4.1-5.x86_64.rpm

Option | Description 
---|---
--port=socks5 | Port 
--ip=;;	| Configured IP address, all enabled by default, use ; to separate 
--user=	| pam authentication username 
--passwd= | pam authentication user password 
--master= | Free authentication address, such as github.com or 8.8.8.8/32 

Example:
>bash danted_install.sh --port="1000" --ip="192.168.199.206" --user="zk" --passwd="123456"

### Features
- 1. Compile and install using dante stable version 1.3.2.
- 2. Automatically identify the system IP (excluding 192.168.0.*, 10.0.0.*, 127.0.0.* by default), select some Ip or all IP installation (multi-IP environment) according to the installation command.
- 3. PAM user authentication, authentication does not need to add system users (by default adding process user sock), delete, add users convenient and safe.
- 4. sock5 running status view, automatically loaded after system startup.
- 5. Perfect support for multiple access to import and export (multi-IP environment, support using IP-1, access website IP query for IP-1).
- 6. Authentication method is optional: No username and password, system username and password, Pam username and password
- 7. Perfect support for Centos/Ubuntu/Debian, automatic identification system for installation and configuration. [Note that Centos 5 is not available after feedback. ]
- 8. Customize the connection authentication method for the client. Supporting whitelisting means that some IP/IP segments can be connected without authentication.

### Installation steps
- 1. Download the danted_install.sh script
- 2. [Optional] Modify the default parameters, DEFAULT_PORT is the default port, DEFAULT_USER PAM user name, DEFAULT_PAWD PAM user password MASTER_IP is the authentication-free whitelist (domain name, IP optional: such as the default buyvm.info or specific Ip 8.8. 8.8/32 )
- 3. After modification, execute bash danted_install.sh (refer to use the default parameters)
- 4. If Dante Server Install Successfuly! is displayed after the end of the run, it indicates success.
      Showing Dante Server Install Failed! indicates that the installation failed.

Note: You can also dynamically know the configuration parameters. The execution commands are as follows:
>bash danted_install.sh --port="1000" --ip="192.168.199.206" --user="zk" --passwd="123456"

### Instructions for use after installation
- 1. Command parameters /etc/init.d/danted {start|stop|restart|status|add|del}
- 2. Restart sock5 /etc/init.d/danted restart or service danted restart
- 3. Close sock5 /etc/init.d/danted stop or service danted stop
- 4. Start sock5 /etc/init.d/danted start or service danted start
- 5. View sock5 status /etc/init.d/danted status or service danted status
- 6. Add SOCK5 PAM User / Change Password /etc/init.d/danted add Username Password
- 7. Delete SOCK5 PAM user /etc/init.d/danted del Username
- 8. Configuration file path /etc/danted/sockd.conf
- 9. Logging path /var/log/danted.log
- 10. danted help command danted --help

### Precautions for use
- 1. Most browsers (except Opera) do not support Socks5 with password authentication, so you need to install software such as proxifier/proxycap for authentication.
- 2. If it is a fixed IP/Ip segment You can modify the configuration file to set up whitelist access.

```
a. Go to /etc/danted/ to find the configuration file

b. Modify the from: Master_IP/32 to: 0.0.0.0/0 under the first pass {} module. Change Master_IP/32 to the Ip segment/IP address that needs to use the proxy such as 114.114.114.0/24 or 5.5.5.5 /32. Multiple access sources, please copy multiple client pass {} modules

c. Restart the Danted process service danted restart
```
- 3. To delete danted, please refer to the following command to delete the program file.

        service danted stop
        
        rm -rf /etc/danted/
        
        rm -f /etc/init.d/danted

### Remaining problem
- The analysis log counts the users connected to the sock5.
