# nginx-vue

前后端部署在同一台主机。

vue-cli搭建的开发环境，localhost:8080,导致cookie获取不到，所以，用nginx反向代理将localhost:8080指到本地项目域名下

### nginx.conf

listen 80;

server_name   w.demo.com;

root          /Users/zhangligang/code/demo/;

index         index.html index.php;

location ^~ /test/2019/ {
     proxy_pass http://localhost:8080;
     proxy_redirect     off;
     proxy_set_header   Host   $http_host;
     proxy_set_header  X-Real-IP         $remote_addr;
     proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
}



location ^~ /static/ {
    proxy_pass http://localhost:8080;
}
location ~ .*\.(js|css)$ {
    proxy_pass http://localhost:8080;
}
location /api {
    proxy_pass http://localhost:8080/api;
    proxy_redirect     off;
    proxy_set_header   Host   $http_host;
    proxy_set_header  X-Real-IP         $remote_addr;
    proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
}

### vue.config.js
devServer: { 

  disableHostCheck: true, 
  
},

通过服务器域名访问时是显示Invalid Host header，这是由于新版的webpack-dev-server出于安全考虑，默认检查hostname，如果hostname不是配置内的，将中断访问
