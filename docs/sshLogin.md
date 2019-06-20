## linux

```bash
#client run here
ssh-keygen
#enter enter enter
cd  && cd .ssh && ls

# 注册到服务器端 
ssh-copy-id -i ~/.ssh/id_rsa.pub root@xxx.xxx.xx.xx

# cat @ server
cat ~/.ssh/authorized_keys
```



## config ssh

```bash
cat ~/.ssh/config
```

```yaml
Host 231
	HostName 192.168.12.231
	User root
	Port 22
```

