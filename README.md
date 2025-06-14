# 🚍 한국 대중교통 GTFS 피드 정제 및 오픈소스 연동 프로젝트

---
## 📌 Goal

- 기존 프로젝트에는 한국(KR) 관련 GTFS 정보가 없었기 때문에,
국내 데이터를 직접 수집·정제하고 새로운 kr.json 지역 정의 파일을 생성하여 등록했습니다.

- KTDB(국토교통부 대중교통정보시스템)에서 제공한 **GTFS(Google Transit Feed Specification)** 형식 공공 데이터를 정제하고
이를 오픈소스 프로젝트인 [`transitous`](https://github.com/public-transport/transitous)와 연동 가능하도록 구성하는 것이 목표입니다.

**지하철, 철도, 버스, 페리, 모노레일 등 다양한 교통수단**을 포함하며,  
다음과 같은 **데이터 전처리 및 정제 과정**을 거쳤습니다:


- ✅ **형식 통일**: 시간 형식(타임존 설정), 경로 ID, 노선 ID 등의 GTFS 명세에 맞는 필드 변환
- ✅ **오류 제거**: 누락된 열, 잘못된 순서 제거
- ✅ **유효성 검증**: `gtfsclean` 도구를 활용한 필드 구조, 참조 무결성, 규격 오류 검증
- ✅ **자동화 스크립트 적용**: `fetch.py`를 통한 GTFS fetch/postprocess 자동화


수정하여 생성된 GTFS .zip 파일은 개인 Dropbox를 통해 공유하였으며, 직접적인 URL 다운로드를 통한 fetch.py 스크립트에서의 검증 테스트를 완료했습니다.
['한국 GTFS'](https://www.dropbox.com/scl/fi/l1rnl88xnegpmiuy44kmc/GTFS_DataSet.zip?rlkey=jub5anlfpsdoi9mgfy4qp7x9w&dl=1)


**GTFS 파일 형식(e.g. stop.txt)**


![image](https://github.com/user-attachments/assets/7226036d-f4fe-44af-b5ff-78e874035132)


## 📌 Requirements

| | 버전 |
|-------------|------|
| Python      | 3.11 |
| git         | 2.39.5 |
| wget        | 1.21.3 |
| unzip       | 6.00 |
| gtfsclean   | snapshot-4 |
| requests | 2.32.3 |
| urllib3  | 2.4.0 |
| certifi  | 2025.4.26 |
| charset-normalizer | 3.4.2 |
| idna     | 3.10 |

## 📌 Docker 이미지 설치 방법

### Docker Hub에서 이미지 다운로드
이미지 다운로드 및 태그 형식 변경
```bash
docker pull rapa0142/final_2021040026:v1
docker tag rapa0142/final_2021040026:v1 final_2021040026:v1
```
---

## 📌 Docker 컨테이너 생성 및 실행

```bash
docker run -it final_2021040026:v1
```

### 명령 실행:

```bash
python3 src/fetch.py feeds/kr.json
```
![화면 캡처 2025-06-05 090649](https://github.com/user-attachments/assets/42112298-085f-4658-9f6b-5a5af32fffe5)

kr.json에 대해 `gtfsclean` 도구를 활용한 필드 구조, 참조 무결성, 규격 오류 검증이 완료된 것을 알 수 있습니다.

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
