# cardterminal

카드단말기 — 전국 카드단말기 판매·설치 안내 (Cloudflare Workers).

- 워커: `cardterminal_worker.js` (wrangler name `cardterminal`)
- 배포: main 푸시 → GitHub Actions (`wrangler deploy --no-bundle`)
- 사이트명·도메인 상수: `cardterminal_worker.js` 상단 `SITE_NAME` / `SITE` (도메인 확정 전 `NEWDOMAIN.com`)
- 통계 D1: 공용 `allcare_stats`, `events.site = cardterminal`
- `_design/`: 디자인·문구 확정본과 구축 프롬프트 (배포에 포함되지 않음)
