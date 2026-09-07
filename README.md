# primeposkorea

프라임포스 — 전국 카드단말기 판매·설치 안내 (Cloudflare Workers).

- 워커: `primeposkorea_worker.js` (wrangler name `primeposkorea`)
- 배포: main 푸시 → GitHub Actions (`wrangler deploy --no-bundle`)
- 사이트명·도메인 상수: `primeposkorea_worker.js` 상단 `SITE_NAME` / `SITE`
- 통계 D1: 공용 `allcare_stats`, `events.site = primeposkorea`
- 사진: `image/` 100장을 jsDelivr(`gh/dandylsk80/primeposkorea@main/image/`)로 서빙
- `_design/`: 디자인·문구 확정본과 구축 프롬프트 (배포에 포함되지 않음)
