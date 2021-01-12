PostgreSQL 10.0 源码安装



#### root用户下安装依赖包

  

```
yum -y install gcc gcc-c++ automake autoconf libtool make readline-devel zlib-devel readline
```

  

#### 安装数据库

  

```
./configure --prefix=/pg_data/postgres --without-readline

make

make install
```

  

#### 添加用户，设置目录权限

  

```
adduser postgres

passwd postgres

chown -R postgres:root /usr/local/pgsql
```

  

#### 添加启动服务（确认文件postgresql内的目录正确）

  

```
cp /root/postgresql-10.0.gz/contrib/start-scripts/linux  /etc/init.d/postgresql 

chmod u+x /etc/init.d/postgresql  #根据需求改该配置文件，否则不操作
```

#### 添加开启自启动

```
chkconfig --add postgresql   #上面的操作后才能进行
```

#### 切换用户，初始化数据并创建测试库（确认data目录下为空）

```
su - postgres

/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data
```

  

#### 设置环境变量

  

```
> su - postgres

> vim ~/.bash_profile   #或者/etc/profile内加

export PATH=$PATH:/usr/local/pgsql/bin

export PGDATA=/usr/local/pgsql/data

> source ~/.bash_profile
```

  

```
> vim ~/.bashrc

export PGDATA=/usr/local/pgsql/data

> source ~/.bash_profile
```

  

#### 启动服务

  

```
service postgresql start  
```

  

#### 启动数据库

  

```
/usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data -l logfile start
```

  

#### 创建和连接数据库

  

```
createdb test

psql test
```

  

#### 允许所有连接

  

```
> vim /usr/local/pgsql/data/pg_hba.conf

host    all             all             0.0.0.0/0               trust
```

  

#### 侦听所有连接

  

```
> vim /pg_data/postgresql/data/postgresql.conf 

listen_addresses = '*'

logging_collector = on
```

  

#### 关闭防火墙

  

```
启动： systemctl start firewalld


关闭： systemctl stop firewalld



查看状态： systemctl status firewalld 



开机禁用  ： systemctl disable firewalld



开机启用  ： systemctl enable firewalld
```