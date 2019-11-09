### step

```bash
yum -y install epel-release
yum install python-pip -y
pip install --upgrade pip
pip install shadowsocks

yum -y install vim wget curl
yum -y install python-setuptools && easy_install pip && pip install --upgrade pip

pip install shadowsocks
```

```bash
vim /etc/shadowsocks.json
```

```json
{ "server": "0.0.0.0", "server_port": 18388, "password": "save11", "method": "aes-256-cfb" }
```

#### 启动服务

```bash
## start 
ssserver -c /etc/shadowsocks.json -d start

## stop
ssserver -c /etc/shadowsocks.json -d stop
```

[details @ here](https://crifan.github.io/scientific_network_summary/website/server_client_mode/ss_server/self_build_ss_server.html)

### bbr

[site](https://www.vultr.com/docs/how-to-deploy-google-bbr-on-centos-7)