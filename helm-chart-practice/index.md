helm values中使用变量
values中使用上文定义的变量


## harbor安装helm 插件

```
docker-compose stop
./install.sh  --with-chartmuseum
```



## 本地使用私有harbor chart

```shell
### http://127.0.0.1:18080/harbor/projects/6/helm-charts/my-app/versions/0.1.0



## 添加helm cm-push插件
✗ helm plugin install https://github.com/chartmuseum/helm-push

## 添加repo
✗ helm repo add xyz http://127.0.0.1:18080/chartrepo/xyz --username admin
Password: {输入密码}
"xyz" has been added to your repositories

## 检查repo
✗ helm repo list
NAME       	URL                                      
stable     	http://mirror.azure.cn/kubernetes/charts/
codecentric	https://codecentric.github.io/helm-charts
csf        	http://127.0.0.1:18080/chartrepo/xyz

## 搜索repo
✗ helm search repo my-app
NAME      	CHART VERSION	APP VERSION	DESCRIPTION                
csf/dv-app	0.1.0        	1.16.0     	A Helm chart for Kubernetes

## 下载chart
✗ helm fetch xyz/my-app

## 打包chart
✗ helm package my-helm-chart

## 上传chart
✗ helm cm-push ./my-app-0.1.0.tgz xyz --insecure

```




> https://www.jianshu.com/p/62ef34f76168


## helm常用指令

```shell
kubectl create deployment demo --image=httpd --port=80 -n my-helm
kubectl expose deployment demo -n my-helm
kubectl create ingress tenant1 --class=nginx --rule xyz.example.com/=demo:80 -n my-helm



## 根据Chart.yaml更新chart目录下的依赖包
helm dependency update

## 打包
helm package my-helm-chart

## 安装
helm install my-app ./my-app-0.1.0.tgz -n my-app

## 卸载
helm uninstall my-app -n my-app

## 展示所有的helm app
helm list -n my-app


## debug
helm install my-app ./my-helm-chart --dry-run --debug > debug.yaml

```
