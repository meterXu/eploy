### 搭建私服

```bash
mkdir -p /data/registry

docker pull registry:2

docker run --restart=always -d -p 5000:5000 --restart=always --name registry -v /data/registry:/var/lib/registry registry:2

```

### nginx

```bash
mkdir -p /data/pack && cd /data/pack
wget http://nginx.org/download/nginx-1.17.0.tar.gz
wget https://www.openssl.org/source/openssl-1.0.2s.tar.gz

yum install gc gcc gcc-c++ pcre-devel zlib-devel make wget openssl-devel libxml2-devel libxslt-devel gd-devel perl-ExtUtils-Embed GeoIP-devel gperftools gperftools-devel libatomic_ops-devel perl-ExtUtils-Embed dpkg-dev libpcrecpp0 libgd2-xpm-dev libgeoip-dev libperl-dev -y

tar xvzf openssl-1.0.2s.tar.gz
tar xvzf nginx-1.17.0.tar.gz
cd nginx-1.17.0
./configure --with-http_ssl_module --with-openssl=/data/pack/openssl-1.0.2s
make && make install

## ssl
touch /etc/pki/CA/{index.txt,serial}
echo 01 > /etc/pki/CA/serial
#生成密钥
(umask 077;openssl genrsa -out  /etc/pki/CA/private/cakey.pem 2048)
#生成根证书
openssl req -new -x509 -key  /etc/pki/CA/private/cakey.pem -out /etc/pki/CA/cacert.pem -days 3650

# 添加信任
cat /etc/pki/CA/cacert.pem >> /etc/pki/tls/certs/ca-bundle.crt

## 签发证书
mkdir /usr/local/nginx/conf/ssl
(umask 077;openssl genrsa -out /usr/local/nginx/conf/ssl/docker.key 2048)
openssl req -new -key /usr/local/nginx/conf/ssl/docker.key -out /usr/local/nginx/conf/ssl/docker.csr

##签署，证书
openssl ca -in /usr/local/nginx/conf/ssl/docker.csr -out /usr/local/nginx/conf/ssl/docker.crt -days 3650

```

### nginx配置

```bash
yum -y install httpd-tools
# 添加认证
htpasswd -cb /usr/local/nginx/conf/docker-registry.htpasswd admin admin

vim /usr/local/nginx/conf/nginx.conf
```

```nginx
	upstream docker-registry {
        server 127.0.0.1:5000;
    }
 
    server {
        listen       443	ssl;
        #server_name  localhost;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        ssl_certificate       /usr/local/nginx/conf/ssl/docker.crt;
        ssl_certificate_key   /usr/local/nginx/conf/ssl/docker.key;
        client_max_body_size 0;
        chunked_transfer_encoding on;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        add_header 'Docker-Distribution-Api-Version' 'registry/2.0' always;
 
        location / {
           auth_basic   "Docker registry";
               auth_basic_user_file /usr/local/nginx/conf/docker-registry.htpasswd;
               proxy_pass  http://docker-registry;
        }
        location /_ping{
               auth_basic off;
               proxy_pass  http://docker-registry;
               }
        location /v2/_ping{
               auth_basic off;
               proxy_pass  http://docker-registry;
        }
}
```





### 连接测试

```bash
#本地测试
# 创建测试用域名 a 为域名
cat >>/etc/hosts <<EOF
192.168.12.235 a
EOF

# 登录
docker login a

docker pull hello-world
docker tag hello-world 192.168.12.135:5000/hello-world
docker push 192.168.12.135:5000/hello-world

curl --user admin:admin  https://a/v2/_catalog


#外部机器连接
scp -p /etc/pki/tls/certs/ca-bundle.crt  root@192.168.10.158:/etc/pki/tls/certs/ca-bundle.crt
scp -p /etc/pki/CA/cacert.pem root@192.168.10.158:/etc/pki/CA/cacert.pem

#外部机器使用
cat /etc/pki/CA/cacert.pem >> /etc/pki/tls/certs/ca-bundle.crt

systemctl restart docker

#then try it , ok
```

