# 쿠버네티스(Kubernetes)

쿠버네티스는 컨테이너화된 애플리케이션이 안정적으로 실행되도록 관리하고 조정하는 역할을 함. 인프라 설정에 대한 세부 사항 없이 애플리케이션 배포 및 운영에 필요한 모든 것을 관리함.

## 클러스터 설정 및 관리

### 클러스터 생성 방법
쿠버네티스 클러스터는 수동 설정, **EKS (Amazon Elastic Kubernetes Service)**, **Kubermatic** 등을 통해 생성할 수 있음.

### kubectl
**kubectl**은 클러스터와 상호작용하며, 배포 및 리소스 설정을 변경하는 데 사용되는 커맨드라인 도구임.

### Minikube
**Minikube**는 로컬 환경에서 쿠버네티스를 테스트하고 학습할 수 있도록 제공되는 경량 클러스터임.

### 설치 및 설정
- [kubectl 설치 가이드](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/#install-nonstandard-package-tools)
- Windows에서 설치 예시:
  ```bash
  choco install kubernetes-cli
  kubectl version --client

## Minikube 설정
Minikube가 사용할 가상 머신 환경을 위해 **VirtualBox** 설치가 필요함.

# 대시보드 (Dashboard)
쿠버네티스 대시보드는 클러스터 리소스 상태를 시각적으로 확인하고, Pod 상태, 노드 상태 등을 조회할 수 있는 웹 기반 인터페이스를 제공함.

---

# 쿠버네티스의 내부 동작

쿠버네티스는 **객체(Object)**들을 통해 클러스터를 구성하고 관리함. 객체는 애플리케이션의 배포, 확장, 네트워킹, 스토리지를 담당하는 핵심 구성 요소임.

## 주요 객체
- **Deployments**: Pod의 수, 컨테이너 수, 배포 상태를 관리함.
- **Pods**: 하나 이상의 컨테이너를 포함한 쿠버네티스의 가장 작은 배포 단위임.
- **Services**: Pod 간 또는 클러스터 외부와의 통신을 담당함.
- **Volumes**: Pod에 영구적인 스토리지를 제공함.

## 객체 생성 방식
- **명령적 방식**: 명령어를 통해 즉시 객체를 생성함.
- **선언적 방식**: YAML 파일로 정의된 상태를 사용해 객체를 생성함.

## Pod
- **Pod**는 쿠버네티스의 가장 작은 단위이며, 하나 이상의 컨테이너로 구성됨.
- Pod 내의 컨테이너들은 `localhost`로 통신 가능하며, Pod는 클러스터 내부 IP로 다른 Pod와 연결됨.
- Pod는 임시적 특성을 가지며, 삭제되면 기본적으로 관련 리소스도 함께 삭제됨.

## Deployment
- **Deployment**는 Pod 수와 같은 배포 지침을 제공하는 객체로, 이를 통해 안정적인 애플리케이션 배포를 보장함.
- Deployment는 **컨트롤러 객체**를 통해 자동으로 리소스를 관리함.
- 일시 중지, 삭제, 롤백, 스케일링이 가능하며, 수신 트래픽과 CPU 사용률을 기반으로 **오토스케일링** 기능을 지원함.

---

# 애플리케이션 배포 과정

1. **컨테이너 이미지 생성**: 애플리케이션을 도커로 빌드하여 이미지를 생성함.
2. **클러스터 실행 확인**: 클러스터가 실행 중인지 확인하고 이미지를 배포할 준비를 함.
3. **Deployment 생성**: `kubectl` 명령어를 사용해 Deployment 객체를 생성
   ```bash
   kubectl create deployment <이름> --image=<이미지이름>

## 상태 조회
배포된 **Deployment**와 **Pod** 상태를 조회할 수 있음.

    ```bash
    kubectl get deployments
    kubectl get pods

## 이미지 위치
쿠버네티스 클러스터에서 이미지를 실행하려면, 클러스터가 해당 이미지에 접근할 수 있는 위치에 저장되어 있어야 함.

---


# 쿠버네티스 개요: 컨트롤 플레인, 워커 노드, 및 서비스 관리

## 컨트롤 플레인과 워커 노드

1. **컨트롤 플레인**에 명령이 전달되면, **스케줄러**가 현재 실행 중인 Pod를 분석하고 새로 생성할 Pod에 적합한 워커 노드를 찾습니다.
2. 선택된 워커 노드에서 **kubelet** 서비스가 Pod를 관리하고 배포를 수행합니다.

## Pod와 Service

- **Pod**는 내부 IP 주소를 가지지만, 이 IP는 자주 변경될 수 있어 외부 통신 수단으로는 적합하지 않습니다.
- Pod를 찾기 어렵기 때문에, 이를 해결하기 위해 **Service** 객체를 통해 Pod를 그룹화하고, 변하지 않는 고정 주소를 제공합니다.
- 기본적으로 내부 IP만 사용하지만, **Service** 객체를 생성하면 외부에서도 Pod에 접근할 수 있습니다.

### Service 생성 명령

```bash
kubectl expose deployment <이름> --type=LoadBalancer --port=<포트번호>
```

이 명령을 통해 Deployment를 외부에 노출하는 Service를 생성합니다. Service의 `type`에는 여러 가지 옵션이 있으며, 주요 옵션은 다음과 같습니다:

- **NodePort**: 클러스터 외부에서 노드의 IP를 통해 접근할 수 있도록 합니다.
- **ClusterIP**: 클러스터 내부에서만 접근 가능하게 합니다 (기본 옵션).
- **LoadBalancer**: 외부 로드 밸런서를 통해 접근할 수 있도록 합니다.

## Pod 확장 (Scaling)

애플리케이션의 부하에 따라 Pod의 인스턴스를 늘리기 위해 `scale` 명령을 사용합니다. 여기서 `replicas`는 생성할 Pod의 개수를 의미합니다.

```bash
kubectl scale deployment/<이름> --replicas=<개수>
```

## 이미지 업데이트

코드나 애플리케이션을 수정한 경우, 새로운 이미지를 빌드하고 푸시한 후 Deployment를 업데이트할 수 있습니다.

```bash
kubectl set image deployment/<이름> <컨테이너명>=도커/<이미지이름>
```

## 배포 상태 확인과 롤백

배포 상태를 확인하고, 필요에 따라 롤백할 수 있습니다.

- **배포 상태 확인**:

  ```bash
  kubectl rollout status deployment/<이름>
  ```

- **롤백**:

  ```bash
  kubectl rollout undo deployment/<이름>
  ```

- **특정 버전으로 롤백**:

  ```bash
  kubectl rollout history deployment/<이름> --revision=<버전번호>
  kubectl rollout undo deployment/<이름> --to-revision=<가고자하는버전>
  ```

## 선언적 접근 방식 (Declarative Approach)

쿠버네티스는 `docker-compose`와 유사하게 YAML 파일을 통해 설정할 수 있습니다. 선언적 방식에서는 `selector`를 사용해 `matchLabels`나 `matchExpressions`로 객체를 선택합니다.

- **matchExpressions**: 특정 조건을 충족하는 표현식.
- **matchLabels**: 라벨을 통해 객체를 그룹화.

### 파일 적용 명령

YAML 파일을 작성한 후 다음 명령으로 설정을 적용할 수 있습니다.

```bash
kubectl apply -f <파일명>.yaml
```

여러 설정을 하나의 YAML 파일에 정의할 수 있습니다. `---`를 사용해 Service, Deployment, Job 등 여러 객체를 하나의 파일에 함께 정의할 수 있습니다.

## 라벨과 필터링

`metadata`의 `labels`를 통해 객체에 라벨을 설정할 수 있으며, `-l` 옵션을 사용해 특정 라벨을 가진 객체만 필터링하여 삭제나 조회가 가능합니다.

## Liveness Probe 설정

`livenessProbe`를 사용해 컨테이너의 상태를 모니터링하고, 문제가 발생할 경우 재시작을 트리거할 수 있습니다.

### 예제 YAML 파일

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector:
    app: second-app
  ports:
    - protocol: 'TCP'
      port: 80
      targetPort: 8080
  type: LoadBalancer
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: second-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: second-app
      tier: backend
  template: 
    metadata:
      labels:
        app: second-app
        tier: backend
    spec:
      containers:
        - name: second-node
          image: <사용자아이디>/<이미지이름>
          imagePullPolicy: Always
          livenessProbe: 
            httpGet:
              path: /
              port: 8080
            periodSeconds: 10
            initialDelaySeconds: 5
```

이 예제는 `backend`라는 이름의 Service와 `second-app-deployment`라는 Deployment를 정의하며, `second-app`이라는 라벨을 사용해 Deployment와 Service를 연결합니다.

