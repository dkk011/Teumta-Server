# 틈타 서버 (Teumta-Server)

퇴근길 수도권 전철 1호선, 역과 장소마다 오늘 들를 수 있는 시간을 알려 주는 Android 앱 **틈타**의 서버예요.

- 앱: [Teumta-Android](https://github.com/dkk011/Teumta-Android)
- 웹 페이지 (GitHub Pages, `docs/`): [홈](https://dkk011.github.io/Teumta-Server/) · [개인정보처리방침](https://dkk011.github.io/Teumta-Server/privacy.html) · [서비스 이용약관](https://dkk011.github.io/Teumta-Server/terms.html) · [계정 삭제 요청](https://dkk011.github.io/Teumta-Server/account-deletion.html)

## 하는 일
- 인증: 이메일 가입·로그인, Google ID 토큰 검증, JWT 발급·회전
- 시간표: 국토교통부 TAGO 지하철정보로 1호선 102개 역 시간표를 매일 받아 열차 단위로 이어 붙이고, CSA(Connection Scan Algorithm)로 역별 출발 마감·도착 시각 계산
- 장소 경유: 카카오 로컬 API 장소 검색, TMAP 보행자 경로로 도보 시간 계산(캐시)

## 기술
Java 21, Spring Boot, Spring Security + JWT, Spring Data JPA, PostgreSQL, springdoc-openapi, Docker

## 설정
API 키와 비밀번호는 저장소에 올리지 않아요. 환경 변수로 넣어요.

| 변수 | 내용 |
| --- | --- |
| `TAGO_SERVICE_KEY` | 공공데이터포털 인증키 (TAGO 지하철정보, 특일 정보) |
| `KAKAO_REST_API_KEY` | 카카오 로컬 API |
| `TMAP_APP_KEY` | TMAP 보행자 경로안내 |
| `GOOGLE_WEB_CLIENT_ID` | Google ID 토큰 aud 검증 |
| `MAIL_USERNAME`, `MAIL_PASSWORD` | 비밀번호 재설정 메일 (Gmail 앱 비밀번호) |
| `JWT_SECRET` | 토큰 서명 키 |
| `DATABASE_URL` | PostgreSQL 접속 주소 |
