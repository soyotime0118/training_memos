
# k8s 구축
## device 설정
> 방화벽,swap, selinux, hosts 등 설정한다

### 방화벽, swap
```shell
systemctl stop firewalld && systemctl disable firewalld
swapon -s
swapoff -a
```
가상디스크가 동작중일경우 k8s가 동작안함

### selinux 설정 끄기
```shell
setenforce 0
vi /etc/selinux/config
> permissive
```

### hosts 설정
DNS서버가 없을 경우, 노드 사이에 식별하기 위해 `/etc/hosts` 수정
```shell
192.168.10.10 node1.example.com node1
192.168.10.20 node2.example.com node2
```

### 모듈설정
쿠버네티스에서 사용하는 모듈
- br_netfilter
- overlay

### nmcli
- [ ] nmcli ??

```shell
nmcli con add con-name eth1 ipv4.addresses 192.168.10.10/24 type ethernet ifname eth1 ipv4.method manual
nmcli con sh
ip a s eth1
```


## 필요 패키지 설치
> kubectl, kubeadm, kubelet 설치

### 쿠버네티스 저장소

### kubeadm
> init 으로 control-plane 을 생성, join 명령으로 worker node에서 control-plane에 join 한다
[kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)

### calico operator 설치

#### pod 사이의 터널링설정
> calico operator 라는 건데, 네트워크와 비슷한것 같다.
[Quickstart for Calico on K8s](https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart)

- [ ] [k8s network policy](https://kubernetes.io/ko/docs/concepts/services-networking/network-policies/) 와 차이점은 뭔지?

- [ ] NetworkPolicy 와 CNI 의 관계는?
  - [CNI](https://github.com/containernetworking/cni?tab=readme-ov-file)
  - [Multus](https://github.com/k8snetworkplumbingwg/multus-cni)
- [ ] 터널링 네트워크?
  - `tigera-operator.yaml` 을 `kubectl create` 로 실행하고 위 `yaml` 을 `kubectl create -f` 로 한번 더 실행하면 터널링네트워크가 생성된다.

#### calico 설치
```shell
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/tigera-operator.yaml
```

구성파일
```yaml
apiVersion: operator.tigera.io/v1
kind: Installation
metadata:
  name: default
spec:
  # Configures Calico networking.
  calicoNetwork:
    # Note: The ipPools section cannot be modified post-install.
    ipPools:
    - blockSize: 26
      cidr: 192.168.0.0/16
      encapsulation: VXLANCrossSubnet
      natOutgoing: Enabled
      nodeSelector: all()
  registry: quay.io
```

## KUBECONFIG export
장비 재시작후 아래 명령어를 입력해야 kubectl 명령이 정상동작한다
[how-to-export-kubeconfig-file-from-existing-cluster](https://stackoverflow.com/questions/61829214/how-to-export-kubeconfig-file-from-existing-cluster)
[.bashrc에 추가](https://stackoverflow.com/a/67827381)
```shell
export KUBECONFIG=/etc/kubernetes/admin.conf 
```

# kubectl 생성
- kubectl create 은 `yaml` 생성하는 것이 목적, 문법체크할 수 있는 `--dry-run`, yaml 을 output 하는것이 목적
- kubectl apply 은 `yaml` 이 있을때 사용하는것이 목적, 컴포넌트를 *갱신* 하는 것이 목적


# 클러스터 네트워킹
- [Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/) : k8s 에서 정의하는 네트워크
- [Kubernetes IP address ranges](https://kubernetes.io/docs/concepts/cluster-administration/networking/#kubernetes-ip-address-ranges) : 구체적으로 어떤 pod, service, node 사이의 어떤관계인가
- [ ] OVN 개념뭔가?
  - [OVN Kubernetes](https://ovn-kubernetes.io/)
- [ ] OVS 개념뭔가?

# 그외 구성
- [ ] 개념확인필요
- kube-scheduler
  - [Kubernetes Scheduler](https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/)

- kube-apiserver
  - [Kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)


# 모니터링 표준
- [ ] kubesphere 확인필요
- [모니터링](https://landscape.cncf.io/guide#observability-and-analysis--observability)
- [ ] 카오스엔지니어링, 개념확인 필요
  - [LitmusChaos](https://github.com/litmuschaos/litmus)

# 참고
- neovim
```shell
dnf install neovim-ale yamllint -y
```