# k8s 의 component
- [x] k8s 환경구축하는데 순서가있을까?
  - kubelet 이 우선순위가 가장 높다

https://kubernetes.io/images/docs/components-of-kubernetes.svg


## control-plane-components
- [kube-controller-manager](https://kubernetes.io/docs/concepts/overview/components/#kube-controller-manager)
  - node의 상태를 결정
- [kube-apiserver](https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver)
  - control-plane 내부의 *API GATEWAY* 역할
- [kube-scheduler](https://kubernetes.io/docs/concepts/overview/components/#kube-scheduler)
  - pods 생성시, 노드 선택 역할
- [etcd](https://kubernetes.io/docs/concepts/overview/components/#etcd)
  - control-plane 의 component 는 stateless로 유지, etcd에 상태저장
  - Persistent Volume 참조

## node components
- [kube-proxy](https://kubernetes.io/docs/concepts/overview/components/#kube-proxy)
  - networkPolicy, service 관련 제어
- [container-runtime](https://kubernetes.io/docs/concepts/overview/components/#container-runtime)
  - containerd, crio 를 선택할 수 있으며 종류에 따라 실행중인 컨테이너상태가 바뀐다거나 그렇지 않을 수 있다
  - 가령 docker 의 containerd 는 docker 서비스가 죽을 경우, 관련 컨테이너가 모두 죽는다
- kubelet
  - pod 가 아닌 hostservice


## pod 와 pause image
- k8s와 podman 의 POD는 pause 라는 이미지의 실행체이다
- pause 를 통해 실행된다

local registry 의 pause 이미지확인하는 명령
```shell
podman images | grep pause

localhost/podman-pause                   4.9.4-rhel-1721801275  29e02abf1104  47 hours ago   814 kB
registry.k8s.io/pause                    3.9                    e6f181688397  22 months ago  750 kB
```
podman과 k8s 의 POD 를 실행하는 image가 각각 다른것을 알 수 있다

- [ ] infra-container 관련 내용 확인 필요
  - [container-runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) 

## Service, EndPoint
> expose 로 k8s-service 를 생성한다
- externalname은 사용안함

- POD IP
- EXTERNAL_IP
  - CLUSTER_IP
- INTERNAL_IP
```shell
kubectl expose pod nginx
kubectl expose pod nginx --type=NordPort
kubectl expose pod nginx --type=ClusterIP
```
- [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport)
- [ ] VXN 터널링

# 실습
- https://k8syaml.com/
- [Pod와 Container 사이의 Process Namespace 공유](https://kubernetes.io/docs/tasks/configure-pod-container/share-process-namespace/)
- [OVS](https://www.openvswitch.org/)
- SEP(Service End Point)
- namespace 당 프로젝트 1개
```
table ip nat
table ip filter
```
- kubectl explain
- [동일한 컨테이너에서 환경설정](https://syhwang.tistory.com/56)
- [ ] gogs 경량화된 git서버
- [ ] tekton git서버와 image registry 웹훅 역할햣
