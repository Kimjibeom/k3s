# k3s (Lightweight Kubernetes)

IoT 및 엣지 컴퓨팅을 위해 제작된 Kubernetes의 경량 시스템

Great for:

* Edge
* IoT
* CI
* Development
* ARM
* Embedding k8s
* Situations where a PhD in k8s clusterology is infeasible



1. 단일 바이너리로 패키지되어 있습니다.
2. sqlite3에 대한 지원을 기본 스토리지 백엔드로 추가합니다. Ettd3, MySQL 및 Postgres도 지원됩니다.
3. Kubernetes 및 기타 구성 요소를 하나의 간단한 런처에 래핑합니다.
4. 기본적으로 경량 환경을 위한 적절한 기본값으로 보호됩니다.
5. OS 종속성이 최소화되거나 전혀 없습니다(정상적인 커널과 cgroup 마운트만 필요).
- 컨테이너 컨테이너
- 플란넬
- 코어DNS
- CNI
- 호스트 유틸리티(IP테이블, 소캣 등)
- 잉그레스 컨트롤러 (트라피크)
- 임베디드 서비스 로드밸러저
- 임베디드 네트워크 정책 컨트롤러
6. 웹 소켓 터널을 통해 Kubernetes 제어부 노드에 이 API를 노출시켜 Kubernetes worker 노드의 포트를 노출할 필요가 없습니다.



# 작동 방식

![1](https://user-images.githubusercontent.com/73589723/124220346-b2d6b880-db38-11eb-8e10-80ad51b9031b.PNG)



# 설치



1. Download `k3s` from latest [release](https://github.com/k3s-io/k3s/releases/latest), x86_64, armhf, and arm64 are supported.
1. Run the server.

The `install.sh` script provides a convenient way to download K3s and add a service to systemd or openrc.

To install k3s as a service, run:

```bash
curl -sfL https://get.k3s.io | sh -
```

A kubeconfig file is written to `/etc/rancher/k3s/k3s.yaml` and the service is automatically started or restarted.
The install script will install K3s and additional utilities, such as `kubectl`, `crictl`, `k3s-killall.sh`, and `k3s-uninstall.sh`, for example:

```bash
sudo kubectl get nodes
```

`K3S_TOKEN` is created at `/var/lib/rancher/k3s/server/node-token` on your server.
To install on worker nodes, pass `K3S_URL` along with
`K3S_TOKEN` or `K3S_CLUSTER_SECRET` environment variables, for example:

```bash
curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=XXX sh -
```
```bash

1. Download `k3s` from latest [release](https://github.com/k3s-io/k3s/releases/latest), x86_64, armhf, and arm64 are supported.
2. Run the server.

sudo k3s server &
# Kubeconfig is written to /etc/rancher/k3s/k3s.yaml
sudo k3s kubectl get nodes

# On a different node run the below. NODE_TOKEN comes from
# /var/lib/rancher/k3s/server/node-token on your server
sudo k3s agent --server https://myserver:6443 --token ${NODE_TOKEN}
```


자세한 설치메뉴얼은 k3s-install-Raspberry-PI 레퍼지토리를 참고.



# Kubeflow 파이프라인 배포



Kubeflow 파이프라인의 설치 프로세스는 이 가이드에서 다루는 세 가지 환경 모두에 대해 동일합니다.

1. Kubeflow 파이프라인을 배포하려면 다음 명령을 실행합니다.

```bash
# env/platform-agnostic-pns hasn't been publically released, so you will install it from master
export PIPELINE_VERSION=1.6.0
kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources?ref=$PIPELINE_VERSION"
kubectl wait --for condition=established --timeout=60s crd/applications.app.k8s.io
kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/platform-agnostic-pns?ref=$PIPELINE_VERSION"
```
Kubeflow 파이프라인 배포를 완료하는 데 몇 분 정도 걸릴 수 있습니다.

2. 포트 포워딩으로 Kubeflow 파이프라인 UI에 액세스할 수 있는지 확인합니다.
```bash
kubectl port-forward -n kubeflow svc/ml-pipeline-ui 8080:80
```

그런 다음 가상 시스템 내에서 종류 또는 K3s를 사용하는 경우 Kubeflow 파이프라인 UI를 엽니다.
```http://localhost:8080/http://{YOUR_VM_IP_ADDRESS}:8080/```

# Kubeflow 파이프라인 제거

다음은 종류, K3s 또는 K3ai에서 Kubeflow 파이프라인을 제거하는 단계입니다.

- 매니페스트 파일을 사용하여 Kubeflow 파이프라인을 제거하려면 매니페스트 파일의 이름으로 대체하여 다음 명령을 실행합니다.{YOUR_MANIFEST_FILE}

```bash
kubectl delete -k {YOUR_MANIFEST_FILE}`
```

- Kubeflow 파이프라인의 GitHub 리포지토리의 매니페스트를 사용하여 Kubeflow 파이프라인을 제거하려면 다음 명령을 실행합니다.

```bash
export PIPELINE_VERSION=1.6.0
kubectl delete -k "github.com/kubeflow/pipelines/manifests/kustomize/env/platform-agnostic-pns?ref=$PIPELINE_VERSION"
kubectl delete -k "github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources?ref=$PIPELINE_VERSION"
```

- 로컬 리포지토리 또는 파일 시스템의 매니페스트를 사용하여 Kubeflow 파이프라인을 제거하려면 다음 명령을 실행합니다.

```bash
kubectl delete -k manifests/kustomize/env/platform-agnostic-pns
kubectl delete -k manifests/kustomize/cluster-scoped-resources
```


# 참고
https://github.com/k3s-io/k3s/blob/master/README.md  //k3s github
https://www.kubeflow.org/docs/components/pipelines/installation/localcluster-deployment/  //kubeflow
