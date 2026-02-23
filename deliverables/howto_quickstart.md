# Claude Code + GitHub 질적 연구 퀵스타트 가이드

**"30분 안에 첫 분석 시작하기"**

> 이 문서는 [전체 가이드(howto_guide.md)](howto_guide.md)의 핵심만 추린 빠른 시작 안내서입니다.
> Git/GitHub이 처음이라면 먼저 [연구자를 위한 GitHub 입문 가이드(howto_github_for_researchers.md)](howto_github_for_researchers.md)를 읽어주세요.

---

## 전체 흐름 한눈에 보기

```
Step 0  준비물 확인                          ⏱ 10분
  │
  ▼
Step 1  데이터 변환                          ⏱ 30-60분
        Google Docs → Markdown, Excel → CSV
  │
  ▼
Step 2  GitHub 리포 생성 & 데이터 업로드      ⏱ 15분
  │
  ▼
Step 3  코드북 설계 (Gemini 활용)             ⏱ 2-4시간
  │
  ▼
Step 4  Claude Code 웹 버전으로 분석 실행     ⏱ 수시간-수일
  │
  ▼
Step 5  검증 및 산출물 정리                   ⏱ 1-2일
```

---

## Step 0: 준비물 확인

| 준비물 | 용도 | 비고 |
|--------|------|------|
| **GitHub 계정** | 리포지토리 호스팅, PR 관리 | [github.com](https://github.com) |
| **Anthropic 계정** | Claude Code 웹 버전 사용 | [claude.ai](https://claude.ai) |
| **Gemini 접근** | 코드북 초안 설계 (긴 컨텍스트) | [gemini.google.com](https://gemini.google.com) |
| **인터뷰 전사 데이터** | 분석 원본 자료 | Google Docs/PDF/Excel 등 |

**CLI(Command-Line Interface) vs 웹 버전**: 이 가이드는 **Claude Code 웹 버전**(claude.ai에서 GitHub 리포를 연동하는 방식)을 기준으로 합니다. 터미널에 익숙하다면 CLI 버전도 동일한 워크플로우로 사용할 수 있습니다.

---

## Step 1: 데이터 변환

### 1-1. 왜 변환이 필요한가

Claude Code는 **Markdown(.md)**과 **CSV(.csv)** 형식의 텍스트를 가장 잘 처리합니다. Google Docs, HWP, PDF, Excel 등의 원본 포맷은 LLM이 직접 읽기 어렵거나 실패할 수 있으므로 사전 변환이 필수입니다.

### 1-2. Google Docs → Markdown

**방법 A: 직접 복사** (소규모 문서)
1. Google Docs에서 전체 선택 (Ctrl+A) → 복사
2. VS Code 등 텍스트 에디터에 붙여넣기
3. `.md` 확장자로 저장
4. 제목은 `#`, 소제목은 `##`, 강조는 `**굵은글씨**`로 정리

**방법 B: 다운로드** (대규모 문서, 권장)
1. Google Docs 메뉴 → 파일 → 다운로드 → **Markdown(.md)** 선택
2. 다운로드된 `.md` 파일을 에디터에서 열어 서식 확인
3. 기관별 구분자(`## KAERI`, `## ETRI` 등)가 명확한지 확인

> **팁**: 수백 페이지 문서는 기관별로 분리된 별도 `.md` 파일로 저장하면 이후 분석이 수월합니다.

### 1-3. Excel → CSV

1. Excel에서 파일 열기
2. 파일 → 다른 이름으로 저장 → **CSV UTF-8 (쉼표로 분리)** 선택
3. **UTF-8 인코딩을 반드시 확인** (한글 깨짐 방지)

CSV 구조 예시 (본 프로젝트 기준):
```csv
기관,대분류 테마,중분류 테마,해당 문장 발췌
KAERI,1. 국제협력의 동인 및 목표,기술 확보 및 R&D 역량 강화,"차세대 원자로 개발하는 데 있어서 협력할 수 있는 분야가 되게 많았다고 치면"
ETRI,3. 기관별 특성 및 협력 환경,기관 임무 기반 협력,"ICT 분야는 기술 변화가 너무 빠르기 때문에..."
```

### 1-4. PDF 처리 주의사항과 대안

> **핵심 경고**: 수백 페이지 PDF를 Claude Code에 직접 읽히면 처리 실패(세션 중단, 응답 없음)가 발생할 수 있습니다.

| 전략 | 방법 | 적합 상황 |
|------|------|-----------|
| **사전 변환** (권장) | PDF → Markdown으로 변환 후 업로드 | 모든 경우 |
| **청크 분할** | PDF를 50-100페이지 단위로 분할 | 변환이 어려운 경우 |
| **페이지 지정** | "p.1-50만 읽어줘" 식으로 범위 지정 | 특정 부분만 필요한 경우 |
| **Gemini 활용** | 긴 PDF는 Gemini에 먼저 투입하여 요약/변환 | 1,000페이지 이상 대용량 |

PDF → Markdown 변환 도구: [Marker](https://github.com/VikParuchuri/marker), [Docling](https://github.com/DS4SD/docling) 등 오픈소스 활용

---

## Step 2: GitHub 리포 생성 및 데이터 업로드

### 2-1. GitHub 웹에서 Private 리포 만들기

1. [github.com](https://github.com) → 우측 상단 `+` → **New repository**
2. Repository name: `my-qualitative-research` (예시)
3. **Private** 선택 (인터뷰 데이터 보호 필수)
4. Add a README file 체크 → **Create repository**

### 2-2. 디렉토리 구조

```
my-qualitative-research/
├── CLAUDE.md            ← Claude Code 프로젝트 지침서 (핵심!)
├── codebook.md          ← 코드북 (Step 3에서 작성)
├── .gitignore           ← Git 추적 제외 파일
├── raw/                 ← 원시 데이터 (수정 금지)
│   ├── interview_01.md
│   ├── Policy_merged.csv
│   └── ...
├── qualitative/         ← 분석 결과
│   └── all_coding.md
└── deliverables/        ← 최종 산출물
    ├── final_report.md
    └── viz_*.html
```

### 2-3. 파일 업로드

1. 리포 페이지에서 **Add file → Upload files**
2. `raw/` 폴더에 넣을 파일들을 드래그앤드롭
3. 커밋 메시지 작성: `"인터뷰 전사 데이터 업로드 (15개 기관)"`
4. **Commit changes** 클릭

> **주의**: GitHub 웹 업로드는 파일당 25MB, 한 번에 100개 파일까지 가능합니다. 초과하는 경우 Git CLI를 사용하세요.

### 2-4. CLAUDE.md 만들기

CLAUDE.md는 Claude Code가 세션 시작 시 **자동으로 읽는 프로젝트 설명 파일**입니다. 이것이 없으면 매번 맥락을 재설명해야 합니다.

**템플릿** (리포 루트에 `CLAUDE.md` 파일로 생성):

```markdown
# 프로젝트: [연구 제목]

## 연구 개요
- 연구 기관: [소속]
- 연구 주제: [주제]
- 데이터 규모: [인터뷰 수], [전사 분량]

## 디렉토리 구조
- raw/: 인터뷰 전사 원본 (수정 금지)
- qualitative/: 코드북 기반 코딩 결과
- deliverables/: 최종 보고서, 시각화
- codebook.md: 분류 기준 정의서

## 핵심 파일
- codebook.md: 주제 코드 + 맥락 코드 정의
- raw/Policy_merged.csv: 구조화 코딩 결과

## 분석 지침
- 모든 코딩은 codebook.md를 따를 것
- 원문을 그대로 인용, 출처(기관명) 명기
- 분석 결과는 마크다운 테이블 형식으로 출력
- raw/ 폴더의 파일은 수정하지 말 것
- 한국어로 분석하되, 학술 용어는 영문 병기

## 언어
- 보고서: 한국어
- 코드명: 영문 (예: OBS_FUND_INSTABILITY)
```

### 2-5. .gitignore 만들기

리포 루트에 `.gitignore` 파일을 생성합니다:

```
.env
*.key
.DS_Store
Thumbs.db
node_modules/
*.tmp
*.bak
```

---

## Step 3: 코드북 설계

### 3-1. 코드북이란?

코드북(Codebook)은 인터뷰 발언을 **어떤 기준으로 분류할지** 정의한 문서입니다.

| | 코드북 없이 | 코드북 있으면 |
|---|---|---|
| 분류 기준 | LLM이 매번 다르게 판단 | 일관된 기준 적용 |
| 재현성 | 같은 데이터도 다른 결과 | 누가 해도 유사한 결과 |
| 품질 | 코드 입도(granularity) 불균일 | 체계적 위계 구조 |

### 3-2. Gemini로 코드북 초안 잡기

**왜 Gemini인가**: 코드북 설계 단계에서는 전체 인터뷰 전사본(수백 페이지)을 한 번에 읽고 패턴을 파악해야 합니다. Gemini는 100만+ 토큰의 긴 컨텍스트를 지원하므로, 전체 데이터를 투입하여 초안을 잡기에 적합합니다.

**프롬프트 예시**:

```
다음은 15개 정부출연연구기관 국제협력 담당자 심층 인터뷰 전사본입니다.

이 데이터를 질적 연구(Thematic Analysis) 관점에서 분석하여
코드북 초안을 작성해주세요.

## 요청 사항
1. 대분류 5-8개, 각 대분류 아래 중분류 3-10개를 도출해주세요
2. 각 코드에 대해:
   - 영문 코드명 (대문자_언더스코어 형식, 예: OBS_FUND_INSTABILITY)
   - 한국어 설명 (괄호 안)
   - 정의 (1-2문장)
   - 해당 발언 예시 (원문에서 1개)
3. 맥락/뉘앙스 코드 10개도 별도로 제안해주세요
   (예: PROBLEM_STATED, SOLUTION_PROPOSED, OPINION_EXPRESSED 등)

## 코드 부여 규칙
- 각 발언에 주제 코드 1개 이상 + 맥락 코드 1개
- 코드명은 영문, 설명은 한국어 병기

[여기에 전사본 전문 붙여넣기]
```

### 3-3. 코드북 구조 템플릿

Gemini 결과를 정리하여 `codebook.md`로 저장합니다:

```markdown
# [연구명] 코드북 v1.0

## 1. 주제 코드 (Thematic Codes)

### 1.1. [대분류명] (CATEGORY_NAME)
* `CODE_NAME`: (한국어 설명) 정의 1-2문장.

### 1.2. 장애물 및 도전과제 (OBSTACLES_CHALLENGES)
* `OBS_FUND_INSTABILITY`: (예산 불안정성) 정부 정책 변화에 따른
  갑작스러운 예산 삭감 또는 예측 불가능성.
* `OBS_POL_FRAGMENTATION_LACK_COORDINATION`: (부처 간 칸막이)
  과기부, 산업부, 외교부 등 부처 간 협력/조정 미흡.

## 2. 맥락 코드 (Context Codes)
*(각 발언에 아래 중 하나를 부여)*
* `PROBLEM_STATED`: (문제점 제기)
* `CAUSE_ANALYSIS`: (원인 분석)
* `SOLUTION_PROPOSED`: (해결책 제안)
* `FACTUAL_REPORTING`: (사실/현황 전달)
* `OPINION_EXPRESSED`: (의견/평가 제시)
...
```

### 3-4. 파일럿 코딩으로 검증

코드북 초안을 만들었으면, 2-3개 기관 인터뷰에 시험 적용합니다:

```
codebook.md를 참조하여 raw/interview_KAERI.md의 처음 50개 발언을
코딩해주세요. 적절한 코드가 없는 발언은 [FLAG: 코드 부족]으로 표시하세요.
```

FLAG가 많으면 코드북을 보완하고, 유사 코드 간 혼동이 잦으면 정의를 명확히 합니다.

---

## Step 4: Claude Code 웹 버전으로 분석 실행

### 4-1. 웹 버전 연동 방법

1. [claude.ai](https://claude.ai) 접속 → 로그인
2. 새 프로젝트(Project) 생성
3. **GitHub 리포 연동**: 프로젝트 설정에서 GitHub 리포지토리를 연결
4. Claude Code가 리포의 파일 구조와 `CLAUDE.md`를 자동으로 인식

### 4-2. 코딩 프롬프트 예시

**탐색적 분석** (처음 시작할 때):

```
raw/interview_KAERI.md를 읽고, 이 인터뷰에서 나타나는
주요 테마 5가지를 도출해주세요. 각 테마에 대해 대표적인
원문 인용 1-2개를 함께 제시해주세요.
```

**코드북 기반 전체 코딩**:

```
codebook.md를 참조하여 raw/interview_KAERI.md 전체를 코딩해주세요.

출력 형식:
| # | 원문 발췌 | 주제 코드 | 맥락 코드 | 비고 |

코딩 기준:
1. 의미 단위(1-3문장)로 분절하여 각각 코딩
2. 주제 코드 1개 이상 + 맥락 코드 1개 부여
3. 원문은 핵심 의미가 보존되도록 발췌
4. 판단이 모호하면 비고에 근거 기록

결과를 qualitative/coding_KAERI.md로 저장해주세요.
```

**종합 분석**:

```
qualitative/ 폴더의 전체 코딩 결과와 raw/Policy_merged.csv를 분석하여:

1. 장애요인 코드별 빈도를 내림차순으로 정리
2. 15개 기관 중 12개 이상에서 공통으로 나타나는 보편적 패턴 식별
3. 특정 기관에서만 두드러지는 특수 패턴 식별
4. 3대 핵심 발견을 대표 인용문과 함께 서술

결과를 qualitative/comprehensive_analysis.md로 저장해주세요.
```

### 4-3. 대용량 파일 에러 방지 전략

| 문제 | 해결 방법 |
|------|-----------|
| 파일이 너무 커서 읽기 실패 | **기관별로 파일 분할** (1개 기관 = 1개 파일, 500-2,000줄 권장) |
| 분석 도중 세션 끊김 | **기관별로 나눠서 지시**, 결과를 매번 파일로 저장 |
| 긴 보고서 생성 중 중단 | **파트별 분할 작성** ("제1장만 먼저 작성해줘") |
| CSV 크기 초과 | **사전에 파일 크기 확인**: "raw/ 폴더의 각 파일 크기를 알려줘" |

**기관별 분할 처리 워크플로우**:

```
# 1번째 기관
"raw/interview_KAERI.md를 codebook.md 기준으로 코딩하고
qualitative/coding_KAERI.md로 저장해줘"

# 2번째 기관
"raw/interview_ETRI.md를 동일 기준으로 코딩하고
qualitative/coding_ETRI.md로 저장해줘"

# ... 15개 기관 반복 후 ...

# 병합
"qualitative/coding_*.md 파일들을 하나의 통합 코딩 테이블로 병합하여
qualitative/all_coding.md로 저장해줘"
```

> **핵심 팁**: 각 기관 코딩이 끝날 때마다 결과를 **파일로 저장**시키세요. 세션이 끊겨도 이전 결과가 보존됩니다.

### 4-4. PR 검토 방법

Claude Code 웹 버전은 작업 결과를 **Pull Request(PR)**로 제출합니다.

1. GitHub 리포 → **Pull requests** 탭 확인
2. Claude가 생성한 PR 클릭 → **Files changed** 탭에서 변경 내용 확인
3. 검토 체크리스트:
   - [ ] 빈도 데이터가 원본 CSV와 일치하는가?
   - [ ] 인용문이 원본 전사록과 일치하는가?
   - [ ] 코드북에 없는 코드가 사용되지 않았는가?
4. 문제없으면 **Merge pull request** 클릭

---

## Step 5: 검증 및 산출물 정리

### 5-1. 교차 검증 (샘플 10-20% 직접 검토)

전체 코딩 결과 중 **10-20%를 무작위로 추출**하여 연구자가 직접 검토합니다.

```
Policy_merged.csv에서 150개 항목을 무작위 추출하여
validation_sample.csv로 저장해줘.
각 대분류 테마의 비율이 전체 분포와 유사하도록
층화 추출(Stratified Sampling)을 적용해줘.
```

검토 후 일치율 확인:
```
validation_sample.csv의 'llm_code'열과 'human_code'열을 비교하여
Cohen's Kappa를 계산하고, 불일치 패턴을 분석해줘.
```

목표: **Kappa 0.70 이상** (Substantial Agreement). 본 프로젝트에서는 0.85를 달성했습니다.

### 5-2. 최종 보고서 생성

```
qualitative/comprehensive_analysis.md의 분석 결과를 바탕으로
학술 보고서 형식의 최종 보고서를 작성해줘.

구조:
1. 연구 개요 및 방법론
2. 주요 발견 (빈도 데이터 + 대표 인용문)
3. 기관 간 비교 분석
4. 정책적 함의 및 제언

한국어로 작성하되, 학술 용어는 영문 병기.
deliverables/final_report.md로 저장해줘.
```

> **팁**: 보고서가 길면 **파트별로 나눠서 작성**을 지시하세요. (예: "제1-2장만 먼저 작성해줘")

### 5-3. 시각화 생성

```
Policy_merged.csv 데이터를 기반으로 15개 기관의 장애요인 빈도를
Plotly.js 히트맵으로 시각화하는 단일 HTML 파일을 만들어줘.
한국어 레이블, 호버 시 상세 정보 표시, 반응형 디자인 적용.
deliverables/viz_heatmap.html로 저장해줘.
```

---

## 트러블슈팅

### 파일 너무 큼

| 증상 | 해결법 |
|------|--------|
| "파일을 읽을 수 없습니다" | 기관별로 분할하여 500-2,000줄 단위로 처리 |
| PDF 읽기 실패 | Markdown으로 사전 변환 후 업로드 |
| 분석 도중 응답 없음 | 범위를 좁혀서 재시도 ("KAERI 부분만 먼저") |
| CSV 처리 중 에러 | 파일 인코딩을 UTF-8로 변환, 특수문자 제거 |

### Claude Code 멈춤

| 증상 | 해결법 |
|------|--------|
| 응답이 멈춤 | 새 대화(세션) 시작, 이전 결과 파일 참조 |
| 같은 실수 반복 | CLAUDE.md에 주의사항 추가 |
| 맥락을 잊어버림 | "codebook.md를 다시 읽고" 명시적 지시 |
| 중간 결과 유실 | **매 기관마다 파일 저장** 습관화 |

### 코딩 품질 낮음

| 증상 | 해결법 |
|------|--------|
| 분류 기준 불일관 | 코드북의 정의를 더 구체적으로 보완 |
| "기타"가 너무 많음 | 코드를 세분화하거나 새 코드 추가 |
| 유사 코드 혼동 | Few-shot 예시를 프롬프트에 포함 |
| 발언 분절이 부자연스러움 | "1-3문장 단위로 분절" 기준 명시 |

### Gemini vs Claude Code 사용 구분

| 작업 | 추천 도구 | 이유 |
|------|-----------|------|
| 코드북 초안 설계 | **Gemini** | 전체 전사본을 한 번에 읽을 수 있는 긴 컨텍스트 |
| 대용량 PDF 요약 | **Gemini** | 100만+ 토큰 컨텍스트 윈도우 |
| 기관별 체계적 코딩 | **Claude Code** | GitHub 연동, 파일 생성/수정, PR 관리 |
| 보고서 작성 | **Claude Code** | 파일 직접 저장, 반복 수정 용이 |
| 시각화(HTML) 생성 | **Claude Code** | JavaScript/HTML 코드 생성 능력 |
| 교차 검증/Kappa 계산 | **Claude Code** | 두 파일 비교 분석, 통계 계산 |

---

## 체크리스트: 프로젝트 완료 전 확인

- [ ] 모든 인터뷰 데이터가 코딩 완료되었는가?
- [ ] 교차 검증(Kappa) 수행 완료 (목표: 0.70 이상)?
- [ ] 코드북 최종 버전이 codebook.md에 반영되었는가?
- [ ] 최종 보고서가 deliverables/에 저장되었는가?
- [ ] 원본 데이터(raw/)가 수정되지 않았는가?
- [ ] 개인정보가 익명화 처리되었는가?
- [ ] 모든 PR이 검토 후 merge 되었는가?
- [ ] LLM 활용 사실이 방법론 섹션에 명시되었는가?

---

## 더 알아보기

| 주제 | 문서 | 내용 |
|------|------|------|
| 전체 방법론 상세 | [howto_guide.md](howto_guide.md) | 5단계 워크플로우 풀 가이드 (4,792줄) |
| Git/GitHub 입문 | [howto_github_for_researchers.md](howto_github_for_researchers.md) | 연구자를 위한 GitHub 기초 |
| 코드북 실물 | [codebook.md](../codebook.md) | 실제 프로젝트에서 사용한 코드북 (68개 주제 코드 + 10개 맥락 코드) |
| 코딩 결과 예시 | [policy_1027_all_coding.md](../qualitative/policy_1027_all_coding.md) | 15개 기관 1,527개 코딩 항목 |
| 종합 분석 예시 | [comprehensive_policy_analysis_kor.md](../qualitative/comprehensive_policy_analysis_kor.md) | 한국어 종합 정책 분석 보고서 |
