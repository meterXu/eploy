## docker

```bash
docker pull nginx
	
#docker run --name runoob-nginx-test -p 8081:80 -d nginx

mkdir -p /data/nginx/www
mkdir -p /data/nginx/logs
docker run -d -p 10000:80 --restart=always --name nginx-test -v /data/nginx/www:/usr/share/nginx/html -v /data/nginx/logs:/var/log/nginx nginx
#-v ~/nginx/conf/nginx.conf:/etc/nginx/nginx.conf 

```

