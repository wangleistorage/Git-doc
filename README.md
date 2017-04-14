## git 本地仓库的初始化及多节点批量同步

#### 需求：创建两个git本地仓库,分别位于192.168.143.202和192.168.143.210,当192.168.143.200修改配置的时候同时提交至两台git仓库中

#### 说明：
```
192.168.143.200  # 修改配置文件的主机
192.168.143.202  # git本地仓库-1
192.168.143.210  # git本地仓库-2
```

#### 1.创建仓库
```
# 192.168.143.210
cd /opt
git init --bare nginx_conf.git

# 192.168.143.202
cd /opt
git init --bare nginx_conf.git
```

#### 2.客户端clone
```
# 192.168.143.200
git clone 192.168.143.202:/opt/nginx_conf.git
cd nginx_conf
```

#### 3.设置仓库地址
```
cd nginx_conf
vim .git/config

[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = 192.168.143.202:/opt/nginx_conf.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[remote "origin"]
        url = 192.168.143.210:/opt/nginx_conf.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
```

#### 4.添加文件提交至仓库
```
[root@centos7-200 nginx_conf]# echo "test1" > test1.txt
[root@centos7-200 nginx_conf]# echo "test2" > test2.txt
[root@centos7-200 nginx_conf]# echo "test3" > test3.txt
[root@centos7-200 nginx_conf]# git add -A .
[root@centos7-200 nginx_conf]# git commit -m 'wl'
[root@centos7-200 nginx_conf]# git push origin master
```

#### 5.查看远程仓库
```
[root@centos7-200 nginx_conf]# git remote -v
origin	192.168.143.202:/opt/nginx_conf.git (fetch)
origin	192.168.143.202:/opt/nginx_conf.git (push)
origin	192.168.143.210:/opt/nginx_conf.git (push)
```

#### 6.测试-删除当前工作目录重新git clone
```
git clone 192.168.143.210:/opt/nginx_conf.git
git clone 192.168.143.202:/opt/nginx_conf.git
```

#### 7.通过在本地提交文件,并同步至三台nginx(集群or主备)
```
touch Makefile
下面命令可以自定义,可以通过Ansible,也可以通过shell,定义好之后执行make push即可

push:	update to nginx1/nginx2/nginx3
	ssh nginx1 "cd /usr/local/nginx/conf; git pull; /usr/local/nginx/sbin/nginx -t && /usr/local/nginx/sbin/ngnix -s reload"
	ssh nginx2 "cd /usr/local/nginx/conf; git pull; /usr/local/nginx/sbin/nginx -t && /usr/local/nginx/sbin/ngnix -s reload"
	ssh nginx3 "cd /usr/local/nginx/conf; git pull; /usr/local/nginx/sbin/nginx -t && /usr/local/nginx/sbin/ngnix -s reload"
```
