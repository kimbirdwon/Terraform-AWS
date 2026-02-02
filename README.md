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

   ```bash
   
   
