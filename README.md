# nginx-vue

前后端部署在同一台主机。

vue-cli搭建的开发环境，localhost:8080,导致cookie获取不到，所以，用nginx反向代理将localhost:8080指到本地项目域名下

### nginx.conf
server

{

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
     
 }

### vue.config.js
devServer: { 

  disableHostCheck: true, 
  
},

新版的webpack-dev-server处于安全考虑，默认检查hostname，如果hostname不是配置内的，将中断访问。所以，这里直接用w.demo.com/test/2019/访问会报错：Invalid Host header。所以要在devServer设置disableHostCheck为true

此时，publicPath是／， 直接用nginx将static，js，css等反向代理

location ^~ /static/ {

    proxy_pass http://localhost:8080;
    
}

location ~ .*\.(js|css)$ {

    proxy_pass http://localhost:8080;
    
}





