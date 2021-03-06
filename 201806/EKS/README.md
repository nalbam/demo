# EKS

#### Elastic Container Service for Kubernetes

## Terraform

-

### 유정열 (nalbam)

---

## Kubernetes

* 컨테이너 작업을 자동화하는 오픈소스 플랫폼
* Cluster 는 Master 와 Nodes 로 구성

> <img src="images/kubernetes.png" height="300">

---

## EKS

* AWS 의 Kubernetes 관리형 서비스
* AWS 에서 Master Node 관리
* 우리는 Worker Node 만 관리

> <img src="images/what-is-eks.png" height="300">

---

## Terraform

* 인프라를 코드로 관리하고, 이를 배포/관리 할 수 있는 오픈 소스 도구
* Infrastructure as Code
* Made by HashiCorp
* https://www.terraform.io/

---

## aws-cli

* version > 1.15.32

```bash
pip3 install awscli --upgrade --user

# 또는

pip install awscli --upgrade --user
```

Note:
- 최신버전을 이용 하도록 하고, 최소 1.15.32 이상

---

## terraform

```bash
brew install terraform
```

* https://www.terraform.io/intro/getting-started/install.html

Note:
- Mac 은 홈브루를 통해 쉽게 설치가 가능
- 기타 환경이라면 문서를 참조

---

## kubectl

```bash
brew install kubectl
```

* https://kubernetes.io/docs/tasks/tools/install-kubectl/
* https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/configure-kubectl.html

Note:
- Mac 은 홈브루를 통해 쉽게 설치가 가능
- 기타 환경이라면 문서를 참조

---

## heptio

```bash
# mac
curl -o heptio-authenticator-aws https://amazon-eks.s3-us-west-2.amazonaws.com/1.10.3/2018-06-05/bin/darwin/amd64/heptio-authenticator-aws
chmod +x ./heptio-authenticator-aws && sudo mv ./heptio-authenticator-aws /usr/local/bin/

# linux
curl -o heptio-authenticator-aws https://amazon-eks.s3-us-west-2.amazonaws.com/1.10.3/2018-06-05/bin/linux/amd64/heptio-authenticator-aws
chmod +x ./heptio-authenticator-aws && sudo mv ./heptio-authenticator-aws /usr/local/bin/
```

* https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/configure-kubectl.html

Note:
- Kubernetes cluster 에 인증하기 위한 툴
- 각 클라이언트 운영체제에 맞는 버전을 설치

---

## bastion

* 바로 시작하고 싶다면!
* 이 AMI 에 모든 tool 을 설치 해놨습니다.

* https://ap-northeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#Images:visibility=public-images;imageId=ami-0c5f492d14b590d42

---

## terraform config

* 소스를 다운 받고, 설정을 수정합니다. `10-aws.tf`

```bash
git clone https://github.com/nalbam/terraform-aws-eks
```

```hcl-terraform
terraform {
  backend "s3" {
    region = "ap-northeast-2"
    bucket = "terraform-nalbam-seoul"
    key = "eks.tfstate"
  }
}
```

Note:
* Terraform 에서 생성한 리소스를 저장/관히하기 위한 S3 Bucket 입니다.

---

## terraform apply

```bash
terraform init

terraform apply
```

Note:
- 테라폼 소스를 살펴 보도록 합니다.
- 소스를 다운받고, terraform apply.
- VPC, Subnet, SG, EKS, ASG, Instance 들이 만들어 집니다.

---

## kube config

```bash
mkdir -p ~/.kube
cat .output/kube_config.yml > ~/.kube/config
```

* https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/create-kubeconfig.html

Note:
- 설치된 클러스터로 부터 인증 정보를 받아서 저장
- 이제 원격에서 kubectl 명령이 가능

---

## kubectl

```bash
kubectl get node

kubectl get deploy,pod,svc --all-namespaces
```

Note:
- 모든 Namespace 에 어떤 Pod 가 올라와 있는지 확인해 봅니다.
- 하지만, Node 가 없네요.

---

## aws auth

```bash
kubectl apply -f .output/aws_auth.yml
```

* https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/add-user-role.html

Note:
- 워커 노드가 마스터 노드에 join 할 수 있도록 권한을 주어야 합니다.

---

## sample

```bash
# apply
kubectl apply -f data/sample-web.yml

# elb endpoint
kubectl get svc -o wide -n default

# delete
kubectl delete -f data/sample-web.yml
```

Note:
- 샘플 앱을 올려 봅니다.
- ELB 에 Endpoint 가 생성됨을 알수 있습니다.

---

## terraform destroy

```bash
terraform destroy
```

Note:
- 하지만 정상적으로 지워지지 않을것 입니다.
- ELB 는 terraform 에 의하여 만들어지지 않아 terraform 으로 지울수 없고.
- ELB 가 VPC, Subnet 등을 물고 있기 때문 입니다.

---

## Thank You

---

## We are hiring...

> ![](images/interest.png)

* jungyoul.yu@bespinglobal.com
