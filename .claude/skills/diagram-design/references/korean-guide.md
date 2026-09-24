# 한국어 빠른 가이드 — 41가지 다이어그램

이 스킬은 [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) (MIT)을 그대로 가져와 이 저장소의 `.claude/skills/diagram-design/`에 설치한 것입니다 (upstream 커밋 `dc1ace4`, v2.6). 이 문서는 한국어 사용자를 위한 **유형 선택표 + 요청 예시**입니다. 실제 그리기 규칙은 `SKILL.md`와 각 `type-*.md`가 기준입니다.

## 사용법

Claude Code에서 자연어로 요청하면 됩니다. 스킬이 유형을 고르고, 계획(유형·크기·빠질 내용)을 먼저 알려준 뒤 단일 HTML 파일(인라인 SVG)로 그립니다.

```
공공심야약국 이용 판단 흐름을 플로차트로 그려줘. 라벨은 한국어로.
제주 지역 약국 수와 인구 대비 밀도를 읍면동별 히트맵으로 그려줘.
```

- 한국어 라벨 규칙: [style-guide.md § Korean labels](style-guide.md#korean-labels) — 한글 최소 12px, 포트·URL 등 기술 보조라벨은 영문 모노 유지.
- 한국어 예시 결과물: [`assets/example-flowchart-ko.html`](../assets/example-flowchart-ko.html)
- 부가 명령: `/import-mermaid`, `/import-drawio`, `/import-excalidraw`, `/export-diagram`, `/profile`, `/doctor`

## 41가지 유형 선택표

| # | 무엇을 보여주나 | 유형 | 요청 예시 (보건의료·지역 정책) | 참조 |
|---|---|---|---|---|
| 1 | 시스템 구성요소와 연결 | 아키텍처 | 의약품안심서비스(DUR) 연계 구조 | [type-architecture](type-architecture.md) |
| 2 | 현재(개편 전) IT 현황 | IT 현황도 | 약국 청구·재고 시스템의 현재 상태 | [type-it-state](type-it-state.md) |
| 3 | 분기가 있는 판단 로직 | 플로차트 | 야간 의약품 이용 경로 판단 | [type-flowchart](type-flowchart.md) |
| 4 | 행위자 간 시간순 메시지 | 시퀀스 | 전자처방전 발행→약국 수신→조제 | [type-sequence](type-sequence.md) |
| 5 | 상태와 전이 | 상태 머신 | 처방전 접수·조제·투약·반려 상태 | [type-state](type-state.md) |
| 6 | 개체·속성·관계 | ER / 데이터 모델 | 환자–처방–약품–약국 관계 | [type-er](type-er.md) |
| 7 | 시간축 위 사건 | 타임라인 | 의약분업 이후 주요 제도 변화 | [type-timeline](type-timeline.md) |
| 8 | 부서 간 인계가 있는 절차 | 스윔레인 | 방문약료: 약사·간호사·보건소 역할 | [type-swimlane](type-swimlane.md) |
| 9 | 2축 포지셔닝·우선순위 | 사분면 | 정책 과제의 시급성 × 실행 용이성 | [type-quadrant](type-quadrant.md) |
| 10 | 3–5개 지표로 여러 대상 비교 | 레이더 / 스파이더 | 시·군별 의료 접근성 지표 비교 | [type-radar](type-radar.md) |
| 11 | 순환 범주별 한 가지 수치 | 폴라 차트 | 월별 감기약 판매량 | [type-polar](type-polar.md) |
| 12 | 강화 순환(플라이휠) | 루프 | 복약상담→순응도↑→신뢰↑→재방문 | [type-loop](type-loop.md) |
| 13 | 포함 관계로 본 계층 | 중첩도 | 국가–광역–기초–읍면동 보건 거버넌스 | [type-nested](type-nested.md) |
| 14 | 부모→자식 관계 | 트리 | 의약품 분류 체계(ATC) | [type-tree](type-tree.md) |
| 15 | 보고·책임·에스컬레이션 | 조직도 | 지역 약사회 조직 | [type-org-chart](type-org-chart.md) |
| 16 | 쌓인 추상화 계층 | 레이어 스택 | 1차–2차–3차 의료전달체계 | [type-layers](type-layers.md) |
| 17 | 집합의 겹침 | 벤 다이어그램 | 고령·만성질환·다제약물 복용 인구 | [type-venn](type-venn.md) |
| 18 | 서열 계층 또는 전환 이탈 | 피라미드 / 퍼널 | 건강검진 대상→수검→사후관리 | [type-pyramid](type-pyramid.md) |
| 19 | 범주별 수치 비교 | 막대 차트 | 읍면별 약국 수 | [type-bar](type-bar.md) |
| 20 | 시작값→증감→최종값 | 워터폴 | 약국 연간 손익 구성 | [type-waterfall](type-waterfall.md) |
| 21 | 전체 대비 부분 크기 | 트리맵 | 건강보험 약제비 구성 | [type-treemap](type-treemap.md) |
| 22 | 교차표 값 | 히트맵 | 지역 × 연령대 의료 이용률 | [type-heatmap](type-heatmap.md) |
| 23 | 시간 추세·순위 변화 | 선 차트 (슬로프·범프·릿지라인 포함) | 제주 고령화율 추이 | [type-line](type-line.md) |
| 24 | 일정 위 작업과 단계 | 간트 | 시범사업 추진 일정 | [type-gantt](type-gantt.md) |
| 25 | 두 변수의 상관·분포 | 산점도 (버블·비스웜 포함) | 인구 밀도 × 약국 밀도 | [type-scatter](type-scatter.md) |
| 26 | 컨테이너 기반 전체 데이터 스택 | 하이레벨 | 지역 보건 데이터 플랫폼 개요 | [type-high-level](type-high-level.md) |
| 27 | 다주체 순차 절차와 데이터 인계 | 프로세스 | 연구 설계→수집→분석→보고 | [type-process](type-process.md) |
| 28 | 품질 단계별 데이터 저장 | 메달리온 | 원자료→정제→분석용 청구자료 | [type-medallion](type-medallion.md) |
| 29 | 역할별 데이터 흐름 | 데이터 플로 | 누가 어떤 데이터를 처리하는가 | [type-data-flow](type-data-flow.md) |
| 30 | 데이터 플랫폼 연계 토폴로지 | DP 연계도 | 심평원·건보·지자체 자료 연계 | [type-dp-integration](type-dp-integration.md) |
| 31 | 역할 × 구성요소 권한 | DP 보안 매트릭스 | 연구자·공무원 데이터 접근 권한 | [type-dp-security-matrix](type-dp-security-matrix.md) |
| 32 | 양의 분기·합류 (폭 = 양) | 생키 | 환자 유입→의료기관→약국 흐름 | [type-sankey](type-sankey.md) |
| 33 | 한 결과의 원인 범주 | 피시본 | 복약 불순응의 원인 분석 | [type-fishbone](type-fishbone.md) |
| 34 | 가치사슬 × 진화 단계 | 워들리 맵 | 약국 서비스 중 직접 할 것 / 외부화할 것 | [type-wardley](type-wardley.md) |
| 35 | 상태별 진행 중 업무 | 칸반 | 정책 제안 과제 진행판 | [type-kanban](type-kanban.md) |
| 36 | 단계별 행동과 감정 | 사용자 여정 | 관광객의 제주 약국 이용 경험 | [type-journey](type-journey.md) |
| 37 | 소프트웨어 배포 위치 | 배포도 | 약국 앱의 서버·DB 배치 | [type-deployment](type-deployment.md) |
| 38 | 의존 관계 (팬인·순환 포함) | 의존성 그래프 | 정책 과제 간 선후 관계 | [type-dependency](type-dependency.md) |
| 39 | 클래스·상속·합성 | UML 클래스 | 처방·조제 도메인 모델 | [type-uml-class](type-uml-class.md) |
| 40 | 서사 흐름을 릴리스로 분할 | 스토리 맵 | 약국 예약 서비스 기능 로드맵 | [type-story-map](type-story-map.md) |
| 41 | 물리 테이블·제약·인덱스 | DB 스키마 | 조제 기록 테이블 설계 | [type-db-schema](type-db-schema.md) |

## 기억할 원칙 (SKILL.md 요약)

- **빼는 것이 최고의 디자인.** 노드 9개 초과면 개요 + 상세 두 장으로 나눈다.
- **강조색(주황)은 1–2개에만.** 독자가 가장 먼저 볼 곳에만 쓴다.
- **표나 문단으로 충분하면 그리지 않는다.**
- 처음 쓰는 프로젝트라면 스타일(브랜드 색·글꼴)을 먼저 묻는다 — 기본값 유지도 선택지.
