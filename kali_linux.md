## Kali Linux pc<br>
记录在使用kali linux过程中遇到的问题集合。<br>

* **使用putty/xshell终端连接不上kali.**<br>
1.0 在kali linux中安装openssh,默认kali中是安装好ssh服务的。<br>
```shell
# sudo apt-get install openssh-client
# sudo apt-get install openssh-server
```
2.0 在安装完服务后，就需要打开ssh服务:<br>
```shell
# service ssh start

## 查看ssh服务状态
# service ssh status

## 关闭ssh服务
# service ssh stop
```
3.0 修改ssh服务配置：修改/etc/ssh/sshd_config文件，对ssh连接做配置。`vim /etc/ssh/sshd_config`。参考:[Kali 2.0使用SSH进行远程登录](http://jingyan.baidu.com/article/eae07827a3e6bc1fec5485e3.html)<br>
在配置文件中做如下修改:<br>
(1) 将PasswordAuthentication yes配置通过#注释掉。即`#PasswordAuthentication yes`。<br>
(2) 将PermitRootLogin without-password修改为PermitRootLogin yes，即:
```shell
PermitRootLogin yes
#PermitRootLogin prohibit-password
```
保存退出。<br>

4.0 在打开ssh服务后，通过putty/xshell连接，若是连接不上，通过`service ssh status`命令查看状态，可以看到如下错误提示:<br>
<font style="color:red">kali-x sshd[1853]: Could not load host key: /etc/ssh/ssh_host_rsa_key...</font><br>
那么，此时如果从客户端连接到服务器时是不会成功的。其原因是在 SSH 连接协议中需要有 RSA 或 DSA 密钥的鉴权。 因此，我们可以在服务器端使用 ssh-keygen 程序来生成一对公钥/私钥对。参考:[启动ssh服务时，提示Could not load host key: /etc/ssh/ssh_host_rsa_key](http://blog.csdn.net/hyholine/article/details/7362073)<br>
```shell
root@kali-x:/etc/ssh# ssh-keygen -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):            #直接回车即可
Enter same passphrase again: 
Your identification has been saved in /etc/ssh/ssh_host_rsa_key.
Your public key has been saved in /etc/ssh/ssh_host_rsa_key.pub.
The key fingerprint is:
3b:a4:b8:df:a9:15:d1:62:df:d5:d1:41:50:59:4a:96 root@bt
The key's randomart image is:
+--[ RSA 2048]----+
|             .***|
|         .   oE+o|
|        + .   o .|
|       . + . .   |
|        S . .    |
|     . o o       |
|    . . +        |
|     . o o       |
|    ..o.o        |
+-----------------+
```
至于密钥文件的名称，其实可以在/etc/ssh/sshd_config文件中可以看到,可以自定义与配置文件中同名即可：<br>
```shell
# HostKeys for protocol version 2
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
#HostKey /etc/ssh/ssh_host_ecdsa_key
#HostKey /etc/ssh/ssh_host_ed25519_key
```
最后，重启ssh服务，查看状态，若是没出现错误，则ssh可以连接上了。
