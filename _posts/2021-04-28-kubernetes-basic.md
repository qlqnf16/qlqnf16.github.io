---
layout: post
title: "Kubernetes 시작하기"
tags: [kubernetes]
---

kubernetes의 기본 개념, 필수적인 오브젝트에 대해 알아보자

# Kubernetes 시작하기

그리스어로 **조타수**라는 뜻의 쿠버네티스는 표준으로 사용되는 컨테이너 오케스트레이션 도구이다. 여러 대의 도커 호스트를 하나의 클러스터로 만들고, 관리를 위한 다양한 기능을 제공한다.

## 쿠버네티스 오브젝트

### 포드 (Pod)

컨테이너 어플리케이션의 기본 단위. 1개 이상의 컨테이너로 구성된 컨테이너의 집합. **하나의 포드는 하나의 완전한 어플리케이션**이다.

하나의 포드에 포함된 컨테이너들은 네트워크를 비롯한 리눅스 네임스페이스를 공유한다.

```shell
$ kubectl exec -it my-nginx-pod -c ubuntu-sidecar-container bash
```

```
root@my-nginx-pod:/# curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
```

하나의 포드에는 기본적으로 하나의 완전한 어플리케이션인 컨테이너가 존재한다. 하지만 하나의 컨테이너가 실행되기 위해 부가적인 기능을 필요로 한다면, 그 역할을 하는 컨테이너를 추가로 포드에 포함시킬 수 있는 것이다. 이 추가된 컨테이너를 sidecar 라고 부르며, 리눅스 네임스페이스를 공유한다.

### 레플리카셋 (Replica Set)

포드를 관리하는 오브젝트. **정해진 수의 동일한 포드가 항상 실행되도록 관리**하고, 포드를 사용할 수 없다면 다른 노드에서 포드를 재생성한다.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-nginx
spec:
  replicas: 4
  selector:
    matchLabels:
      app: my-nginx-pods-label
  template:
  ...
```

위와 같이 `spec.replicas` 에 정의된 숫자만큼 동일한 포드를 생성한다.

레플리카와 포드는 <u>라벨 셀렉터를 이용한 느슨한 연결</u>을 유지한다. `spec.selector.matchLabels` 에 정의된 라벨을 통해 포드를 관리한다. 먼저 포드를 찾고, 만약 포드의 수가 정의된 숫자와 다르다면 `spec.template` 항목을 참고해 포드를 생성/삭제한다.

### 디플로이먼트 (Deployment)

레플리카셋, 포드의 배포를 관리하는 상위 오브젝트.

![img](https://blog.kakaocdn.net/dn/mVW7g/btqFNFa7u1E/RTURWaMRdcLNpDzxj3GAwk/img.png)

일반적으로 쿠버네티스에서는 delopyment.yaml 파일을 작성해 관리한다. 레플리카셋의 리비전 관리, 포드의 롤링 업데이트 정책 등을 사용할 수 있기 때문이다.

예를 들어 `kubectl apply -f [deploymentfild].yaml --record` 를 이용해 디플로이를 생성하고, 템플릿을 수정한다면 기존 포드는 사라지고 수정 사항이 반영된 새로운 포드가 생긴다. 레플리카셋의 경우 아래와 같이 기존의 레플리카셋과 새로 바뀐 레플리카셋이 모두 남아있다.

```
NAME                             DESIRED   CURRENT   READY   AGE
my-nginx-deployment-556b57945d   3         3         3       34s
my-nginx-deployment-7484748b57   0         0         0       5m42s
```

그 후 `kubectl describe deploy my-nginx-deployment` 명령어를 실행하면, 아래와 같이 `revision: 2` 와 old, new replica set 에 대한 정보 등이 표시된다.

```
Name:                   my-nginx-deployment
Namespace:              default
CreationTimestamp:      Tue, 27 Apr 2021 23:31:20 +0900
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 2
                        kubernetes.io/change-cause: kubectl set image deployment my-nginx-deployment nginx=nginx:1.11 --record=true
...
Pod Template:
  Labels:  app=my-nginx
  Containers:
   nginx:
    Image:        nginx:1.11
	...
OldReplicaSets:  <none>
NewReplicaSet:   my-nginx-deployment-556b57945d (3/3 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  6m39s  deployment-controller  Scaled up replica set my-nginx-deployment-7484748b57 to 3
  Normal  ScalingReplicaSet  91s    deployment-controller  Scaled up replica set my-nginx-deployment-556b57945d to 1
  Normal  ScalingReplicaSet  78s    deployment-controller  Scaled down replica set my-nginx-deployment-7484748b57 to 2
  Normal  ScalingReplicaSet  78s    deployment-controller  Scaled up replica set my-nginx-deployment-556b57945d to 2
  Normal  ScalingReplicaSet  74s    deployment-controller  Scaled down replica set my-nginx-deployment-7484748b57 to 1
  Normal  ScalingReplicaSet  74s    deployment-controller  Scaled up replica set my-nginx-deployment-556b57945d to 3
  Normal  ScalingReplicaSet  69s    deployment-controller  Scaled down replica set my-nginx-deployment-7484748b57 to 0
```

### 서비스 (Service)

디플로이먼트의 포드를 외부에 노출하거나 내부적으로 서로 접근할 수 있도록 하는 오브젝트. 포드에 도메인명을 부여하고, 포드를 외부로 노출하며, 여러 개의 포드에 접근할 때 로드밸런서의 기능을 수행한다.

#### **서비스의 종류**

- ClusterIp 타입: 외부에 포트 노출 X. 클러스터 내부에서만 포드에 접근
- NodePort 타입: 클러스터의 모든 포드에 대해 외부에 포트 노출
- LoadBalancer 타입: 외부에 포트 노출. AWS, GCP 같은 클라우드 환경에서 사용
- ExternalName: 서비스가 외부 도메인을 가리키도록 설정. 쿠버네티스와 별개로 존재하는 레거시 시스템과의 연동

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hostname-svc-clusterip
spec:
  ports:
    - name: web-port
      port: 8080
      targetPort: 80
  selector:
    app: webserver
  type: ClusterIP
```

위와 같이 작성된 yaml 파일에서 spec.selector 에 명시된 라벨을 가진 deployment 를 찾아 그에 속한 포드와 연결해 endpoint 를 생성한다. 서비스가 생성되면 서비스의 IP와 `spec.ports.port` 에 명시된 포트를 통해 포드로 접근이 가능하다. 또는 `metadata.name` 에 명시된 이름을 도메인으로 접속할 수 있다. 서비스는 로드밸런서 기능을 수행하므로, 매 호출마다 각각 다른 포드의 IP 로 접근하게 된다.

```bash
curl [IP]:8080
curl hostname-svc-clusterip:8080
```

#### 서비스의 옵션

**externalTrafficPolicy**: 로드밸런싱 옵션 관리

- Cluster (default): 모든 노드에 랜덤한 포트 개방, 노드A에 들어온 요청도 포드B에 로드밸런싱될 수 있음
- Local: 포드가 생성된 노드에서만 접근 가능, 로컬 노드에 위치한 포드 중 로드밸런싱. 노드A에 들어온 요청은 노드A 내부에 있는 포드에만 전달됨.

> 출처: 시작하세요! 도커/쿠버네티스 6장
