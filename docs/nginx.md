## docker

```bash
docker pull nginx
	
#docker run --name runoob-nginx-test -p 8081:80 -d nginx

mkdir -p /data/nginx/www
mkdir -p /data/nginx/logs
docker run -d -p 10000:80 --restart=always --name nginx-test -v /data/nginx/www:/usr/share/nginx/html -v /data/nginx/logs:/var/log/nginx nginx
#-v ~/nginx/conf/nginx.conf:/etc/nginx/nginx.conf 

```

### ssh反向代理配置
```nginx

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}

stream {
	upstream ssh191{
		server 192.168.112.191:22;
	}
	upstream ssh192{
		server 192.168.112.192:22;
	}
	upstream ssh193{
		server 192.168.112.193:22;
	}
	
	server {
		listen 52191 so_keepalive=on;
		proxy_connect_timeout 10h;
		proxy_timeout 10h;
		proxy_pass ssh191;
	}
	server {
		listen 52192 so_keepalive=on;
		proxy_connect_timeout 10h;
		proxy_timeout 10h;
		proxy_pass ssh192;
	}
	server {
		listen 52193 so_keepalive=on;
		proxy_connect_timeout 10h;
		proxy_timeout 10h;
		proxy_pass ssh193;
	}

}


```