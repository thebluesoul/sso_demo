# SSO Demo 프로젝트

## 개요
SSO Demo 프로젝트는 Keycloak을 활용한 싱글 사인온(SSO) 기반의 인증 시스템을 포함한 백엔드 및 프론트엔드 애플리케이션입니다.

- **Secured 서비스:** Node.js 기반 API 서버
- **사용자 애플리케이션:** Node.js 기반 웹 애플리케이션
- **Managed Keycloak:** 인증 및 권한 부여 시스템 (https://keycloakbb.demo.genians.co.kr)
- **Docker Compose:** 컨테이너 환경에서 손쉽게 실행 가능

## 프로젝트 실행 방법

### 1. 프로젝트 클론
```sh
git clone https://github.com/thebluesoul/sso_demo.git
```

### 2. Docker 컨테이너 빌드 및 실행
```sh
cd sso_demo
docker-compose build --no-cache
docker-compose up -d
```

### 3. 실행 상태 확인
```sh
docker-compose ps
```

### 4. 실행된 서비스 접속
- **사용자 애플리케이션**: `http://localhost:3000`
- **Secured 서비스**: `http://localhost:8000`

### 5. 로그 확인 (실시간)
```sh
docker-compose logs -f
```

### 6. 컨테이너 중지 및 정리
```sh
docker-compose down
```

## 주요 파일 및 디렉토리 구조
```
.
├── backend/              # 백엔드 서버 코드
│   ├── Dockerfile
│   ├── app.js
│   ├── package.json
│   ├── keycloak.json
├── frontend/             # 프론트엔드 애플리케이션 코드
│   ├── Dockerfile
│   ├── app.js
│   ├── index.html
│   ├── package.json
├── docker-compose.yml    # Docker Compose 설정 파일
├── README.md             # 프로젝트 문서
└── .git/                 # Git 저장소 정보
```

## 오류 해결
### 포트 충돌 문제 해결
기존 컨테이너가 8000 포트를 사용 중이라면 다음 명령어로 확인 후 종료하세요:
```sh
$ docker-compose up -d
Creating network "sso_demo_ch2_network" with driver "bridge"
Creating sso_demo_backend_1 ... 
Creating sso_demo_frontend_1 ... 
Creating sso_demo_frontend_1 ... error
Creating sso_demo_backend_1  ... done
_1 (ea7dc979c4c9df98971e25dbc81b35b5b467ee8e2fd5b61d37eb10e0653fa495): Bind for 0.0.0.0:8000 failed: port is already allocated

ERROR: for frontend  Cannot start service frontend: driver failed programming external connectivity on endpoint sso_demo_frontend_1 (ea7dc979c4c9df98971e25dbc81b35b5b467ee8e2fd5b61d37eb10e0653fa495): Bind for 0.0.0.0:8000 failed: port is already allocated
ERROR: Encountered errors while bringing up the project.


docker ps -a  # 실행 중인 컨테이너 확인
CONTAINER ID   IMAGE                                COMMAND                   CREATED          STATUS                     PORTS                                     NAMES
b72bca502793   sso_demo_frontend                    "docker-entrypoint.s…"   16 seconds ago   Created                                                              sso_demo_frontend_1
5318529f7d05   sso_demo_backend                     "docker-entrypoint.s…"   16 seconds ago   Exited (1) 6 seconds ago                                             sso_demo_backend_1
b243c5391f85   nginx:latest                         "/docker-entrypoint.…"   5 hours ago      Up 5 hours                 0.0.0.0:8000->80/tcp, [::]:8000->80/tcp   nginx_server            <----- 이전에 실행된 컨테이너
[][ubuntu:172x29x99x41]:sso_demo$

docker stop b243c5391f85  # 특정 컨테이너 종료
docker-compose up -d  # 다시 실행
```

### Keycloak 서버 설정 확인
SSO 시스템을 정상적으로 동작시키기 위해 Keycloak 서버가 실행 중인지 확인하세요. Keycloak URL 및 서비스 URL을 환경에 맞게 조정하세요.
```sh
Managed Keycloak 서버:  https://keycloakbb.demo.genians.co.kr
사용자 애플리케이션:  http://192.168.35.177:8000
Secured 서비스: http://192.168.35.177:3000
```

