# use the travis to sync the docker images of the gcr.io and quay.io 

[![Build Status](https://travis-ci.org/sqeven/quay.io.svg?branch=master)](https://travis-ci.org/sqeven/quay.io)

GOOLE_NAMESPACE:
```
google_containers kubernetes-helm runconduit google-samples k8s-minikube tf-on-k8s-dogfood spinnaker-marketplace heptio-images
```
QUAY_NAMESPACE:
```
calico coreos prometheus outline weaveworks hellofresh kubernetes-ingress-controller replicated kubernetes-service-catalog 3scale wire
```
同步以上镜像,另外
k8s.gcr.io <==> gcr.io/google-containers <==> gcr.io/google_containers 


## How to use?

### 拉取
假设需要拉取gcr.io/google_containers/pause:3.0
```
$ curl -s https://www.sqeven.com/pull | bash -s -- gcr.io/google_containers/pause:3.0
```
### 查询
#### 查询域名仓库下的namespace和namespace里的镜像列表
```
$ curl -s https://www.sqeven.com/pull | bash -s search gcr.io
google-samples
google_containers
k8s-minikube
kubernetes-helm
runconduit
spinnaker-marketplace
tf-on-k8s-dogfood
$ curl -s https://www.sqeven.com/pull | bash -s search gcr.io/google_containers
addon-builder
addon-resizer-amd64
addon-resizer-arm
addon-resizer-arm64
addon-resizer-ppc64le
addon-resizer-s390x
......
```

#### 查询镜像的所有tag或者是否存在tag时
```
$ curl -s https://www.sqeven.com/pull | bash -s -- search gcr.io/google_containers/kube-apiserver-amd64
v1.10.0-alpha.0
v1.10.0-alpha.1
v1.10.0-alpha.2
v1.10.0-alpha.3
v1.10.0-beta.0
......
$ curl -s https://www.sqeven.com/pull | bash -s -- search gcr.io/google_containers/kube-apiserver-amd64:v1.9.3
v1.9.3
```

或者自己把内容保存为脚本拉取


## 过程:
### [click me to see](https://sqeven.github.io/2018/07/08/travis-sync-gcr-io/)
方便后期扩展
利用shell的先展开变量这一特点来实现了伪接口扩展来拉取其他仓库
```bash
foo(){
    while read img;do
        while read tag;do
            echo docker pull $img:$tag
            echo docker push repo/$img:$tag
        done < <( $@_get_img_tags $img)
    done < <( $@_get_names )
}


google_get_names(){

}

google_get_img_tags(){

}

quay_io_get_names(){

}
quay_io_get_img_tags(){

}


foo google
foo quay_io
```
