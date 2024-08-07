## 배경지식 
k8s 사용 플랫폼 : https://landscape.cncf.io/?group=projects-and-products
컨테이너표준이 docker 에서 docker를 표준화한 사양으로 변경됨

- [kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) :

## 설치시 고려해야할 것들
> CRI, CSI 
[CRI](https://kubernetes-csi.github.io/docs/drivers.html)
- [CRI: the Container Runtime Interface](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md)
kubelet 이 Pod를 실행할 수 있게 하는 플러그인 인터페이스
[cri-o](https://cri-o.io/)
[Containerd](https://containerd.io/) : docker기반이라 문제가 발생할 수 있음, 기본적으로 docker image
[Cri-dockerd](https://github.com/Mirantis/cri-dockerd) : Mirantis 에 docker가 인수 후에 개편된 docker기반의 컨테이너 스펙, OCI 스펙을 따르는게 유리하다

CNI 는 네트워크 인프라 영역, 이쪽은 고려내용없음

CSI 는 스토리지
- [CSI 드라이버 목록](https://kubernetes-csi.github.io/docs/drivers.html)
- 스토리지의 life cycle 지원

## HyperV 설치

## Container 전환
- Rootless container 적용으로 scale out, scale up 시 시간 이득이 있음
- k8s 로 전환할 수 있는 형태인지 podman 으로 확인할 수 있다. 
- [ ] Podman 정의
  - 컨테이너 생성, 관리하는 오케스트레이션 도구
  - 여기서 말하는 `Pod`는 k8s의 `Pod`와 비슷한 개념이다. 즉 같은말은 아니다.
    - [What are pods](https://www.redhat.com/en/topics/containers/what-is-podman#what-are-pods)
    - [Podman vs Docker](https://www.redhat.com/en/topics/containers/what-is-podman#podman-vs-docker)
  - 도커와 거의 동일한 호환성
  - 컨테이너 이미지빌드 및 POD 테스트
  - 컨테이너런타임인터페이스(CRI) 로의 사용은 불가능
  - [ ] [Why podMan](https://www.redhat.com/en/topics/containers/what-is-podman#why-podman)
    - 읽어봐야함
  - [ ] infra-container 차이점
    - podman 의 pod를 inspect 하면 `SharedNameSpace` 에서 net 의 공유자원을 설정하여 네트워크 대역가능
- 코드가 컨테이너에 맞는 형태인지 확인

### generate kube
이미지를 기반으로, Pod와 연관된 container 를 생성

## k8s_cluster
- controller node
- compute node
- infra node
 - ingress 관련설정
보통 3가지 노드로 구분한다