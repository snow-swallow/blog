## termial打开rancher配置容器
$ LIMA_HOME="$HOME/Library/Application Support/rancher-desktop/lima" "/Applications/Rancher Desktop.app/Contents/Resources/resources/darwin/lima/bin/limactl" shell 0

## 修改rancher docker配置
lima-rancher-desktop:/Users/kitty/Library/Application Support$ sudo vi /etc/conf.d/docker

## 添加以下配置至末尾
DOCKER_OPTS="--insecure-registry=x.x.x.x:1234"


## 重启docker service
lima-rancher-desktop:/Users/kitty/Library/Application Support$ sudo service docker restart
 * Caching service dependencies ...                                                                                                                                                                    [ ok ]
 * Stopping Docker Daemon ...                                                                                                                                                                          [ ok ]
 * Starting Docker Daemon ...                                                                                                                                                                          [ ok ]

## 退出配置界面
lima-rancher-desktop:/Users/kitty/Library/Application Support$ exit

