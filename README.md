<p align="center">
    <img width="200px;" src="https://raw.githubusercontent.com/woowacourse/atdd-subway-admin-frontend/master/images/main_logo.png"/>
</p>
<p align="center">
  <img alt="npm" src="https://img.shields.io/badge/npm-%3E%3D%205.5.0-blue">
  <img alt="node" src="https://img.shields.io/badge/node-%3E%3D%209.3.0-blue">
  <a href="https://edu.nextstep.camp/c/R89PYi5H" alt="nextstep atdd">
    <img alt="Website" src="https://img.shields.io/website?url=https%3A%2F%2Fedu.nextstep.camp%2Fc%2FR89PYi5H">
  </a>
  <img alt="GitHub" src="https://img.shields.io/github/license/next-step/atdd-subway-service">
</p>

<br>

# 인프라공방 샘플 서비스 - 지하철 노선도

<br>

## 🚀 Getting Started

### Install
#### npm 설치
```
cd frontend
npm install
```
> `frontend` 디렉토리에서 수행해야 합니다.

### Usage
#### webpack server 구동
```
npm run dev
```
#### application 구동
```
./gradlew clean build
```
<br>

## 미션

* 미션 진행 후에 아래 질문의 답을 작성하여 PR을 보내주세요.

### 1단계 - 인프라 운영하기
1. 각 서버내 로깅 경로를 알려주세요
   - syslog: `/var/log/syslog`
   - nginx-access: `/var/log/nginx/access.log`
   - nginx-error: `/var/log/nginx/error.log`
   - app-log: `/home/ubuntu/log/file.log`

2. Cloudwatch 대시보드 URL을 알려주세요
   - https://ap-northeast-2.console.aws.amazon.com/cloudwatch/home?region=ap-northeast-2#dashboards:name=DASHBOARD_wrallee

---

### 2단계 - 성능 테스트
1. 웹 성능예산은 어느정도가 적당하다고 생각하시나요
   - 주요 페이지 정량/시간/규칙 기반 산정
   - 예산은 경쟁사 대비 최대 120% 전후 성능으로 예산을 산정합니다.
   - 시간 기반 규칙의 경우 1초 이내의 항목에 대해선 120%를 초과해도 허용합니다.
   - 각 항목에 경쟁 사이트 점수 괄호로 표기(⇔ `subwayLine.naver`)
     - Quantity based Metric
       - 리소스 합계 최대 `1MB`(⇔ `773KB`)
     - Timing based Metric
       - FCP `1.0s`(⇔ `0.641s`)
       - TTI `1.2s`(⇔ `1.003s`)
       - LCP `3s`(⇔ `2.885s`)
     - Rule based Metric
       - Lighthouse `80점` 이상(⇔ `89점`)

2. 웹 성능예산을 바탕으로 현재 지하철 노선도 서비스는 어떤 부분을 개선하면 좋을까요
   - 응답에 gzip 압축 적용
      - 응답을 압축하여 네트워크 전송 시간을 줄입니다.
   - 자바스크립트 실행 시간 단축
      - vendors.js 코드를 분할하여 파싱, 컴파일, 실행 소요시간을 줄입니다.
      - 분할 된 js 중 필수적이지 않은 코드를 지연로딩합니다.
   - 캐시 컨트롤 적용
      - 캐시 정책을 적용하여 재방문에 대한 속도를 높입니다.

3. 부하테스트 전제조건은 어느정도로 설정하셨나요
   - 예상 서비스 규모 ⇔ (네이버 지도 MAU `1,112만`)
      - 내 서비스 DAU `200,000`
      - 피크시간대 집중율 `10`
      - 1일 요청 수 `5`
      - 1일 총 접속 수 `1,000,000`
      - 1일 평균 rps `11.57`
      - 1일 최대 rps `57.87`
      - Latency `100ms`
   - 대상 메뉴
      - `메인 페이지`
      - `로그인 페이지`
      - `경로 검색 페이지`
      - `내 정보 수정 페이지`
   - 테스트 방식
      - 각 테스트 당 일정 시간 부하 유지
         - smoke `10초`
         - load `1분`
         - stress `5분`
      - stress 조건 `20분` 유지 후 복구되는지 확인
      - 내 정보 수정 페이지에서 `1,000,000` 건 업데이트 수행
   
4. Smoke, Load, Stress 테스트 스크립트와 결과를 공유해주세요
