# Amazon EKS Trace Demo

## 1 前提要求
1、已经提前创建Amazon EKS集群
<br>2、将本项目中的文件下载到本地
## 2 部署Jaeger Operater
执行如下命令，以部署Jaeger Operater
```
kubectl create -f jaeger-operator.yaml
```
## 3 部署Trace Demo
执行如下命令，以部署Trace Demo需要的所有资源，包括Front,Middle和Back-end
```
kubectl create -f x-ray-sample-front-k8s.yml
kubectl create -f x-ray-sample-middle-k8s.yml
kubectl create -f x-ray-sample-back-k8s.yml
```
## 4 访问Trace Demo前端页面
执行如下命令，获取Front对应Service创建的AWS ELB地址
```
kubectl get svc | grep front
x-ray-sample-front-k8s        LoadBalancer   10.100.57.86     ad362e88235b246e8bea848687c42069-904326315.ap-northeast-2.elb.amazonaws.com    80:31001/TCP 
```
复制front对应的ELB URL，复制到浏览器中打开
## 5 访问Jaeger页面
执行如下命令，获取Jaeger Simplest Service创建的AWS ELB地址
```
kubectl get svc | grep simplest-service
simplest-service              LoadBalancer   10.100.71.141    a82668ccd10154416a15b33fd26a7efb-1642856466.ap-northeast-2.elb.amazonaws.com   16686:30039/TCP
```
复制Simplest Service对应的ELB URL，使用simplest-service-elb-url:16686访问Jaeger UI
## 6 Trace Demo演示
在Trace Demo前端页面中进行多次点击，分别查看AWS X-Ray中的Serive Map页面和Jaeger页面
## 7 清理环境
执行如下命令清理环境
```
kubectl delete -f x-ray-sample-front-k8s.yml
kubectl delete -f x-ray-sample-middle-k8s.yml
kubectl delete -f x-ray-sample-back-k8s.yml
kubectl delete -f jaeger-operator.yaml
```
