# FLATTEN — 팬덤 &amp; 브랜드 데이터 미디어 플랫폼 랜딩페이지

**Live: https://hhhodo.github.io/flatten-landing/**

첨부된 Figma 레퍼런스(`get_design_context`, node `11:683`, 캔버스 2544px 기준)를 **그리드·비율·스타일까지
그대로** 구현한 미디어 주제 원페이지 랜딩입니다. 브랜드명(FLATTEN)과 한글 콘텐츠(솔루션 설명, 캠페인 케이스,
뉴스룸 기사)는 레퍼런스를 그대로 따랐고, 사용자 지시에 따라 ①페이지 이동처럼 보이는 화살표·버튼은 전부
제거, ②모든 이미지 슬롯은 실사진 대신 `#d9d9d9` 플레이스홀더로 처리했습니다.

## Step 1 — 레퍼런스 실측 (get_design_context)

| 항목 | 실측값 | 판정 |
|---|---|---|
| 이미지/카드 radius | 비디오·카드·태그 프레임 전반 `5.3px` 반복 | `soft` → `radius-sm`(12px) |
| border | 솔루션 리스트 구분선 `#666 1px`, 푸터 구분선 `#444 1px` | `hairline` |
| 버튼 | 솔리드 버튼 없음, 전부 텍스트 링크 | `button-style=text` |
| 폰트 굵기 | 섹션 타이틀 Pretendard Bold(700), 본문 Pretendard Regular/Medium(400) | `700 / 400` |
| 히어로 타이포 | Helvetica Neue Bold **212px**, tracking `-8.48px`, 3줄 킨네틱 타이포 중간에 비디오 이미지 인라인 삽입 | 디자인 키트 최대 토큰(`--fs-display-lg` 102px)을 초과 — 전용 반응형 토큰 `--fs-kinetic` 신설, 폰트는 실측대로 Helvetica Neue 유지 |
| 섹션 padding | Our Solutions/Campaign Highlights/Newsroom 전부 `212px` | `--space-section`을 `--space-12`(200px)로 오버라이드 |
| 컨테이너 폭 | 본문 컨테이너 1908px / 캔버스 2544px ≈ 75% | `container--wide`(1600px)로 스냅 |

## Variant

첫 줄 코드 주석: `<!-- variant: typo=loud / image=high / color=mono / image-radius=soft / card-radius=soft / button-radius=soft / border=hairline / button-style=text / fw=700/400 / spacing=space-12 -->`

| 축 | 값 | 근거 |
|---|---|---|
| 타이포그래피 태도 | `loud` | 히어로가 화면 대부분을 차지하는 킨네틱 대형 타이포 |
| 이미지 비중 | `high` | 히어로 인라인 이미지 3개 + 캠페인 카드 4개 + 뉴스룸 카드 3개 |
| 컬러 | `mono` | 브랜드 컬러 없이 흑백+그레이스케일만 사용 (Figma 실측에도 액센트 컬러 없음) |

## 레이아웃 — 그리드 값 (요청에 따라 가장 신경 쓴 부분)

```
Header             — full-bleed (topbar + nav)
Hero               — full-bleed — center-stacked kinetic type (비분할, inline 텍스트 흐름)
Our Solutions      — full-bleed, wide container — 4-8 (라벨 4 : 설명 8, 3행 반복)
Campaign Highlights— full-bleed, wide container — 6-6 offset (오른쪽 컬럼만 --space-13만큼 아래로 오프셋)
Newsroom           — full-bleed, wide container — 4-4-4 (균등 3열)
Footer             — full-bleed, wide container
```

- 연속 섹션끼리 동일한 분할을 반복하지 않도록 4-8 → 6-6 → 4-4-4 순서로 구성했습니다.
- Campaign Highlights는 Figma 원본처럼 오른쪽 컬럼 카드가 왼쪽보다 아래에서 시작하는 오프셋 배치를 그대로
  재현했습니다.
- Tablet(≤1024px)은 Campaign Highlights·Our Solutions가 1열로 스택, Newsroom은 2열로 전환됩니다.
- Mobile(≤768px)은 전 섹션 1열입니다.

## 반영한 사용자 지시

1. **화살표/버튼 전부 제거** — 원본에는 "Our Solutions →", "Campaign Highlights →", "Newsroom →",
   상단바 "shoeprize →", 푸터 "Careers →" 등 화살표 링크가 있었으나, 페이지 이동처럼 보이는 요소이므로
   전부 일반 텍스트로 대체했습니다.
2. **이미지 영역 `#d9d9d9`** — 모든 이미지 슬롯(히어로 인라인 이미지 3개, 캠페인 카드 4개, 뉴스룸 카드 3개)은
   실사진 대신 디자인 키트의 `.img` 플레이스홀더(`--color-placeholder:#d9d9d9`)를 사용했습니다.
3. **브랜드명 영문 / 콘텐츠 국문** — 브랜드명 "FLATTEN"과 제품명(shoeprize, FLATTEN ERP, FLATTEN QUEUE)은
   영문 그대로, 솔루션 설명·캠페인 케이스·뉴스룸 기사 등 콘텐츠는 한글 그대로 유지했습니다.

## 알려진 이슈 — 히어로 반응형 폰트 크기

Figma 실측값(212px)은 디자인 키트 최대 토큰을 초과하므로 `clamp()` 기반 전용 토큰(`--fs-kinetic`)을
신설했습니다. 각 줄에서 가장 긴 단어("POWERING", "THROUGH")가 가장 좁은 실기기 뷰포트(약 320~375px)에서도
잘리지 않도록 실측 글자 폭(예: POWERING ≈ 5.274em)을 기준으로 최소값을 계산해 여유를 두었고, 그래도 한
줄이 다 안 들어가는 초소형 화면에서는 인라인 이미지가 자연스럽게 다음 줄로 줄바꿈됩니다(잘림 없음).
