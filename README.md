# xiaodi-docker-lnmp

### Docker [必须安装](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe)

### Docker 加速
在桌面右下角状态栏中右键 docker 图标，修改在 Docker Daemon 标签页中的 json ，把下面的地址:
"registry-mirrors"
~~~
http://f1361db2.m.daocloud.io
~~~

### 创建项目
1. 在workspace目录 创建项目
2. 配置虚拟主机```conf/conf.d/project_name.conf```, listen 添加你自定义的端口 参考```tp5.conf.examplte```
3. 自定义配置启动端口映射 docker-compose.yml （可选）
注！ ```9503:9503``` 指 ```本地端口:容器端口```
~~~
nginx:
    ...
    ports:
      - 9503:9503
~~~

### 启动
~~~
$ docker-compose up
~~~

### 指定PHP版本
修改 ```conf/conf.d/你的项目.conf```
~~~
location ~ \.php$ {
  fastcgi_pass   php56:9000;

  # php7.3
  # fastcgi_pass php73:9000;

}
~~~

### 数据库连接
~~~
mysqli_connect('容器名称', 'root', 1234, 'database')
~~~
#### 例子
连接mysql5.6: ```mysqli_connect('mysql56', 'root', 1234, 'database')```
连接mysql5.7: ```mysqli_connect('mysql57', 'root', 1234, 'database')```

### 数据库工具连接
直接使用当前映射的端口访问内部

## MYSQL linux问题
注意启动时的提示，如果出现以下提示，你数据库的数据卷应该无法同步到本地
`Warning: World-writable config file '/etc/mysql/my.cnf' is ignored`
解决
在linux服务器，更改my.cnf的权限，再重启
`chmod 644 my.cnf`
