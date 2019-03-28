# nginx-vue
nginx proxy_pass vue

用nginx反向代 vue本地开发环境

前后端部署在同一台主机。

开发环境localhost:8080,导致cookie获取不到，所以，用nginx反向代理将localhost指到本地项目域名下

### vue.config.js
devServer: { 
  disableHostCheck: true, 
},
