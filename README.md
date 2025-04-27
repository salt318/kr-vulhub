# CVE-2025-1974
> 화이트햇 스쿨 3기 - [김소은 (@salt318)] (https://github.com/salt318/vulhub)

### 요약

- 쿠버네티스의 핵심 보안 메커니즘인 Ingress-NGINX Admission Controller의 결함으로 인해 발생
- Ingress-NGINX Admission Controller는 인증 없이 네트워크에 노출됨
- 이로 인해 공격자가 악성 AdmissionReview 요청을 조작하여 Ingress 리소스에 무단 구성을 삽입할 수 있음
- 다른 취약점과 연계될 겨여우 원격 코드 실행 가능 (CVE-2025-24514, CVE-2025-1097 또는 CVE-2025-1098)


### 환경 구성 및 실행
- `docker compose up -d`를 실행하여 테스트 환경을 실행 (K3s 기반 Kubernetes 환경)
- 악성 쉘 코드를 컴파일
```
gcc -shared -fPIC -o shell.so shell.c
```
- 아래의 명령어를 통해 취약점 악용
  ```
  python exploit.py -a https://localhost:30443/networking/v1/ingresses -i http://localhost:30080/fake/addr -s shell.so
  ```
### 실행 결과
![CVE-2025-1974](https://github.com/salt318/CVE-2025-1974/blob/main/CVE-2025-1974.png)

### 정리
