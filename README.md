## Terraform 설치

1. Windows Terminal에서 Ubuntu WSL 설치
   ```bash
   wsl --install -d Ubuntu

- account: user
- password: user

2. Windows Terminal에서 Ubuntu 접속
   
   <img alt="image" src="https://github.com/user-attachments/assets/09d70dda-29ed-4e5b-b72e-386a78d86a5c" width="500"/>
   <img alt="image" src="https://github.com/user-attachments/assets/d2fdddc4-b8b5-45cd-abfa-5cc6b7aa53bd" width="500"/>

4. HashiCorp 공식 문서 기준 Terraform 설치 (WSL Ubuntu)
   ```bash
   # 필수 패키지
   sudo apt update
   sudo apt install -y gnupg software-properties-common curl

   # HashiCorp GPG 키 등록
   curl -fsSL https://apt.releases.hashicorp.com/gpg | \
   sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

   # HashiCorp 저장소 추가
   echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
   https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
   sudo tee /etc/apt/sources.list.d/hashicorp.list

   # Terraform 설치
   sudo apt update
   sudo apt install terraform

- 실무에서 복붙. 외울 필요X. 왜 필요한지 이해O
- 설치 확인:
  ```bash
  terraform -version
  
## AWS 자격증명 붙이기
   
   - Terraform은 혼자 EC2 생성 불가 → AWS 권한(Access Key) 필요
   - AWS CLI 설치 → aws configure

1. AWS CLI v2 공식 바이너리 설치  (WSL Ubuntu)
   ```bash
   # 필요 패키지
   sudo apt update
   sudo apt install -y curl unzip

   # AWS CLI v2 다운로드
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

   # 압축 해제
   unzip awscliv2.zip

   # 설치
   sudo ./aws/install

   # 설치 확인
   aws --version

2. AWS 자격증명 연결 (WSL Ubuntu)
   
   - AWS 콘솔에서 Access Key 생성 후 자격증명 연결

   ```bash
   aws configure

   AWS Access Key ID [None]: AKIAxxxxxxxx
   AWS Secret Access Key [None]: xxxxxxxxxxxx
   Default region name [None]: ap-northeast-2
   Default output format [None]: json

   AWS 연결 확인:
   ```bash
   aws sts get-caller-identity

## Terraform으로 EC2 생성

1. 작업 폴더 생성 (WSL Ubuntu)

   ```bash
   mkdir tf-ec2
   cd tf-ec2

2. Terraform 파일 생성 (WSL Ubuntu)
   
   ```bash
   code .

- 자동으로 VS Code 열림

- 다음 창이 뜨면 Permanently allow host ‘wsl.localhost’ 체크 후 [Allow]
  
  <img alt="image" src="https://github.com/user-attachments/assets/be243004-62e5-48dc-859f-6a617860ee32" width="500"/>

- [New File…] 버튼 눌러서 main.tf 파일 생성

  <img alt="image" src="https://github.com/user-attachments/assets/8b8bc7f3-dae7-451b-9202-1c624254aaf9" width="500"/>

- main.tf에 작성

  ```bash
  provider "aws" {
     region = "ap-northeast-2"
  }

  data "aws_ami" "amazon_linux" {
     most_recent = true

     filter {
       name   = "name"
       values = ["al2023-ami-*-x86_64"]
     }

     owners = ["amazon"]
  }

  resource "aws_instance" "k3s_node" {
     ami           = data.aws_ami.amazon_linux.id
     instance_type = "t3.micro"

     tags = {
       Name = "dev-k3s-node-01"
       Environment = "dev"
       Project = "infra-auto"
     }
  }
