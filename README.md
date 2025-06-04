# 🚍 대한민국 전국 대중교통 GTFS 피드 정제 및 오픈소스 연동 프로젝트

---
## 📌 Goal

KTDB(국토교통부 대중교통정보시스템)에서 제공한 공공 데이터를 기반으로,  
대한민국 전국 대중교통 정보를 **GTFS(Google Transit Feed Specification)** 형식으로 정제하고,  
이를 오픈소스 프로젝트인 [`transitous`](https://github.com/public-transport/transitous)와 연동 가능하도록 구성하는 것을 목표로 합니다.

**지하철, 철도, 버스, 페리, 모노레일 등 다양한 교통수단**을 포함하며,  
다음과 같은 **데이터 전처리 및 정제 과정**을 거쳤습니다:

- ✅ **형식 통일**: 시간 형식, 경로 ID, 노선 ID 등의 GTFS 명세에 맞는 필드 변환
- ✅ **오류 제거**: 누락된 열, 잘못된 순서, 비정상적인 속도(travel-too-fast) 경로 제거
- ✅ **중복 제거**: 동일 노선/경로/운행일자에 대한 중복 서비스 제거
- ✅ **유효성 검증**: `gtfsclean` 도구를 활용한 필드 구조, 참조 무결성, 규격 오류 검증
- ✅ **자동화 스크립트 적용**: `fetch.py`를 통한 GTFS fetch/postprocess 자동화


## 📌 Requirements

| | 버전 |
|-------------|------|
| Python      | 3.11 (이미지: `python:3.11-slim`) |
| git         | 2.39.5 |
| wget        | 1.21.3 |
| unzip       | 6.00 |
| gtfsclean   | snapshot-4 (GitHub release 제공 바이너리) |
| requests | 2.32.3 |
| urllib3  | 2.4.0 |
| certifi  | 2025.4.26 |
| charset-normalizer | 3.4.2 |
| idna     | 3.10 |

## 📌 Docker 이미지 설치 방법 (Docker & Git 설치된 환경 기준)

### 1. Docker Hub에서 이미지 다운로드

```bash
docker pull <image_name>:<tag>
```

### 2. tar 파일에서 이미지 불러오기

```bash
docker load -i <image_tar_file_name>.tar
```

### 3. Dockerfile 기반 이미지 빌드

```bash
docker build -t <image_name>:<tag> .
```

---

## 📌 Docker 컨테이너 생성 및 실행

```bash
docker run -it final_2021040026:v1
```

### 컨테이너 안에서 다음 명령을 실행하세요:

```bash
cd /home/사용자명/final_2021040026
python3 src/fetch.py feeds/kr.json
```

정제된 GTFS 파일은 `/out/` 디렉토리에 생성됩니다.

---

## 📌 프로젝트 디렉토리 구조

```
final_2021040026/
├── README.md                  # 설명서
├── Dockerfile                 # 이미지 생성용 도커파일
├── feeds/
│   └── kr.json                # 대한민국 GTFS 피드 정의
├── src/
│   └── fetch.py               # 피드 수집 및 정제 실행 스크립트
├── out/                       # 실행 후 결과 저장 폴더
├── transitland-atlas/        # 피드 메타데이터 서브모듈
└── ...
```

---

## 📌 종료 및 정리 방법

```bash
# 1. 컨테이너 내부에서 종료
Ctrl + D
또는
exit

# 2. 컨테이너 정지
docker stop <CONTAINER_ID>

# 3. 컨테이너 삭제
docker rm <CONTAINER_ID>

# 4. 이미지 삭제
docker rmi final_2021040026:v1
```

---

## 📌 라이선스

- 데이터 출처: [KTDB (https://www.ktdb.go.kr)](https://www.ktdb.go.kr)
- 정제 도구: [gtfsclean](https://github.com/public-transport/gtfsclean)
- 기반 오픈소스: [transitous](https://github.com/public-transport/transitous)
