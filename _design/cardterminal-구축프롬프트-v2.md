# 카드단말기(cardterminal) 신규 사이트 구축 프롬프트 v2

> 사이트키: **cardterminal** (디렉터리·wrangler name·워커명·저장소·D1 site 값 공통)
> 임시 사이트명: **카드단말기** / 도메인: **미정 → `NEWDOMAIN.com` 자리표시**
> 기준 코드: thecardpos 저장소 복사
> 전화·문자: 010-9876-8282

## 함께 넘길 파일 3개
| 파일 | 용도 |
|---|---|
| `cardterminal-main-v1.html` | 메인페이지 디자인·문구 확정본. 이 모양 그대로 워커에 옮길 것 |
| `cardterminal-dong-sample.html` | 읍면동 페이지 레이아웃·CSS 확정본 (본문은 content_pool.js 출력) |
| `content_pool.js` | 지역 페이지 문단 풀 48개 + renderPool() 함수 |

## 절대 규칙 (매 단계 적용)
- `/api/contact`·GAS 코드 수정 금지
- 전환 추적(tel/sms/contact → /api/track) 템플릿은 thecardpos 것 그대로, 사이트명·URL→한글 변환만 교체
- gzip 500KB 이하, `check.mjs` 통과, `node --check` 통과
- npx wrangler 금지 → `wr` 래퍼
- wrangler.toml에 routes 배열 넣지 말 것 (도메인은 대시보드에서 수동)
- 시킨 것만 수정, 결과 보고는 10줄 이내
- 세 파일에 없는 문구를 임의로 지어내지 말 것. 특히 **가격, 택배·배송·수령 시점, 업종별 장비 추천** 문구는 어디에도 넣지 말 것

---

## 0단계. 저장소 복사·이름 교체
```
thecardpos 저장소를 복사해 cardterminal을 만든다.
- 디렉터리 ~/sites/cardterminal, wrangler name "cardterminal", 메인파일 cardterminal_worker.js, 저장소 dandylsk80/cardterminal (퍼블릭)
- 사이트명 "카드단말기", 도메인 NEWDOMAIN.com — 둘 다 파일 상단 상수 한 곳에 모을 것
- D1 바인딩 공용 allcare_stats 그대로, events.site 값 'cardterminal'
- 텔레그램 알림 사이트명·URL→한글 변환만 교체. /api/contact·GAS 절대 손대지 말 것
- 지역 페이지 4,677개 구조 유지
- wrangler.toml [triggers] 크론 확인, routes 배열 없음 확인
- check.mjs 통과 후 바뀐 파일·줄만 보고
```

## 1단계. 메인페이지 교체
```
cardterminal-main-v1.html을 메인 라우트(/)에 그대로 옮긴다.
- HTML 구조·CSS·문구를 바꾸지 말고 옮길 것. 색상(딥그린 #1E5A3C / 노랑 #E9A62A)·폰트(Pretendard) 유지
- 지역 버튼 18개는 /region/{시도} 실제 링크로 연결
- 전화·문자 링크(tel:/sms:)에 기존 전환 추적 속성/핸들러를 thecardpos와 동일하게 붙일 것 — 추적 코드 자체는 수정 금지
- 헤더·푸터·플로팅 버튼도 이 파일 것으로 교체
- 기존 thecardpos 메인의 3D 히어로·레이더 지도·타임라인·와인색은 전부 제거
- 완료 후 로컬 렌더 캡처와 원본 html 비교해 다른 점 보고
```

## 2단계. 지역 페이지 본문 교체
```
content_pool.js를 워커 파일에 인라인으로 넣는다 (const 아닌 var 유지).
- 읍면동 페이지 본문: renderPool(slug, {sido,gugun,dong,tel}, {skip:['S12']})
- 인근지역 섹션 도입 문장: renderNearIntro(slug, v) + 기존 인근 동 링크 목록
- FAQPage JSON-LD: poolFaqList(slug, v) 결과로 생성
- 출력 HTML은 기존 fixJosa()를 통과시킬 것
- 페이지 CSS·상단(빵부스러기·h1·발행일·썸네일)·CTA 박스 위치는 cardterminal-dong-sample.html과 동일하게. CTA는 "매출 관리" 섹션 뒤, "사용 중 문제 대응" 앞
- 시도·구군 페이지는 thecardpos 구조 유지하되 색상·헤더·푸터·플로팅만 새 디자인 적용
- 완료 후 임의 동 3곳(서울·경기·지방 각 1) 렌더해서 글자 수와 선택된 변형 id 보고 (목표 2,500자 이상, 세 페이지 변형 조합이 서로 달라야 함)
```

## 3단계. 사진·부가 파일
```
- Pexels 실사진 100장 새로 선정 (기존 5개 결제 사이트와 겹치지 않게), jsDelivr 경로 ASCII 폴더명 (image/)
- 썸네일 회전 로직은 thecardpos 것 유지
- sitemap.xml(urlset, sitemapIndex 중첩 금지) / rss.xml / robots.txt(AI봇 허용) / llms.txt 도메인·사이트명 교체
- 구조화 데이터(WebPage·BreadcrumbList·FAQPage·Organization·Service) 사이트명 교체, 가격 필드 있으면 삭제
- 로고: 메인 html의 .logo 스타일(초록 사각형+노랑 띠) 기준으로 SVG·PNG·ICO 파비콘 제작·내장
```

## 4단계. 유사도 검사
```
thecardpos 임의 동 페이지 10개와 cardterminal 같은 동 페이지 10개를 텍스트로 뽑아 유사도 측정.
- 메인끼리도 1회 측정
- 결과 수치 보고 (목표 30% 미만). 넘으면 어느 섹션이 겹치는지만 보고하고 임의 수정하지 말 것
```

## 5단계. 배포·라이브 검증
```
- wr 래퍼로 배포 → cardterminal.{계정}.workers.dev 주소 확인
- 라이브 메인·동 페이지에서 전화·문자 버튼 클릭 → 대시보드(allcarestudy.com/dashboard) D1 기록 + 텔레그램 도착 실제 확인
- /sitemap-regions Cloudflare CPU 오류 없는지 확인
- 모바일 폭(390px)에서 하단 CTA 바·3칸 도형·흐름도 깨짐 없는지 캡처 확인
- git push
```

---

## 도메인 확정 후
- NEWDOMAIN.com 상수 교체 → 재배포
- Workers 대시보드에서 도메인 수동 연결
- 구글 서치콘솔(도메인 속성)·네이버 서치어드바이저·다음·빙 등록, 사이트맵·RSS 제출
- 대시보드 SITE_LIST 더세이브 그룹에 cardterminal 추가 (전환 추적 영향 → 1개 사이트 먼저 확인 규칙 적용)
