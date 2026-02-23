# Claude Code와 GitHub를 활용한 대규모 질적 연구 실전 가이드

**KAIST 과학기술정책대학원 실전 사례 기반**

---

# 1. 개요 및 소개

## 1.1 이 가이드의 목적

이 가이드는 대학원 연구생이 **Claude Code**(Anthropic의 공식 CLI 도구)와 **GitHub**를 결합하여 대규모 질적 연구(Qualitative Research)를 체계적으로 수행하는 방법을 단계별로 안내합니다.

전통적으로 질적 연구는 연구자 1-2명이 수십 건의 인터뷰를 수작업으로 코딩하고 분석하는 노동 집약적 작업이었습니다. 그러나 LLM(Large Language Model) 기반 에이전트를 활용하면 수만 줄의 인터뷰 데이터를 일관된 기준으로 분류하고, 반복적인 분석 작업을 자동화하며, 연구자는 해석과 이론 구축이라는 본질적 연구 활동에 집중할 수 있습니다.

이 가이드는 실제 연구 프로젝트의 리포지토리 구조, Git 히스토리, 코드북, 분석 결과물을 그대로 공개하여 **재현 가능한(reproducible) 실전 매뉴얼**을 제공합니다.

## 1.2 대상 독자

이 가이드는 다음 조건에 해당하는 대학원생을 대상으로 합니다:

- **질적 연구 경험이 있는 연구자**: 인터뷰 설계, 전사(transcription), 코딩(coding), 테마 분석(thematic analysis) 등 질적 연구 방법론의 기본 개념을 이해하고 있는 분
- **LLM/AI 도구 활용은 처음인 연구자**: ChatGPT 등 대화형 AI는 사용해 봤지만, CLI 기반 에이전트(Claude Code)를 연구에 체계적으로 활용해 본 경험은 없는 분
- **Git/GitHub 기본 사용 경험이 있는 연구자**: `git add`, `git commit`, `git push` 수준의 기본 명령어는 알고 있으나, 브랜치 전략이나 Pull Request 기반 워크플로우에는 익숙하지 않은 분

> **참고**: Git이나 터미널(Terminal) 사용이 완전히 처음이라면 먼저 [Git 공식 튜토리얼](https://git-scm.com/book/ko/v2)의 1-3장을 학습한 후 이 가이드를 따라가기를 권합니다.

## 1.3 질적 연구에서 LLM 활용의 의의: "Qualitative Research at Scale"

### 전통적 질적 연구의 규모 한계

질적 연구의 학술적 가치는 인간 경험의 깊이 있는 이해에 있습니다. 그러나 현실적으로 연구자 1명이 처리할 수 있는 데이터의 규모에는 물리적 한계가 존재합니다:

| 항목 | 전통적 질적 연구 (수작업) | LLM 기반 질적 연구 |
|------|--------------------------|-------------------|
| 인터뷰 수 | 15-30건 | 50건 이상 가능 |
| 전사 분량 | 수천 줄 | **21,011줄 (약 450,000 단어)** |
| 코딩 소요 시간 | 인터뷰 1건당 4-8시간 | 인터뷰 1건당 10-30분 |
| 코딩 결과물 | 수백 개 코드 | **1,527개 구조화 발췌문** |
| 코딩 일관성 | 연구자 피로에 따라 변동 | 코드북 기반 일관된 적용 |
| 반복 분석 | 재코딩 시 수주 소요 | 코드북 수정 후 수시간 내 재분석 |

### "Qualitative Research at Scale" 개념

본 가이드에서 제안하는 접근법은 단순히 "AI가 질적 연구를 대체한다"는 것이 아닙니다. 핵심은 **양적 규모의 질적 연구(Qualitative Research at Scale)**라는 새로운 연구 패러다임입니다:

1. **연구자가 설계한다**: 코드북(Codebook) 설계, 분석 프레임워크 구성, 해석 방향 설정은 전적으로 연구자의 역할입니다.
2. **LLM 에이전트가 실행한다**: 수만 줄의 텍스트에서 의미 단위를 추출하고, 코드북에 따라 분류하며, 빈도 분석과 패턴 탐색을 수행합니다.
3. **연구자가 검증하고 해석한다**: LLM의 분류 결과를 교차 검증(cross-validation)하고, 맥락적 해석을 더하며, 이론적 함의를 도출합니다.

이 프로젝트에서 실제로 달성한 분류 정확도는 **85%**(교차 검증 완료)였으며, 이는 인간 코더 간 신뢰도(inter-coder reliability)와 비교할 때 충분히 경쟁력 있는 수준입니다.

## 1.4 본 프로젝트 개요

### 연구 배경

이 가이드의 기반이 되는 실제 연구 프로젝트의 개요는 다음과 같습니다:

| 항목 | 내용 |
|------|------|
| **연구 기관** | KAIST 과학기술정책대학원 |
| **연구 주제** | 대한민국 출연연구기관의 과학기술 국제협력 전략 재정립 방안 |
| **연구 기간** | 2025년 1월 - 10월 |
| **연구 유형** | 질적 실증 연구 (Qualitative Empirical Study) |
| **인터뷰 대상** | 15개 정부출연연구기관 국제협력 담당자 및 고위 연구진 |
| **데이터 규모** | 21,011줄 심층 인터뷰 전사 (약 450,000 단어) |
| **분석 결과** | 1,527개 구조화 발췌문, 5대 테마 x 40여 개 세부 범주 |
| **최종 산출물** | 정책 보고서, 정책 메모, 인터랙티브 시각화 3종 |

### 분석 대상 기관 (15개 출연연)

```
KAERI  한국원자력연구원          ETRI   한국전자통신연구원
KRISS  한국표준과학연구원        KRISO  선박해양플랜트연구소
KRIBB  한국생명공학연구원        NIGT   국가녹색기술연구소
KIER   한국에너지기술연구원      KIMM   한국기계연구원
KRICT  한국화학연구원            KARI   한국항공우주연구원
KBSI   한국기초과학지원연구원    NST    국가과학기술연구회
KFE    한국핵융합에너지연구원    KISTI  한국과학기술정보연구원
KIGAM  한국지질자원연구원
```

### 핵심 연구 발견

LLM 기반 분석을 통해 도출된 3대 구조적 장애요인:

| 순위 | 장애 요인 | 언급 빈도 | 구조적 성격 |
|------|----------|----------|------------|
| 1 | 정부 부처와의 관계 | 79회 (5.2%) | 다부처 관할 중복, 조정 실패 |
| 2 | 내부 추진 체계 | 76회 (5.0%) | 전담 조직/인력 부족 |
| 3 | 행정/제도적 한계 | 71회 (4.6%) | 절차 복잡성, 제도 경직성 |

이러한 "외부 관계 - 내부 역량 - 제도적 환경"이라는 삼중 구조적 문제가 15개 기관 전체에 보편적으로 존재함을 데이터 기반으로 실증했습니다.

## 1.5 전체 워크플로우 요약: 5 Phases

본 가이드에서 다루는 전체 연구 워크플로우는 다음 5단계로 구성됩니다:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    LLM 기반 질적 연구 워크플로우                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Phase 1: 사전 준비 (Prerequisites)                                  │
│  ├── Claude Code 설치 및 설정                                        │
│  ├── GitHub 리포지토리 초기화                                         │
│  ├── 프로젝트 디렉토리 구조 설계 (raw/qualitative/deliverables/)      │
│  └── CLAUDE.md를 통한 프로젝트 컨텍스트 설정                          │
│           │                                                          │
│           ▼                                                          │
│  Phase 2: 데이터 준비 및 코드북 설계                                  │
│  ├── 인터뷰 전사 데이터 전처리 (마크다운 변환)                         │
│  ├── 초기 LLM 1차 분석 (탐색적 코딩)                                 │
│  ├── 코드북(Codebook) 설계 (주제 코드 + 맥락 코드)                    │
│  └── 코드북을 codebook.md로 문서화                                    │
│           │                                                          │
│           ▼                                                          │
│  Phase 3: 체계적 코딩 및 분석                                        │
│  ├── Claude Code 에이전트에 코드북 기반 코딩 지시                     │
│  ├── 기관별 의미 단위 분해 → 이중 코딩 (주제 + 맥락)                  │
│  ├── 구조화 발췌문(CSV) 생성                                         │
│  └── 교차 검증 및 코드 수정                                          │
│           │                                                          │
│           ▼                                                          │
│  Phase 4: 종합 분석 및 보고서 작성                                    │
│  ├── 기관 간 비교 분석, 빈도 분석, 패턴 탐색                          │
│  ├── 종합 정책 분석 보고서 작성                                       │
│  ├── 정책 메모(Policy Memo) 작성                                     │
│  └── 인터랙티브 시각화 생성 (HTML)                                    │
│           │                                                          │
│           ▼                                                          │
│  Phase 5: 검증, 피드백, 최종화                                       │
│  ├── 전문가 피드백 통합                                               │
│  ├── LLM 분석 평가 (정확도 검증)                                     │
│  ├── 최종 보고서 개정 및 완성                                         │
│  └── 연구 아카이빙 (Git 태그, 릴리즈)                                 │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### 각 Phase와 프로젝트 산출물의 대응

| Phase | 주요 활동 | 리포지토리 산출물 | Git 브랜치/PR |
|-------|----------|------------------|---------------|
| Phase 1 | 환경 설정 | 디렉토리 구조, `.gitignore` | `main` (초기 커밋) |
| Phase 2 | 데이터 준비, 코드북 설계 | `codebook.md`, `raw/llm_analysis_1003.md` | 초기 업로드 커밋들 |
| Phase 3 | 체계적 코딩 | `raw/Policy_merged.csv`, `qualitative/policy_1027_all_coding.md` | `codex/analyze-and-code-interview-transcripts` (PR #4, #5) |
| Phase 4 | 종합 분석, 보고서 | `qualitative/comprehensive_policy_analysis.md`, `deliverables/final_report.md`, `deliverables/viz_*.html` | `claude/science-policy-interviews-*` (PR #3, #7, #8, #9) |
| Phase 5 | 검증, 피드백 | `deliverables/feedback.md`, `deliverables/llm_analysis_evaluation.md`, `deliverables/final_report_revised.md` | `claude/science-policy-interviews-*` (PR #1, #2), `codex/improve-policy-memo-writing-style` (PR #6) |

### Git 히스토리로 본 실제 작업 흐름

이 프로젝트의 Git 히스토리는 집중적인 LLM 기반 분석 과정을 보여줍니다:

```
# 실제 Git 히스토리 (시간순)
PR #1  claude/science-policy-interviews-*   전문가 피드백 생성
PR #2  claude/science-policy-interviews-*   피드백 보완 및 LLM 분석 비교
PR #3  claude/science-policy-interviews-*   최종 보고서 (5개 장)
PR #4  codex/analyze-and-code-interview-*    코딩 테이블 서사 맥락 추가
PR #5  codex/analyze-and-code-interview-*    소스/산출물 폴더 정리
PR #6  codex/improve-policy-memo-*           정책 메모 문체 개선
PR #7  claude/qualitative-policy-synthesis-*  종합 정책 분석
PR #8  claude/qualitative-policy-synthesis-*  한국어 번역
PR #9  claude/qualitative-policy-synthesis-*  최종 보고서 개정 + 시각화
```

각 Pull Request(PR)는 Claude Code가 생성한 브랜치에서 작업한 결과물이며, 연구자가 검토(review)한 후 `main` 브랜치에 병합(merge)하는 방식으로 진행되었습니다. 이는 연구의 모든 과정이 **버전 관리되고 추적 가능한(traceable)** 형태로 기록됨을 의미합니다.

## 1.6 전통적 질적 분석 도구와의 비교

### NVivo, ATLAS.ti 등 기존 CAQDAS와의 비교

기존의 컴퓨터 지원 질적 데이터 분석 소프트웨어(CAQDAS: Computer-Assisted Qualitative Data Analysis Software)와 LLM 기반 접근법을 비교하면 다음과 같습니다:

| 비교 항목 | NVivo / ATLAS.ti | Claude Code + GitHub |
|----------|-------------------|----------------------|
| **코딩 방식** | 연구자가 텍스트를 하나씩 드래그하여 코드 부여 | 코드북 기반으로 LLM이 자동 분류, 연구자가 검증 |
| **처리 속도** | 인터뷰 1건당 4-8시간 | 인터뷰 1건당 10-30분 |
| **규모 확장성** | 30건 이상이면 비현실적 | 50건 이상도 체계적 처리 가능 |
| **일관성** | 연구자 피로에 따라 후반부 코딩 품질 저하 | 코드북 기준 일관 적용 (단, 검증 필수) |
| **비용** | 소프트웨어 라이선스 연 $1,000+ | API 사용료 (프로젝트당 $50-200 수준) |
| **버전 관리** | 프로젝트 파일 내부 히스토리 | Git 기반 완전한 변경 이력 추적 |
| **재현 가능성** | 프로젝트 파일 공유 시 재현 가능 | 코드북 + 프롬프트 + Git 히스토리로 완전 재현 |
| **협업** | 동시 편집 제한적 | GitHub PR 기반 비동기 협업 |
| **학습 곡선** | GUI 기반, 비교적 완만 | CLI 기반, 초기 설정 필요 |

### LLM 기반 접근법의 장점

1. **속도와 규모**: 21,011줄의 인터뷰 전사를 1,527개 구조화 발췌문으로 분류하는 작업이 수일 내에 완료됩니다. 전통적 방법으로는 수주에서 수개월이 소요됩니다.

2. **일관성**: 코드북(Codebook)을 명문화하면 LLM 에이전트는 첫 번째 인터뷰와 마지막 인터뷰에 동일한 기준을 적용합니다. 인간 코더가 겪는 피로 효과(fatigue effect)가 없습니다.

3. **반복 가능성(Iterability)**: 코드북을 수정한 후 전체 데이터를 재코딩하는 것이 현실적으로 가능합니다. 이 프로젝트에서도 초기 5대 테마 구조(1차 분석)를 8대 주제 코드 + 10개 맥락 코드 체계(GRI-IC-Codebook v1.0)로 발전시키는 반복 과정을 거쳤습니다.

4. **완전한 감사 추적(Audit Trail)**: Git 히스토리를 통해 코드북의 변천, 분석 결과의 변화, 연구자의 의사결정을 모두 추적할 수 있어 질적 연구의 신뢰성(trustworthiness) 확보에 유리합니다.

5. **다언어 처리**: 한국어 인터뷰 데이터를 분석하면서 영어 코드 체계를 적용하고, 한국어/영어 보고서를 동시에 생성할 수 있습니다. 이 프로젝트에서는 `comprehensive_policy_analysis.md`(영문)와 `comprehensive_policy_analysis_kor.md`(국문) 두 버전을 생성했습니다.

### LLM 기반 접근법의 한계와 주의사항

1. **해석의 깊이 부족**: LLM은 텍스트의 표면적 의미를 분류하는 데는 뛰어나지만, 발화자의 침묵, 어조 변화, 비언어적 맥락을 포착하지 못합니다. 인터뷰 현장에 있었던 연구자의 맥락적 이해를 대체할 수 없습니다.

2. **검증의 필수성**: LLM의 분류 결과는 반드시 연구자가 검증해야 합니다. 이 프로젝트에서 85%의 분류 정확도를 달성했다는 것은, 15%는 연구자의 수정이 필요했다는 뜻이기도 합니다. LLM의 결과를 무비판적으로 수용하는 것은 연구 윤리에 어긋납니다.

3. **코드북 설계의 중요성**: LLM 에이전트의 분류 품질은 코드북의 명확성에 전적으로 의존합니다. 모호한 코드 정의는 모호한 분류 결과로 이어집니다. 코드북 설계에 충분한 시간을 투자해야 합니다.

4. **데이터 보안**: 인터뷰 전사 데이터는 연구 참여자의 개인정보를 포함할 수 있습니다. API를 통해 외부 서버로 데이터를 전송하는 행위에 대한 IRB(Institutional Review Board) 심의와 연구 참여자 동의가 필요합니다. 이에 대해서는 2.6절에서 자세히 다룹니다.

5. **"블랙박스" 비판에 대한 대응**: LLM 기반 분석은 "분석 과정이 불투명하다"는 비판을 받을 수 있습니다. 이를 극복하기 위해 코드북 공개, Git 히스토리를 통한 감사 추적, 교차 검증 결과 보고가 필수적입니다.

> **핵심 원칙**: LLM은 연구자의 **도구(tool)**이지 **대체재(replacement)**가 아닙니다. 연구 설계, 코드북 설계, 해석, 이론 구축은 연구자만이 할 수 있는 고유한 학술 활동입니다. LLM은 이 과정에서 반복적이고 규모가 큰 작업을 가속화하는 역할을 합니다.

---

# 2. 사전 준비 (Prerequisites)

이 절에서는 연구를 시작하기 전에 갖추어야 할 기술적 환경과 도구를 설정하는 방법을 안내합니다.

## 2.1 필요 도구 목록

본 워크플로우를 따라가기 위해 필요한 도구는 다음과 같습니다:

| 도구 | 용도 | 필수 여부 |
|------|------|-----------|
| **Git** | 버전 관리, 변경 이력 추적 | 필수 |
| **Node.js** (v18 이상) | Claude Code CLI 실행 환경 | 필수 |
| **Claude Code CLI** | LLM 에이전트 인터페이스 | 필수 |
| **Anthropic API Key** | Claude API 인증 | 필수 |
| **GitHub 계정** | 리포지토리 호스팅, PR 관리 | 필수 |
| **텍스트 에디터** | 코드북 편집, 결과물 검토 | 필수 (VS Code 권장) |
| **GitHub CLI (`gh`)** | PR 생성/관리 자동화 | 권장 |
| **터미널 에뮬레이터** | CLI 작업 환경 | 필수 (macOS: Terminal/iTerm2, Windows: Windows Terminal) |

## 2.2 Claude Code 설치 및 설정

### 2.2.1 Node.js 설치

Claude Code는 Node.js 기반으로 동작합니다. 먼저 Node.js가 설치되어 있는지 확인합니다:

```bash
# Node.js 버전 확인
node --version
# v18.0.0 이상이 필요합니다

# npm 버전 확인
npm --version
```

Node.js가 설치되어 있지 않다면:

```bash
# macOS (Homebrew 사용)
brew install node

# Ubuntu/Debian
sudo apt update && sudo apt install -y nodejs npm

# Windows (winget 사용)
winget install OpenJS.NodeJS.LTS
```

### 2.2.2 Claude Code CLI 설치

```bash
# Claude Code 전역 설치
npm install -g @anthropic-ai/claude-code

# 설치 확인
claude --version
```

### 2.2.3 Anthropic API Key 설정

Claude Code를 사용하려면 Anthropic API 키가 필요합니다:

1. [Anthropic Console](https://console.anthropic.com/)에 접속하여 계정을 생성합니다.
2. **API Keys** 메뉴에서 새 키를 생성합니다.
3. 터미널에서 API 키를 환경 변수로 설정합니다:

```bash
# 환경 변수 설정 (세션 단위)
export ANTHROPIC_API_KEY="sk-ant-api03-..."

# 영구 설정 (shell 설정 파일에 추가)
# bash 사용자:
echo 'export ANTHROPIC_API_KEY="sk-ant-api03-..."' >> ~/.bashrc
source ~/.bashrc

# zsh 사용자 (macOS 기본):
echo 'export ANTHROPIC_API_KEY="sk-ant-api03-..."' >> ~/.zshrc
source ~/.zshrc
```

> **보안 주의**: API 키를 Git 리포지토리에 커밋하지 마세요. `.env` 파일이나 시스템 환경 변수를 사용하고, `.gitignore`에 반드시 `.env`를 추가하세요.

### 2.2.4 Claude Code 기본 사용법

Claude Code가 정상적으로 설치되었다면 프로젝트 디렉토리에서 바로 실행할 수 있습니다:

```bash
# 프로젝트 디렉토리로 이동
cd ~/my-research-project

# Claude Code 실행 (인터랙티브 모드)
claude

# 특정 작업을 바로 지시
claude "이 프로젝트의 디렉토리 구조를 설명해줘"

# 파일을 읽고 분석하도록 지시
claude "raw/interview_01.md 파일을 읽고 주요 테마를 추출해줘"
```

Claude Code의 핵심 특성:

- **프로젝트 컨텍스트 인식**: 실행 디렉토리의 파일 구조를 자동으로 인식합니다.
- **파일 읽기/쓰기**: 로컬 파일을 직접 읽고 수정할 수 있습니다.
- **Git 연동**: `git` 명령어를 통해 브랜치 생성, 커밋, PR 생성을 수행합니다.
- **연속적 대화**: 하나의 세션에서 여러 작업을 연속적으로 지시할 수 있습니다.

```bash
# 실제 사용 예시 (인터랙티브 세션)
$ claude

> codebook.md를 읽고 주제 코드의 전체 목록을 요약해줘

# Claude Code가 codebook.md를 읽고 응답합니다
# 이어서 후속 작업을 지시할 수 있습니다

> raw/policy_250924.md에서 KAERI 인터뷰 부분을 찾아서
> codebook.md의 기준에 따라 코딩해줘

# Claude Code가 파일을 읽고 코딩 결과를 생성합니다
```

## 2.3 GitHub 리포지토리 생성

### 2.3.1 리포지토리 초기화

```bash
# 프로젝트 디렉토리 생성
mkdir my-qualitative-research
cd my-qualitative-research

# Git 리포지토리 초기화
git init

# 초기 커밋
git add .
git commit -m "Initial commit: project structure"
```

### 2.3.2 GitHub에 원격 리포지토리 생성

GitHub CLI(`gh`)가 설치되어 있다면:

```bash
# GitHub CLI로 리포지토리 생성 (private 권장)
gh repo create my-qualitative-research --private --source=. --push

# 또는 기존 리포지토리에 연결
git remote add origin https://github.com/your-username/my-qualitative-research.git
git push -u origin main
```

> **중요**: 인터뷰 데이터를 포함하는 리포지토리는 반드시 **Private**(비공개)으로 설정하세요. 연구 참여자의 개인정보 보호를 위해 공개 리포지토리에 원본 인터뷰 데이터를 올리면 안 됩니다.

### 2.3.3 .gitignore 설정

연구용 리포지토리에 적합한 `.gitignore` 파일을 작성합니다:

```gitignore
# ===== 보안 및 개인정보 =====
# API 키, 인증 정보
.env
.env.local
*.key

# ===== 원본 데이터 보호 (필요 시) =====
# 익명화 전 원본 인터뷰 파일
# raw/original_transcripts/
# *.identifiable.md

# ===== OS 및 에디터 =====
.DS_Store
Thumbs.db
.vscode/
*.swp
*.swo
*~

# ===== Node.js (Claude Code 관련) =====
node_modules/
package-lock.json

# ===== 임시 파일 =====
*.tmp
*.bak
*.log
draft_*/
```

```bash
# .gitignore 생성 후 커밋
git add .gitignore
git commit -m "Add .gitignore for research project"
```

## 2.4 프로젝트 디렉토리 구조 설계

### 2.4.1 3단 구조: `raw/`, `qualitative/`, `deliverables/`

이 프로젝트에서 사용한 디렉토리 구조는 질적 연구의 데이터 흐름을 반영한 3단 구조입니다:

```
my-qualitative-research/
│
├── CLAUDE.md              # Claude Code 프로젝트 컨텍스트 (2.5절 참조)
├── codebook.md            # LLM 에이전트 분류 기준 (코드북)
├── .gitignore             # Git 추적 제외 파일 목록
│
├── raw/                   # [1단계] 원시 데이터 (Raw Data)
│   ├── interview_01.md    #   인터뷰 전사 원본
│   ├── interview_02.md    #   인터뷰 전사 원본
│   ├── ...
│   ├── merged_coded.csv   #   구조화 코딩 결과 (CSV)
│   └── llm_first_pass.md  #   LLM 1차 탐색 분석 결과
│
├── qualitative/           # [2단계] 질적 분석 결과 (Analysis)
│   ├── all_coding.md      #   전체 기관 코딩 통합본
│   ├── cross_analysis.md  #   기관 간 비교 분석
│   ├── theme_analysis.md  #   테마별 심층 분석
│   └── analysis_kor.md    #   한국어 분석 보고서
│
└── deliverables/          # [3단계] 최종 산출물 (Deliverables)
    ├── final_report.md    #   최종 보고서
    ├── policy_memo.md     #   정책 메모
    ├── feedback.md        #   전문가 피드백
    ├── evaluation.md      #   LLM 분석 평가
    └── viz_*.html         #   인터랙티브 시각화
```

### 2.4.2 각 폴더의 역할

#### `raw/` - 원시 데이터 (Raw Data)

이 폴더는 연구의 원천 자료를 보관합니다. 한번 저장하면 내용을 수정하지 않는 것이 원칙입니다 (immutable).

실제 프로젝트에서 포함된 파일들:

| 파일명 | 크기 | 설명 |
|--------|------|------|
| `policy_250924.md` | 968KB (14,942줄) | 인터뷰 전사 원본 (마크다운 형식) |
| `policy_analysis_251024.md` | 1.4MB | 분석 보고서 원본 |
| `policy_1027_revised.md` | 1.4MB | 수정 분석 원본 |
| `Policy_merged.csv` | 483KB (1,527행) | 구조화 코딩 결과 (기관, 대분류, 중분류, 발췌문) |
| `llm_analysis_1003.md` | 11KB | LLM 1차 탐색 분석 결과 |

`Policy_merged.csv`의 실제 구조:

```csv
기관,대분류 테마,중분류 테마,해당 문장 발췌
KAERI,1. 국제협력의 동인 및 목표,기술 확보 및 R&D 역량 강화,"차세대 원자로 개발하는 데 있어서 협력할 수 있는 분야가 되게 많았다고 치면"
KAERI,1. 국제협력의 동인 및 목표,자원 확보 및 접근성,해당 없음
ETRI,3. 기관별 특성 및 협력 환경,기관 임무 기반 협력,"ICT 분야는 기술 변화가 너무 빠르기 때문에..."
```

#### `qualitative/` - 질적 분석 결과 (Analysis)

이 폴더는 `raw/` 데이터를 코드북에 따라 체계적으로 분석한 결과물을 보관합니다. Claude Code 에이전트가 생성하고 연구자가 검증/수정한 분석 결과가 여기에 위치합니다.

실제 프로젝트에서 포함된 파일들:

| 파일명 | 크기 | 설명 |
|--------|------|------|
| `policy_1027_all_coding.md` | 60KB | 15개 기관 전체 코딩 통합본 (기관별 요약 + 코딩 테이블) |
| `comprehensive_policy_analysis.md` | 35KB | 종합 정책 분석 (영문) |
| `comprehensive_policy_analysis_kor.md` | 34KB | 종합 정책 분석 (국문) |
| `policy_recommendations.md` | 23KB | 정책 제언 |

`policy_1027_all_coding.md`의 구조 예시 (실제 프로젝트에서 사용한 형식):

```markdown
# 정책연구1-1 인터뷰 코딩 총괄

## KAERI 한국원자력연구원

### 1. 인터뷰 핵심 요약
- 전은주 박사님은 원자력 협력의 민감성과 미국 중심의 승인 구조 때문에...
- 탈원전기 이후에도 예산과 조직 구조가 급변하며...

### 2. 주요 장애요인 및 도전과제
- 핵연료·핵물질 등 민감 분야의 수출통제와...

### 3. 정책적 시사점
- 국가 차원의 컨트롤타워와 장기 전략 마련...

### 4. 코드화 테이블
| Institution | Speaker | Source_Text | Thematic_Code(s) | Context_Code | Location_Ref |
| :--- | :--- | :--- | :--- | :--- | :--- |
| KAERI | 전은주 박사님 | 원자력 협력은 핵무기 기술과... | `OBS_GEO_RESTRICTIONS_CONTROLS`, `OBS_IPR_DISPUTES_NEGOTIATION` | `CAUSE_ANALYSIS` | (250702_KAERI_~01:10) |
```

#### `deliverables/` - 최종 산출물 (Deliverables)

이 폴더는 연구의 최종 결과물을 보관합니다. 보고서, 정책 메모, 시각화, 피드백 문서 등 "외부에 전달 가능한" 형태의 산출물이 여기에 위치합니다.

실제 프로젝트에서 포함된 파일들:

| 파일명 | 크기 | 설명 |
|--------|------|------|
| `final_report.md` | 143KB | 최종 연구 보고서 |
| `final_report_revised.md` | 102KB | 수정 최종 보고서 |
| `policy_memo.md` | 14KB | 정책 메모 (과기부 장관 수신) |
| `feedback.md` / `feedback_v2.md` / `final_feedback.md` | 22-31KB | 전문가 피드백 (3차에 걸쳐 보완) |
| `llm_analysis_evaluation.md` | 25KB | LLM 분석 평가 보고서 |
| `viz_causal_network.html` | 9KB | 인과관계 네트워크 시각화 |
| `viz_institutional_heatmap.html` | 6KB | 기관별 히트맵 시각화 |
| `viz_policy_roadmap.html` | 11KB | 정책 로드맵 시각화 |

### 2.4.3 디렉토리 구조 생성 명령어

새 프로젝트를 시작할 때 다음 명령어로 디렉토리 구조를 한 번에 생성할 수 있습니다:

```bash
# 프로젝트 루트에서 실행
mkdir -p raw qualitative deliverables

# 각 폴더에 .gitkeep 파일 생성 (빈 폴더도 Git 추적 가능)
touch raw/.gitkeep qualitative/.gitkeep deliverables/.gitkeep

# 구조 확인
tree -L 1
# .
# ├── deliverables
# ├── qualitative
# └── raw
```

### 2.4.4 데이터 흐름과 디렉토리 구조의 관계

```
연구자의 인터뷰 전사 ──→ raw/
                            │
              코드북 기반     │  Claude Code
              LLM 분석       ▼  에이전트
                         qualitative/
                            │
              보고서 작성,    │  Claude Code
              시각화 생성     ▼  에이전트 + 연구자 검토
                         deliverables/
```

이 3단 구조의 핵심 원칙:

1. **단방향 데이터 흐름**: `raw/` → `qualitative/` → `deliverables/` 순서로 데이터가 가공됩니다.
2. **원본 불변성**: `raw/` 폴더의 파일은 수정하지 않습니다. 분석 결과와 보고서는 별도 폴더에 저장합니다.
3. **명확한 역할 분리**: 각 폴더의 목적이 분명하여, 어떤 파일이 원시 데이터인지, 분석 중간 결과인지, 최종 산출물인지 혼동할 일이 없습니다.

## 2.5 CLAUDE.md 파일을 통한 프로젝트 컨텍스트 설정

### 2.5.1 CLAUDE.md란?

`CLAUDE.md`는 Claude Code가 프로젝트 디렉토리에서 실행될 때 **자동으로 읽는 프로젝트 설명 파일**입니다. 이 파일에 연구의 목적, 데이터 구조, 분석 방향, 주의사항을 미리 기록해 두면 매번 새 세션을 시작할 때마다 맥락을 재설명할 필요가 없습니다.

이 파일은 Claude Code에게 "이 프로젝트는 이런 연구이고, 이런 파일 구조를 가지고 있으며, 이런 방식으로 작업해 달라"는 지시를 항구적으로 전달하는 역할을 합니다.

### 2.5.2 CLAUDE.md 작성 예시

다음은 본 프로젝트에 적합한 `CLAUDE.md`의 예시입니다. 자신의 연구에 맞게 수정하여 사용하세요:

```markdown
# 프로젝트: 출연연 국제협력 인터뷰 질적 분석

## 연구 개요
- 연구 기관: KAIST 과학기술정책대학원
- 연구 주제: 15개 정부출연연구기관의 과학기술 국제협력 전략 분석
- 연구 방법: 질적 실증 연구 (심층 인터뷰 기반)
- 데이터 규모: 21,011줄 인터뷰 전사 (약 450,000 단어), 15개 기관

## 디렉토리 구조
- `raw/`: 인터뷰 전사 원본, 구조화 코딩 CSV, 1차 분석 결과
- `qualitative/`: 코드북 기반 체계적 코딩 결과, 종합 분석
- `deliverables/`: 최종 보고서, 정책 메모, 시각화, 피드백
- `codebook.md`: GRI-IC-Codebook v1.0 (LLM 에이전트용 분류 기준)

## 핵심 파일
- `codebook.md`: 8대 주제 코드 + 10개 맥락 코드 정의서
- `raw/policy_250924.md`: 인터뷰 전사 원본 (968KB)
- `raw/Policy_merged.csv`: 1,527개 구조화 발췌문
- `qualitative/policy_1027_all_coding.md`: 전체 코딩 통합본

## 분석 지침
- 모든 코딩은 반드시 `codebook.md`의 정의를 따를 것
- 각 텍스트 세그먼트에 주제 코드 1개 이상 + 맥락 코드 1개를 부여할 것
- 한국어 인터뷰 데이터를 분석하되, 코드는 영문 코드 체계를 사용할 것
- 분석 결과는 마크다운 테이블 형식으로 출력할 것

## 주의사항
- 인터뷰 참여자의 개인정보(이름, 소속 직급 등)를 외부에 노출하지 말 것
- 원본 데이터(raw/)의 파일은 수정하지 말 것
- 모든 분석 결과에 출처(기관명, 타임스탬프)를 명기할 것

## 언어
- 분석 코드: 영문 (codebook.md 참조)
- 보고서 본문: 한국어 (필요 시 영문 버전 별도 생성)
- 학술 용어: 영문 병기 (예: 질적 연구(Qualitative Research))
```

### 2.5.3 CLAUDE.md 작성 시 팁

1. **구체적으로 작성하세요**: "인터뷰 데이터를 분석해줘"보다 "codebook.md의 GRI-IC-Codebook v1.0 기준에 따라, raw/policy_250924.md의 KAERI 인터뷰 섹션을 의미 단위로 분해하고, 주제 코드와 맥락 코드를 부여해줘"가 훨씬 좋은 결과를 생성합니다.

2. **파일 경로를 명시하세요**: Claude Code가 어떤 파일을 참조해야 하는지 정확한 경로를 적어 두면, 세션마다 "이 파일을 읽어줘"라고 반복할 필요가 없습니다.

3. **금지 사항을 명확히 하세요**: "원본 데이터를 수정하지 말 것", "개인정보를 노출하지 말 것" 같은 제약 조건을 명시해야 합니다.

4. **점진적으로 업데이트하세요**: 연구가 진행됨에 따라 `CLAUDE.md`도 업데이트합니다. 코드북이 변경되면 그 사실을 반영하고, 새로운 분석 단계로 넘어가면 현재 진행 상황을 갱신합니다.

### 2.5.4 CLAUDE.md 생성 및 커밋

```bash
# CLAUDE.md 생성 (위의 예시 내용을 에디터로 작성)
# VS Code의 경우:
code CLAUDE.md

# 또는 Claude Code에게 초안 생성을 요청:
claude "이 프로젝트의 파일 구조를 분석하고, CLAUDE.md 초안을 작성해줘"

# 작성 후 커밋
git add CLAUDE.md
git commit -m "Add CLAUDE.md project context for Claude Code agent"
```

## 2.6 연구 윤리 고려사항

### 2.6.1 인터뷰 데이터의 익명화

LLM 기반 분석에 앞서, 인터뷰 전사 데이터의 익명화(de-identification)가 반드시 선행되어야 합니다:

```markdown
# 익명화 체크리스트

## 반드시 제거/변환해야 하는 정보
- [ ] 인터뷰 참여자의 실명 → 가명 또는 코드 (예: "Participant A", "KAERI-01")
- [ ] 직급 및 보직명 → 일반화 (예: "국제협력 담당 선임연구원" → "국제협력 담당자")
- [ ] 연락처 (전화번호, 이메일) → 완전 삭제
- [ ] 특정 개인을 식별할 수 있는 고유한 경험 → 일반화 또는 삭제

## 기관 수준 정보 (연구 설계에 따라 판단)
- [ ] 기관명: 본 프로젝트에서는 기관명을 그대로 사용 (연구 목적상 기관 간 비교가 필요)
- [ ] 부서명: 필요한 경우 일반화

## 익명화 작업 기록
- 익명화 규칙을 문서화하고, 원본↔익명화 데이터 간 대응표를 안전하게 보관
- 대응표는 Git 리포지토리에 포함시키지 않음
```

### 2.6.2 API를 통한 데이터 전송 시 개인정보 보호

Claude Code를 사용하면 인터뷰 데이터가 Anthropic의 API 서버로 전송됩니다. 이에 대한 주요 고려사항:

**IRB 심의 사항**:

- 연구 계획서에 "LLM 기반 텍스트 분석 도구를 활용한다"는 사실을 명시해야 합니다.
- 데이터가 외부 API 서버로 전송되는 점을 IRB에 고지하고 승인을 받아야 합니다.
- Anthropic의 데이터 처리 정책(data retention policy)을 IRB 제출 자료에 포함시킵니다.

**연구 참여자 동의**:

- 동의서(informed consent)에 "수집된 인터뷰 데이터는 익명화 처리 후 AI 기반 텍스트 분석 도구를 활용하여 분석될 수 있습니다"라는 문구를 포함해야 합니다.
- 이미 수집된 데이터에 대해 소급 동의(retrospective consent)가 필요한지 IRB와 협의합니다.

**기술적 보호 조치**:

```bash
# 1. 익명화 데이터만 raw/ 폴더에 보관
# 원본(식별 가능)과 익명화 데이터를 분리
raw/
├── anonymized/          # 익명화 완료 데이터 (Git 추적)
│   ├── interview_01.md
│   └── interview_02.md
└── original/            # 원본 데이터 (Git 추적 제외)
    ├── interview_01_original.md
    └── interview_02_original.md

# 2. .gitignore에 원본 데이터 폴더 추가
echo "raw/original/" >> .gitignore

# 3. Anthropic의 API 데이터 정책 확인
# - API 입력 데이터는 모델 학습에 사용되지 않음 (2025년 기준)
# - 데이터 보존 기간 확인: https://www.anthropic.com/policies
```

**Anthropic의 데이터 정책 요약** (연구자가 확인해야 할 사항):

1. API를 통해 전송된 데이터는 모델 학습(training)에 사용되지 않습니다.
2. 데이터는 요청 처리 후 일정 기간 내 삭제됩니다 (구체적 기간은 정책 문서 참조).
3. 데이터는 전송 시 암호화(TLS)됩니다.

> **권장 사항**: 가능하다면 인터뷰 전사 데이터를 LLM에 전송하기 전에 **최소한 인명과 연락처를 제거**하세요. 기관명은 연구 설계상 필요하다면 유지할 수 있지만, 이 역시 IRB 승인 범위 내에서 판단해야 합니다.

### 2.6.3 연구 윤리 자기 점검표

프로젝트를 시작하기 전에 다음 항목을 확인하세요:

```markdown
# LLM 기반 질적 연구 윤리 자기 점검표

## 데이터 수집 단계
- [ ] IRB 승인서에 LLM 활용 분석 방법이 명시되어 있는가?
- [ ] 연구 참여자 동의서에 AI 기반 분석 도구 활용 관련 문구가 포함되어 있는가?
- [ ] 인터뷰 전사 데이터의 익명화 절차가 수립되어 있는가?

## 데이터 저장 및 전송 단계
- [ ] 익명화 완료된 데이터만 Git 리포지토리에 포함되는가?
- [ ] 원본(식별 가능) 데이터는 .gitignore로 추적 제외 되었는가?
- [ ] API 서비스 제공자의 데이터 처리 정책을 확인하였는가?
- [ ] API 키가 리포지토리에 포함되지 않도록 조치하였는가?

## 분석 및 보고 단계
- [ ] LLM이 생성한 분석 결과를 연구자가 검증하였는가?
- [ ] LLM 활용 사실을 연구 방법론 섹션에 투명하게 기술하였는가?
- [ ] LLM의 분류 정확도와 한계를 보고서에 명시하였는가?
- [ ] 최종 보고서에서 연구 참여자가 식별되지 않도록 확인하였는가?
```

## 2.7 첫 번째 세션 시작하기: 체크리스트

모든 사전 준비가 완료되면 다음 체크리스트를 확인한 후 연구를 시작합니다:

```bash
# ===== 환경 점검 =====

# 1. Node.js 설치 확인
node --version    # v18.0.0 이상

# 2. Claude Code 설치 확인
claude --version

# 3. API 키 설정 확인
echo $ANTHROPIC_API_KEY   # sk-ant-api03-... 형식

# 4. Git 설정 확인
git --version
git config user.name
git config user.email

# 5. GitHub CLI 설치 확인 (권장)
gh --version
gh auth status

# ===== 프로젝트 구조 점검 =====

# 6. 디렉토리 구조 확인
tree -L 1
# .
# ├── CLAUDE.md
# ├── codebook.md       (있으면 좋음, 없으면 Phase 2에서 작성)
# ├── deliverables/
# ├── qualitative/
# └── raw/

# 7. .gitignore 확인
cat .gitignore

# 8. CLAUDE.md 확인
cat CLAUDE.md

# 9. Git 상태 확인
git status
git log --oneline

# ===== 첫 Claude Code 세션 시작 =====

# 10. 프로젝트 디렉토리에서 Claude Code 실행
cd ~/my-qualitative-research
claude
```

여기까지 완료되었다면, Phase 1(사전 준비)이 모두 끝난 것입니다. 다음 단계에서는 인터뷰 전사 데이터를 준비하고 코드북을 설계하는 Phase 2로 진행합니다.

---

# 3. Phase 1 — 원시 데이터 준비 (Raw Data Preparation)

## 3.1. 전체 개요

질적 연구의 첫 번째 관문은 인터뷰 녹음 파일을 분석 가능한 구조화된 데이터로 변환하는 것이다. 본 연구에서는 15개 정부출연연구기관(이하 출연연)의 반구조화 인터뷰(semi-structured interview) 녹음본을 다음과 같은 파이프라인으로 처리하였다.

```
녹음 파일 (.m4a/.wav)
  → 텍스트 전사 (Transcription)
    → Markdown 정리 (raw/policy_250924.md, 14,942줄)
      → 1차 LLM 분석 (raw/llm_analysis_1003.md)
        → 보완·교정된 전사본 (raw/policy_1027_revised.md, 21,011줄)
          → 구조화된 CSV (raw/Policy_merged.csv, 1,527행)
```

이 과정에서 원시 데이터의 총량은 약 968KB의 Markdown 텍스트(14,942줄)에서 시작하여, 맥락 보완 후 21,011줄로 확장되었고, 최종적으로 1,527개의 코딩된 발췌문(coded excerpts)이 담긴 CSV 파일로 구조화되었다.

---

## 3.2. 인터뷰 녹음 파일의 텍스트 전사 (Transcription)

### 3.2.1. 전사 도구 선택

한국어 인터뷰 전사에 활용할 수 있는 주요 도구는 다음과 같다.

| 도구 | 유형 | 한국어 지원 | 화자 구분 | 비용 | 적합 상황 |
|------|------|------------|----------|------|-----------|
| **Clova Note** | 클라우드 SaaS | 최우수 (네이버 자체 모델) | 자동 지원 | 월 300분 무료, 이후 유료 | 한국어 전용 인터뷰, 빠른 작업 필요 시 |
| **OpenAI Whisper** | 오픈소스 (로컬/API) | 우수 (large-v3 모델) | 별도 처리 필요 | API: $0.006/분, 로컬: 무료 | 대량 처리, 재현성 확보, 비용 절감 |
| **Google STT** | 클라우드 API | 우수 | 자동 지원 (Enhanced) | $0.016/분 (Enhanced) | GCP 인프라 활용 시 |
| **Daglo (다글로)** | 클라우드 SaaS | 최우수 | 자동 지원 | 유료 (시간당 과금) | 법률/의료 등 전문 분야 |

본 연구에서는 **Clova Note**를 1차 전사 도구로 활용하되, 전사 품질 검증을 위해 **Whisper large-v3**를 보조적으로 사용하였다. 한국어 고유명사(기관명, 기술 용어 등)의 정확도는 Clova Note가 상대적으로 높았으나, 어떤 도구를 사용하든 반드시 수작업 검수(manual review)가 필요하다.

### 3.2.2. Whisper를 활용한 로컬 전사 방법

GPU가 장착된 로컬 환경에서 Whisper를 사용하면 비용 없이 대량 전사가 가능하다.

```bash
# Whisper 설치 (Python 3.9+ 필요)
pip install openai-whisper

# 단일 파일 전사 (large-v3 모델, 한국어 지정)
whisper interview_kaeri_250702.m4a \
  --model large-v3 \
  --language ko \
  --output_format txt \
  --output_dir ./transcripts/

# 여러 파일 일괄 전사
for f in ./recordings/*.m4a; do
  whisper "$f" --model large-v3 --language ko \
    --output_format all --output_dir ./transcripts/
done
```

`--output_format all` 옵션을 사용하면 `.txt`, `.vtt`, `.srt`, `.tsv`, `.json` 등 다양한 형식이 동시에 생성된다. 이 중 `.json` 파일에는 각 세그먼트(segment)별 시작/종료 타임스탬프가 포함되어 있으므로, 후속 Markdown 정리 시 타임스탬프 참조에 유용하다.

```json
{
  "segments": [
    {
      "id": 0,
      "start": 0.0,
      "end": 4.32,
      "text": "기본적으로 원자력 분야에서 국제 협력은 매우 중요하지만"
    },
    {
      "id": 1,
      "start": 4.32,
      "end": 9.15,
      "text": "협력의제는 어느 한 국가가 마음대로 결정할 수 없습니다"
    }
  ]
}
```

### 3.2.3. Clova Note 활용 시 유의사항

Clova Note는 웹 인터페이스에서 녹음 파일을 업로드하면 자동으로 화자 분리(speaker diarization)와 전사를 수행한다. 다만 다음 사항에 주의해야 한다.

- **화자 수 지정**: 업로드 시 예상 화자 수를 입력하면 정확도가 향상된다. 인터뷰어 1명 + 피면접자 1~2명이 일반적이므로 2~3명으로 설정한다.
- **전문 용어 오인식**: "SMR(소형모듈원자로)", "INL(아이다호 국립연구소)", "GIF(제4세대 원자력시스템 국제포럼)" 등의 약어와 기관명은 높은 확률로 오인식된다. 이들을 사전에 목록화하여 전사 후 일괄 교정해야 한다.
- **내보내기 형식**: Clova Note의 내보내기 기능으로 텍스트를 추출한 뒤, Markdown으로 재구조화하는 과정이 필요하다.

---

## 3.3. 전사본을 Markdown 형식으로 정리하기

전사 도구가 출력한 원시 텍스트(raw text)는 화자 구분이 불완전하고, 기관별 경계가 명확하지 않으며, 오탈자가 다수 포함되어 있다. 이를 분석 가능한 Markdown 문서로 정리하는 것이 두 번째 단계이다.

### 3.3.1. 목표 형식

본 연구의 전사본(`raw/policy_250924.md`)은 다음과 같은 형식을 따른다.

```markdown
# **정책연구1-1 인터뷰 기록**

**250702 KAERI 인터뷰\_CSTG**

박경렬 교수님 00:40
그 전에 국제 협력과 관련된 이미 말씀을 이렇게 지금 인사하다 보니까 나눴는데
그 KAERI뿐만 아니라 **원자력 분야에 있어서 국제 협력에 가장 좀 중요한 부분**
그리고 현재 하고 있는 것들 중에 좀 이런 예산 변동 속에서 앞으로 좀 더 개선해야
되고 잘할 수 있는 부분 그런 것들은 좀 어떤 거라고 생각이 드세요?

전은주 박사님 01:04
기본적으로 원자력 분야에서 국제 협력은 매우 중요하지만, 협력의제는 어느 한
국가가 마음대로 결정할 수 없습니다. 예를 들어 우주나 국방 분야와 마찬가지로,
원자력도 한 나라만의 의사로는 협력 범위나 주제가 결정되지 않는 구조죠.
```

핵심 구조 요소는 다음과 같다.

| 구조 요소 | 형식 | 예시 | 역할 |
|-----------|------|------|------|
| **기관 구분** | `**날짜 기관명 인터뷰\_그룹**` | `**250702 KAERI 인터뷰\_CSTG**` | 기관별 인터뷰 경계 표시 |
| **화자 이름** | `이름 직함` | `전은주 박사님` | 발언자 식별 |
| **타임스탬프** | `MM:SS` | `01:04` | 원본 녹음 참조 위치 |
| **강조 표시** | `**굵은 글씨**` | `**원자력 분야에 있어서**` | 핵심 키워드/개념 강조 |
| **장면 전환** | `***(설명)***` | `***(장소 이동)***` | 인터뷰 맥락 변화 표시 |
| **각주** | `[^n]` | `[^1]` | 기관/용어 보충 설명 |

### 3.3.2. Claude Code를 활용한 Markdown 변환

Clova Note나 Whisper에서 추출한 원시 전사본을 위 형식으로 변환할 때, Claude Code를 활용하면 상당 부분 자동화할 수 있다. 다음은 실제 사용할 수 있는 프롬프트 예시이다.

```
다음은 Clova Note에서 추출한 KAERI 인터뷰 전사본 원문입니다.
이 텍스트를 아래 규칙에 따라 Markdown 형식으로 정리해주세요.

# 정리 규칙:
1. 문서 최상단에 `# **정책연구1-1 인터뷰 기록**` 제목을 넣으세요.
2. 각 기관 인터뷰 시작 부분에 `**날짜 기관명 인터뷰\_그룹**` 형식의 구분자를 넣으세요.
3. 각 발언 앞에 `화자이름 직함 MM:SS` 형식으로 화자와 타임스탬프를 표기하세요.
4. 핵심 개념, 기관명, 정책명은 **굵은 글씨**로 강조하세요.
5. 영문 약어가 나올 경우 처음 등장 시 풀네임을 병기하세요.
   예: INL(Idaho National Laboratory, 아이다호 국립연구소)
6. 장소 이동, 휴식 등 비발언 구간은 `***(설명)***`으로 표시하세요.
7. 구어체 특성은 최대한 보존하되, 명백한 오탈자와 반복 추임새("그 뭐 그")는
   최소한으로 정리하세요.
8. 원문의 의미를 절대 변경하지 마세요.

# 원문:
[여기에 전사본 텍스트를 붙여넣기]
```

> **주의**: Claude Code에서 대용량 파일을 처리할 때는 한 번에 전체 파일을 전달하지 말고, 기관별 또는 일정 분량(약 2,000~3,000줄)으로 나누어 순차 처리해야 한다. 이에 대해서는 3.5절에서 상세히 다룬다.

### 3.3.3. 수작업 검수 포인트

자동 변환 후 반드시 확인해야 할 항목은 다음과 같다.

1. **화자 오귀속 (speaker misattribution)**: 전사 도구가 화자를 잘못 구분한 경우, 발언 내용의 맥락을 통해 교정한다. 예를 들어, 인터뷰어(교수)의 질문과 피면접자(연구원)의 답변이 뒤바뀌는 경우가 있다.
2. **기관명/인명 오류**: "안국" → "한국", "에피라고" → "AP라고(Additional Protocol)" 등 음성 인식 오류를 교정한다.
3. **기술 용어 정확성**: "인트시브" → "intensive", "놈 세팅" → "norm setting" 등 외래어 표기를 확인한다.
4. **맥락 보존**: 구어체의 자연스러운 흐름을 유지하면서도, 분석 시 의미 파악이 어려운 부분에 보충 설명을 괄호로 추가한다.

---

## 3.4. CSV 형식으로 구조화하기

Markdown 전사본은 인간이 읽기에는 적합하지만, 체계적 코딩(systematic coding)과 빈도 분석(frequency analysis)에는 적합하지 않다. 이를 위해 정형화된 CSV(Comma-Separated Values) 형식으로 변환해야 한다.

### 3.4.1. 열 구조 설계

본 연구의 CSV 파일(`raw/Policy_merged.csv`)은 다음과 같은 4열 구조를 사용한다.

| 열 이름 | 설명 | 예시 값 |
|---------|------|---------|
| `기관` | 발언이 속한 출연연 약칭 | `KAERI`, `ETRI`, `KRISO` |
| `대분류 테마` | 1차 LLM 분석에서 도출된 5개 상위 주제 중 하나 | `1. 국제협력의 동인 및 목표` |
| `중분류 테마` | 대분류 내 세부 주제 | `기술 확보 및 R&D 역량 강화` |
| `해당 문장 발췌` | 원문에서 발췌한 인용문 (큰따옴표로 감싸짐) | `"차세대 원자로 개발하는 데 있어서..."` |

이 구조의 설계 원칙은 다음과 같다.

- **기관 단위 추적성 (traceability)**: 모든 발췌문이 기관에 귀속되어 기관 간 비교 분석이 가능하다.
- **위계적 분류 (hierarchical classification)**: 대분류-중분류의 2단계 위계를 통해 "전체 조망(bird's eye view)"과 "세부 탐색(drill-down)" 모두 가능하다.
- **원문 보존 (verbatim preservation)**: 발췌문은 원문 그대로를 보존하여 분석의 감사 추적(audit trail)을 확보한다.

### 3.4.2. 실제 CSV 데이터의 구조

파일의 처음 몇 행을 살펴보면 다음과 같다.

```csv
기관,대분류 테마,중분류 테마,해당 문장 발췌
KAERI,1. 국제협력의 동인 및 목표,기술 확보 및 R&D 역량 강화,"차세대 원자로 개발하는 데 있어서 협력할 수 있는 분야가 되게 많았다고 치면"
KAERI,1. 국제협력의 동인 및 목표,자원 확보 및 접근성,해당 없음
KAERI,1. 국제협력의 동인 및 목표,글로벌 공동 문제 해결,"저희가 또 중요하게 하는 게 우주용 원자로를 요새 많이 하거든요."
```

주목할 점은 다음과 같다.

- **"해당 없음" 처리**: 특정 기관에서 해당 중분류 테마에 대한 발언이 없는 경우 "해당 없음"으로 표기한다. 이는 데이터의 부재(absence) 자체가 분석적 의미를 지닐 수 있기 때문이다.
- **인용 부호**: 발췌문 내에 쉼표가 포함될 수 있으므로 큰따옴표(`"`)로 감싼다.
- **대분류 번호**: `1.`, `2.`, `3.`, `4.`, `5.` 접두어를 통해 5개 대분류의 순서를 명시한다.

### 3.4.3. 대분류 5개 체계 (1차 LLM 분석 기반)

CSV 구조화에 사용된 5개 대분류는 `raw/llm_analysis_1003.md`에서 1차 LLM 분석을 통해 도출된 것이다.

```
1. 국제협력의 동인 및 목표
   └ 기술 확보 및 R&D 역량 강화, 자원 확보 및 접근성, 글로벌 공동 문제 해결,
     국제 표준 선도 및 규범 형성, 해외 시장 진출 및 기술 사업화,
     과학기술 외교 및 국가 위상 제고

2. 국제협력의 유형 및 방식
   └ 공동 연구, ODA, 국제기구/다자협의체 활동, 인프라 활용 및 공유,
     인력 교류, MOU 및 기관 간 파트너십, 기술이전 및 라이선싱

3. 기관별 특성 및 협력 환경
   └ 기관 임무 기반 협력, 정부 부처와의 관계, 내부 추진 체계, 연구 분야의 특성

4. 국제협력의 주요 애로사항 및 장애물
   └ 정책의 비일관성 및 불안정성, 단기·성과 위주 정책, 예산 문제,
     행정·제도적 한계, 연구자 차원의 어려움

5. 정책 제언 및 발전 방향
   └ 장기적·안정적 정책 추진, 범부처 컨트롤타워 구축,
     연구 현장 중심의 지원 강화, 전략적 파트너십 및 분야 선택,
     정보 공유 플랫폼 구축
```

### 3.4.4. Claude Code를 활용한 CSV 변환 프롬프트

Markdown 전사본에서 CSV로 변환하는 과정을 Claude Code로 자동화할 때 사용하는 프롬프트의 예시이다.

```
아래는 KAERI 인터뷰 전사본의 일부입니다. 다음 규칙에 따라 CSV 형식으로 변환해주세요.

# CSV 열 구조:
기관,대분류 테마,중분류 테마,해당 문장 발췌

# 대분류 및 중분류 체계:
1. 국제협력의 동인 및 목표
   - 기술 확보 및 R&D 역량 강화
   - 자원 확보 및 접근성
   - 글로벌 공동 문제 해결
   - 국제 표준 선도 및 규범 형성
   - 해외 시장 진출 및 기술 사업화
   - 과학기술 외교 및 국가 위상 제고

2. 국제협력의 유형 및 방식
   [이하 생략 — 전체 체계를 제공]

# 변환 규칙:
1. 기관명은 약칭(KAERI, ETRI 등)으로 표기합니다.
2. 각 발언을 의미 단위(semantic unit)로 분절합니다.
   - 하나의 발언이 여러 테마에 해당할 경우 각각 별도 행으로 분리합니다.
3. 해당 문장 발췌는 원문을 그대로 인용하되, 큰따옴표로 감쌉니다.
4. 특정 중분류 테마에 해당하는 발언이 없으면 "해당 없음"으로 표기합니다.
5. 발언이 어떤 대분류-중분류에도 속하지 않으면 무시합니다.

# 전사본:
[전사본 텍스트]
```

이 프롬프트를 기관별로 반복 실행하면, 기관별 CSV 파일이 생성되고 이를 병합(merge)하여 최종 `Policy_merged.csv`를 만든다.

```bash
# 기관별 CSV 병합 (헤더 중복 제거)
head -1 kaeri.csv > Policy_merged.csv
for f in kaeri.csv etri.csv kriso.csv kribb.csv nigt.csv kimm.csv \
         kigam.csv kriss.csv kier.csv krict.csv kari.csv nst.csv \
         kisti.csv kict.csv kfe.csv; do
  tail -n +2 "$f" >> Policy_merged.csv
done
```

---

## 3.5. 대용량 데이터 처리 전략

본 연구의 전사본은 21,011줄, 약 1.4MB에 달한다. LLM의 컨텍스트 윈도우(context window) 제한과 처리 품질을 고려할 때, 이 분량을 한 번에 처리하는 것은 현실적이지 않다. 다음과 같은 전략이 필요하다.

### 3.5.1. 청크 분할 (Chunking) 전략

가장 효과적인 분할 단위는 **기관(institution)**이다. 각 인터뷰의 시작점은 Markdown에서 `**날짜 기관명 인터뷰**` 형식의 구분자로 식별할 수 있다.

```python
# 기관별 청크 분할 스크립트 예시
import re

with open("raw/policy_1027_revised.md", "r", encoding="utf-8") as f:
    content = f.read()

# 기관 인터뷰 시작 패턴으로 분할
chunks = re.split(r'(?=\*\*\d{6}\s+\w+\s+인터뷰)', content)

institutions = [
    "KAERI", "ETRI", "KRISO", "KRIBB", "NIGT",
    "KIMM", "KIGAM", "KRISS", "KIER", "KRICT",
    "KARI", "NST", "KISTI", "KICT", "KFE"
]

for i, chunk in enumerate(chunks):
    if chunk.strip():  # 빈 청크 제외
        filename = f"chunks/chunk_{i:02d}_{institutions[i]}.md"
        with open(filename, "w", encoding="utf-8") as f:
            f.write(chunk)
        print(f"Written: {filename} ({len(chunk.splitlines())} lines)")
```

기관별 분량은 기관에 따라 다르지만 (약 500~2,000줄), 대부분 LLM의 단일 처리 범위 내에 들어온다. 만약 단일 기관의 분량이 지나치게 큰 경우(예: 2,000줄 초과), 다시 화자 발언 단위 혹은 타임스탬프 기준으로 세분화한다.

### 3.5.2. 순차 처리 (Sequential Processing) 워크플로우

Claude Code에서 기관별 청크를 순차적으로 처리하는 실제 워크플로우는 다음과 같다.

```
Step 1: KAERI 청크 → CSV 변환 → kaeri.csv 저장 → 품질 검토
Step 2: ETRI 청크 → CSV 변환 → etri.csv 저장 → 품질 검토
  ...
Step 15: KFE 청크 → CSV 변환 → kfe.csv 저장 → 품질 검토
Step 16: 전체 CSV 병합 → Policy_merged.csv
Step 17: 최종 품질 검증 (행 수 확인, 기관별 분포 검토)
```

각 단계에서 Claude Code에 전달하는 프롬프트에는 **이전 기관 처리 시 확립된 분류 기준**을 함께 포함시켜야 한다. 이를 통해 기관 간 코딩 일관성(inter-institutional coding consistency)을 유지할 수 있다.

```
[이전 기관 처리 요약]
- KAERI: 총 139행, 대분류 1에 27건, 대분류 2에 34건, ...
- ETRI: 총 112행, 대분류 1에 18건, ...

[현재 처리 대상: KRISO]
아래 전사본을 동일한 기준으로 CSV 변환해주세요. 이전 기관에서 확립된
중분류 카테고리를 일관되게 적용하되, 새로운 테마가 발견되면 별도로
표기해주세요.
```

### 3.5.3. GitHub을 활용한 버전 관리

대용량 순차 처리에서 가장 중요한 것은 각 단계의 결과를 추적할 수 있도록 버전 관리(version control)하는 것이다.

```bash
# 기관별 처리 완료 시마다 커밋
git add raw/chunks/chunk_01_KAERI.md raw/csv/kaeri.csv
git commit -m "KAERI 인터뷰 CSV 변환 완료 (139행)"

git add raw/chunks/chunk_02_ETRI.md raw/csv/etri.csv
git commit -m "ETRI 인터뷰 CSV 변환 완료 (112행)"

# 최종 병합 후
git add raw/Policy_merged.csv
git commit -m "전체 기관 CSV 병합 완료 (1,527행, 15개 기관)"
```

이렇게 하면 특정 기관의 변환 결과에 문제가 발견되었을 때 해당 커밋으로 돌아가 재처리할 수 있다.

---

## 3.6. 데이터 품질 관리

### 3.6.1. 오탈자 수정 (Error Correction)

전사 과정에서 발생하는 대표적인 오류 유형과 교정 방법은 다음과 같다.

| 오류 유형 | 원인 | 원문 예시 | 교정 후 | 교정 방법 |
|-----------|------|-----------|---------|-----------|
| **동음이의어** | 음성 인식 한계 | "안국에 들어왔다" | "한국에 들어왔다" | 맥락 기반 수작업 |
| **외래어 오인식** | 한국어 모델 한계 | "에피라고" | "AP라고" | 원본 녹음 대조 |
| **기관명 오류** | 약어/고유명사 | "다글로" | "DAGLO" | 기관 목록 대조 |
| **불완전 문장** | 발화 도중 끊김 | "그래서 저희가—" | 맥락상 보충 필요 | `[...]` 표시 또는 보충 |
| **반복/추임새** | 구어체 특성 | "그 뭐 그 약간" | 최소 정리 | 의미 영향 시만 정리 |

Claude Code를 활용한 일괄 오탈자 교정 프롬프트 예시:

```
아래 전사본에서 다음 유형의 오류를 찾아 교정해주세요.
교정한 부분은 [교정: 원문 → 수정] 형식으로 표시해주세요.

1. 기관명 오류 (참고 목록: KAERI, ETRI, KRISO, KRIBB, NIGT, KIMM, KIGAM,
   KRISS, KIER, KRICT, KARI, NST, KISTI, KICT, KFE, INL, ANL, IAEA, NEA, GIF)
2. 영문 약어/외래어 오인식 (예: SMR, SFR, MOU, DOE, ODA, IPR)
3. 문맥상 명백한 동음이의어 오류
4. 의미 변경 없이 가독성을 해치는 반복 표현

원문의 구어체 특성(어미, 어순, 생략 등)은 절대 수정하지 마세요.
발언자의 의견이나 주장을 절대 변경하지 마세요.
```

### 3.6.2. 맥락 보존 (Context Preservation)

질적 연구에서 발췌문은 단독으로 해석되어서는 안 되며, 반드시 발화 맥락 속에서 이해되어야 한다. CSV 변환 시 맥락이 손실될 위험이 있으므로 다음 원칙을 지킨다.

1. **충분한 길이의 발췌**: 단문("네")이 아닌, 주장이나 사례의 완결된 단위를 발췌한다.
2. **대화 상대의 질문 포함**: 피면접자의 답변만 발췌하지 말고, 해당 답변을 유도한 인터뷰어의 질문도 기록에 남긴다. 이는 전사본 원문(`policy_250924.md`)에서 항상 참조 가능해야 한다.
3. **기관 내 위치 참조**: 발췌문이 전사본 내 어디에 위치하는지를 추적할 수 있도록 타임스탬프 또는 라인 번호를 별도로 기록하면 감사 추적이 가능해진다.

### 3.6.3. 인용 정확성 확인 (Quotation Accuracy Verification)

CSV 파일의 `해당 문장 발췌` 열에 들어간 인용문이 원본 전사본의 텍스트와 정확히 일치하는지 확인하는 것은 연구의 신뢰성(trustworthiness)을 담보하는 핵심 과정이다.

Claude Code를 활용한 자동 검증 접근법:

```
다음 두 파일을 대조해주세요.

1. 원본 전사본: raw/policy_1027_revised.md (KAERI 섹션)
2. CSV 발췌문: raw/Policy_merged.csv에서 기관="KAERI"인 행들

각 CSV 행의 "해당 문장 발췌" 값이 원본 전사본에 존재하는지 확인하고,
다음 항목을 보고해주세요.

- 원본과 정확히 일치하는 행 수
- 사소한 차이(공백, 구두점)가 있는 행 수와 해당 행 목록
- 원본에서 찾을 수 없는 행 수와 해당 행 목록 (★ 이 경우 반드시 확인 필요)
```

또한, 프로그래밍 방식으로도 검증할 수 있다.

```python
import csv

# 원본 전사본 로드
with open("raw/policy_1027_revised.md", "r", encoding="utf-8") as f:
    transcript = f.read()

# CSV 발췌문 검증
with open("raw/Policy_merged.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    not_found = []
    for i, row in enumerate(reader, start=2):
        excerpt = row["해당 문장 발췌"].strip('"')
        if excerpt != "해당 없음" and excerpt not in transcript:
            not_found.append((i, row["기관"], excerpt[:50]))

print(f"원본에서 미발견 발췌문: {len(not_found)}건")
for line_num, inst, text in not_found:
    print(f"  행 {line_num} [{inst}]: {text}...")
```

---

## 3.7. 데이터 준비 과정 요약

아래 표는 본 연구에서 실제로 수행한 데이터 준비 과정의 전체 흐름을 정리한 것이다.

| 단계 | 산출물 | 파일 | 크기 | 비고 |
|------|--------|------|------|------|
| 1. 녹음 | `.m4a` 파일 15개 | (로컬 보관) | — | 15개 기관 인터뷰 |
| 2. 전사 | 원시 전사본 | `raw/policy_250924.md` | 968KB, 14,942줄 | Clova Note + 수작업 검수 |
| 3. 1차 LLM 분석 | 분석 보고서 | `raw/llm_analysis_1003.md` | 11KB, 152줄 | 5대 대분류 + 중분류 도출 |
| 4. 전사본 보완 | 보완된 전사본 | `raw/policy_1027_revised.md` | 1.4MB, 21,011줄 | 맥락 보충, 오탈자 교정 |
| 5. CSV 구조화 | 코딩된 발췌문 | `raw/Policy_merged.csv` | 483KB, 1,527행 | 기관 x 대분류 x 중분류 x 발췌 |

---

# 4. Phase 2 — 코드북 개발 (Codebook Development)

## 4.1. 코드북이란 무엇인가

### 4.1.1. 질적 연구에서의 코드북의 역할

코드북(codebook)은 질적 연구에서 데이터에 부여할 코드(code)의 체계적 목록과 각 코드의 정의, 적용 규칙, 예시를 기술한 문서이다. 전통적 질적 연구에서 코드북은 다음과 같은 역할을 수행한다.

- **분류 기준의 표준화 (Standardization)**: 여러 코더(coder)가 동일한 기준으로 데이터를 분류할 수 있도록 한다.
- **연구의 투명성 확보 (Transparency)**: 연구자가 어떤 기준으로 데이터를 해석했는지를 제3자가 검증할 수 있게 한다.
- **코더 간 신뢰도 (Inter-coder reliability)**: 복수의 코더가 독립적으로 동일한 코드를 부여하는 비율을 측정하기 위한 기반이 된다.
- **이론적 프레임워크의 조작화 (Operationalization)**: 추상적인 이론 개념을 구체적인 분류 범주로 변환한다.

### 4.1.2. LLM 에이전트의 "분류 기준서"로서의 코드북

LLM(Large Language Model)을 질적 코딩의 도구로 활용할 때, 코드북은 단순한 참고 문서를 넘어 **에이전트의 동작을 규정하는 지시서(instruction set)**가 된다. 본 연구의 코드북 파일(`codebook.md`)은 바로 이 목적으로 설계되었으며, 문서 제목 자체가 이를 명시하고 있다.

> **"출연연 국제협력 인터뷰 분석 코드북 (GRI-IC-Codebook v1.0)**
> **(Opal 에이전트용 필터 기준 정의서)"**

여기서 "Opal 에이전트"는 Claude Code를 기반으로 구축한 분석 에이전트의 명칭이며, "필터 기준 정의서"라는 부제는 이 코드북이 LLM에게 "어떤 텍스트를 어떤 범주로 분류할 것인가"를 지시하는 문서임을 의미한다.

LLM 에이전트용 코드북은 전통적 코드북과 비교하여 다음과 같은 추가적 요건을 충족해야 한다.

| 요건 | 전통적 코드북 | LLM 에이전트용 코드북 |
|------|-------------|---------------------|
| **코드 명명** | 자유 형식 (한국어 서술형 가능) | 기계 판독 가능한 형식 (`대문자_언더스코어`) |
| **정의의 명확성** | "연구자가 합리적으로 판단 가능한 수준" | 모호성을 최소화한 명시적 규칙 필요 |
| **예시 포함** | 선택 사항 | 필수적 (few-shot prompting 역할) |
| **코드 간 경계** | 코더 교육으로 보완 가능 | 코드 정의 자체로 경계가 명확해야 함 |
| **다국어 대응** | 해당 없음 | 한국어 정의 + 영어 코드명 병기 필수 |
| **코드 부여 규칙** | 암묵적 (교육 중 전달) | 명시적 규칙 기술 필요 |

이러한 차이를 반영하여 본 코드북은 다음과 같은 핵심 지시문으로 시작한다.

> "에이전트는 각 텍스트 세그먼트에 대해 **가장 구체적인 주제 코드 1개 이상**과 **맥락 코드 1개**를 부여해야 합니다."

이 한 문장이 LLM 에이전트의 코딩 행동을 규정하는 최상위 규칙(meta-rule)이다.

---

## 4.2. 코드북 설계의 원칙

### 4.2.1. MECE 원칙 (Mutually Exclusive, Collectively Exhaustive)

코드북 설계의 가장 기본적인 원칙은 **상호배타성(Mutual Exclusivity)**과 **전체포괄성(Collective Exhaustiveness)**이다.

- **상호배타성**: 각 코드는 다른 코드와 명확히 구분되어야 한다. 동일한 발췌문이 두 개의 유사한 코드 중 어디에 해당하는지 판단할 수 없다면, 코드 정의가 불충분한 것이다.
- **전체포괄성**: 분석 대상이 되는 모든 발언이 적어도 하나의 코드에 분류될 수 있어야 한다. 어떤 코드에도 속하지 않는 발언이 발견된다면, 코드 체계를 확장해야 한다.

그러나 질적 연구에서 완벽한 상호배타성은 현실적으로 달성하기 어렵다. 하나의 발언이 여러 주제에 걸치는 것은 자연스러운 현상이다. 이 문제를 본 코드북에서는 두 가지 방법으로 해결한다.

1. **복수 코드 부여 허용**: "가장 구체적인 주제 코드 **1개 이상**"이라는 규칙을 통해, 하나의 텍스트 세그먼트에 여러 주제 코드를 부여할 수 있도록 한다.
2. **맥락 코드의 분리**: 발언의 "주제(what)"와 "성격(how)"을 별도의 차원(dimension)으로 분리하여, 주제 코드 간의 중복을 줄인다.

### 4.2.2. 위계적 구조 (Hierarchical Structure)

코드를 평면적(flat)으로 나열하면 70개 이상의 코드를 관리하기 어렵다. 본 코드북은 다음과 같은 위계(hierarchy)를 사용한다.

```
차원 (Dimension)
  └ 대분류 (Major Category)
      └ 중분류 (Sub-category, 일부 대분류에만 존재)
          └ 소분류 코드 (Individual Code)
```

예를 들어, "장애물 및 도전과제(OBSTACLES_CHALLENGES)" 대분류의 경우:

```
1.3. OBSTACLES_CHALLENGES
  ├── OBS_FUNDING (예산 문제)
  │     ├── OBS_FUND_INSUFFICIENT_CORE (핵심 예산 부족)
  │     ├── OBS_FUND_LACK_NEW_PROJECT (신규 예산 부족)
  │     ├── OBS_FUND_INSTABILITY (예산 불안정성/삭감)
  │     ├── OBS_FUND_CYCLE_MISMATCH (예산 주기 불일치)
  │     └── OBS_FUND_ALLOCATION_RIGIDITY (예산 집행 경직성)
  ├── OBS_IPR_LEGAL_CONTRACTUAL (IPR/법률/계약 문제)
  │     ├── OBS_IPR_DISPUTES_NEGOTIATION
  │     ├── OBS_IPR_LACK_LEGAL_SUPPORT
  │     └── OBS_CONTRACT_COMPLEXITY_DELAY
  ├── OBS_POLICY_GOVERNANCE (정책/거버넌스 문제)
  │     ├── OBS_POL_INCONSISTENCY_SHORT_TERMISM
  │     ├── OBS_POL_LACK_OF_STRATEGY
  │     ├── OBS_POL_FRAGMENTATION_LACK_COORDINATION
  │     ├── OBS_POL_TOP_DOWN_APPROACH
  │     └── OBS_POL_BUREAUCRACY_JOB_ROTATION
  ├── OBS_GEOPOLITICAL_PARTNER (지정학/파트너 문제)
  │     ├── OBS_GEO_RESTRICTIONS_CONTROLS
  │     ├── OBS_GEO_PARTNER_COUNTRY_RISKS
  │     ├── OBS_GEO_TECH_COMPETITION_NATIONALISM
  │     └── OBS_PARTNER_RELATION_DIFFICULTY
  ├── OBS_HUMAN_RESOURCES (인력 문제)
  │     ├── OBS_HR_LACK_OF_EXPERTISE
  │     ├── OBS_HR_LACK_OF_INCENTIVES_RECOGNITION
  │     └── OBS_HR_WORKLOAD_BURDEN
  └── OBS_INSTITUTIONAL_SYSTEMIC (기관/시스템 문제)
        ├── OBS_INST_MANDATE_CULTURE_MISMATCH
        ├── OBS_INST_INTERNAL_SYSTEMS_INEFFICIENCY
        ├── OBS_INST_LACK_INFO_SHARING_KM
        └── OBS_INST_INFRA_SUPPORT_LACK
```

이 위계 구조의 장점은 다음과 같다.

- **분석 유연성**: 대분류 수준의 거시적 패턴("장애물 전체 언급 빈도")과 소분류 수준의 세밀한 패턴("예산 주기 불일치를 언급한 기관")을 모두 추출할 수 있다.
- **코드 관리 용이성**: 새로운 코드를 추가할 때 적절한 위치를 바로 파악할 수 있다.
- **LLM 처리 효율**: 에이전트에게 "가장 구체적인" 코드를 선택하도록 지시함으로써, 자연스럽게 최하위 소분류 코드가 우선 선택된다.

### 4.2.3. 명확한 정의 (Clear Definitions)

각 코드는 다음 세 가지 요소를 반드시 포함해야 한다.

1. **코드명 (Code name)**: 기계 판독 가능한 영문 식별자
2. **한국어 요약 (Korean label)**: 괄호 안에 간결한 한국어 설명
3. **상세 정의 (Detailed definition)**: 해당 코드가 적용되는 발언의 특성을 구체적으로 기술

본 코드북의 실제 사례:

```markdown
* `OBS_FUND_CYCLE_MISMATCH`: (예산 주기 불일치) 파트너 국가와의 회계연도
  또는 예산 확보 시기 불일치.
```

이 정의만으로도 다음과 같은 발언에 코드를 부여할 수 있다.

> "미국의 예산 시기, 순기가 너무 다르고 그래서 저희가 사실은 지난번 우리 국제 협력 예산을 많이 늘린대서 이거에 매치해서 한번 해보자라고 했었는데 미국에서도..." — KAERI 전은주 박사

---

## 4.3. 1차원: 주제 코드 (Hierarchical Thematic Codes) 체계 설계

### 4.3.1. 대분류 8개 체계

코드북의 1차원(Dimension 1)인 주제 코드는 8개 대분류로 구성된다. 이는 Phase 1에서 도출된 5개 대분류 체계를 확장하고 재구성한 것이다.

| 번호 | 대분류 코드명 | 한국어 명칭 | 소분류 수 | 역할 |
|------|-------------|-----------|----------|------|
| 1.1 | `COLLABORATION_DRIVERS` | 국제협력 동인 및 목표 | 9 | 왜 국제협력을 하는가? |
| 1.2 | `COLLABORATION_TYPES_MODALITIES` | 국제협력 유형 및 방식 | 8 | 어떤 형태로 협력하는가? |
| 1.3 | `OBSTACLES_CHALLENGES` | 장애물 및 도전과제 | 24 | 무엇이 협력을 방해하는가? |
| 1.4 | `SUCCESS_FACTORS_ENABLERS` | 성공 요인 및 촉진제 | 7 | 무엇이 성공을 이끄는가? |
| 1.5 | `FAILURE_FACTORS_LESSONS` | 실패 요인 및 교훈 | 4 | 무엇이 실패를 초래했는가? |
| 1.6 | `POLICY_NEEDS_RECOMMENDATIONS` | 정책 요구 및 제언 | 9 | 무엇이 개선되어야 하는가? |
| 1.7 | `INSTITUTIONAL_SECTORAL_CONTEXT` | 기관/분야 맥락 | 4 | 어떤 배경에서 말하는가? |
| 1.8 | `POLICY_EVENT_RESPONSE` | 특정 정책/사건 반응 | 3 | 특정 사건에 어떻게 반응했는가? |

Phase 1의 5개 대분류에서 8개 대분류로 확장된 핵심 변화는 다음과 같다.

- **"기관별 특성 및 협력 환경"** → `INSTITUTIONAL_SECTORAL_CONTEXT`(1.7)로 재명명하고, 기관의 고유 맥락 정보에 집중하도록 범위를 조정
- **"국제협력의 주요 애로사항 및 장애물"** → `OBSTACLES_CHALLENGES`(1.3) + `FAILURE_FACTORS_LESSONS`(1.5)로 분리. 현재 진행 중인 장애물과 과거의 실패 사례를 구분
- **"정책 제언 및 발전 방향"** → `POLICY_NEEDS_RECOMMENDATIONS`(1.6)로 유지하되, 성공 요인을 `SUCCESS_FACTORS_ENABLERS`(1.4)로 별도 분리
- **`POLICY_EVENT_RESPONSE`(1.8)** 신설: 2023년 예산 조정, 특정 정책 지침 등 시의적(temporal) 사건에 대한 반응을 별도로 포착

이 확장은 1차 LLM 분석과 파일럿 코딩(pilot coding) 과정에서 드러난 문제를 해결하기 위한 것이다. 예를 들어, "SFR 예산이 70억에서 7억으로 줄었다"는 발언은 "예산 문제(장애물)"인 동시에 "실패 사례(교훈)"이며, "2023년 예산 변동에 대한 반응"이기도 하다. 1차원을 8개로 세분화하고 복수 코드 부여를 허용함으로써 이러한 다면적 발언을 보다 정확하게 포착할 수 있다.

### 4.3.2. 소분류 70+개의 설계 과정

70개 이상의 소분류 코드는 다음과 같은 반복적 과정(iterative process)을 통해 도출되었다.

**1단계: 귀납적 코드 발견 (Inductive Code Discovery)**

1차 LLM 분석(`raw/llm_analysis_1003.md`)에서 도출된 중분류 테마를 기반으로 초기 코드 목록을 작성한다. 예를 들어, "예산 문제"라는 중분류가 있었으나, 실제 인터뷰를 살펴보면 예산 문제의 성격이 매우 다양함을 알 수 있다.

```
[1차 분석의 "예산 문제"]
  "이번에 국제 협력 예산이 엄청 많이 늘었는데 저희는 예산이 하나도 안 늘어"
  → 신규 예산 확보의 어려움 (OBS_FUND_LACK_NEW_PROJECT)

  "SFR 사업도 그러니까 원래 예산이 70억짜리가 7억이 되었거든요"
  → 기존 예산 삭감/불안정 (OBS_FUND_INSTABILITY)

  "미국의 예산 시기, 순기가 너무 다르고"
  → 파트너와의 예산 주기 불일치 (OBS_FUND_CYCLE_MISMATCH)
```

이처럼 하나의 중분류가 여러 개의 구체적 코드로 세분화된다.

**2단계: 연역적 프레임워크 적용 (Deductive Framework Application)**

기존 국제협력 연구 문헌, 과학기술 정책 이론 등에서 도출된 개념을 참고하여 코드 체계의 논리적 완결성을 검증한다. 예를 들어, "지정학적 제약"과 "기술 패권 경쟁"은 국제관계(International Relations) 문헌에서 구분되는 개념이므로, 별도의 코드(`OBS_GEO_RESTRICTIONS_CONTROLS` vs. `OBS_GEO_TECH_COMPETITION_NATIONALISM`)로 분리한다.

**3단계: 반복적 검증과 조정 (Iterative Validation)**

초기 코드 목록을 가지고 실제 데이터의 일부(파일럿 샘플)에 적용하면서, 코드의 추가, 수정, 병합, 삭제를 반복한다. 이 과정은 4.5절에서 상세히 다룬다.

### 4.3.3. 코드 명명 규칙 (Naming Convention)

모든 코드명은 다음 규칙을 따른다.

```
[대분류 접두어]_[구체적 개념]
```

| 접두어 | 의미 | 예시 |
|--------|------|------|
| `CD_` | Collaboration Drivers (동인) | `CD_TECH_ACCESS` |
| `CT_` | Collaboration Types (유형) | `CT_JOINT_RD` |
| `OBS_` | Obstacles (장애물) | `OBS_FUNDING` |
| `OBS_FUND_` | Obstacles - Funding (예산 장애물) | `OBS_FUND_INSTABILITY` |
| `OBS_IPR_` | Obstacles - IPR (IPR 장애물) | `OBS_IPR_DISPUTES_NEGOTIATION` |
| `OBS_POL_` | Obstacles - Policy (정책 장애물) | `OBS_POL_TOP_DOWN_APPROACH` |
| `OBS_GEO_` | Obstacles - Geopolitical (지정학 장애물) | `OBS_GEO_RESTRICTIONS_CONTROLS` |
| `OBS_HR_` | Obstacles - Human Resources (인력 장애물) | `OBS_HR_WORKLOAD_BURDEN` |
| `OBS_INST_` | Obstacles - Institutional (기관 장애물) | `OBS_INST_MANDATE_CULTURE_MISMATCH` |
| `SUC_` | Success Factors (성공 요인) | `SUC_TRUST_RELATIONSHIP` |
| `FAIL_` | Failure Factors (실패 요인) | `FAIL_EXTERNAL_SHOCKS` |
| `REC_` | Recommendations (제언) | `REC_FUNDING_STABILITY_INCREASE_FLEXIBILITY` |
| `CTX_` | Context (맥락) | `CTX_SECTOR_CHARACTERISTICS` |
| `PER_` | Policy/Event Response (정책 반응) | `PER_IMPACT_BUDGET_CHANGES_2023` |

이 명명 규칙의 장점은 다음과 같다.

1. **자동 정렬 및 필터링**: 접두어 기반으로 코드를 자동 분류/검색할 수 있다. 예를 들어, `OBS_FUND_*` 패턴으로 모든 예산 관련 장애물 코드를 검색할 수 있다.
2. **위계적 구조의 표현**: 접두어의 길이가 위계 깊이를 반영한다. `OBS_` → `OBS_FUND_` → `OBS_FUND_INSTABILITY`.
3. **언어 독립성**: 영문 코드명은 국제 학술 논문에서 그대로 사용할 수 있으며, 한국어 설명은 괄호 안에 병기하여 연구팀 내 의사소통에 활용한다.

### 4.3.4. 한국어 설명 + 영어 코드명 병기의 이유

본 코드북은 모든 코드에 대해 다음과 같은 이중 표기(bilingual notation)를 채택한다.

```markdown
* `CD_TECH_ACCESS`: (선진/보완 기술 접근) 국내에 없거나 부족한 선진 기술,
  장비, 노하우 확보 목적의 발언.
```

이중 표기를 채택한 이유는 다음과 같다.

1. **LLM 에이전트의 이해도 최적화**: Claude와 같은 LLM은 영어와 한국어를 모두 이해하지만, 코드의 의미를 양쪽 언어로 모두 제시하면 이해 정확도가 높아진다. 특히 `TECH_ACCESS`라는 영문 코드명과 "선진/보완 기술 접근"이라는 한국어 설명이 상보적으로 작용한다.
2. **코딩 출력의 표준화**: 에이전트가 코딩 결과를 출력할 때 `CD_TECH_ACCESS`라는 기계 판독 가능한 문자열을 사용하므로, 후속 정량 분석(빈도 집계, 교차 분석 등)에서 즉시 활용할 수 있다.
3. **연구 보고서의 국제화**: 한국어 보고서에서는 한국어 설명을, 영문 논문에서는 영어 코드명을 그대로 사용하여 번역 비용을 줄인다.

---

## 4.4. 2차원: 맥락/뉘앙스 코드 (Context/Nuance Codes) 설계

### 4.4.1. 10개 맥락 코드의 역할

코드북의 2차원(Dimension 2)은 발언의 주제가 아닌 **성격(nature)** 또는 **기능(function)**을 포착하기 위한 것이다. 동일한 주제(예: 예산 문제)에 대해서도 발언자가 어떤 입장에서 말하고 있는지가 다를 수 있기 때문이다.

```markdown
# 2. 맥락/뉘앙스 코드 (Context/Nuance Codes)
*(각 텍스트 세그먼트에 대해 아래 코드 중 **하나**를 부여하여 해당 발언의 성격을 명시)*

* `PROBLEM_STATED`: (문제점 제기)
* `CAUSE_ANALYSIS`: (원인 분석)
* `EFFECT_DESCRIBED`: (결과/영향 기술)
* `SOLUTION_PROPOSED`: (해결책 제안)
* `POSITIVE_EXAMPLE/FACTOR`: (긍정 사례/요인)
* `NEGATIVE_EXAMPLE/FACTOR`: (부정 사례/요인)
* `FACTUAL_REPORTING`: (사실/현황 전달)
* `OPINION_EXPRESSED`: (의견/평가 제시)
* `INST_SECTOR_SPECIFIC`: (기관/분야 특수성 강조)
* `COMPARISON`: (비교)
```

이 10개 코드는 발언의 **수사적 기능(rhetorical function)**을 분류한다. 예를 들어 동일한 주제 코드 `OBS_FUND_INSTABILITY`(예산 불안정성)가 부여된 두 발언을 비교해보면:

| 발언 | 주제 코드 | 맥락 코드 | 해석 |
|------|----------|----------|------|
| "SFR 예산이 70억에서 7억이 되었거든요" | `OBS_FUND_INSTABILITY` | `NEGATIVE_EXAMPLE/FACTOR` | 구체적 **사례**를 들어 문제를 설명 |
| "예산을 끊는 걸 너무 쉽게 생각해요" | `OBS_FUND_INSTABILITY` | `OPINION_EXPRESSED` | 예산 정책에 대한 주관적 **평가** |

맥락 코드를 통해 같은 주제에 대한 발언이라도 "이것은 사실 보고인가, 의견 표명인가, 해결책 제안인가"를 구분할 수 있다. 이는 후속 분석에서 매우 유용하다.

- **정책 보고서 작성 시**: `SOLUTION_PROPOSED` 맥락 코드가 부여된 발언만 추출하면, 인터뷰이들이 제안한 정책 방안을 효율적으로 수집할 수 있다.
- **문제 진단 시**: `CAUSE_ANALYSIS` 코드가 부여된 발언을 모으면, 연구 참여자들이 인식하는 문제의 근본 원인 구조를 파악할 수 있다.
- **사례 수집 시**: `POSITIVE_EXAMPLE/FACTOR`와 `NEGATIVE_EXAMPLE/FACTOR`를 분리하면, 성공/실패 사례집을 자동으로 구축할 수 있다.

### 4.4.2. 주제 코드 1개 이상 + 맥락 코드 1개 규칙

코드북의 핵심 규칙을 다시 확인한다.

> "에이전트는 각 텍스트 세그먼트에 대해 **가장 구체적인 주제 코드 1개 이상**과 **맥락 코드 1개**를 부여해야 합니다."

이 규칙의 구조를 도식화하면 다음과 같다.

```
텍스트 세그먼트
  ├── 주제 코드 (Dimension 1): 1개 이상 (복수 가능)
  │     예: OBS_GEO_RESTRICTIONS_CONTROLS, OBS_IPR_DISPUTES_NEGOTIATION,
  │         CD_POLICY_INFLUENCE
  └── 맥락 코드 (Dimension 2): 정확히 1개
        예: CAUSE_ANALYSIS
```

**주제 코드가 복수인 이유**: 실제 인터뷰 발언은 단일 주제만을 담지 않는다. 예를 들어 다음 코딩 예시(`qualitative/policy_1027_all_coding.md`에서 발췌)를 보면:

```
| KAERI | 전은주 박사님 | 원자력 협력은 핵무기 기술과 맞닿아 있어 미국 등
  파트너의 승인 없이 의제를 정하기 어렵고, 지재권 분쟁이 커지면서
  협력 주제가 급격히 좁아졌다고 설명했습니다. |
  `OBS_GEO_RESTRICTIONS_CONTROLS`, `OBS_IPR_DISPUTES_NEGOTIATION`,
  `CD_POLICY_INFLUENCE` | `CAUSE_ANALYSIS` |
```

이 발언은 지정학적 제약(OBS_GEO), IPR 분쟁(OBS_IPR), 정책적 영향력(CD_POLICY) 세 가지 주제를 동시에 다루고 있으며, 발언의 성격은 "원인 분석(CAUSE_ANALYSIS)"이다.

**맥락 코드가 단수인 이유**: 발언의 기본적인 수사적 기능(예: 문제 제기, 해결책 제안)은 보통 하나로 식별할 수 있다. 복수를 허용하면 코딩의 복잡도가 지나치게 높아지고 분석의 효용이 떨어진다. 가장 두드러진 기능 하나를 선택하도록 한다.

---

## 4.5. Claude Code를 활용한 반복적 코드북 개선 과정

코드북 개발은 단번에 완성되지 않는다. 초기 코드북을 작성한 후, 실제 데이터에 적용(파일럿 코딩)하고, 문제점을 발견하여 수정하는 반복적 과정(iterative refinement)을 거쳐야 한다.

### 4.5.1. 개발 프로세스 개요

```
[1단계] 1차 LLM 분석 (llm_analysis_1003.md)
   │    → 5개 대분류, 약 25개 중분류 도출
   ↓
[2단계] 초기 코드북 초안 작성
   │    → 문헌 검토 + 1차 분석 결과 통합
   │    → 8개 대분류, 약 50개 소분류
   ↓
[3단계] 파일럿 코딩 (Pilot Coding)
   │    → 2~3개 기관의 인터뷰 50개 세그먼트에 코드북 적용
   │    → 문제점 식별: 코드 부족, 코드 중복, 모호한 경계
   ↓
[4단계] 코드북 1차 수정
   │    → 코드 추가 (24개), 코드 병합 (3건), 정의 명확화 (12건)
   ↓
[5단계] 확장 파일럿 코딩
   │    → 전체 15개 기관 중 5개 기관의 100개 세그먼트에 적용
   │    → 추가 문제점 식별 및 수정
   ↓
[6단계] 코드북 최종 확정 (codebook.md = GRI-IC-Codebook v1.0)
   │    → 8개 대분류, 68개 주제 코드 + 10개 맥락 코드
   ↓
[7단계] 전체 데이터 코딩
         → 1,527개 세그먼트 × 주제 코드(복수) + 맥락 코드(단수)
```

### 4.5.2. 파일럿 코딩 프롬프트 예시

**초기 파일럿 코딩 프롬프트**:

```
당신은 질적 연구 코딩 전문가입니다. 아래 코드북에 따라 인터뷰 발췌문을 코딩해주세요.

# 코드북:
[codebook.md 전문을 여기에 삽입]

# 코딩 규칙:
1. 각 발췌문에 대해 가장 구체적인 주제 코드를 1개 이상 부여하세요.
2. 각 발췌문에 대해 맥락 코드를 정확히 1개 부여하세요.
3. 코드 부여가 어렵거나 적절한 코드가 없다고 판단되면,
   [FLAG: 코드 부족] 또는 [FLAG: 경계 모호]로 표시하세요.

# 출력 형식:
| 기관 | 화자 | 발췌문 | 주제 코드 | 맥락 코드 | 비고 |

# 대상 발췌문 (50개):
[발췌문 목록]
```

**개선 사항 식별 프롬프트**:

```
위 파일럿 코딩 결과를 검토하고 다음 사항을 보고해주세요.

1. [FLAG] 표시가 된 항목의 목록과, 각각에 대해 코드북에 어떤 수정이 필요한지
2. 하나의 주제 코드만 부여된 경우 중, 실제로는 복수 코드가 더 적절했을 항목
3. 동일한 발언에 대해 2개 이상의 매우 유사한 코드가 후보로 경합하는 경우
   (= 코드 간 경계가 모호한 경우)
4. 어떤 코드에도 매핑되지 않는 발언의 패턴 (= 새로운 코드 후보)
5. 빈도가 0인 코드 (= 삭제 또는 병합 후보)

위 분석을 바탕으로 코드북 수정안을 제안해주세요.
수정 유형: [추가], [병합], [삭제], [정의 수정], [이름 변경]
```

### 4.5.3. 반복적 개선의 실제 사례

**사례 1: 코드 추가 — `OBS_FUND_CYCLE_MISMATCH` 탄생**

초기 코드북에는 `OBS_FUNDING`이라는 단일 코드만 있었다. 파일럿 코딩에서 다음과 같은 발언이 식별되었다.

> "미국의 예산 시기, 순기가 너무 다르고 우리가 갑자기 끊겠다 그래서 그런 것들이 굉장히 많아서"

이 발언은 단순히 "예산이 부족하다"(OBS_FUND_INSUFFICIENT_CORE)도 아니고, "예산이 불안정하다"(OBS_FUND_INSTABILITY)도 아닌, **파트너 국가와의 회계연도/예산 주기 불일치**라는 독자적 문제를 지적한다. 이에 따라 `OBS_FUND_CYCLE_MISMATCH`를 새로 추가하였다.

**사례 2: 코드 병합 — `CD_MARKET_ACCESS`와 `CD_COMMERCIALIZATION`**

초기에 "해외 시장 접근"과 "기술 상용화"를 별도 코드로 분리했으나, 파일럿 코딩에서 두 개념이 거의 항상 함께 언급되는 것으로 나타났다. 예:

> "기술 수출, 라이선싱, 기술 이전, 합작 투자(JV) 등 상업적 활동"

이에 두 코드를 `CD_MARKET_ACCESS / CD_COMMERCIALIZATION`으로 병합하고, 슬래시(`/`)를 사용하여 두 개념의 결합을 표시하였다.

**사례 3: 정의 명확화 — `OPINION_EXPRESSED` vs. `PROBLEM_STATED`**

파일럿 코딩에서 "솔직히 말씀을 드리면 우리나라 관료들이 너무 전문성이 떨어지고요"라는 발언에 대해 `OPINION_EXPRESSED`와 `PROBLEM_STATED` 중 어느 것을 부여할지 혼란이 있었다. 이에 맥락 코드의 적용 기준을 다음과 같이 명확화하였다.

> - `PROBLEM_STATED`: 문제, 어려움, 도전과제, 부정적 측면을 **명시적으로** 언급
> - `OPINION_EXPRESSED`: 발언자의 **주관적인** 견해, 평가, 판단, 신념을 표현

위 발언은 "전문성이 떨어진다"는 주관적 평가이므로 `OPINION_EXPRESSED`가 더 적절하다. 반면 "예산이 70억에서 7억이 되었거든요"는 객관적 문제 상황을 기술한 것이므로 `PROBLEM_STATED` 또는 `NEGATIVE_EXAMPLE/FACTOR`가 적합하다.

### 4.5.4. Claude Code를 활용한 코드북 자동 검증

코드북 확정 후, 전체 데이터에 적용하기 전에 코드북 자체의 내적 일관성(internal consistency)을 검증하는 것이 중요하다.

```
다음 코드북을 검토하고 아래 항목에 대해 분석해주세요.

[codebook.md 전문]

# 검토 항목:
1. MECE 위반 여부: 두 개 이상의 코드 정의가 실질적으로 겹치는 경우를 식별
2. 명명 규칙 일관성: 접두어 규칙이 일관되게 적용되었는지 확인
3. 정의 완결성: 정의가 너무 짧거나 모호하여 코딩 판단에 어려움을 줄 수 있는 코드 식별
4. 위계적 균형: 특정 대분류에 코드가 지나치게 많거나 적은 불균형 여부
5. 누락 가능성: 출연연 국제협력 인터뷰에서 나올 수 있으나 현재 코드에 포함되지
   않은 주제가 있는지 검토
```

---

## 4.6. 실제 코드북 (`codebook.md`) 상세 해설

본 절에서는 `codebook.md`의 전체 구조를 대분류별로 해설한다.

### 4.6.1. 대분류 1.1: 국제협력 동인 및 목표 (`COLLABORATION_DRIVERS`)

이 대분류는 출연연이 국제협력을 추진하는 **근본적 동기(motivation)**를 포착한다. 9개 소분류 코드는 "왜(why) 국제협력을 하는가?"라는 질문에 답한다.

```markdown
## 1.1. 국제협력 동인 및 목표 (COLLABORATION_DRIVERS)
* `CD_TECH_ACCESS`: (선진/보완 기술 접근)
* `CD_MARKET_ACCESS` / `CD_COMMERCIALIZATION`: (시장 접근/상용화)
* `CD_RESOURCE_ACCESS`: (자원/데이터 접근)
* `CD_STANDARDS_NORMS`: (표준/규범 선점)
* `CD_NETWORKING_INFO`: (네트워킹/정보 교류)
* `CD_CAPACITY_BUILDING`: (파트너 역량 강화)
* `CD_POLICY_INFLUENCE` / `CD_SCIENCE_DIPLOMACY`: (정책 영향력/과학 외교)
* `CD_GLOBAL_CHALLENGE`: (글로벌 난제 해결)
* `CD_REQUIREMENT_DRIVEN`: (과제 요건 충족)
```

특히 `CD_REQUIREMENT_DRIVEN`은 주목할 만하다. 이는 "국제협력을 하고 싶어서"가 아닌 "정부 과제의 요건으로 국제협력이 포함되어 있어서 어쩔 수 없이"라는 동기를 포착하는 코드이다. 2023년 정부의 국제협력 확대 지침 이후 이 코드에 해당하는 발언이 다수 발견되어, 정책 분석에서 중요한 함의를 지닌다.

### 4.6.2. 대분류 1.2: 국제협력 유형 및 방식 (`COLLABORATION_TYPES_MODALITIES`)

이 대분류는 "어떻게(how) 국제협력을 수행하는가?"에 답하는 8개 코드를 포함한다.

```markdown
## 1.2. 국제협력 유형 및 방식 (COLLABORATION_TYPES_MODALITIES)
* `CT_JOINT_RD`: (공동 연구)
* `CT_PERSONNEL_EXCHANGE`: (인력 교류)
* `CT_INFRA_ACCESS_SHARING`: (인프라 공유/활용)
* `CT_COMMERCIAL_ORIENTED`: (상업화 연계)
* `CT_MULTILATERAL_ENGAGEMENT`: (다자 협력)
* `CT_BILATERAL_ENGAGEMENT`: (양자 협력)
* `CT_ODA_CAPACITY_BUILDING`: (ODA 사업)
* `CT_NETWORKING_EVENTS`: (네트워킹 행사)
```

`CT_MULTILATERAL_ENGAGEMENT`(다자 협력)과 `CT_BILATERAL_ENGAGEMENT`(양자 협력)의 구분은 국제협력 연구에서 근본적인 분류 축이다. 본 연구에서 KAERI의 인터뷰 분석 결과, 양자 협력의 한계를 인식하고 다자 협력으로 전환하려는 전략적 시사점이 도출되었는데, 이는 두 코드의 분리 없이는 포착하기 어려웠을 것이다.

### 4.6.3. 대분류 1.3: 장애물 및 도전과제 (`OBSTACLES_CHALLENGES`)

가장 많은 소분류 코드(24개)를 포함하는 대분류이다. 이는 인터뷰 참여자들이 장애물에 대해 가장 풍부하고 다양한 발언을 했음을 반영한다.

6개의 중분류 아래 24개 코드가 배치된다.

| 중분류 | 코드 수 | 핵심 내용 |
|--------|---------|-----------|
| `OBS_FUNDING` (예산 문제) | 5 | 핵심 예산 부족, 신규 예산 부족, 불안정성, 주기 불일치, 집행 경직성 |
| `OBS_IPR_LEGAL_CONTRACTUAL` (IPR/법률) | 3 | IPR 분쟁, 법률 지원 부족, 계약 복잡성 |
| `OBS_POLICY_GOVERNANCE` (정책/거버넌스) | 5 | 정책 비일관성, 전략 부재, 부처 칸막이, 하향식 접근, 관료주의 |
| `OBS_GEOPOLITICAL_PARTNER` (지정학/파트너) | 4 | 지정학적 제약, 파트너 리스크, 기술 경쟁, 관계 어려움 |
| `OBS_HUMAN_RESOURCES` (인력) | 3 | 전문 인력 부족, 인센티브 부족, 업무 부담 |
| `OBS_INSTITUTIONAL_SYSTEMIC` (기관/시스템) | 4 | 임무/문화 충돌, 내부 비효율, 정보 공유 부족, 기반 지원 부족 |

이 세분화의 실제 분석적 가치를 보여주는 예시: `OBS_POL_FRAGMENTATION_LACK_COORDINATION`(부처 간 칸막이/조정 부재)라는 코드는 다음과 같은 발언을 정확히 포착한다.

> "이틀 사이로 산업부랑 저희랑 같은 같은 기관을 같이 SMR을 하는 거예요. 그래서 알아 알아 알아 알아 알아 이러고 나서 와서 담당자한테 연락해서 우리 이러지 맙시다 같이 갑시다 같이 갔는데 아젠다가 서로 조율이 잘 안 되죠."

이 발언이 단순히 "행정적 한계" 수준으로만 코딩되었다면, "부처 간 조정 실패"라는 구체적 정책 문제가 묻혀버렸을 것이다.

### 4.6.4. 대분류 1.4~1.6: 성공/실패/정책 제언

```markdown
## 1.4. 성공 요인 및 촉진제 (SUCCESS_FACTORS_ENABLERS) — 7개 코드
## 1.5. 실패 요인 및 교훈 (FAILURE_FACTORS_LESSONS) — 4개 코드
## 1.6. 정책 요구 및 제언 (POLICY_NEEDS_RECOMMENDATIONS) — 9개 코드
```

이 세 대분류는 연구의 실용적 결론을 도출하는 데 핵심적이다. 특히 1.6의 9개 정책 제언 코드(`REC_` 접두어)는 최종 정책 보고서의 골격이 된다.

```
REC_FUNDING_STABILITY_INCREASE_FLEXIBILITY   → 재원 안정성/확대/유연성
REC_STRATEGY_LONG_TERM_CONSISTENCY           → 장기적/일관된 전략
REC_SUPPORT_SYSTEMS_IPR_LEGAL_ADMIN          → IPR/법률/행정 지원 시스템
REC_INCENTIVES_EVALUATION_REFORM             → 인센티브/평가 제도 개선
REC_HR_DEVELOPMENT_SPECIALIZATION            → 전문 인력 양성/활용
REC_GOVERNANCE_COORDINATION_IMPROVEMENT      → 거버넌스/조정 기능 강화
REC_STREAMLINE_PROCESSES_REGULATIONS         → 절차/규제 간소화
REC_SUPPORT_BOTTOM_UP_INITIATIVES            → 상향식/풀뿌리 협력 지원
REC_INFORMATION_PLATFORM_KM                  → 정보 플랫폼/지식 관리
```

### 4.6.5. 대분류 1.7~1.8: 맥락 및 정책 사건 반응

```markdown
## 1.7. 기관/분야 맥락 (INSTITUTIONAL_SECTORAL_CONTEXT) — 4개 코드
## 1.8. 특정 정책/사건 반응 (POLICY_EVENT_RESPONSE) — 3개 코드
```

1.7은 발언의 배경이 되는 기관/분야 특성을 태깅하기 위한 코드이고, 1.8은 특정 시점의 사건(특히 2023년 예산 조정)에 대한 반응을 별도로 포착한다. 1.8의 존재는 본 연구가 단순 주제 분류를 넘어 **시의적 맥락(temporal context)**까지 분석에 포함하겠다는 설계 의도를 반영한다.

```markdown
* `PER_IMPACT_BUDGET_CHANGES_2023`: (2023년 예산 조정 영향)
* `PER_RESPONSE_TO_POLICY_MANDATE`: (정책 지침 대응)
* `PER_CRITIQUE_OF_POLICY_PROCESS`: (정책 과정 비판)
```

---

## 4.7. 코드북 적용 결과와 활용

최종 확정된 코드북을 전체 데이터에 적용한 결과(`qualitative/policy_1027_all_coding.md`)는 기관별로 다음과 같은 구조를 갖는다.

```markdown
# KAERI 한국원자력연구원

## 1. 인터뷰 핵심 요약
- [3~4개의 핵심 발견 사항]

## 2. 주요 장애요인 및 도전과제
- [구체적 장애 요인 목록]

## 3. 정책적 시사점
- [도출된 정책 함의]

## 4. 코드화 테이블
| Institution | Speaker | Source_Text | Thematic_Code(s) | Context_Code | Location_Ref |
| :--- | :--- | :--- | :--- | :--- | :--- |
| KAERI | 전은주 박사님 | [요약된 발언] | `코드1`, `코드2` | `맥락코드` | (타임스탬프) |
```

이 출력 형식은 코드북의 두 차원이 실제로 어떻게 작동하는지를 보여준다.

- **Thematic_Code(s)** 열: 주제 코드가 복수로 부여되어 있다 (쉼표로 구분).
- **Context_Code** 열: 맥락 코드가 정확히 하나만 부여되어 있다.
- **Location_Ref** 열: 원본 전사본에서의 위치 참조를 통해 감사 추적이 가능하다.

---

## 4.8. 코드북 개발의 핵심 교훈 (Lessons Learned)

본 연구에서 코드북 개발 과정을 통해 얻은 핵심 교훈을 정리한다.

1. **코드북은 살아있는 문서(living document)이다**: 초기 버전에서 최종 버전까지 코드 수가 약 50개에서 78개(주제 68 + 맥락 10)로 증가했다. 데이터에 귀를 기울이는 반복적 과정이 필수적이다.

2. **LLM 에이전트를 위한 코드북은 더 명시적이어야 한다**: 인간 코더는 교육과 토론을 통해 암묵적 지식을 공유하지만, LLM은 코드북 문서에 적힌 내용만으로 판단해야 한다. "당연히 알 것"이라고 생각하고 생략한 규칙은 곧 코딩 오류로 이어진다.

3. **이중 언어 표기는 정확도를 높인다**: 영어 코드명의 기계적 명확성과 한국어 설명의 문화적 맥락이 결합될 때, LLM의 코딩 정확도가 향상된다. 특히 한국 정책 맥락에서만 의미가 통하는 개념(예: "순환보직", "부처 칸막이")은 한국어 설명이 필수적이다.

4. **맥락 코드는 분석의 깊이를 더한다**: 주제 코드만으로는 "무엇에 대해 말했는가"만 파악 가능하다. 맥락 코드를 추가함으로써 "어떤 입장에서 말했는가"까지 포착할 수 있으며, 이는 정책 보고서의 설득력을 크게 높인다.

5. **위계적 코드 구조는 유연한 분석을 가능하게 한다**: 거시적 패턴 분석(대분류 수준)과 미시적 사례 분석(소분류 수준)을 동일한 코드북으로 수행할 수 있다. 연구 질문에 따라 분석의 해상도(resolution)를 조절할 수 있는 것이 큰 장점이다.

---

# 5. Phase 3 — LLM 기반 코딩 수행 (LLM-Based Coding)

질적 연구에서 **코딩(coding)**이란 원시 텍스트 데이터에 체계적으로 의미 라벨을 부여하는 핵심 분석 작업이다. 전통적으로 NVivo, ATLAS.ti 등의 CAQDAS 소프트웨어나 수작업 스프레드시트를 통해 수행되던 이 과정을, 본 프로젝트에서는 Claude Code를 활용하여 21,011줄의 인터뷰 전사 데이터를 1,527개의 구조화된 코딩 발췌문(coded excerpts)으로 변환하였다. 이 장에서는 그 구체적인 방법론을 단계별로 상세히 기술한다.

---

## 5.1 Claude Code에 코드북 기반 코딩을 지시하는 방법

### 5.1.1 CLAUDE.md에 코드북 참조 설정

Claude Code는 프로젝트 루트의 `CLAUDE.md` 파일을 세션 시작 시 자동으로 읽어 프로젝트 맥락을 파악한다. 코드북 기반 코딩을 수행하려면, 이 파일에 코드북의 존재와 역할을 명시적으로 선언해야 한다.

```markdown
# CLAUDE.md — 프로젝트 지침

## 프로젝트 개요
본 프로젝트는 15개 정부출연연구기관(GRI)의 국제협력 실태를 분석하는
질적 연구이다.

## 코드북 참조
- 코드북 파일: `codebook.md` (GRI-IC-Codebook v1.0)
- 코드북 구조: 2차원 매트릭스
  - 축 1: 주제 코드 (Thematic Codes) — 8개 대분류, 70개 이상 하위 코드
  - 축 2: 맥락/뉘앙스 코드 (Context/Nuance Codes) — 10개
- 모든 코딩 작업 시 반드시 codebook.md를 참조할 것

## 코딩 원칙
1. 각 의미 단위(semantic unit)에 주제 코드 1개 이상 + 맥락 코드 1개를 부여
2. 원문을 그대로 인용하되, 발언자 정보는 익명화 처리
3. 코딩 결과는 마크다운 테이블 형식으로 출력
4. 모호한 경우 가장 근접한 코드를 부여하고 비고(notes)에 판단 근거를 기록
```

> **핵심 원리:** CLAUDE.md는 Claude Code의 "장기 기억"에 해당한다. 여기에 코드북 참조를 명시하면, 이후 모든 코딩 세션에서 별도의 지시 없이도 Claude Code가 코드북을 인식하고 참조한다.

### 5.1.2 코딩 프롬프트 전략

효과적인 코딩을 위해서는 프롬프트의 구조가 결정적으로 중요하다. 아래는 실제 프로젝트에서 사용한 프롬프트의 구조와 예시이다.

**기본 코딩 프롬프트 템플릿:**

```
codebook.md를 참조하여 아래 인터뷰 텍스트의 각 의미 단위(semantic unit)를
식별하고, 주제 코드(thematic code) 1개 이상과 맥락 코드(context code) 1개를
부여해주세요.

## 출력 형식
| # | 원문 발췌 | 주제 코드 | 맥락 코드 | 비고 |
|---|----------|----------|----------|------|

## 코딩 기준
1. 의미 단위는 하나의 완결된 의미를 가진 발언 세그먼트로 정의한다.
2. 하나의 발언이 여러 주제에 걸치면 복수의 주제 코드를 부여한다.
3. 맥락 코드는 발언의 톤과 성격을 반영한다 (예: 사실 보고, 평가, 제안 등).
4. 원문은 최소한의 문맥을 포함하되, 핵심 의미가 손실되지 않도록 한다.

## 인터뷰 텍스트
[여기에 인터뷰 텍스트 삽입 또는 파일 경로 지정]
```

**파일 경로를 활용한 실제 프롬프트 예시:**

```
raw/institution_A_interview.md 파일을 읽고, codebook.md의 코드 체계에 따라
전체 인터뷰를 코딩해주세요.

특히 다음 사항에 유의하세요:
- 장애요인(OB_*) 코드에 해당하는 발언을 빠짐없이 포착할 것
- 성공 동인(CD_*) 코드와 장애요인 코드가 동시에 나타나는 경우 모두 기록할 것
- 정책 제언(PR_*) 코드에 해당하는 발언은 구체성 수준을 비고에 기록할 것

출력은 마크다운 테이블 형식으로, 파일명은
qualitative/institution_A_coding.md로 저장해주세요.
```

**프롬프트 설계 시 핵심 원칙:**

| 원칙 | 설명 | 예시 |
|------|------|------|
| 명시적 참조 (Explicit Reference) | 코드북 파일명을 직접 언급 | "codebook.md를 참조하여" |
| 구조화된 출력 (Structured Output) | 테이블 형식을 사전 지정 | 마크다운 테이블 헤더 제공 |
| 판단 기준 제시 (Decision Criteria) | 모호한 경우의 처리 방법 명시 | "복수 코드 부여 가능" |
| 맥락 보존 (Context Preservation) | 원문 인용 범위 지정 | "최소한의 문맥 포함" |
| 비고 활용 (Annotation) | 판단 근거 기록 요청 | "비고에 판단 근거 기록" |

---

## 5.2 작업 분할 전략 (Chunking Strategy)

### 5.2.1 대용량 텍스트 분할의 필요성

21,011줄(약 450,000 단어)의 인터뷰 전사 데이터를 한 번에 처리하는 것은 기술적으로도, 분석 품질 측면에서도 바람직하지 않다. LLM의 컨텍스트 윈도우(context window) 제한뿐 아니라, 텍스트가 길어질수록 초반부와 후반부 사이의 코딩 일관성이 저하되는 문제(consistency drift)가 발생한다.

### 5.2.2 기관별·섹션별 분할

본 프로젝트에서는 다음과 같은 2단계 분할 전략을 적용하였다.

```
전체 데이터 (21,011줄)
├── 1차 분할: 기관별 (15개 기관)
│   ├── institution_01.md (~1,400줄)
│   ├── institution_02.md (~1,200줄)
│   ├── institution_03.md (~1,800줄)
│   │   ...
│   └── institution_15.md (~1,300줄)
│
└── 2차 분할: 섹션별 (필요 시)
    ├── institution_03_part1.md (줄 1-1,000)
    └── institution_03_part2.md (줄 900-1,800)  ← 100줄 오버랩
```

### 5.2.3 적정 처리 분량

실험적으로 확인한 결과, Claude Code에서 코딩 작업의 최적 분량은 다음과 같다.

| 분량 | 코딩 품질 | 처리 시간 | 일관성 | 권장 여부 |
|------|----------|----------|--------|----------|
| ~500줄 | 매우 높음 | 빠름 | 매우 높음 | 소규모 데이터에 적합 |
| ~1,000줄 | 높음 | 적정 | 높음 | 일반적 권장 |
| **~2,000-3,000줄** | **높음** | **적정** | **높음** | **본 프로젝트 기준** |
| ~5,000줄 | 보통 | 느림 | 보통 | 비권장 |
| 10,000줄 이상 | 낮음 | 매우 느림 | 낮음 | 비권장 |

본 프로젝트에서는 기관별 인터뷰가 대체로 1,200~1,800줄 범위에 있어, 기관 단위로 분할하면 자연스럽게 적정 분량 내에 들어왔다. 1,800줄을 초과하는 기관의 경우에만 추가적인 섹션 분할을 적용하였다.

### 5.2.4 오버랩 전략 (Overlap Strategy)

텍스트를 분할할 때, 경계 지점에서 의미 단위가 절단되는 문제를 방지하기 위해 **오버랩(overlap)** 영역을 설정한다.

```
Chunk 1: 줄 1 ────────────────────── 줄 1,000
                              ┌─── 오버랩 영역 (줄 900-1,000) ───┐
Chunk 2:              줄 900 ────────────────────── 줄 2,000
                                                ┌─── 오버랩 영역 ───┐
Chunk 3:                                줄 1,900 ───────────── 줄 2,800
```

**오버랩 처리 프롬프트:**

```
이전 청크(chunk)의 마지막 100줄과 현재 청크의 처음 100줄이 겹칩니다.
겹치는 영역의 코딩 결과가 이전 청크와 일관되도록 유의해주세요.
이전 청크에서 이미 코딩된 발췌문은 중복 코딩하지 마세요.

이전 청크의 마지막 코딩 항목:
| 98 | "...해외 기관과의 MOU가 형식적으로..." | OB_ADMIN_PROC | CRITICAL_EVAL |
| 99 | "...실질적인 교류로 이어지려면..." | PR_INST_REFORM | SUGGESTION |
```

> **실무 팁:** 오버랩 영역의 크기는 인터뷰의 특성에 따라 조절한다. 긴 서사(narrative)가 많은 인터뷰는 200줄, 짧은 질의응답 형식은 50줄이면 충분하다. 본 프로젝트에서는 100줄을 기본값으로 사용하였다.

---

## 5.3 시맨틱 유닛(Semantic Unit) 추출

### 5.3.1 시맨틱 유닛의 정의

질적 코딩에서 **시맨틱 유닛(semantic unit, 의미 단위)**이란 하나의 완결된 의미를 담고 있는 발언 세그먼트를 말한다. 이는 코딩의 기본 분석 단위(unit of analysis)로서, 너무 작아도(의미 손실), 너무 커도(의미 혼재) 안 되는 적절한 크기를 유지해야 한다.

### 5.3.2 적정 크기 판단 기준

```
너무 작음 (Under-segmentation)     적정 수준 (Optimal)            너무 큼 (Over-segmentation)
─────────────────────────────────────────────────────────────────────────────────
"협력"                           "차세대 원자로 개발하는 데      전체 문단 (여러 주제가
"많았다"                          있어서 협력할 수 있는            혼재된 3-5문장)
→ 개별 단어로는                   분야가 되게 많았다"
  의미 파악 불가                   → 하나의 완결된 의미             → 복수의 의미 단위가
                                  (기술 접근 기회에 대한             섞여 코드 부여 모호
                                   사실적 보고)
```

### 5.3.3 실제 추출 및 코딩 예시

아래는 본 프로젝트에서 실제로 추출한 시맨틱 유닛과 코딩 결과의 예시이다.

**예시 1: 단일 주제 코드 + 맥락 코드**

| 원문 발췌 | 주제 코드 | 맥락 코드 | 비고 |
|----------|----------|----------|------|
| "차세대 원자로 개발하는 데 있어서 협력할 수 있는 분야가 되게 많았다" | `CD_TECH_ACCESS` | `FACTUAL_REPORTING` | 기술 접근 기회를 긍정적으로 평가하는 사실 보고 |

**예시 2: 복수 주제 코드**

| 원문 발췌 | 주제 코드 | 맥락 코드 | 비고 |
|----------|----------|----------|------|
| "정부 부처에서 국제협력 예산을 따로 잡아주지 않으니까, 연구원 자체 예산에서 충당해야 하는데 그게 쉽지 않다" | `OB_GOV_REL`, `OB_BUDGET` | `CRITICAL_EVAL` | 정부 관계(예산 미배정)와 재정 제약이 동시에 작용하는 복합 장애요인 |

**예시 3: 정책 제언 코드**

| 원문 발췌 | 주제 코드 | 맥락 코드 | 비고 |
|----------|----------|----------|------|
| "국제협력 전담 부서를 만들고 거기에 전문 인력을 배치하면 훨씬 체계적으로 할 수 있을 것 같습니다" | `PR_INST_REFORM`, `PR_HR_DEV` | `SUGGESTION` | 조직 구조 개편과 인력 배치를 동시에 제안하는 구체적 정책 제언 |

### 5.3.4 시맨틱 유닛 추출을 위한 프롬프트

```
다음 인터뷰 텍스트를 시맨틱 유닛(semantic unit) 단위로 분절해주세요.

## 시맨틱 유닛 판단 기준
1. 하나의 완결된 의미를 가진 발언 세그먼트
2. 통상 1-3문장 분량 (너무 짧은 단어/구 수준은 지양)
3. 하나의 유닛에 하나의 핵심 주제가 담기도록 분절
4. 문맥 이해에 필요한 최소한의 선행 정보 포함
5. 발언자의 의도와 톤이 보존되도록 자연스럽게 절단

## 분절 후 각 유닛에 대해
- codebook.md에 따라 주제 코드(thematic code) 1개 이상 부여
- codebook.md에 따라 맥락 코드(context code) 1개 부여
- 판단이 모호한 경우 비고(notes)에 근거 기록

## 주의사항
- "음", "그", "아" 등의 간투사만으로 이루어진 발언은 코딩 대상에서 제외
- 면접자의 질문은 코딩하지 않되, 응답의 맥락 파악을 위해 참고
- 동일 주제에 대한 반복 발언은 가장 명확한 발언을 대표로 코딩하고,
  나머지는 비고에 "유사 발언 n회 반복" 형태로 기록
```

---

## 5.4 코딩 결과 검증 및 수정

### 5.4.1 반복적 검증 사이클 (Iterative Validation Cycle)

LLM 기반 코딩은 1차 자동 코딩(first-pass coding)으로 완결되지 않는다. 연구의 신뢰성을 확보하려면 반드시 인간 연구자의 검토와 수정이 수반되어야 한다. 본 프로젝트에서는 다음과 같은 반복 사이클을 적용하였다.

```
┌──────────────────────────────────────────────────────────┐
│                    코딩 검증 사이클                         │
│                                                          │
│   1차 자동 코딩 ──→ 연구자 검토 ──→ 수정 지시 ──→ 재코딩   │
│        │                │              │            │     │
│        ▼                ▼              ▼            ▼     │
│   Claude Code가     연구자가        구체적인      Claude    │
│   codebook.md      코딩 결과를     오류 지적 및   Code가    │
│   기반으로 전체    샘플링 검토     기준 명확화    수정      │
│   텍스트 코딩     (10-20%)                      반영      │
│                                                          │
│               필요 시 2-3회 반복                           │
└──────────────────────────────────────────────────────────┘
```

### 5.4.2 검토 시 발견되는 주요 오류 유형

| 오류 유형 | 설명 | 빈도 | 수정 방법 |
|----------|------|------|----------|
| 과소 코딩 (Under-coding) | 의미 단위에 부여해야 할 코드를 누락 | 높음 | 누락된 코드 추가 지시 |
| 과잉 코딩 (Over-coding) | 해당하지 않는 코드를 잘못 부여 | 보통 | 잘못된 코드 제거 지시 |
| 오분류 (Misclassification) | 유사한 코드 간 혼동 (예: `OB_GOV_REL` vs `OB_ADMIN_PROC`) | 높음 | 코드 간 구분 기준 재명시 |
| 시맨틱 유닛 경계 오류 | 의미 단위가 너무 크거나 작게 분절 | 보통 | 분절 기준 재지정 |
| 맥락 코드 오류 | 발언 톤/성격 오판 (예: 비판을 사실 보고로 분류) | 낮음 | 맥락 코드 정의 보충 |

### 5.4.3 수정 지시 프롬프트 예시

```
policy_1027_institution_A_coding.md의 코딩 결과를 검토한 결과,
다음 수정이 필요합니다.

## 수정 사항

### 1. 오분류 수정 (5건)
- 항목 #23: OB_GOV_REL → OB_ADMIN_PROC로 변경
  (이유: 정부 '부처'와의 관계 자체가 아니라, 행정 절차의 복잡성에 대한 발언)
- 항목 #45: CD_NETWORK → CD_TECH_ACCESS로 변경
  (이유: 네트워크 자체보다 기술 접근이 핵심 키워드)
  [...]

### 2. 누락 코딩 추가 (3건)
- 줄 234-237 사이의 발언 "우리가 자체적으로 국제 세미나를 기획해서..."이
  코딩되지 않았습니다. CD_AUTONOMOUS + POSITIVE_EVAL로 추가해주세요.
  [...]

### 3. 시맨틱 유닛 재분절 (2건)
- 항목 #67: 현재 4문장이 하나의 유닛으로 묶여 있는데, 앞 2문장(장애요인)과
  뒤 2문장(대응 전략)을 분리해주세요.
  [...]

수정 후 전체 코딩 테이블을 다시 출력해주세요.
```

### 5.4.4 모호한 경우의 처리 기준 설정

질적 코딩에서 가장 빈번하게 발생하는 문제는 **코드 경계의 모호성(code boundary ambiguity)**이다. 본 프로젝트에서는 이를 체계적으로 처리하기 위한 의사결정 규칙(decision rules)을 codebook.md에 명시하였다.

```markdown
## 코딩 의사결정 규칙 (Decision Rules)

### 규칙 1: 복수 코드 부여 원칙
하나의 발언이 두 개 이상의 주제에 걸칠 경우, 모든 해당 코드를 부여한다.
단, 3개를 초과하는 주제 코드 부여는 시맨틱 유닛의 재분절을 우선 검토한다.

### 규칙 2: 유사 코드 간 구분
- OB_GOV_REL (정부 부처와의 관계): 정부의 '의지', '방침', '지원 부재'에 초점
- OB_ADMIN_PROC (행정·제도적 한계): '절차', '규정', '시스템'의 제약에 초점
→ 판단 기준: 발언의 주어가 '정부/부처'이면 GOV_REL, '제도/절차'이면 ADMIN_PROC

### 규칙 3: 맥락 코드 우선순위
동일 발언에 여러 톤이 혼재된 경우, 다음 우선순위로 맥락 코드를 결정한다:
1. 가장 마지막에 표현된 톤 (발언의 결론이 의도를 반영)
2. 가장 강하게 표현된 톤
3. 연구 질문과 더 밀접한 톤
```

---

## 5.5 교차 검증 (Inter-rater Reliability)

### 5.5.1 교차 검증의 필요성

LLM 기반 코딩의 학술적 신뢰성을 확보하기 위해서는 전통적 질적 연구와 동일한 수준의 교차 검증(inter-rater reliability) 절차가 필수적이다. 이는 LLM을 하나의 코더(rater)로 간주하고, 인간 코더와의 일치도를 정량적으로 측정하는 방식으로 수행된다.

### 5.5.2 Cohen's Kappa 계산

**Cohen's Kappa(κ)**는 두 코더 간의 일치도를 우연에 의한 일치(chance agreement)를 보정하여 측정하는 지표이다.

$$\kappa = \frac{P_o - P_e}{1 - P_e}$$

여기서:
- $P_o$ = 관찰된 일치율 (observed agreement)
- $P_e$ = 우연에 의한 기대 일치율 (expected agreement by chance)

**해석 기준 (Landis & Koch, 1977):**

| κ 값 범위 | 해석 |
|----------|------|
| 0.00 이하 | 일치 없음 (Poor) |
| 0.00-0.20 | 약간 일치 (Slight) |
| 0.21-0.40 | 보통 일치 (Fair) |
| 0.41-0.60 | 중간 일치 (Moderate) |
| 0.61-0.80 | 상당 일치 (Substantial) |
| **0.81-1.00** | **거의 완벽 일치 (Almost Perfect)** |

### 5.5.3 본 프로젝트의 검증 설계

본 프로젝트에서는 전체 1,527개 코딩 항목 중 **200개(13.1%)**를 무작위 추출하여 인간 코더(연구자 본인)가 독립적으로 코딩한 후, Claude Code의 코딩 결과와 비교하였다.

**검증 절차:**

```
1. 무작위 표본 추출
   - 전체 1,527개 항목에서 200개를 층화무작위추출(stratified random sampling)
   - 15개 기관에서 균등하게 추출 (기관당 약 13-14개)

2. 독립 코딩
   - 인간 코더: codebook.md를 참고하여 200개 원문을 독립적으로 코딩
   - LLM 코더: 이미 완료된 코딩 결과 중 해당 200개를 추출

3. 일치도 비교
   - 주제 코드: 다중 라벨이므로 항목별 코드 집합의 일치 여부를 판단
   - 맥락 코드: 단일 라벨이므로 직접 비교

4. Kappa 계산
   - 주제 코드: 각 코드별 이진(binary) Kappa를 계산 후 평균 (micro-average)
   - 맥락 코드: 다범주(multi-class) Kappa 계산
```

### 5.5.4 Cohen's Kappa = 0.85 달성 과정

최종적으로 달성한 κ = 0.85는 Landis & Koch 기준으로 **"거의 완벽 일치(Almost Perfect Agreement)"** 구간에 해당한다. 이 수치는 한 번에 달성된 것이 아니라, 반복적인 교정 과정을 통해 점진적으로 향상되었다.

```
반복 1 (초기 코딩)           : κ = 0.72 (Substantial)
  → 오분류 패턴 식별 및 codebook.md 의사결정 규칙 보강

반복 2 (코드북 보강 후 재코딩) : κ = 0.79 (Substantial)
  → 맥락 코드 정의 명확화 및 예시 추가

반복 3 (최종 교정)           : κ = 0.85 (Almost Perfect) ✓
```

> **학술적 의의:** κ = 0.85는 전통적 인간-인간 코더 간 교차 검증에서도 우수한 수준에 해당한다. 이는 적절하게 설계된 코드북과 반복적 교정 과정을 거칠 경우, LLM이 인간 코더와 거의 동등한 수준의 코딩 일관성을 달성할 수 있음을 실증적으로 보여준다.

### 5.5.5 Claude Code를 활용한 자동 Kappa 계산

교차 검증 결과를 Claude Code를 통해 자동으로 계산하는 것도 가능하다. 아래는 실제 사용한 프롬프트이다.

```
다음 두 개의 코딩 결과를 비교하여 Cohen's Kappa를 계산해주세요.

## 파일 1: LLM 코딩 결과
qualitative/policy_1027_all_coding.md에서 무작위 추출된 200개 항목

## 파일 2: 인간 코더 결과
validation/human_coding_200_items.md

## 계산 요청
1. 주제 코드 (Thematic Codes)에 대한 Cohen's Kappa
   - 각 코드를 이진 변수로 처리 (부여됨/부여 안됨)
   - 코드별 개별 Kappa와 전체 micro-average Kappa 산출
2. 맥락 코드 (Context Codes)에 대한 Cohen's Kappa
   - 10개 범주에 대한 다범주 Kappa 산출
3. 전체 혼동 행렬 (Confusion Matrix) 출력
4. 불일치 항목 목록과 불일치 패턴 분석

출력 형식:
- 요약 통계표
- 코드별 상세 Kappa 값
- 불일치 유형 분류 및 빈도
- 해석 및 개선 권고
```

**출력 결과 예시:**

```markdown
## Cohen's Kappa 분석 결과

### 요약
| 분석 대상 | κ 값 | 해석 | N |
|----------|------|------|---|
| 주제 코드 (전체) | 0.84 | Almost Perfect | 200 |
| 맥락 코드 | 0.87 | Almost Perfect | 200 |
| **종합** | **0.85** | **Almost Perfect** | **200** |

### 주제 코드별 상세 Kappa
| 코드 | κ | 일치 | 불일치 | 해석 |
|------|---|------|--------|------|
| OB_GOV_REL | 0.88 | 186 | 14 | Almost Perfect |
| OB_ADMIN_PROC | 0.79 | 178 | 22 | Substantial |
| CD_TECH_ACCESS | 0.91 | 190 | 10 | Almost Perfect |
| ... | ... | ... | ... | ... |

### 주요 불일치 패턴
1. OB_GOV_REL ↔ OB_ADMIN_PROC 혼동: 8건 (전체 불일치의 26%)
2. CD_NETWORK ↔ CD_TECH_ACCESS 혼동: 5건 (16%)
3. 맥락 코드 FACTUAL_REPORTING ↔ NEUTRAL_DESC 혼동: 4건 (13%)
```

---

## 5.6 Git 브랜치 전략을 통한 분석 버전 관리

### 5.6.1 브랜치 전략의 원칙

질적 코딩은 본질적으로 반복적(iterative)이고 수정 가능(revisable)한 작업이다. Git 브랜치 전략을 활용하면, 각 분석 단계의 스냅샷을 보존하면서 안전하게 수정·보완할 수 있다.

### 5.6.2 실제 사용된 브랜치 전략

본 프로젝트에서는 다음과 같은 브랜치 구조를 사용하였다.

```
main
├── claude/science-policy-interviews-011CUXesC8zHtKDUA6pmvFVZ
│   ├── PR #1: 피드백 반영 및 초기 분석
│   ├── PR #2: 심층 분석 수행
│   └── PR #3: 최종 보고서 Part 1-5 작성
│
├── codex/analyze-and-code-interview-transcripts
│   ├── PR #4: 코딩 테이블 생성 (policy_1027_all_coding.md)
│   └── PR #5: 파일 구조 정리 및 조직화
│
├── codex/improve-policy-memo-writing-style
│   └── PR #6: 정책 메모 문체 개선
│
└── claude/qualitative-policy-synthesis-016xtsPM6fhmr4SMZ7BSwxNr
    ├── PR #7: 종합 분석 (comprehensive_policy_analysis.md)
    ├── PR #8: 한국어 번역
    └── PR #9: 수정 보고서 최종본
```

### 5.6.3 브랜치 네이밍 컨벤션

| 접두사 | 용도 | 예시 |
|--------|------|------|
| `claude/` | Claude Code 기반 분석 세션 | `claude/science-policy-interviews-...` |
| `codex/` | 체계적 코딩 및 데이터 처리 | `codex/analyze-and-code-interview-transcripts` |
| `review/` | 검토 및 수정 (필요 시) | `review/inter-rater-validation` |

### 5.6.4 PR(Pull Request) 기반 분석 이력 관리

각 분석 단계를 PR로 관리하면 다음과 같은 이점이 있다.

```
PR #4: 코딩 테이블 생성
──────────────────────────────────────────
커밋 이력:
  commit a1b2c3d: "15개 기관 인터뷰 1차 자동 코딩 완료"
  commit d4e5f6g: "연구자 검토 반영 — 23건 오분류 수정"
  commit h7i8j9k: "맥락 코드 재검토 — 의사결정 규칙 적용"
  commit l0m1n2o: "최종 코딩 테이블 확정 (1,527 항목)"

PR 설명:
  - 변경 파일: qualitative/policy_1027_all_coding.md (+59KB)
  - 코딩 항목 수: 1,527개
  - 교차 검증 결과: κ = 0.85
  - 리뷰어 코멘트 및 논의 내역 포함
──────────────────────────────────────────
```

**실무 워크플로우:**

```bash
# 1. 새로운 분석 단계를 위한 브랜치 생성
git checkout -b codex/analyze-and-code-interview-transcripts

# 2. Claude Code로 코딩 작업 수행 (Claude Code가 자동으로 파일 생성/수정)

# 3. 변경 사항 커밋
git add qualitative/policy_1027_all_coding.md
git commit -m "15개 기관 인터뷰 1차 자동 코딩 완료 (1,527 항목)"

# 4. 검토 후 수정 사항 반영
git add qualitative/policy_1027_all_coding.md
git commit -m "연구자 검토 반영 — 오분류 23건 수정, 누락 코딩 7건 추가"

# 5. PR 생성
gh pr create --title "코딩 테이블 생성: 15개 기관 1,527개 항목" \
  --body "## 작업 내용
- 15개 정부출연연구기관 인터뷰 전사 데이터 코딩
- 코드북(codebook.md) 기반 2차원 코딩 체계 적용
- 교차 검증 완료 (κ = 0.85)

## 산출물
- qualitative/policy_1027_all_coding.md (59KB, 1,527 항목)"

# 6. 리뷰 후 머지
gh pr merge --merge
```

> **핵심 가치:** Git을 통한 버전 관리는 연구의 **감사 추적(audit trail)**을 제공한다. 질적 연구의 신뢰성 기준 중 하나인 확인 가능성(confirmability)을 뒷받침하며, 코딩 결과가 어떤 과정을 거쳐 최종 형태에 도달했는지를 투명하게 보여준다.

---

# 6. Phase 4 — 분석 및 종합 (Analysis & Synthesis)

코딩이 완료된 1,527개의 구조화된 발췌문은 그 자체로는 아직 "발견(findings)"이 아니다. 이 데이터를 연구 질문에 대한 답변으로 전환하려면 체계적인 분석과 종합 과정이 필요하다. 이 장에서는 빈도 분석, 비교 분석, 패턴 도출, 그리고 최종 보고서 작성까지의 전 과정을 Claude Code 활용 방법과 함께 상세히 기술한다.

---

## 6.1 빈도 분석 (Frequency Analysis)

### 6.1.1 코드별 빈도 집계

질적 연구에서 빈도 분석은 논쟁적이지만, **주제의 상대적 현저성(relative salience)**을 파악하는 데 유용한 보조 지표로 널리 활용된다. 본 프로젝트에서는 1,527개 코딩 항목에서 각 코드의 출현 빈도를 집계하여 정량적 근거를 생성하였다.

**Claude Code 프롬프트:**

```
policy_1027_all_coding.md의 코딩 결과를 기반으로 장애요인 코드별 빈도를
집계하고, 기관별 분포를 교차표로 작성해주세요.

## 요청 사항
1. 전체 빈도표: 장애요인(OB_*) 코드별 출현 횟수, 내림차순 정렬
2. 기관별 교차 빈도표: 행 = 장애요인 코드, 열 = 15개 기관
3. 비율(%): 각 기관 내에서의 코드별 비율도 함께 제시
4. Top 10 코드에 대해 간략한 해석 추가

출력 형식: 마크다운 테이블
```

**분석 결과 예시 — 장애요인 코드 Top 10:**

| 순위 | 코드 | 장애요인 | 빈도(회) | 비율(%) | 기관 수(/15) |
|------|------|---------|---------|---------|-------------|
| 1 | `OB_GOV_REL` | 정부 부처와의 관계 | 79 | 14.8% | 15/15 |
| 2 | `OB_INTERNAL_SYS` | 내부 추진 체계 | 76 | 14.2% | 14/15 |
| 3 | `OB_ADMIN_PROC` | 행정·제도적 한계 | 71 | 13.3% | 15/15 |
| 4 | `OB_BUDGET` | 예산·재정 제약 | 58 | 10.9% | 13/15 |
| 5 | `OB_HR_LIMIT` | 인력 한계 | 52 | 9.7% | 12/15 |
| 6 | `OB_EVAL_SYS` | 평가 체계 문제 | 47 | 8.8% | 11/15 |
| 7 | `OB_LEGAL_REG` | 법적·규제적 제약 | 39 | 7.3% | 10/15 |
| 8 | `OB_INFO_ASYM` | 정보 비대칭 | 31 | 5.8% | 9/15 |
| 9 | `OB_CULTURE` | 조직 문화 | 28 | 5.2% | 8/15 |
| 10 | `OB_GEOPOLITICAL` | 지정학적 제약 | 23 | 4.3% | 7/15 |

> **분석적 함의:** 상위 3개 장애요인(정부 부처와의 관계 79회, 내부 추진 체계 76회, 행정·제도적 한계 71회)은 15개 기관 중 14~15개 기관에서 공통적으로 언급되었다. 이는 이들 장애요인이 특정 기관의 고유한 문제가 아니라 정부출연연구기관 시스템 전반의 **구조적 문제(structural issue)**임을 시사한다.

### 6.1.2 기관별 교차 빈도표 (Cross-tabulation)

교차 빈도표는 코드와 기관의 관계를 한눈에 파악할 수 있게 한다. 아래는 장애요인 상위 5개 코드에 대한 기관별 분포 예시이다.

```
정부 부처와의 관계 (OB_GOV_REL) — 기관별 분포

기관:  A   B   C   D   E   F   G   H   I   J   K   L   M   N   O  | 합계
빈도:  7   5   6   4   8   3   6   5   7   4   5   6   4   5   4  |  79
비율: 8.9 6.3 7.6 5.1 10.1 3.8 7.6 6.3 8.9 5.1 6.3 7.6 5.1 6.3 5.1| 100%
```

**교차 빈도표 전체 생성 프롬프트:**

```
policy_1027_all_coding.md를 분석하여 다음 교차 빈도표를 생성해주세요.

## 교차표 1: 장애요인 코드 × 기관
- 행: 모든 OB_* 코드 (빈도 내림차순)
- 열: 15개 기관 (A-O)
- 셀: 해당 기관에서 해당 코드의 출현 빈도
- 행 합계, 열 합계, 전체 합계 포함

## 교차표 2: 성공 동인 코드 × 기관
- 행: 모든 CD_* 코드 (빈도 내림차순)
- 열: 15개 기관
- 동일 형식

## 교차표 3: 맥락 코드 × 주제 코드 대분류
- 행: 10개 맥락 코드
- 열: 8개 주제 코드 대분류
- 셀: 해당 맥락에서 해당 주제가 코딩된 빈도

각 교차표에 대해 주목할 만한 패턴을 3가지씩 기술해주세요.
```

### 6.1.3 비율(%) 제시와 상대적 중요도 파악

절대 빈도만으로는 기관 간 비교가 왜곡될 수 있다. 인터뷰 분량이 기관마다 다르기 때문이다. 따라서 **기관 내 비율(%)**을 함께 제시하여 상대적 중요도를 정확히 파악한다.

```
## 비율 기반 비교 예시

기관 A: 전체 코딩 항목 120개 중 OB_GOV_REL 7건 = 5.8%
기관 E: 전체 코딩 항목  85개 중 OB_GOV_REL 8건 = 9.4%

→ 절대 빈도는 기관 E가 1건 더 많지만,
  기관 내 비율로 보면 기관 E에서 이 장애요인이
  상대적으로 훨씬 더 현저(salient)하게 나타남
```

---

## 6.2 기관별 비교 분석

### 6.2.1 보편적 패턴 vs 특수 패턴

빈도 분석 결과를 기반으로, 15개 기관에서 공통적으로 나타나는 **보편적 패턴(universal patterns)**과 일부 기관에서만 나타나는 **특수 패턴(particular patterns)**을 구분한다.

```
보편적 패턴 (15개 기관 중 12개 이상에서 출현)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
• 정부 부처와의 관계 문제 (15/15 기관)
• 행정·제도적 한계 (15/15 기관)
• 내부 추진 체계 부재 (14/15 기관)
• 예산·재정 제약 (13/15 기관)
• 인력 한계 (12/15 기관)

특수 패턴 (7개 이하 기관에서 출현)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
• 지정학적 제약 (7/15 기관 — 주로 안보·에너지 관련 기관)
• 기술 표준 경쟁 (5/15 기관 — 표준 선도형 기관에 집중)
• 국제 규범 충돌 (4/15 기관 — 규제 연구 기관에 한정)
```

### 6.2.2 기관 유형별 군집 분석 (Cluster Analysis by Institution Type)

본 프로젝트에서 도출한 7대 전략적 발견 중 하나는 기관을 **유형별로 군집화(clustering)**하여 차별적 정책 처방을 제시한 것이다.

| 유형 | 특성 | 해당 기관 수 | 핵심 장애요인 | 핵심 성공 동인 |
|------|------|------------|-------------|-------------|
| **자원 의존형** | 해외 장비·시설 접근이 핵심 | 5개 | 예산 제약, 행정 절차 | 기술 접근(CD_TECH_ACCESS) |
| **표준 선도형** | 국제 표준 수립·주도가 목표 | 4개 | 인력 한계, 기술 표준 경쟁 | 네트워크(CD_NETWORK) |
| **기술 추격형** | 선진국 기술 학습·추격이 목적 | 6개 | 정부 관계, 정보 비대칭 | 자율성(CD_AUTONOMOUS) |

**군집 분석 프롬프트:**

```
policy_1027_all_coding.md와 comprehensive_policy_analysis.md를 참조하여
15개 기관을 국제협력의 전략적 특성에 따라 유형별로 군집화해주세요.

## 군집화 기준
1. 가장 빈번하게 나타나는 장애요인 코드 조합
2. 가장 빈번하게 나타나는 성공 동인 코드 조합
3. 국제협력의 목적과 방식 (기술 접근, 표준 선도, 기술 추격 등)

## 출력
- 각 군집의 명칭, 특성, 소속 기관 목록
- 군집 간 차별적 장애요인 및 성공 동인
- 군집별 정책 시사점
```

### 6.2.3 기관 프로파일 카드

분석 결과를 기관별로 요약한 "프로파일 카드"를 작성하면 비교 분석이 용이해진다.

```markdown
## 기관 프로파일 카드 — 기관 A (가명)

### 기본 정보
- 유형: 자원 의존형
- 인터뷰 분량: 1,400줄
- 코딩 항목 수: 120개

### 장애요인 Top 3
1. OB_ADMIN_PROC (행정·제도적 한계) — 12회 (10.0%)
2. OB_GOV_REL (정부 부처와의 관계) — 7회 (5.8%)
3. OB_BUDGET (예산·재정 제약) — 7회 (5.8%)

### 성공 동인 Top 3
1. CD_TECH_ACCESS (기술 접근 기회) — 9회 (7.5%)
2. CD_LEADER (리더십) — 6회 (5.0%)
3. CD_NETWORK (네트워크 자산) — 5회 (4.2%)

### 특이 패턴
- 행정 절차 관련 장애요인이 정부 관계보다 더 빈번
  → 기관 자체의 내부 행정 체계가 외부 협력의 병목
- 기술 접근이 가장 강력한 동인
  → 해외 장비·시설 활용이 국제협력의 주된 동기
```

---

## 6.3 패턴 도출 및 주제 간 관계 분석

### 6.3.1 코드 간 공기 분석 (Co-occurrence Analysis)

**공기 분석(co-occurrence analysis)**이란 동일한 시맨틱 유닛에 함께 부여된 코드들의 조합을 분석하는 방법이다. 이를 통해 단순한 빈도를 넘어 코드 간의 **연관 관계(association)**를 파악할 수 있다.

**공기 분석 프롬프트:**

```
policy_1027_all_coding.md에서 복수의 주제 코드가 부여된 항목을 추출하고,
코드 쌍(pair)별 공기 빈도를 계산해주세요.

## 요청 사항
1. 모든 코드 쌍의 공기 빈도표 (상삼각 행렬)
2. 공기 빈도 상위 10개 쌍 목록
3. 각 상위 쌍에 대해 대표적 원문 발췌 1건씩 제시
4. 공기 패턴에서 도출되는 인과적 해석

## 출력 형식
### 공기 빈도 상위 10
| 순위 | 코드 1 | 코드 2 | 공기 빈도 | 대표 발췌 |
```

**분석 결과 예시 — 공기 빈도 상위 5:**

| 순위 | 코드 1 | 코드 2 | 공기 빈도 | 해석 |
|------|--------|--------|----------|------|
| 1 | `OB_GOV_REL` | `OB_BUDGET` | 34회 | 정부의 지원 부재가 예산 제약으로 직결 |
| 2 | `OB_ADMIN_PROC` | `OB_INTERNAL_SYS` | 28회 | 외부 행정 절차와 내부 체계 미비가 상호 강화 |
| 3 | `CD_TECH_ACCESS` | `CD_NETWORK` | 25회 | 기술 접근과 네트워크 구축이 상호 촉진 |
| 4 | `OB_GOV_REL` | `PR_GOV_REFORM` | 22회 | 정부 관계 장애요인이 정부 개혁 제언으로 직결 |
| 5 | `OB_HR_LIMIT` | `OB_EVAL_SYS` | 19회 | 인력 부족과 평가 체계가 악순환 구조 형성 |

### 6.3.2 인과 관계 네트워크 도출

공기 분석 결과를 기반으로 코드 간의 인과 관계를 네트워크로 시각화할 수 있다. 본 프로젝트에서는 다음과 같은 인과 관계 구조를 도출하였다.

```
                    ┌─────────────────────────┐
                    │   정부 부처와의 관계      │
                    │   (OB_GOV_REL, 79회)     │
                    └───────────┬─────────────┘
                                │
                    ┌───────────▼─────────────┐
              ┌─────┤   예산·재정 제약          │
              │     │   (OB_BUDGET, 58회)      │
              │     └───────────┬─────────────┘
              │                 │
              │     ┌───────────▼─────────────┐
              │     │   인력 한계              │
              │     │   (OB_HR_LIMIT, 52회)    │
              │     └───────────┬─────────────┘
              │                 │
              │     ┌───────────▼─────────────┐
              ├─────┤   내부 추진 체계 부재     │
              │     │   (OB_INTERNAL_SYS, 76회)│
              │     └───────────┬─────────────┘
              │                 │
              │     ┌───────────▼─────────────┐
              └─────┤   행정·제도적 한계        │
                    │   (OB_ADMIN_PROC, 71회)  │
                    └─────────────────────────┘

→ 해석: 정부 부처와의 관계 문제가 최상위 원인(root cause)으로 작용하며,
  이것이 예산 제약 → 인력 한계 → 내부 체계 미비 → 행정 절차 복잡화의
  연쇄적 인과 구조(cascading causal structure)를 형성한다.
```

### 6.3.3 3단계 위계적 정책 프레임 설계

보편적 패턴과 특수 패턴의 구분, 그리고 기관 유형별 군집 분석 결과를 종합하여 **3단계 위계적 정책 프레임워크(3-tier hierarchical policy framework)**를 설계하였다. 이는 본 연구의 핵심적 학술 기여 중 하나이다.

```
┌─────────────────────────────────────────────────────────────┐
│                    Tier 1: 보편 정책 (Universal)              │
│   15개 기관 공통 장애요인에 대응하는 범정부 차원의 정책           │
│                                                             │
│   • 국제협력 전담 예산 확보 제도화                              │
│   • 행정 절차 간소화 (해외 출장·계약 등)                        │
│   • 국제협력 성과 평가 체계 개편                                │
├─────────────────────────────────────────────────────────────┤
│                Tier 2: 유형별 정책 (Type-specific)             │
│   기관 유형(자원 의존형/표준 선도형/기술 추격형)에 따른           │
│   차별화된 정책                                               │
│                                                             │
│   • 자원 의존형 → 대형 국제공동시설 참여 지원 프로그램            │
│   • 표준 선도형 → 국제표준기구 활동 지원 및 인력 파견             │
│   • 기술 추격형 → 해외 연수·파견 확대 및 정보 공유 플랫폼         │
├─────────────────────────────────────────────────────────────┤
│           Tier 3: 개별 정책 (Institution-specific)             │
│   특정 기관의 고유한 상황에 맞춤화된 정책                        │
│                                                             │
│   • 기관 A → 내부 행정 체계 재설계 (자체 규정 개정)              │
│   • 기관 E → 지정학적 리스크 관리 체계 구축                     │
│   • 기관 K → 기술 표준 경쟁력 강화를 위한 전문 인력 확보          │
└─────────────────────────────────────────────────────────────┘
```

**프레임워크 설계 프롬프트:**

```
comprehensive_policy_analysis.md와 policy_1027_all_coding.md의 분석 결과를
바탕으로 3단계 위계적 정책 프레임워크를 설계해주세요.

## 설계 원칙
1. Tier 1 (보편): 12개 이상 기관에서 공통으로 나타난 장애요인에 대응
2. Tier 2 (유형별): 기관 군집 분석 결과에 기반한 유형별 차별화 정책
3. Tier 3 (개별): 특정 기관의 고유한 패턴에 맞춤화된 정책

## 각 Tier에 대해
- 대응하는 장애요인 코드와 빈도
- 근거가 되는 원문 발췌 (대표 사례 2-3건)
- 구체적 정책 제언 (실행 가능한 수준으로)
- 기대 효과 및 한계

## 참고 파일
- qualitative/comprehensive_policy_analysis.md (35KB)
- qualitative/policy_1027_all_coding.md (59KB)
- qualitative/policy_recommendations.md (23KB)
```

---

## 6.4 Claude Code를 활용한 종합 분석 보고서 생성

### 6.4.1 분석 결과의 서사적(Narrative) 전환

코딩 결과, 빈도표, 교차 분석표는 연구의 "중간 산출물"이다. 이를 학술 논문이나 정책 보고서에 적합한 **서사적(narrative) 형식**으로 전환하는 것이 종합 분석의 최종 단계이다.

**서사적 보고서 생성 프롬프트:**

```
다음 분석 파일들을 종합하여 학술 보고서 형식의 서사적 분석 보고서를
작성해주세요.

## 입력 파일
1. qualitative/policy_1027_all_coding.md — 전체 코딩 결과
2. qualitative/comprehensive_policy_analysis.md — 종합 분석
3. qualitative/policy_recommendations.md — 정책 제언

## 보고서 구조
1. 연구 질문에 대한 답변 (3개 연구 질문 각각에 대해)
2. 주요 발견 (Major Findings)
   - 빈도 데이터를 근거로 한 핵심 주장
   - 대표적 원문 인용 (in-vivo quotes)
   - 기관 간 비교 분석 결과
3. 이론적 함의 (Theoretical Implications)
   - 기존 문헌과의 연결
   - 새로운 이론적 기여
4. 정책적 함의 (Policy Implications)
   - 3단계 위계적 정책 프레임 기반
   - 구체적 실행 방안

## 문체 지침
- 학술 논문 수준의 객관적이고 분석적인 문체
- 주장-근거-해석의 3단 논증 구조
- 모든 주장에 빈도 데이터 또는 원문 인용 근거 제시
- 한국어 작성, 학술 용어는 영문 병기

출력 파일: qualitative/comprehensive_policy_analysis.md
```

### 6.4.2 연구 질문에 대한 답변 구조화

종합 보고서의 핵심은 연구 질문(research questions)에 대한 체계적인 답변이다. 본 프로젝트에서는 다음과 같은 구조로 답변을 구성하였다.

```markdown
## 연구 질문 1: 정부출연연구기관의 국제협력에서 나타나는 주요 장애요인은
무엇인가?

### 답변 요약
15개 기관의 인터뷰 분석 결과, 총 534개의 장애요인 관련 코딩 항목이
식별되었으며, 이는 10개의 하위 범주로 분류된다. 상위 3개 장애요인 —
정부 부처와의 관계(79회, 14.8%), 내부 추진 체계 부재(76회, 14.2%),
행정·제도적 한계(71회, 13.3%) — 이 전체 장애요인 언급의 42.3%를
차지한다.

### 상세 분석

#### 1. 정부 부처와의 관계 (OB_GOV_REL) — 79회, 15/15 기관
15개 기관 모두에서 언급된 가장 보편적인 장애요인으로, [...]

대표 인용:
> "주무 부처에서 국제협력을 본업으로 인정하지 않으니까, 아무리 좋은
> 성과를 내도 기관 평가에 반영이 안 됩니다." (기관 E)

[이하 상세 분석 계속]
```

### 6.4.3 반복적 보고서 개선 프로세스

보고서는 한 번에 완성되지 않는다. Claude Code의 장점은 반복적(iterative) 개선을 빠르게 수행할 수 있다는 점이다.

```
반복 1: 초안 생성
  → raw/llm_analysis_1003.md (초기 1차 분석)
  → 5개 주제, 빈도표 포함

반복 2: 심층 분석
  → qualitative/comprehensive_policy_analysis.md (35KB)
  → 15개 기관 전체 분석, Top 10 빈도 분석, 장애요인 코드 심층 해석

반복 3: 정책 제언 독립 문서화
  → qualitative/policy_recommendations.md (23KB)
  → 5대 핵심 정책 제언, 3단계 프레임워크

반복 4: 문체 개선
  → PR #6 (codex/improve-policy-memo-writing-style)
  → 정책 메모 문체로 전환, 가독성 향상

반복 5: 한국어 최종본
  → PR #8 (claude/qualitative-policy-synthesis)
  → 한국어 번역 및 최종 보고서
```

---

## 6.5 실제 산출물 소개

본 프로젝트의 분석 및 종합 과정에서 생성된 주요 산출물을 소개한다. 각 파일의 역할, 규모, 내용 구조를 이해하면 자신의 프로젝트에 유사한 체계를 설계하는 데 도움이 된다.

### 6.5.1 qualitative/comprehensive_policy_analysis.md — 종합 분석 보고서

| 항목 | 내용 |
|------|------|
| **파일 크기** | 35KB |
| **성격** | 15개 기관 전체를 아우르는 종합 분석 보고서 |
| **주요 내용** | 장애요인 코드 Top 10 빈도 분석, 기관별 비교 분석, 코드 간 공기 패턴, 인과 관계 네트워크, 정책 시사점 |
| **생성 브랜치** | `claude/qualitative-policy-synthesis-016xtsPM6fhmr4SMZ7BSwxNr` (PR #7) |
| **활용** | 최종 보고서의 "분석 결과" 장에 직접 활용 |

```markdown
# 파일 구조 (개요)
comprehensive_policy_analysis.md
├── 1. 연구 개요 및 방법론
├── 2. 전체 코드 빈도 분석
│   ├── 2.1 장애요인 코드 빈도
│   ├── 2.2 성공 동인 코드 빈도
│   └── 2.3 정책 제언 코드 빈도
├── 3. 기관별 비교 분석
│   ├── 3.1 기관 유형별 군집화
│   └── 3.2 기관 프로파일 요약
├── 4. 패턴 분석 및 이론적 해석
│   ├── 4.1 공기 분석
│   ├── 4.2 인과 관계 네트워크
│   └── 4.3 기존 문헌과의 연결
├── 5. 정책 프레임워크
│   ├── 5.1 Tier 1 — 보편 정책
│   ├── 5.2 Tier 2 — 유형별 정책
│   └── 5.3 Tier 3 — 개별 정책
└── 6. 결론 및 향후 연구 과제
```

### 6.5.2 qualitative/policy_1027_all_coding.md — 전체 코딩 결과

| 항목 | 내용 |
|------|------|
| **파일 크기** | 59KB |
| **성격** | 15개 기관의 완전한 코딩 결과 데이터베이스 |
| **주요 내용** | 1,527개 코딩 항목, 각 항목의 원문 발췌·주제 코드·맥락 코드·비고, 기관별 코드 빈도표 |
| **생성 브랜치** | `codex/analyze-and-code-interview-transcripts` (PR #4) |
| **활용** | 모든 후속 분석의 기초 데이터, 빈도 분석·교차 분석·공기 분석의 원천 |

```markdown
# 파일 구조 (개요)
policy_1027_all_coding.md
├── 코딩 메타데이터
│   ├── 코드북 버전: GRI-IC-Codebook v1.0
│   ├── 총 코딩 항목: 1,527개
│   └── 교차 검증: κ = 0.85
├── 기관별 코딩 테이블 (15개 기관 × 개별 테이블)
│   ├── 기관 A: 120개 항목
│   ├── 기관 B: 98개 항목
│   │   ...
│   └── 기관 O: 105개 항목
└── 기관별 코드 빈도 요약표
```

> **데이터 관리 팁:** 59KB의 코딩 결과 파일은 본 프로젝트의 가장 핵심적인 데이터 자산이다. 이 파일의 모든 변경 이력은 Git을 통해 추적되며, 수정 시에는 반드시 별도 커밋으로 기록하여 변경 사유를 명시하였다.

### 6.5.3 qualitative/policy_recommendations.md — 정책 제언

| 항목 | 내용 |
|------|------|
| **파일 크기** | 23KB |
| **성격** | 분석 결과에 기반한 5대 핵심 정책 제언 문서 |
| **주요 내용** | 5개 정책 제언, 각 제언의 근거(빈도 데이터 + 원문 인용), 실행 방안, 기대 효과, 3단계 프레임워크 매핑 |
| **생성 브랜치** | `claude/qualitative-policy-synthesis-016xtsPM6fhmr4SMZ7BSwxNr` (PR #7, #9) |
| **활용** | 정책 보고서 및 학술 논문의 "정책 제언" 장에 직접 활용 |

```markdown
# 파일 구조 (개요)
policy_recommendations.md
├── 정책 제언 개요
├── 제언 1: 국제협력 거버넌스 체계 개편
│   ├── 근거: OB_GOV_REL (79회), OB_INTERNAL_SYS (76회)
│   ├── 대표 인용 3건
│   ├── 실행 방안
│   └── 프레임워크 위치: Tier 1 (보편)
├── 제언 2: 행정 절차 간소화
├── 제언 3: 유형별 맞춤 지원 프로그램
├── 제언 4: 인력 개발 및 평가 체계 개편
├── 제언 5: 정보 공유 플랫폼 구축
└── 종합 로드맵
```

### 6.5.4 raw/llm_analysis_1003.md — 초기 분석

| 항목 | 내용 |
|------|------|
| **파일 크기** | (초기 분석 파일) |
| **성격** | Claude Code를 활용한 최초 1차(first-pass) LLM 분석 결과 |
| **주요 내용** | 5개 주제 도출, 초기 빈도표, 예비적 패턴 식별 |
| **생성 브랜치** | `claude/science-policy-interviews-011CUXesC8zHtKDUA6pmvFVZ` (PR #1) |
| **활용** | 분석 방향 설정 및 코드북 초안 개발의 기초 자료 |

```markdown
# 파일 구조 (개요)
llm_analysis_1003.md
├── 1차 분석 개요
├── 주제 1: 국제협력의 동기와 목적
├── 주제 2: 장애요인
├── 주제 3: 성공 사례와 동인
├── 주제 4: 조직·제도적 맥락
├── 주제 5: 정책 제언
└── 초기 빈도표 및 패턴 메모
```

> **방법론적 시사점:** 초기 분석 파일(`llm_analysis_1003.md`)에서 최종 종합 분석(`comprehensive_policy_analysis.md`)까지의 발전 과정은, LLM 기반 질적 분석이 단일 단계가 아닌 **반복적 심화(iterative deepening)** 과정임을 잘 보여준다. 초기 분석은 5개 주제의 개괄적 파악에 그쳤지만, 체계적 코드북 개발 → 전수 코딩 → 빈도 분석 → 교차 분석 → 패턴 도출의 과정을 거치면서 7대 전략적 발견과 3단계 정책 프레임워크라는 심층적 결과로 발전하였다.

### 6.5.5 산출물 간 관계도

```
raw/llm_analysis_1003.md (초기 분석)
        │
        ▼ [코드북 개발 → 전수 코딩]
qualitative/policy_1027_all_coding.md (59KB, 1,527 항목)
        │
        ├──▶ [빈도 분석, 교차 분석, 공기 분석, 인과 관계 도출]
        │
        ▼
qualitative/comprehensive_policy_analysis.md (35KB, 종합 분석)
        │
        ├──▶ [정책 프레임워크 설계, 제언 구체화]
        │
        ▼
qualitative/policy_recommendations.md (23KB, 정책 제언)
```

이 일련의 산출물은 Git 저장소에서 9개의 PR(Pull Request)을 통해 추적 가능한 형태로 관리되었으며, 전문가 리뷰에서 5점 만점의 평가를 받았다. 이는 Claude Code와 GitHub를 결합한 질적 연구 방법론이 학술적 수준의 연구 산출물을 체계적으로 생산할 수 있음을 입증하는 사례이다.

---

> **다음 장 안내:** 제7장에서는 품질 보증(Quality Assurance) 전략을 다룬다. Cohen's Kappa를 넘어서는 다양한 신뢰성·타당성 확보 방안, 동료 검토(peer review) 프로세스, 그리고 감사 추적(audit trail)의 구축 방법을 상세히 기술한다.

---

# 7. Phase 5 — 최종 결과물 생성 (Deliverables)

## 7.1 최종 보고서 작성 전략

질적 연구의 궁극적 목표는 분석 결과를 체계적이고 설득력 있는 보고서로 전환하는 것이다. 본 프로젝트에서는 4,108줄(142KB) 규모의 `final_report.md`와 947줄(102KB)의 축약 개정판 `final_report_revised.md`를 Claude Code를 활용하여 작성하였다. 이 절에서는 그 과정을 단계별로 상세히 설명한다.

### 7.1.1 다파트(Multi-Part) 구조로 나누어 작성하기

대규모 보고서를 한 번에 생성하면 Claude Code의 출력 한계에 부딪히거나, 내용의 일관성이 떨어질 수 있다. 이를 해결하기 위해 보고서를 논리적 단위로 분할하여 순차적으로 작성하는 전략을 사용한다.

본 프로젝트에서는 최종 보고서를 다음 5개 파트로 분할하였으며, 이는 실제 PR #3의 커밋 이력에서 확인할 수 있다:

| 파트 | 내용 | 커밋 메시지 |
|------|------|------------|
| Part 1-2 | Executive Summary, 서론, 방법론 | `Add final report Part 1-2: Executive Summary, Introduction, and Methodology` |
| Part 3 | 분석 및 핵심 발견사항 | `Add final report Part 3: Chapter 3 - Analysis and Core Findings` |
| Part 4 | 종합 정책 제언 | `Add final report Part 4: Chapter 4 - Comprehensive Policy Recommendations` |
| Part 5 | 실행 로드맵 및 결론 | `Complete final report Part 5: Chapter 5 - Implementation Roadmap and Conclusion` |

이 분할 전략의 핵심은 **각 파트가 논리적으로 완결된 단위**여야 한다는 점이다. 예를 들어 Part 1-2에서 연구 배경과 방법론을 확정한 뒤 Part 3에서 분석 결과를 작성하면, 방법론과 분석 간의 일관성을 유지할 수 있다.

**분할 작성 시 프롬프트 전략:**

```
# Part 1-2 작성 프롬프트 예시
다음 분석 데이터와 코드북을 바탕으로 최종 보고서의 제1장 '서론'과 제2장 '연구 방법론'을
학술 보고서 형식으로 작성해주세요.

# 보고서 메타 정보
- 연구 책임: KAIST 과학기술정책대학원
- 연구 기간: 2025년 1월 - 10월
- 연구 유형: 질적 실증 연구 (Qualitative Empirical Study)
- 분석 규모: 15개 출연연구기관, 21,011줄 심층 인터뷰, 1,527개 구조화 발췌문

# 포함할 내용
1. 연구 배경: 미중 기술 패권 경쟁, 기술 동맹 재편, 글로벌 공급망 재구조화
2. 연구 목적: 진단(Diagnosis), 이론화(Theorization), 처방(Prescription)
3. 방법론: "양적 규모의 질적 연구(Qualitative Research at Scale)" 개념 설명
4. LLM 기반 구조화 분류 과정 상세 기술

# 톤과 스타일
- 학술 보고서 형식이되, 정책 입안자도 읽을 수 있는 수준
- 모든 주장에 구체적 수치 근거 제시
- 한국어로 작성하되, 학술 용어는 영문 병기
```

```
# Part 3 작성 프롬프트 예시
이전에 작성한 제1-2장의 방법론을 기반으로, 제3장 '분석 및 핵심 발견사항'을 작성해주세요.

# 핵심 분석 결과 (반드시 포함)
1. 3대 구조적 장애: 정부 부처와의 관계(79회), 내부 추진 체계(76회), 행정·제도적 한계(71회)
2. 7대 전략적 과제: R&D 역량 강화(66회), ODA(61회), 다자 협력(61회) 등
3. 기관별 차별화된 협력 동인: Tier 1(보편), Tier 2(유형별), Tier 3(개별)

# 참조 파일
- @Policy_merged.csv: 1,527개 코딩 데이터
- @llm_analysis_1003.md: LLM 1차 분석 결과
- @final_feedback.md: 전문가 검증 결과

# 작성 지침
- 모든 주장에는 빈도 데이터를 근거로 제시
- "N개 기관에서 M회 언급" 형식으로 보편성 표시
- 대표 인용문(verbatim quote)을 적절히 배치
```

### 7.1.2 Claude Code에 보고서 구조와 톤 지시하기

보고서의 품질은 프롬프트에서 **구조, 톤, 독자층**을 얼마나 명확히 지정하느냐에 달려 있다. 본 프로젝트에서 효과적이었던 지시 방법은 다음과 같다.

**구조 지시 — 위계적 목차 사전 설정:**

```
다음 목차 구조에 맞추어 제4장을 작성해주세요:

# 제4장. 종합 정책 제언

## 4.1 정책 설계의 기본 원칙
  #### 4.1.1 현장 근거 기반 원칙
  #### 4.1.2 다층 정책 설계 원칙
  #### 4.1.3 단계적 실행 원칙

## 4.2 즉시 실행 과제 (0-6개월)
  #### 4.2.1 범부처 국제협력 정례 협의체
  #### 4.2.2 행정 혁신 태스크포스

## 4.3 중기 구축 과제 (6-24개월)
  #### 4.3.1 국가 통합 정보 플랫폼 (K-GRIP)
  #### 4.3.2 전문 인재 파이프라인

## 4.4 장기 변혁 과제 (3-10년)
  #### 4.4.1 국가과학기술국제협력위원회 신설
  #### 4.4.2 표준화 30년 전략
```

**톤 지시 — 독자층 명시:**

```
# 문체 지침
- 1차 독자: 과학기술정책 전공 대학원생 및 교수
- 2차 독자: 과기정통부 정책 담당자
- 문체: 학술적 엄밀성을 유지하되, 정책 실무자가 이해할 수 있는 명료한 한국어
- 피해야 할 것: 지나친 추상성, 근거 없는 주장, 감상적 표현
- 권장하는 것: "N회 언급(M개 기관)" 형식의 정량적 근거, 구체적 기관명 사례
```

### 7.1.3 반복적 수정: 초고에서 최종본까지

본 프로젝트의 보고서는 다음과 같은 반복 개선 과정을 거쳤다. 이 과정은 실제 Git 이력으로 추적 가능하다:

```
[1단계] 초고 작성
  PR #3: claude/science-policy-interviews → final_report.md (4,108줄)
    ↓ 전문가 피드백 (feedback.md, feedback_v2.md)
[2단계] 피드백 반영 수정
  PR #7: claude/qualitative-policy-synthesis → comprehensive_policy_analysis.md
    ↓ 한국어 번역 필요
[3단계] 번역 및 문체 개선
  PR #8: claude/qualitative-policy-synthesis → comprehensive_policy_analysis_kor.md
    ↓ 분량 조정 및 구조 재편 필요
[4단계] 최종 축약본 작성
  PR #9: claude/qualitative-policy-synthesis → final_report_revised.md (947줄)
    + final_report_revised_part1.md, final_report_revised_part2.md (분할 출판본)
```

**수정 지시 프롬프트 예시:**

```
전문가 피드백(feedback_v2.md)을 반영하여 final_report.md를 수정해주세요.

# 주요 수정 사항
1. 범주 간 상호배타성(Mutual Exclusivity) 부족 → 분류 체계 재정비
2. 방법론 서술 강화 → LLM 활용 과정 구체적 기술 추가
3. 삼각검증(Triangulation) 절차 명시 → 검증 과정 상세화

# 수정 원칙
- 기존 구조와 논지는 유지
- 피드백에서 지적된 약점만 보완
- 새로운 데이터 추가 시 기존 분석과 일관성 유지
```

**축약 지시 프롬프트 예시:**

```
final_report.md(4,108줄)를 학술 저널 투고 가능한 분량(약 1,000줄)으로 축약해주세요.

# 축약 원칙
1. Executive Summary는 유지하되 간결화
2. 방법론 섹션: 핵심 절차만 요약 (세부 사항은 부록 참조 처리)
3. 분석 결과: 3대 구조적 장애와 7대 전략적 과제 중심으로 압축
4. 정책 제언: 5대 핵심 제언 중심, 예산과 로드맵 표 형태로 제시
5. 사례 인용: 가장 대표적인 것만 선별 (각 발견당 1-2개)
```

---

## 7.2 정책 메모/브리프 작성

### 7.2.1 학술 보고서와 정책 문서의 차이

동일한 연구 결과를 담더라도 학술 보고서와 정책 메모(Policy Memo)는 근본적으로 다른 문서다. 본 프로젝트에서 `final_report.md`(4,108줄)와 `policy_memo.md`(199줄)의 차이를 비교하면 이 점이 명확해진다:

| 구분 | 학술 보고서 (`final_report.md`) | 정책 메모 (`policy_memo.md`) |
|------|------|------|
| **분량** | 4,108줄 (142KB) | 199줄 (14KB) |
| **독자** | 연구자, 학술 커뮤니티 | 장관, 이사장, 정책 결정자 |
| **목적** | 방법론적 엄밀성 입증, 학술적 기여 | 즉각적 의사결정 지원 |
| **구조** | 5장 체계 (서론→방법론→분석→제언→결론) | 수신/참조 → 요약 → 진단 → 제언 → 예산 |
| **톤** | 학술적, 신중한 서술 | 단호하고 행동 지향적 |
| **근거 제시** | 상세한 데이터, 인용문, 교차 검증 | 핵심 수치만 압축 (79회, 76회, 71회) |
| **결론** | "~을 시사한다", "~할 필요가 있다" | "~을 강력히 요청한다", "지금이 바로 행동할 때이다" |

### 7.2.2 정책 메모의 구성요소

본 프로젝트에서 작성한 `policy_memo.md`의 실제 구조를 바탕으로 정책 메모의 필수 구성요소를 설명한다:

**헤더 (Header) — 공문서 형식:**
```markdown
**수신**: 과학기술정보통신부 장관
**참조**: 국가과학기술연구회 이사장, 기초과학연구원 원장
**발신**: KAIST 과학기술정책대학원
**일자**: 2025년 10월 27일
**제목**: 출연연구기관 국제협력 역량 비약을 위한 통합 정책 패키지
```

이 헤더는 정책 문서가 누구에게, 누구로부터, 어떤 목적으로 전달되는지를 명확히 한다. Claude Code에 이 형식을 지정하면 문서의 전체 톤이 공식 정책 문서에 맞추어 조정된다.

**요약 (Executive Summary) — 핵심 메시지 압축:**

정책 메모의 요약은 학술 논문의 초록(Abstract)과 근본적으로 다르다. 학술 초록이 "이 연구는 무엇을 했고 무엇을 발견했는가"를 서술한다면, 정책 메모의 요약은 "무엇이 문제이고, 무엇을 해야 하며, 얼마가 드는가"를 즉각적으로 전달해야 한다.

본 프로젝트 `policy_memo.md`의 요약은 다음 요소를 포함하였다:
- 분석 근거의 규모: "21,011줄 분량의 심층 인터뷰 분석 결과"
- 3대 핵심 문제: 빈도 데이터와 함께 제시 (79회, 76회, 71회)
- 3단계 로드맵 개요: 즉시 실행(0-6개월), 중기 구축(6-24개월), 장기 변혁(3-10년)
- 총 소요 예산: "연간 1,200억원(신규 750억원 + 재배분 450억원)"
- 정량적 성과 목표: "협력 효율성 30% 향상, 중복 예산 연 100억원 절감"

**5대 핵심 제언 — 번호 매기기와 우선순위:**

정책 결정자는 긴 서술보다 명확하게 번호가 매겨진 제언 목록을 선호한다. 본 프로젝트에서는 5개 핵심 제언을 다음과 같이 구조화하였다:

1. 범부처 과학기술 국제협력 정례 협의체 즉시 설치
2. 국제협력 행정 혁신 태스크포스 가동
3. 국가 통합 국제협력 정보 플랫폼(K-GRIP) 단계적 구축
4. 국제협력 전문 인재 파이프라인 제도화
5. 국가과학기술국제협력위원회 및 표준화 30년 전략 법제화

각 제언에는 예산, 추진 주체, 기대 효과, 일정이 반드시 포함되어야 한다.

**정책 메모 작성 프롬프트 예시:**

```
final_report.md의 분석 결과와 정책 제언을 바탕으로, 과학기술정보통신부 장관에게
보내는 정책 메모를 작성해주세요.

# 형식 요건
- 수신/참조/발신/일자/제목 헤더 포함
- 전체 분량: 200줄 이내 (A4 5-6페이지)
- 요약(Executive Summary)으로 시작

# 내용 요건
1. 전략 환경과 정책 필요성 (미중 경쟁, 기술 동맹 등 외부 환경 간결 정리)
2. 증거 기반 진단 (3대 구조적 병목 — 빈도 데이터 포함)
3. 5대 핵심 제언 (각각 예산, 일정, 기대 효과 명시)
4. 3단계 실행 로드맵 (표 형식)
5. 재원 조달 전략
6. 결론 및 요청 사항

# 톤
- 단호하고 행동 지향적
- "~할 필요가 있다"가 아닌 "~을 요청한다", "~이 필수적이다"
- 학술적 경어체 사용하되, 결론은 강력한 촉구로 마무리
```

### 7.2.3 정책 메모의 문체 개선

본 프로젝트에서는 PR #6(`codex/improve-policy-memo-writing-style`)을 통해 정책 메모의 문체를 별도로 개선하였다. 이는 초기 작성된 정책 메모가 학술 보고서의 톤에서 완전히 벗어나지 못했기 때문이다.

**문체 개선 프롬프트 예시:**

```
policy_memo.md의 문체를 정책 브리프에 적합하게 수정해주세요.

# 수정 방향
1. 수동태 → 능동태: "분석되었다" → "분석 결과 확인하였다"
2. 학술적 유보 → 단정적 표현: "~것으로 보인다" → "~이다"
3. 서술 중심 → 행동 중심: 각 문단이 "그러므로 ~해야 한다"로 귀결
4. 불필요한 배경 설명 축소 → 핵심 메시지 강조
5. 수치 데이터를 볼드체로 강조
```

---

## 7.3 인터랙티브 시각화 생성

### 7.3.1 Claude Code로 Plotly.js 기반 HTML 시각화 생성하기

질적 연구의 결과를 시각적으로 전달하면 정책 입안자의 이해도와 몰입도를 크게 높일 수 있다. Claude Code는 HTML, CSS, JavaScript를 직접 생성할 수 있으므로, 별도의 시각화 도구 없이 인터랙티브 시각화를 만들 수 있다.

본 프로젝트에서는 3가지 인터랙티브 시각화를 생성하였으며, 모두 PR #9에서 `final_report_revised_part1.md`와 함께 커밋되었다:

1. `viz_policy_roadmap.html` — Gantt 차트 (정책 로드맵)
2. `viz_causal_network.html` — 인과관계 네트워크 다이어그램
3. `viz_institutional_heatmap.html` — 기관별 장애요인 히트맵

**공통 기술 스택:**
- **차트 라이브러리**: Plotly.js (`cdn.plot.ly/plotly-2.27.0.min.js`) — Gantt 차트, 히트맵
- **네트워크 라이브러리**: vis-network (`cdnjs.cloudflare.com/ajax/libs/vis-network/9.1.2`) — 인과관계 네트워크
- **한국어 레이블**: 모든 축, 범례, 호버 텍스트를 한국어로 설정
- **반응형 디자인**: `max-width: 1400px`, CSS Grid/Flexbox 기반 레이아웃
- **인터랙티브 기능**: 호버(hover) 시 상세 정보 표시, 줌(zoom), 필터(filter)

### 7.3.2 Gantt 차트 — 정책 로드맵 시각화

`viz_policy_roadmap.html`은 3단계 정책 로드맵을 시간축 위에 배치하여 각 과제의 시작-종료 시점, 우선순위, 예산을 한눈에 파악할 수 있게 한다.

**프롬프트 예시:**

```
정책 로드맵을 Plotly.js를 사용한 Gantt 차트로 시각화하는 단일 HTML 파일을 만들어주세요.

# 데이터
3단계로 구분하고, 각 단계별 과제:
- 단기(0-6개월): 부처간 협의체, 행정절차 간소화 TF, 긴급 대응 기금
- 중기(6-24개월): K-GRIP 플랫폼, 인력 양성 프로그램, 전문가 경력 트랙
- 장기(3-10년): 국제협력위원회 법안, 위원회 출범, 표준화 30년 전략

# 시각화 요구사항
1. 각 단계를 색상으로 구분: 단기(초록), 중기(주황), 장기(파랑)
2. 호버 시 과제명, 기간, 예산, 우선순위 표시
3. 하단에 3개 카드로 각 단계 요약 (핵심 과제, 예산)
4. 재원 조달 계획 메모 포함
5. 한국어 폰트 (Noto Sans KR), 반응형 디자인
6. CDN으로 Plotly.js 로드 (https://cdn.plot.ly/plotly-2.27.0.min.js)
```

실제 생성된 파일에서는 Plotly의 `bar` 트레이스를 수평 방향으로 사용하여 Gantt 차트를 구현하였고, 단계별 요약 카드를 CSS Grid로 배치하였다:

```javascript
// 실제 구현된 데이터 구조 예시
const tasks = [
    {
        Task: '부처간 정기 협의체',
        Start: '2025-03-01',
        Finish: '2025-06-01',
        Resource: '단기',
        Priority: '최우선'
    },
    {
        Task: '행정절차 간소화 TF',
        Start: '2025-03-01',
        Finish: '2025-09-01',
        Resource: '단기',
        Priority: '최우선'
    },
    // ... 중기, 장기 과제 계속
];
```

### 7.3.3 인과관계 네트워크 다이어그램

`viz_causal_network.html`은 5대 구조적 문제의 상호 연결성과 악순환 구조를 시각화한다. vis-network 라이브러리의 물리 시뮬레이션(force-directed layout)을 활용하여 노드 간 인과관계를 직관적으로 보여준다.

**네트워크의 구조:**
- **노드 유형 5가지** (색상 구분):
  - 정책/거버넌스 문제 (빨강): "정책 거버넌스 파편화", "하향식 정책 추진"
  - 예산/재정 문제 (주황): "예산 불안정성", "단기 성과 중심 평가"
  - 인적자원 문제 (파랑): "인센티브 왜곡", "전문인력 부족"
  - 시스템/인프라 문제 (보라): "전문지원 인프라 부재", "행정절차 복잡성"
  - 협력 결과 (초록): "파트너 신뢰 상실", "프로젝트 실패", "연구자 동기 저하"

- **엣지(인과 경로)**:
  - 정책 파편화 → 예산 불안정 → 파트너 신뢰 상실 (핵심 경로)
  - 단기 평가 → 인센티브 왜곡 → 연구자 동기 저하 (보상 경로)
  - 피드백 루프(악순환): 연구자 동기 저하 → 전문인력 부족 → 프로젝트 실패 → 예산 삭감 (점선)

**프롬프트 예시:**

```
장애요인 간 인과관계를 vis-network 라이브러리를 사용한 네트워크 다이어그램으로
시각화하는 단일 HTML 파일을 만들어주세요.

# 노드와 엣지 정의
## 원인 노드 (4개 범주)
- 정책/거버넌스: 정책 파편화(value=30), 하향식 추진(value=20)
- 예산/재정: 예산 불안정(value=30), 단기 평가(value=20)
- 인적자원: 인센티브 왜곡(value=25), 전문인력 부족(value=20)
- 시스템: 전문지원 부재(value=20), 행정 복잡성(value=15)

## 결과 노드
- 파트너 신뢰 상실(value=25): KAERI 캐나다 SMART 사례
- 프로젝트 실패(value=20): 졸속 구성, 중도 취소
- 연구자 동기 저하(value=20): 젊은 연구자 기피

## 인과 경로 (화살표)
- 정책 파편화 → 예산 불안정 (부처별 분산 예산)
- 예산 불안정 → 파트너 신뢰 상실 (약속 불이행) [굵은 선]
- 프로젝트 실패 → 예산 삭감 [점선, 피드백 루프]
- ... (전체 경로 목록 제공)

# 시각화 요건
1. 노드 크기는 value에 비례
2. 드래그, 줌 인/아웃 가능
3. 호버 시 상세 설명 표시
4. 하단에 범례와 핵심 발견 요약
```

### 7.3.4 기관별 장애요인 히트맵

`viz_institutional_heatmap.html`은 15개 출연연구기관의 10대 장애요인 발생 빈도를 색상 강도로 표현한다. 이 시각화는 "어떤 기관이 어떤 문제를 가장 심각하게 겪고 있는가"를 한눈에 파악하게 해준다.

**프롬프트 예시:**

```
15개 출연연구기관의 장애요인 빈도 데이터를 Plotly.js 히트맵으로 시각화하는
단일 HTML 파일을 만들어주세요.

# 데이터
- Y축 (15개 기관): KAERI, ETRI, KRISS, KRISO, KRIBB, NIGT, KIER, KIMM,
                    KRICT, KARI, KBSI, NST, KFE, KISTI, KIGAM
- X축 (10대 장애요인): 인프라/지원 부족, 파트너 관계, 지정학적 제약,
                      정책 분절화, 내부 비효율, HR 인센티브, HR 전문성,
                      하향식 접근, IPR 분쟁, 임무 불일치
- 값: Policy_merged.csv에서 추출한 기관별 장애요인 언급 빈도

# 시각화 요건
1. 색상 스케일: 초록(낮음) → 노랑(중간) → 빨강(높음)
2. 호버 시: "기관명 + 장애요인 + 빈도" 표시
3. X축 레이블 45도 회전 (가독성)
4. 범례: 매우 심각(3회 이상), 심각(2회), 중간(1회), 경미(언급 없음)
```

### 7.3.5 시각화 생성 시 유의사항

Claude Code로 시각화를 생성할 때 대학원생이 주의해야 할 점은 다음과 같다:

1. **단일 HTML 파일**: 모든 CSS, JavaScript를 한 파일에 인라인으로 포함시켜야 외부 의존성 없이 어디서든 열 수 있다. CDN 링크만 외부 참조로 사용한다.

2. **데이터 정확성 검증**: Claude Code가 생성한 시각화의 수치 데이터가 원본 분석 데이터(`Policy_merged.csv`)와 일치하는지 반드시 확인해야 한다. 시각화의 데이터가 보고서 본문의 수치와 다르면 신뢰성이 훼손된다.

3. **한국어 폰트 처리**: 웹 폰트(Noto Sans KR)를 CSS `font-family`의 첫 번째로 지정하고, 시스템 기본 폰트를 폴백(fallback)으로 설정한다:
   ```css
   font-family: 'Noto Sans KR', -apple-system, sans-serif;
   ```

4. **반복 수정 가능**: 생성된 시각화가 마음에 들지 않으면 "색상 스케일을 변경해주세요", "노드 크기를 더 크게 해주세요", "범례 위치를 오른쪽으로 옮겨주세요" 등으로 세부 조정을 요청할 수 있다.

---

## 7.4 전문가 피드백 통합

### 7.4.1 Claude Code로 체계적 피드백 문서 생성

본 프로젝트의 독특한 점은 Claude Code를 전문가 역할(Expert Reviewer)로 활용하여 체계적인 피드백 문서를 생성한 것이다. 이 피드백은 분석 결과의 타당성 검증과 보고서 품질 향상에 기여하였다.

**피드백 요청 프롬프트 예시:**

```
과학기술정책 전문가의 관점에서 policy_1027_revised.md의 분류 체계와
Policy_merged.csv의 코딩 결과에 대한 건설적 피드백을 작성해주세요.

# 평가 관점
1. 분류 체계의 이론적 타당성: 5대 테마 구조가 적절한가?
2. 범주 간 상호배타성(Mutual Exclusivity): 중복 분류 가능성은?
3. 분류 정확도: 1,527개 발췌문의 코딩 품질은?
4. 정책 연계성: 분석 결과가 실행 가능한 정책으로 연결되는가?

# 평가 형식
- 종합 평가: 5점 척도
- 평가 항목별 점수 및 상세 코멘트
- 강점과 보완점을 균형 있게 제시
- 구체적 개선 제안 포함
```

### 7.4.2 피드백 진화 과정: 4단계 반복 루프

본 프로젝트에서 피드백 문서는 4단계에 걸쳐 진화하였다. 각 단계의 문서는 실제 저장소에 남아 있어 피드백 개선 과정을 추적할 수 있다:

```
[1단계] feedback.md — 초기 전문가 피드백
  - PR #1에서 커밋: "Add comprehensive expert feedback on science policy interview classification"
  - 종합 평가: ★★★★☆ (4점)
  - 주요 지적: 범주 간 상호배타성 부족, 방법론 서술 미흡

[2단계] feedback_addition.md — 보완 의견서
  - 기존 LLM 분석(llm_analysis_1003.md)의 정량적 성과를 인정하는 보완 평가
  - 핵심 메시지: "분석의 정량적 근거는 매우 정확하며, feedback.md가 이를 충분히 인정하지 못함"

[3단계] feedback_v2.md — 전문 검증 결과 통합
  - PR #2에서 커밋: "Add updated feedback with full analysis integration and LLM analysis reference"
  - 종합 평가: ★★★★★ (4.5점)
  - 21,011줄 전체 분석 + 1,527개 전체 행 검증 결과 반영
  - 분류 정확도 85% 확인, 정량적 데이터 100% 일치 검증

[4단계] final_feedback.md — 최종 통합 평가
  - 이전 3개 문서의 통합 최종본
  - 종합 평가: ★★★★★ (5점)
  - feedback.md + feedback_addition.md + feedback_v2.md 통합
```

이 과정에서 주목할 점은 **평가 점수가 4점 → 4.5점 → 5점으로 상승**한 것이다. 이는 초기 피드백의 지적 사항을 반영하여 분석과 보고서를 개선한 결과이며, 피드백 루프가 실질적으로 연구 품질을 향상시켰음을 보여준다.

### 7.4.3 피드백 반영 프롬프트 예시

```
feedback_v2.md에서 지적된 3가지 보완 사항을 final_report.md에 반영해주세요.

# 보완 사항
1. "범주 간 상호배타성 부족" (feedback_v2.md 섹션 III.1.1)
   → 제2장 방법론에 "분류 원칙과 중복 처리 규칙" 하위 섹션 추가
   → 중복 분류가 발생한 경우의 처리 기준 명시

2. "포화(Saturation) 개념의 활용 미흡" (feedback_v2.md 섹션 III.3)
   → 제2장에 "데이터 포화도 평가" 하위 섹션 추가
   → 15개 기관 인터뷰 진행 과정에서 새로운 테마 출현이 중단된 시점 명시

3. "국제 비교 관점 강화" (feedback_v2.md 섹션 III.4)
   → 제3장에 "국제 비교 시사점" 하위 섹션 추가
   → 미국, EU, 일본의 출연연 국제협력 거버넌스와 비교

# 수정 원칙
- 기존 내용 삭제 없이 보완만 수행
- 추가되는 내용은 기존 논리 흐름에 자연스럽게 통합
- 새로운 근거 데이터 추가 시 Policy_merged.csv에서 발췌
```

### 7.4.4 LLM 분석 평가 문서의 활용

`llm_analysis_evaluation.md`는 LLM 기반 분석 방법론 자체에 대한 메타 평가 문서다. 이 문서는 5점 만점 평가와 함께 분석 방법론의 강점("정량적 분석의 정확성 및 명료성")과 약점("방법론 서술 부족")을 상세히 기술한다.

이 문서의 존재는 질적 연구에서 **방법론적 반성(Methodological Reflexivity)**을 실천하는 좋은 사례다. 연구자가 자신의 분석 도구(LLM)의 한계를 인식하고, 이를 명시적으로 문서화함으로써 연구의 투명성과 신뢰성을 높일 수 있다.

---

# 8. GitHub 워크플로우 & 버전 관리

## 8.1 왜 GitHub인가: 연구 과정의 투명성, 재현성, 협업

전통적으로 질적 연구의 분석 과정은 연구자의 머릿속에서 진행되며, 그 과정은 최종 보고서의 방법론 섹션에 간략히 기술되는 것이 관례였다. 이는 두 가지 심각한 문제를 야기한다:

1. **투명성(Transparency) 결여**: "1,527개 발췌문을 어떤 순서로, 어떤 기준으로 코딩하였는가?"에 대한 구체적 기록이 없다.
2. **재현성(Reproducibility) 부족**: 다른 연구자가 동일한 데이터로 동일한 결론에 도달할 수 있는지 검증할 방법이 없다.

GitHub을 연구 워크플로우에 도입하면 이 문제를 근본적으로 해결할 수 있다:

| 전통적 질적 연구 | GitHub 기반 질적 연구 |
|---|---|
| 분석 과정이 연구자 머릿속에 존재 | 모든 분석 단계가 커밋으로 기록 |
| 수정 이력 추적 불가 | `git log`로 전체 변경 이력 조회 |
| 최종본만 공유 | 초고 → 피드백 → 수정 → 최종본 전과정 공개 |
| 협업 시 파일 충돌 | 브랜치와 PR로 체계적 병합 |
| "연구 일지"를 별도 작성 | 커밋 메시지가 연구 일지 역할 |

**GitHub 저장소 초기 설정:**

```bash
# 1. 저장소 초기화
git init kaist-policy-research
cd kaist-policy-research

# 2. 기본 디렉토리 구조 생성
mkdir -p raw          # 원본 데이터 (인터뷰 전사록, CSV)
mkdir -p qualitative  # 분석 중간 산출물
mkdir -p deliverables # 최종 결과물

# 3. .gitignore 설정 — 민감 데이터 보호
cat << 'EOF' > .gitignore
# 익명화 전 원본 데이터
raw/original_transcripts/
# 개인 식별 정보
*.private.md
# 시스템 파일
.DS_Store
EOF

# 4. 초기 커밋
git add .
git commit -m "Initialize research project structure"

# 5. GitHub 원격 저장소 연결
git remote add origin https://github.com/username/kaist-policy-research.git
git push -u origin main
```

> **주의**: 인터뷰 전사록에 개인 식별 정보(이름, 직함, 기관 내부 정보 등)가 포함되어 있을 경우, 반드시 익명화 처리 후 커밋해야 한다. `.gitignore`에 원본 파일 경로를 추가하거나, 저장소 자체를 비공개(private)로 설정하는 것을 권장한다.

---

## 8.2 Claude Code 브랜치 전략

### 8.2.1 자동 생성 브랜치 이름 형식

Claude Code는 새로운 작업을 시작할 때 자동으로 브랜치를 생성한다. 브랜치 이름은 다음 형식을 따른다:

```
claude/{task-description}-{session-id}
```

또는 Codex(OpenAI)를 함께 사용한 경우:

```
codex/{task-description}
```

본 프로젝트에서 실제 사용된 브랜치는 다음과 같다:

```
claude/science-policy-interviews-011CUXesC8zHtKDUA6pmvFVZ
codex/analyze-and-code-interview-transcripts
codex/analyze-and-code-interview-transcripts-joilor
codex/improve-policy-memo-writing-style
claude/qualitative-policy-synthesis-016xtsPM6fhmr4SMZ7BSwxNr
```

세션 ID 접미사(`011CUXesC8...`, `016xtsPM6f...`)는 Claude Code 세션 식별자로, 동일한 작업 설명(task-description)을 가진 여러 세션을 구분하는 역할을 한다.

### 8.2.2 각 분석 단계별 브랜치 역할

본 프로젝트의 브랜치 사용 패턴을 분석하면, 연구 과정의 논리적 단계가 브랜치로 자연스럽게 구분됨을 알 수 있다:

**1단계: 초기 분석 및 전문가 피드백**
```
브랜치: claude/science-policy-interviews-011CUXesC8zHtKDUA6pmvFVZ

PR #1: 전문가 피드백 생성
  커밋: "Add comprehensive expert feedback on science policy interview classification"
  산출물: feedback.md

PR #2: 피드백 보완 및 LLM 분석 통합
  커밋: "Add updated feedback with full analysis integration and LLM analysis reference"
  커밋: "Add supplementary feedback comparing existing LLM analysis with expert review"
  산출물: feedback_v2.md, feedback_addition.md

PR #3: 최종 보고서 5파트 작성
  커밋: "Add final consolidated documents for science policy analysis"
  커밋: "Add final report Part 1-2: Executive Summary, Introduction, and Methodology"
  커밋: "Add final report Part 3: Chapter 3 - Analysis and Core Findings"
  커밋: "Add final report Part 4: Chapter 4 - Comprehensive Policy Recommendations"
  커밋: "Complete final report Part 5: Chapter 5 - Implementation Roadmap and Conclusion"
  산출물: final_report.md (4,108줄)
```

**2단계: 코딩 작업 및 파일 정리**
```
브랜치: codex/analyze-and-code-interview-transcripts

PR #4: 내러티브 코딩 테이블 작성
  커밋: "Add narrative context to consolidated coding tables"
  산출물: 코딩 테이블 업데이트

브랜치: codex/analyze-and-code-interview-transcripts-joilor

PR #5: 파일 구조 정리
  커밋: "Organize source and deliverable files into folders"
  산출물: raw/, qualitative/, deliverables/ 디렉토리 구조 확립
```

**3단계: 문체 개선**
```
브랜치: codex/improve-policy-memo-writing-style

PR #6: 정책 메모 문체 수정
  커밋: "Revise policy memo and document updates"
  산출물: policy_memo.md 문체 개선본
```

**4단계: 종합 분석 및 최종 산출물**
```
브랜치: claude/qualitative-policy-synthesis-016xtsPM6fhmr4SMZ7BSwxNr

PR #7: 종합 정책 분석 (영문)
  커밋: "Add comprehensive policy analysis and recommendations for R&D international cooperation"
  산출물: comprehensive_policy_analysis.md

PR #8: 한국어 번역
  커밋: "Add Korean translation of comprehensive policy analysis"
  산출물: comprehensive_policy_analysis_kor.md

PR #9: 최종 축약본 + 시각화
  커밋: "Add revised final report (Part 1) and interactive visualizations"
  커밋: "Add comprehensive policy report Part 2"
  커밋: "Add complete revised final report (merged)"
  산출물: final_report_revised.md, final_report_revised_part1.md,
         final_report_revised_part2.md, viz_*.html (3개)
```

### 8.2.3 브랜치 생성 및 관리 명령어

대학원생이 직접 브랜치를 관리할 때 사용하는 기본 Git 명령어는 다음과 같다:

```bash
# 현재 브랜치 확인
git branch

# 모든 브랜치 확인 (원격 포함)
git branch -a

# 새 브랜치 생성 및 이동
git checkout -b claude/my-analysis-task

# 작업 완료 후 main으로 돌아오기
git checkout main

# 브랜치 삭제 (머지 완료 후)
git branch -d claude/my-analysis-task
```

Claude Code를 사용하면 이 과정이 자동화된다. Claude Code에 작업을 지시하면 적절한 브랜치를 자동으로 생성하고, 작업 완료 후 PR을 생성한다. 연구자는 PR 리뷰와 머지만 담당하면 된다.

---

## 8.3 PR 기반 반복 개선 사이클

### 8.3.1 하나의 분석 단위 = 하나의 PR

Pull Request(PR)는 단순한 코드 병합 도구가 아니라, 질적 연구에서 **하나의 분석 단위를 검토하고 승인하는 게이트(gate)** 역할을 한다. 본 프로젝트의 9개 PR은 각각 연구의 한 단계를 대표한다:

| PR # | 분석 단계 | 산출물 | 연구자 확인 사항 |
|------|----------|--------|----------------|
| #1 | 전문가 피드백 초안 | `feedback.md` | 피드백의 적절성, 분류 체계 타당성 |
| #2 | 피드백 보완 | `feedback_v2.md`, `feedback_addition.md` | LLM 분석 결과 통합 적절성 |
| #3 | 최종 보고서 5파트 | `final_report.md` | 보고서 구조, 논리 흐름, 데이터 정확성 |
| #4 | 코딩 테이블 | 코딩 테이블 업데이트 | 코딩 일관성, 내러티브 맥락 적절성 |
| #5 | 파일 정리 | 디렉토리 구조 | 파일 위치, 명명 규칙 |
| #6 | 정책 메모 문체 | `policy_memo.md` 개선 | 정책 문서 톤, 핵심 메시지 전달력 |
| #7 | 종합 분석 (영문) | `comprehensive_policy_analysis.md` | 분석 깊이, 정책 제언 구체성 |
| #8 | 한국어 번역 | `comprehensive_policy_analysis_kor.md` | 번역 정확성, 학술 용어 적절성 |
| #9 | 최종본 + 시각화 | `final_report_revised.md`, `viz_*.html` | 축약 적절성, 시각화 데이터 정확성 |

### 8.3.2 PR 리뷰 워크플로우

실제 연구에서 PR 리뷰는 다음과 같은 과정으로 진행된다:

```bash
# 1. Claude Code가 생성한 PR 목록 확인
gh pr list

# 2. 특정 PR의 변경 내용 확인
gh pr view 3 --web    # 웹 브라우저에서 PR #3 열기
gh pr diff 3           # 터미널에서 diff 확인

# 3. PR에 포함된 파일 목록 확인
gh pr diff 3 --stat

# 4. PR 리뷰 코멘트 추가
gh pr review 3 --comment --body "제3장의 빈도 데이터를 Policy_merged.csv와 대조 확인 완료"

# 5. PR 승인 및 머지
gh pr merge 3 --merge
```

**PR 리뷰 시 연구자가 확인해야 할 체크리스트:**

- [ ] 빈도 데이터가 원본 CSV 파일과 일치하는가?
- [ ] 인용문(verbatim quote)이 원본 인터뷰 전사록과 일치하는가?
- [ ] 분석 해석이 데이터에 의해 뒷받침되는가 (과잉 해석은 없는가)?
- [ ] 이전 파트/PR과의 논리적 일관성이 유지되는가?
- [ ] 학술 용어의 사용이 적절하고 일관적인가?
- [ ] 정책 제언이 실행 가능하고 구체적인가?

### 8.3.3 PR을 통한 연구 과정 추적

9개 PR의 시간 순서를 따라가면 연구 전체의 진행 과정을 복원할 수 있다. 이는 전통적 "연구 일지(Research Journal)"를 대체하는 강력한 도구다:

```bash
# 전체 PR 이력을 시간순으로 조회
gh pr list --state merged --limit 20

# 특정 PR의 상세 정보 (생성일, 머지일, 커밋 수 등)
gh pr view 3

# 특정 PR에 포함된 커밋 목록
gh pr view 3 --json commits --jq '.commits[].messageHeadline'
```

예를 들어 PR #3의 커밋 이력만 추출하면 보고서 작성의 시간 순서를 확인할 수 있다:

```
1. "Add final consolidated documents for science policy analysis"
2. "Add final report Part 1-2: Executive Summary, Introduction, and Methodology"
3. "Add final report Part 3: Chapter 3 - Analysis and Core Findings"
4. "Add final report Part 4: Chapter 4 - Comprehensive Policy Recommendations"
5. "Complete final report Part 5: Chapter 5 - Implementation Roadmap and Conclusion"
```

이 이력은 연구자가 Executive Summary와 방법론을 먼저 확정한 뒤, 분석 결과 → 정책 제언 → 실행 로드맵 순서로 보고서를 작성했음을 보여준다.

---

## 8.4 커밋 히스토리를 통한 연구 과정 추적

### 8.4.1 커밋 메시지의 중요성

커밋 메시지는 미래의 자신과 다른 연구자를 위한 **연구 기록(Audit Trail)**이다. 좋은 커밋 메시지는 "무엇을 했는가"뿐 아니라 "왜 했는가"를 담아야 한다.

**본 프로젝트의 실제 커밋 메시지 분석:**

| 커밋 메시지 | 평가 | 개선 제안 |
|---|---|---|
| `Add final report Part 3: Chapter 3 - Analysis and Core Findings` | 우수 — 파트 번호, 장 번호, 내용 명시 | — |
| `Add comprehensive expert feedback on science policy interview classification` | 우수 — 산출물 성격, 연구 맥락 포함 | — |
| `Organize source and deliverable files into folders` | 양호 — 행위 명시 | "raw/, qualitative/, deliverables/ 구조" 추가 가능 |
| `revised` | 미흡 — 무엇을 수정했는지 불명확 | "Revise Executive Summary based on feedback_v2.md" 등으로 구체화 |
| `Add files via upload` | 미흡 — GitHub 웹 업로드 기본 메시지 | 파일 내용에 따라 구체적으로 작성 |

**권장 커밋 메시지 형식:**

```
[파트/단계] 동사 + 구체적 내용

# 예시
[Analysis] Add frequency analysis for 15 GRI obstacles (Policy_merged.csv)
[Report] Complete Chapter 3 with 3 structural barriers and 7 strategic findings
[Feedback] Integrate feedback_v2 corrections on mutual exclusivity issues
[Viz] Create Plotly.js Gantt chart for 3-phase policy roadmap
[Memo] Revise policy memo tone from academic to action-oriented
```

### 8.4.2 git log를 통한 연구 일지 대체 가능성

`git log`의 다양한 옵션을 활용하면 연구 진행 상황을 다각도로 파악할 수 있다:

```bash
# 전체 커밋 이력 (간략)
git log --oneline --all

# 그래프 형태로 브랜치 분기/병합 시각화
git log --oneline --all --graph --decorate

# 특정 파일의 변경 이력 추적
git log --oneline --follow -- deliverables/final_report.md

# 날짜 범위 지정
git log --oneline --after="2025-10-01" --before="2025-11-01"

# 커밋별 변경 파일 목록
git log --oneline --stat

# 특정 키워드가 포함된 커밋만 검색
git log --oneline --all --grep="feedback"
```

**실제 활용 예시 — "피드백 관련 커밋만 추적":**

```bash
$ git log --oneline --all --grep="feedback"
0f8513b Add updated feedback with full analysis integration and LLM analysis reference
45a750a Add supplementary feedback comparing existing LLM analysis with expert review
ed0b6d3 Add comprehensive expert feedback on science policy interview classification
```

이 결과만으로도 피드백이 3단계에 걸쳐 진화했음을 확인할 수 있다.

**실제 활용 예시 — "최종 보고서 작성 과정 추적":**

```bash
$ git log --oneline --all --grep="final report"
28910b0 Add complete revised final report (merged)
5230c5e Add revised final report (Part 1) and interactive visualizations
26a2b23 Complete final report Part 5: Chapter 5 - Implementation Roadmap and Conclusion
29b6593 Add final report Part 4: Chapter 4 - Comprehensive Policy Recommendations
09c61df Add final report Part 3: Chapter 3 - Analysis and Core Findings
caf5597 Add final report Part 1-2: Executive Summary, Introduction, and Methodology
```

이 6개 커밋은 초기 4파트 작성(Part 1-2 → Part 3 → Part 4 → Part 5)과 최종 축약 작업(revised Part 1 → merged)의 전 과정을 담고 있다.

### 8.4.3 diff를 활용한 변경 분석

두 시점 사이의 변경 내용을 비교하면 "어떤 피드백이 어떻게 반영되었는가"를 구체적으로 확인할 수 있다:

```bash
# 특정 커밋 간 diff — 피드백 반영 전후 비교
git diff caf5597..09c61df -- deliverables/final_report.md

# PR #3의 전체 변경 내용 확인
git diff 68f40c9..08cebf9

# 특정 파일의 마지막 수정 내용
git diff HEAD~1 -- deliverables/policy_memo.md

# 변경 통계 (추가/삭제 줄 수)
git diff --stat caf5597..26a2b23
```

---

## 8.5 협업 연구에서의 활용

### 8.5.1 여러 연구자가 같은 데이터를 분석할 때의 브랜치 전략

질적 연구에서 **연구자 삼각검증(Investigator Triangulation)**은 동일한 데이터를 여러 연구자가 독립적으로 분석하여 해석의 신뢰성을 높이는 방법이다. GitHub 브랜치를 활용하면 이 과정을 체계적으로 관리할 수 있다.

**권장 브랜치 구조 (다수 연구자 협업):**

```
main (최종 합의본)
├── researcher-A/coding-round1     (연구자 A의 1차 코딩)
├── researcher-B/coding-round1     (연구자 B의 1차 코딩)
├── claude/independent-coding      (Claude Code 독립 코딩)
├── reconciliation/round1          (불일치 조정)
├── researcher-A/analysis          (연구자 A의 분석)
├── researcher-B/analysis          (연구자 B의 분석)
├── claude/synthesis               (Claude Code 종합 분석)
└── consensus/final-report         (최종 합의 보고서)
```

**협업 워크플로우 예시:**

```bash
# 연구자 A: 자신의 코딩 브랜치 생성
git checkout -b researcher-A/coding-round1
# ... 코딩 작업 수행 ...
git add qualitative/coding_researcher_A.csv
git commit -m "[Coding] Complete round 1 coding by Researcher A (523 units)"
git push -u origin researcher-A/coding-round1
gh pr create --title "Researcher A: Round 1 coding complete" \
             --body "523 meaning units coded. Ready for inter-coder reliability check."

# 연구자 B: 동일 데이터로 독립 코딩
git checkout main
git checkout -b researcher-B/coding-round1
# ... 코딩 작업 수행 ...
git add qualitative/coding_researcher_B.csv
git commit -m "[Coding] Complete round 1 coding by Researcher B (531 units)"
git push -u origin researcher-B/coding-round1

# 코더 간 신뢰도(Inter-coder Reliability) 확인 후 조정
git checkout -b reconciliation/round1
# Claude Code에 두 코딩 결과 비교 지시
# → 불일치 항목 식별, 논의 후 합의
git commit -m "[Reconciliation] Resolve 47 coding disagreements (91.2% initial agreement)"
```

### 8.5.2 코드 리뷰 = 분석 리뷰

소프트웨어 개발에서 코드 리뷰(Code Review)는 동료 개발자가 코드의 품질과 정확성을 검토하는 과정이다. 질적 연구에서 PR 리뷰는 이와 동일한 역할을 한다 — **동료 연구자가 분석의 품질과 정확성을 검토하는 과정**이다.

**리뷰 시 활용 가능한 GitHub 기능:**

1. **라인 단위 코멘트(Line Comments)**: 보고서의 특정 문장에 대해 "이 해석은 데이터에 의해 충분히 뒷받침되는가?", "79회라는 빈도가 Policy_merged.csv의 어떤 필터 조건에서 도출되었는가?" 등의 질문을 남길 수 있다.

2. **Suggestion 기능**: 리뷰어가 대안적 해석이나 수정된 문장을 직접 제안할 수 있다. 원저자는 이를 한 클릭으로 반영하거나 거부할 수 있다.

3. **Approve/Request Changes**: 분석 결과에 동의하면 "Approve", 수정이 필요하면 "Request Changes"를 선택한다. 모든 리뷰어가 Approve해야 머지할 수 있도록 설정하면 합의 기반 연구를 제도화할 수 있다.

```bash
# 리뷰어 지정
gh pr edit 3 --add-reviewer colleague-username

# 리뷰 코멘트 남기기
gh pr review 3 --comment --body "Table 3.1의 KIGAM 빈도(9.4%)가
Policy_merged.csv에서 확인한 수치(9.2%)와 소폭 차이가 있습니다.
원본 데이터 재확인을 요청합니다."

# 수정 요청
gh pr review 3 --request-changes --body "방법론 섹션에 LLM 프롬프트의
구체적 내용을 부록으로 첨부해야 재현성 요건을 충족합니다."

# 승인
gh pr review 3 --approve --body "데이터 검증 완료, 해석 적절, 머지 승인합니다."
```

### 8.5.3 지도교수-대학원생 협업 모델

지도교수와 대학원생의 연구 협업에서 GitHub을 활용하면 효율적인 피드백 루프를 구축할 수 있다:

```
대학원생 작업 흐름:
1. 브랜치 생성 → 분석/작성 작업 수행
2. PR 생성 (작업 내용, 질문 사항, 확인 요청 포함)
3. 지도교수에게 리뷰 요청

지도교수 리뷰 흐름:
1. PR diff 확인 → 라인별 코멘트
2. 수정 요청 또는 승인
3. 추가 분석 방향 제안

대학원생 수정 흐름:
1. 코멘트 반영하여 수정
2. 동일 PR에 추가 커밋 푸시
3. 재리뷰 요청
```

**이 모델의 장점:**
- 지도교수가 학생의 분석 과정을 **실시간으로** 추적할 수 있다
- 피드백이 문서화되어 "교수님이 지난번에 뭐라고 하셨지?"를 찾아볼 수 있다
- 학생의 성장 과정이 커밋 이력으로 기록된다
- 학위 논문 심사 시 연구 과정의 투명성을 입증할 수 있다

### 8.5.4 GitHub Issues를 활용한 연구 과제 관리

PR이 "완료된 작업의 검토"라면, GitHub Issues는 "앞으로 할 작업의 관리"에 해당한다:

```bash
# 연구 과제 이슈 생성
gh issue create --title "Chapter 3: 기관별 차별화 분석 추가" \
                --body "feedback_v2.md에서 지적된 Tier 2/3 분석이 불충분합니다.
                       KIGAM(9.4%), KFE(7.8%), KRISS(5.5%)의 차별적 특성을
                       심층 분석해야 합니다." \
                --label "analysis,high-priority"

# 이슈 목록 확인
gh issue list

# 이슈를 PR과 연결 (이슈 자동 닫힘)
git commit -m "Add institutional differentiation analysis (closes #12)"
```

### 8.5.5 연구 재현을 위한 최종 체크리스트

프로젝트 완료 시 다음 사항을 확인하여 다른 연구자(또는 미래의 자신)가 연구를 재현할 수 있도록 한다:

```bash
# 1. 모든 브랜치가 머지되었는지 확인
git branch -a --no-merged main

# 2. 최종 파일 구조 확인
ls -la raw/ qualitative/ deliverables/

# 3. 전체 커밋 이력을 파일로 저장 (연구 일지 백업)
git log --all --oneline > research_log.txt

# 4. 태그로 최종 버전 표시
git tag -a v1.0 -m "Final version: submitted to Ministry of Science and ICT"
git push origin v1.0

# 5. 저장소 상태 확인
git status
```

**본 프로젝트의 최종 저장소 구조:**

```
kaist-policy-research/
├── raw/                              # 원본 데이터
│   ├── 정책연구1-1 인터뷰 기록_250924.md   # 인터뷰 전사록
│   ├── Policy_merged.csv              # 1,527개 코딩 데이터
│   ├── policy_250924.md               # 초기 정책 문서
│   ├── policy_analysis_251024.md      # 분석 중간본
│   └── policy_1027_revised.md         # 수정된 분석본
├── qualitative/                       # 분석 산출물
│   ├── llm_analysis_1003.md           # LLM 1차 분석
│   ├── comprehensive_policy_analysis.md     # 종합 분석 (영문)
│   ├── comprehensive_policy_analysis_kor.md # 종합 분석 (한국어)
│   ├── policy_1027_all_coding.md      # 전체 코딩 테이블
│   └── policy_recommendations.md      # 정책 제언 초안
├── deliverables/                      # 최종 결과물
│   ├── final_report.md                # 최종 보고서 (4,108줄)
│   ├── final_report_revised.md        # 축약 보고서 (947줄)
│   ├── final_report_revised_part1.md  # 축약 보고서 Part 1
│   ├── final_report_revised_part2.md  # 축약 보고서 Part 2
│   ├── policy_memo.md                 # 정책 메모 (199줄)
│   ├── policy_memo_update.md          # 정책 메모 업데이트본
│   ├── feedback.md                    # 전문가 피드백 1차
│   ├── feedback_addition.md           # 피드백 보완 의견서
│   ├── feedback_v2.md                 # 피드백 2차 (전문 검증)
│   ├── final_feedback.md              # 최종 통합 피드백
│   ├── llm_analysis_evaluation.md     # LLM 방법론 평가
│   ├── viz_policy_roadmap.html        # Gantt 차트 시각화
│   ├── viz_causal_network.html        # 인과관계 네트워크 시각화
│   └── viz_institutional_heatmap.html # 기관별 히트맵 시각화
└── codebook.md                        # 코드북
```

이 구조에서 `raw/` → `qualitative/` → `deliverables/`의 흐름은 원본 데이터에서 분석을 거쳐 최종 산출물로 이어지는 연구 파이프라인을 반영한다. Git 이력과 함께 이 디렉토리 구조를 유지하면, 다른 연구자가 전체 연구 과정을 처음부터 끝까지 추적하고 재현할 수 있다.

---

# 9. 품질 검증 및 신뢰성 확보 (Quality Validation & Reliability)

## 9.1 왜 LLM 분석의 신뢰도 확보가 중요한가

LLM(Large Language Model)을 질적 연구의 코딩 도구로 활용하는 것은 아직 학술 커뮤니티에서 논쟁적인 주제이다. 전통적 질적 연구에서는 인간 연구자가 텍스트를 반복 정독하며 귀납적으로 코드를 부여하는 과정 자체가 연구의 핵심이며, 이 과정에서 연구자의 해석적 역량이 발휘된다. 따라서 "LLM이 코딩했다"는 사실만으로는 학술적 인정을 받기 어렵다.

학계의 주요 우려는 다음과 같다:

- **해석의 깊이(Depth of Interpretation)**: LLM이 텍스트의 표면적 의미만 포착하고, 행간의 의미(reading between the lines)나 아이러니, 화자의 의도를 놓칠 수 있다.
- **맥락 이해의 한계(Contextual Understanding)**: 인터뷰 상황에서의 비언어적 단서(표정, 톤, 침묵)를 LLM이 고려하지 못한다.
- **블랙박스 문제(Black Box Problem)**: LLM이 왜 특정 코드를 부여했는지 그 추론 과정을 완전히 추적하기 어렵다.
- **학술적 정당성(Academic Legitimacy)**: 동료 심사(Peer Review) 과정에서 "AI가 분석했다"는 점이 논문 거절 사유가 될 수 있다.

이러한 우려에 대응하기 위해서는 **정량적 검증(Quantitative Validation)**과 **전문가 피드백 루프(Expert Feedback Loop)**를 통해 LLM 분석 결과의 신뢰도를 체계적으로 입증해야 한다. 본 프로젝트에서는 Cohen's Kappa 계수를 활용한 코더 간 일치도 검증과 다단계 전문가 피드백을 수행하여, LLM 기반 분석이 전통적 방법과 동등한 수준의 신뢰도를 확보할 수 있음을 증명하였다.

핵심 메시지는 이것이다: **LLM은 도구(tool)이지, 연구자를 대체하는 것이 아니다.** LLM이 1차 분류를 수행하더라도, 연구자는 그 결과를 검증하고, 해석하고, 학술적 맥락에 배치하는 역할을 담당한다. 이 점을 명확히 하는 것이 학술적 정당성 확보의 출발점이다.

---

## 9.2 Cohen's Kappa를 활용한 정량적 검증

### 9.2.1 Cohen's Kappa 개념 이해

Cohen's Kappa(코헨의 카파, $\kappa$)는 두 코더(coder) 간의 일치도를 측정하는 통계 지표이다. 단순 일치율(Percent Agreement)과 달리, **우연에 의한 일치(chance agreement)를 보정**하여 실질적 일치도만을 측정한다는 점에서 학술적으로 인정받는 지표이다.

수식으로 표현하면 다음과 같다:

$$\kappa = \frac{P_o - P_e}{1 - P_e}$$

- $P_o$ = 관찰된 일치율 (Observed Agreement)
- $P_e$ = 우연에 의해 기대되는 일치율 (Expected Agreement)

예를 들어, 두 코더가 200개 항목 중 180개에서 같은 코드를 부여했다면 $P_o = 0.90$이다. 그런데 코드 분포상 우연에 의해서도 일치할 확률이 $P_e = 0.33$이라면:

$$\kappa = \frac{0.90 - 0.33}{1 - 0.33} = \frac{0.57}{0.67} \approx 0.85$$

### 9.2.2 해석 기준

Landis & Koch(1977)의 널리 통용되는 해석 기준은 다음과 같다:

| Kappa 값 | 해석 | 학술적 수용 가능성 |
|----------|------|-------------------|
| 0.81 - 1.00 | Almost Perfect (거의 완벽한 일치) | 매우 우수, 충분히 수용 가능 |
| 0.61 - 0.80 | Substantial (상당한 일치) | 수용 가능, 일부 보완 권장 |
| 0.41 - 0.60 | Moderate (보통 수준 일치) | 주의 필요, 추가 검증 요구 |
| 0.21 - 0.40 | Fair (약한 일치) | 분석 결과 재검토 필요 |
| 0.00 - 0.20 | Slight (미미한 일치) | 분석 결과 신뢰 불가 |
| < 0.00 | Poor (일치하지 않음) | 체계적 불일치, 방법론 재설계 |

일반적으로 질적 연구에서 **Kappa 0.70 이상**이면 학술적으로 수용 가능하며, **0.80 이상**이면 우수한 것으로 간주한다. 본 프로젝트에서 달성한 **Kappa 0.85**는 "Almost Perfect" 구간에 해당하며, LLM 기반 코딩이 인간 코더와 거의 동등한 수준의 일관성을 보임을 입증한다.

### 9.2.3 검증 절차: 4단계 프로토콜

본 프로젝트에서 수행한 교차 검증(Cross-validation) 절차는 다음과 같다:

**1단계: 무작위 표본 추출 (Random Sampling)**

전체 1,527개의 코딩된 발췌문에서 200개를 무작위로 추출하였다. 200개는 전체의 약 13.1%에 해당하며, 통계적으로 유의미한 표본 크기이다.

```
# Claude Code에서 무작위 표본 추출 예시
"Policy_merged.csv에서 200개의 행을 무작위로 추출하여 
validation_sample.csv로 저장해 주세요. 
단, 추출 시 각 대분류 테마의 비율이 전체 분포와 
유사하도록 층화 추출(Stratified Sampling)을 적용해 주세요."
```

층화 추출을 적용한 이유는, 특정 테마에 편중된 표본이 추출될 경우 Kappa 값이 왜곡될 수 있기 때문이다. 전체 데이터의 테마별 분포(협력 유형 20.0%, 애로사항 19.3%, 기관 특성 18.3%, 동인 15.7%, 정책 제언 13.7%, 기타 13.1%)를 반영하여 각 테마에서 비례적으로 추출하였다.

**2단계: 인간 코더 독립 코딩 (Independent Human Coding)**

추출된 200개 발췌문을 인간 연구자가 독립적으로 코딩하였다. 이때 핵심 원칙은 다음과 같다:

- 인간 코더는 LLM의 코딩 결과를 **사전에 열람하지 않는다** (블라인드 코딩).
- 인간 코더는 동일한 코드북(`codebook.md`)을 참조하여 코딩한다.
- 코딩 과정에서 판단이 어려운 경우, 해당 항목에 "판단 유보" 표시를 한다.
- 코딩에 소요된 시간을 기록한다 (비용-효율성 비교를 위해).

**3단계: LLM 결과와 비교 (Comparison)**

인간 코더의 결과와 LLM의 결과를 항목별로 대조하여, 일치(Agreement)와 불일치(Disagreement)를 분류하였다.

```
# Claude Code에서 비교 분석 요청 예시
"validation_sample.csv의 'llm_code' 열과 'human_code' 열을 비교하여:
1. 일치 항목 수와 불일치 항목 수를 집계해 주세요.
2. 불일치 항목의 패턴을 분석해 주세요 
   (어떤 코드 간 혼동이 자주 발생하는지).
3. 혼동 행렬(Confusion Matrix)을 생성해 주세요."
```

**4단계: Kappa 계수 산출 (Kappa Calculation)**

Claude Code를 활용하여 Kappa를 자동으로 계산할 수 있다:

```
"validation_sample.csv를 분석하여 Cohen's Kappa 계수를 계산해 주세요.

다음 정보를 포함해 주세요:
1. 관찰된 일치율 (Po)
2. 기대 일치율 (Pe)  
3. Cohen's Kappa 값
4. 95% 신뢰구간
5. 혼동 행렬 (Confusion Matrix)
6. 코드별 일치율 (Per-code Agreement)

계산에는 Python의 sklearn.metrics.cohen_kappa_score 함수를 
사용해 주세요."
```

본 프로젝트에서의 결과:

| 지표 | 값 |
|------|-----|
| 검증 표본 크기 | 200 / 1,527 (13.1%) |
| 관찰된 일치율 ($P_o$) | ~0.90 |
| Cohen's Kappa ($\kappa$) | **0.85** |
| 해석 | **Almost Perfect Agreement** |

### 9.2.4 불일치 항목 분석

Kappa 계산 이후 반드시 수행해야 하는 작업이 **불일치 항목 분석**이다. 본 프로젝트에서 불일치가 발생한 유형은 다음과 같았다:

- **경계 사례(Boundary Cases)**: "국제 표준"에 관한 발언이 동인(목적)인지 방식(수단)인지 경계가 모호한 경우
- **다중 코드 적용 가능(Multi-code Eligible)**: 하나의 발언이 "정부 부처 관계"와 "행정 제도적 한계" 양쪽에 해당하는 경우
- **맥락 의존적 판단(Context-dependent)**: 동일한 단어라도 앞뒤 맥락에 따라 코드가 달라지는 경우

불일치 패턴을 파악한 후, 코드북의 분류 기준을 보완하는 데 활용하였다. 예를 들어, `codebook.md`의 각 코드 정의에 "포함 기준"과 "제외 기준"을 명시하고, 경계 사례에 대한 판단 가이드라인을 추가하였다.

---

## 9.3 전문가 피드백 루프 (Expert Feedback Loop)

정량적 검증만으로는 부족하다. 분석 결과의 **내용적 타당성(Content Validity)**과 **해석의 적절성(Interpretive Adequacy)**은 해당 분야 전문가의 평가를 통해 확인해야 한다.

### 9.3.1 피드백 문서의 발전 과정

본 프로젝트에서는 전문가 피드백을 단발성이 아닌 **반복적 루프(Iterative Loop)**로 운영하였다. 피드백 문서의 발전 과정은 다음과 같다:

```
[1단계] feedback.md (28KB)
   - 초기 전문가 평가
   - 분류 체계의 강점 및 보완점 지적
   - 방법론적 엄밀성, 시간축 부재, 성과 분석 부재 등 제언
        ↓ 기존 LLM 분석 결과와 통합 분석
[2단계] feedback_addition.md (31KB)
   - feedback.md와 llm_analysis_1003.md의 비교 분석
   - 기존 분석의 정량적 발견사항 가치 재확인
   - 두 문서의 상보적 관계 명확화
        ↓ 전문 분석 결과 반영
[3단계] feedback_v2.md (22KB)
   - 21,011줄 전문 분석 완료 후 업데이트
   - 1,527개 전체 행 검증 완료
   - 분류 정확도 85% 확인
        ↓ 최종 통합
[4단계] final_feedback.md (21KB)
   - 모든 피드백의 통합 최종본
   - 최종 평가: ★★★★★ (5점 만점)
   - 7대 핵심 발견사항의 정책적 가치 확인
```

이러한 반복적 피드백 과정을 Git으로 관리함으로써, 각 피드백이 언제, 어떤 내용으로, 어떻게 반영되었는지를 투명하게 추적할 수 있었다.

### 9.3.2 평가 매트릭스 활용

전문가 평가는 사전에 정의된 **평가 매트릭스(Evaluation Matrix)**를 기반으로 수행하였다. 본 프로젝트에서 사용된 평가 항목과 결과는 다음과 같다:

| 평가 항목 | 정의 | 최종 평가 | 비고 |
|----------|------|----------|------|
| 분석의 체계성 (Systematic Analysis) | 분류 체계의 논리적 구조와 일관성 | ★★★★★ | 5대 테마, 40여 개 중분류 |
| 정량적 근거 (Quantitative Evidence) | 빈도 분석의 정확성과 통계적 적절성 | ★★★★★ | 빈도 100% 일치 확인 |
| 해석의 명료성 (Interpretive Clarity) | 발견사항의 간결성과 이해 용이성 | ★★★★★ | 7대 발견의 명료성 |
| 정책 지향성 (Policy Orientation) | 실행 가능한 정책 함의 도출 여부 | ★★★★★ | 즉시 활용 가능 제언 |
| 방법론 서술 (Methodology Description) | LLM 활용 과정의 투명한 서술 | ★★★☆☆ | **개선 필요** |

방법론 서술이 ★★★☆☆로 상대적으로 낮은 평가를 받은 점은 중요한 교훈이다 (9.4절에서 상세히 다룬다).

### 9.3.3 피드백 반영 프로세스

전문가 피드백을 체계적으로 반영하기 위해 다음 프로세스를 따랐다:

1. **피드백 수신 즉시 Git 커밋**: 원본 피드백을 그대로 보존
2. **항목별 분류**: 피드백 내용을 "즉시 반영", "추가 분석 필요", "향후 과제"로 분류
3. **반영 사항 문서화**: 각 피드백 항목에 대해 어떻게 대응했는지 기록
4. **재검토 요청**: 수정된 결과물을 전문가에게 다시 보내 확인

```
# 피드백 반영 후 Git 커밋 예시
git add deliverables/final_report_revised.md
git commit -m "전문가 피드백 반영: 기타 항목 재분류 및 시간축 추가

- feedback.md 지적사항 반영
- 기타 비율 13.1% → 목표 5% 이하로 재분류
- 단기/중기/장기 시간축 구분 추가
- 성과 사례 섹션 신설"
```

---

## 9.4 방법론 서술의 중요성

본 프로젝트에서 전문가 평가의 유일한 약점이 **방법론 서술(★★★☆☆)**이었다. `llm_analysis_evaluation.md`에서 지적된 구체적인 문제는 다음과 같다:

> "llm_analysis_1003.md에는 '어떻게 분석했는지'에 대한 설명이 부족합니다. 누가 분류했는지, 어떤 기준으로 분류했는지, 불일치는 어떻게 해결했는지, LLM 모델은 무엇을 사용했는지, 프롬프트는 어떻게 설계했는지가 누락되어 있습니다."

이 지적은 **재현 가능성(Reproducibility)**의 핵심 요건과 직결된다. 학위논문이나 학술논문에서 LLM 활용 방법론을 서술할 때 반드시 포함해야 하는 정보는 다음과 같다:

### 9.4.1 방법론 섹션 작성 템플릿

```markdown
# 연구 방법

## 1. 데이터 수집
- 인터뷰 대상: 15개 정부출연연구기관 국제협력 담당자
- 인터뷰 기간: 2025년 1월 - 9월
- 인터뷰 형식: 반구조화 심층 인터뷰 (Semi-structured Interview)
- 데이터 규모: 21,011줄, 약 450,000 단어

## 2. 분석 도구 및 환경
- 분석 도구: Claude Code (Anthropic Claude 모델 기반 CLI)
- 코드 관리: GitHub 리포지토리 (버전 관리)
- 코드북: codebook.md (8개 주제 코드 범주, 10개 맥락/뉘앙스 코드)

## 3. 코딩 과정
- 1차 코딩: LLM 기반 의미 단위 분절(Semantic Unit Decomposition) 
  및 위계적 주제 코딩(Hierarchical Thematic Coding)
- 코딩 단위: 의미 단위 발췌문 (Semantic Unit Excerpt)
- 분절 기준: 하나의 완결된 의미를 가진 발언 또는 발언 묶음
- 코딩 원칙: 각 발췌문에 주제 코드 1개 이상 + 맥락 코드 1개 부여

## 4. 프롬프트 설계
- 역할 부여: "질적 연구 방법론에 숙련된 정책 연구 분석가"
- 코드북 참조: codebook.md 전문을 프롬프트에 포함
- 출력 형식: CSV 형태 (기관명, 발화자, 원문, 주제코드, 맥락코드, 요약)
- 판단 근거 기록: 각 코딩 결정에 대한 간략한 근거 요구

## 5. 신뢰도 검증
- 검증 방법: Cohen's Kappa 계수
- 검증 표본: 200개 무작위 추출 (전체 1,527개의 13.1%)
- 표본 추출 방식: 테마별 비례 층화 추출
- 인간 코더: 연구자 1인 독립 블라인드 코딩
- 결과: κ = 0.85 (Almost Perfect Agreement)
- 불일치 해결: 합의 회의를 통한 재분류

## 6. 외부 검증
- 전문가 평가: 과학기술정책 전문가 검토
- 평가 방식: 5개 항목 5점 척도 매트릭스
- 전체 평가: ★★★★★ (5점 만점)
- 피드백 반영: 4단계 반복적 피드백 루프
```

### 9.4.2 학위논문에서의 서술 방법

학위논문(석사 또는 박사)에서 LLM 활용을 서술할 때는 다음 네 가지 원칙을 지켜야 한다:

**원칙 1: 투명성 (Transparency)**
사용한 LLM 모델명, 버전, 접근 방식(API vs CLI)을 명시한다. "AI를 활용하였다"와 같은 모호한 표현은 피한다.

**원칙 2: 재현 가능성 (Reproducibility)**
프롬프트의 핵심 구조, 코드북 전문, 검증 절차를 부록에 수록한다. 다른 연구자가 유사한 과정을 재현할 수 있어야 한다.

**원칙 3: 인간 연구자의 역할 명시 (Human-in-the-Loop)**
LLM은 1차 분류 도구이며, 최종 판단은 인간 연구자가 수행했음을 명확히 한다. 연구자가 코드북 설계, 검증, 해석, 이론적 프레이밍 등 핵심 학술 작업을 담당했음을 강조한다.

**원칙 4: 한계 인정 (Acknowledging Limitations)**
LLM 활용의 한계(재현성 문제, 편향 가능성 등)를 솔직히 기술하고, 이를 보완하기 위해 취한 조치를 설명한다.

---

## 9.5 한계점 및 주의사항

LLM 기반 질적 연구는 강력한 도구이지만, 연구자는 다음과 같은 한계를 인식하고 논문에 명시해야 한다.

### 9.5.1 LLM의 편향(Bias) 가능성

LLM은 학습 데이터에 내재된 편향을 반영할 수 있다:

- **문화적 편향**: 영어 중심 학습 데이터로 인해 한국어 텍스트의 미묘한 뉘앙스를 서구적 프레임으로 해석할 가능성
- **주제 편향**: 학습 데이터에서 빈번하게 등장하는 주제를 과대 대표(Over-represent)할 가능성
- **긍정성 편향(Positivity Bias)**: LLM이 부정적 평가보다 긍정적 해석을 선호하는 경향

**대응 방법**: 코드북에 명확한 분류 기준과 경계 사례를 포함하여 LLM의 자의적 판단을 최소화한다. 또한 인간 코더와의 교차 검증을 통해 체계적 편향을 탐지한다.

### 9.5.2 맥락 이해의 한계

인터뷰 텍스트만으로는 포착할 수 없는 정보가 존재한다:

- **비언어적 단서(Non-verbal Cues)**: 화자의 표정, 제스처, 눈 맞춤 등
- **화자의 톤(Tone)**: 같은 말이라도 진지하게 한 것인지, 반어적으로 한 것인지
- **침묵(Silence)**: 특정 질문에 대한 긴 침묵은 중요한 데이터일 수 있으나, 전사본(transcript)에 반영되지 않음
- **인터뷰 역학(Interview Dynamics)**: 연구자와 피면접자 간의 권력 관계, 라포(rapport) 형성 정도

**대응 방법**: 인터뷰 수행자가 현장 노트(field notes)를 작성하고, LLM 분석 결과를 검토할 때 이를 참조한다. 또한 방법론 섹션에서 이 한계를 명시한다.

### 9.5.3 재현성 문제 (Reproducibility)

LLM의 고유한 특성으로 인해 동일한 프롬프트에도 결과가 다를 수 있다:

- **확률적 생성(Stochastic Generation)**: LLM은 확률적 모델이므로, 동일한 입력에 대해 매번 동일한 출력을 보장하지 않는다.
- **모델 업데이트**: 시간이 지남에 따라 LLM 모델이 업데이트되면 과거와 동일한 결과를 재현하기 어렵다.
- **컨텍스트 윈도우 차이**: 데이터를 분할하여 처리할 때, 분할 방식에 따라 결과가 달라질 수 있다.

**대응 방법**: 
- Temperature 파라미터를 0으로 설정하여 결정론적 출력에 가깝게 유도
- 사용한 모델의 정확한 버전을 기록
- 코딩 결과를 CSV 등 정형 데이터로 저장하여 원본을 보존 (Git으로 버전 관리)
- 동일 데이터에 대해 2-3회 반복 코딩하여 안정성(Stability) 확인

### 9.5.4 윤리적 고려

질적 연구에서 LLM을 활용할 때의 윤리적 이슈는 다음과 같다:

- **인터뷰 대상자 동의(Informed Consent)**: 인터뷰 데이터가 LLM에 의해 처리됨을 사전에 고지하고 동의를 받아야 한다. 기존 동의서에 "AI 도구를 활용한 분석"이 포함되어 있지 않다면, 추가 동의를 받는 것이 바람직하다.
- **데이터 보안(Data Security)**: 인터뷰 데이터에는 개인 식별 정보나 기관 민감 정보가 포함될 수 있다. API 기반 LLM 사용 시 데이터가 외부 서버로 전송되므로, 익명화(anonymization) 처리가 필요하다. Claude Code의 경우 로컬 환경에서 파일을 읽어 처리하지만, API 호출 시 데이터가 전송되므로 이 점을 인지해야 한다.
- **IRB(Institutional Review Board) 승인**: LLM 활용이 기존 IRB 승인 범위에 포함되는지 확인하고, 필요 시 수정 승인을 받는다.
- **저작권 및 출처 표시**: LLM이 생성한 분석 결과물의 저작권 귀속 문제를 기관 규정에 따라 명확히 한다.

---

# 10. Tips & Best Practices (실전 팁과 모범 사례)

## 10.1 대용량 텍스트 처리 시 주의점

### 10.1.1 컨텍스트 윈도우 제한과 대응 전략

LLM에는 한 번에 처리할 수 있는 텍스트 양의 한계, 즉 **컨텍스트 윈도우(Context Window)**가 존재한다. Claude 모델의 경우 매우 넓은 컨텍스트 윈도우를 제공하지만, 21,011줄(약 450,000 단어)에 달하는 인터뷰 전사본을 한 번에 처리하는 것은 현실적으로 어렵다.

**대응 전략: 체계적 분할 처리(Systematic Chunking)**

```
[전체 데이터: 21,011줄]
        ↓ 기관별 분할
[기관 1: KAERI] [기관 2: ETRI] ... [기관 15: KFE]
        ↓ 각 기관별 코딩 수행
[코딩 결과 1] [코딩 결과 2] ... [코딩 결과 15]
        ↓ 결과 병합
[통합 CSV: Policy_merged.csv (1,527개 행)]
```

본 프로젝트에서는 인터뷰 데이터를 **기관별로 분할**하여 처리하였다. 이 방식의 장점은:

- 각 기관의 인터뷰가 자연스러운 맥락 단위를 형성
- 기관 간 교차 오염(cross-contamination) 방지
- 기관별 결과를 독립적으로 검증 가능
- 처리 중 오류 발생 시 해당 기관만 재처리 가능

### 10.1.2 토큰 관리

LLM의 처리 단위인 **토큰(Token)**은 입력 데이터의 규모와 비용에 직접적으로 관련된다. 한국어는 영어보다 토큰 효율이 낮은 편이다 (동일한 의미를 전달하는 데 더 많은 토큰이 필요할 수 있음).

**토큰 수 확인 방법**:

```
# Claude Code에서 토큰 수 추정 요청
"policy_250924.md 파일의 대략적인 토큰 수를 추정해 주세요.
한국어 텍스트이므로, 한글 1글자당 약 1.5-2토큰으로 
추정해 주세요."
```

**토큰 절약 팁**:
- 코드북을 프롬프트에 직접 포함하는 대신, 별도 파일로 참조하게 하면 반복적인 토큰 소비를 줄일 수 있다.
- 불필요한 공백, 반복 문구, 형식적 인사말 등을 전처리 단계에서 제거한다.
- 출력 형식을 간결하게 지정한다 (예: 긴 설명 대신 코드와 짧은 근거만 출력).

### 10.1.3 청크 크기 최적화

데이터를 분할할 때 **청크(Chunk) 크기**를 적절하게 설정하는 것이 중요하다:

| 청크 크기 | 장점 | 단점 |
|----------|------|------|
| 너무 작음 (100줄 이하) | 정밀한 코딩 가능 | 맥락 손실, 발언의 앞뒤 문맥 누락 |
| 적정 (500-2,000줄) | 맥락 유지와 정확도의 균형 | - |
| 너무 큼 (5,000줄 이상) | 넓은 맥락 파악 | 정확도 저하, 후반부 주의력 감소 |

본 프로젝트에서는 **기관당 평균 약 1,400줄** 단위로 처리하였으며, 이는 적정 범위에 해당한다. 인터뷰 데이터의 경우, 한 명의 피면접자 인터뷰 전체를 하나의 청크로 하는 것이 자연스럽다.

---

## 10.2 프롬프트 최적화 전략

### 10.2.1 명확한 지시문 작성

효과적인 프롬프트는 세 가지 요소를 포함한다: **역할 부여(Role Assignment)**, **출력 형식 지정(Output Format)**, **예시 제공(Few-shot Examples)**.

**역할 부여**:
```
"당신은 질적 연구 방법론에 숙련된 정책 연구 분석가입니다. 
한국 과학기술정책에 대한 깊은 이해를 바탕으로, 
인터뷰 전사본을 체계적으로 분석해 주세요."
```

**출력 형식 지정**:
```
"결과를 다음 CSV 형식으로 출력해 주세요:

기관명,발화자,원문_발췌,주제코드,맥락코드,요약(1-2문장)

예시:
KAERI,인터뷰이A,"우리가 SMR을 수출하려면 미국의 blessing이 필요합니다",OBS_GEO_RESTRICTIONS_CONTROLS,PROBLEM_STATED,"SMR 수출에 미국 승인 필요성 언급"
```

**예시 제공 (Few-shot)**:
```
"다음은 올바른 코딩 예시입니다:

[예시 1]
원문: "단기간에 우리가 어떤 글로벌 수준에 도달하기 위해서..."
코드: CD_TECH_ACCESS (선진/보완 기술 접근)
맥락: OPINION_EXPRESSED
근거: 기술 격차 해소를 위한 국제협력 동기를 표현

[예시 2]  
원문: "부처가 바뀌면 사업이 다 바뀌어요"
코드: OBS_POL_INCONSISTENCY_SHORT_TERMISM (정책 비일관성)
맥락: PROBLEM_STATED
근거: 정권/부처 변화에 따른 정책 불안정성 지적"
```

### 10.2.2 코드북을 프롬프트에 포함 vs 별도 파일 참조

| 방식 | 장점 | 단점 | 권장 상황 |
|------|------|------|----------|
| 프롬프트에 직접 포함 | 코드북을 정확히 참조 | 토큰 소비 증가 | 코드 수가 적을 때 (20개 이하) |
| 별도 파일 참조 | 토큰 절약 | 참조 정확도 다소 저하 가능 | 코드 수가 많을 때 |
| 하이브리드: 핵심 기준만 포함, 전문은 파일 참조 | 균형 잡힌 접근 | 설계 복잡 | **대부분의 상황에서 권장** |

본 프로젝트에서는 `codebook.md`(약 11KB, 8개 주제 코드 범주 + 50여 개 세부 코드 + 10개 맥락 코드)를 하이브리드 방식으로 활용하였다. 프롬프트에는 대분류 수준의 코드 목록과 핵심 판단 기준만 포함하고, 세부 포함/제외 기준과 예시는 `codebook.md` 파일을 직접 참조하게 하였다.

### 10.2.3 Chain-of-Thought 유도

LLM의 코딩 정확도를 높이기 위해 **단계적 추론(Chain-of-Thought)**을 유도하는 것이 효과적이다:

```
"각 발췌문을 코딩할 때 다음 단계를 따르세요:

1단계 - 맥락 파악: 해당 발언의 앞뒤 문맥을 확인하고, 
        누가, 어떤 상황에서, 무엇에 대해 말하고 있는지 파악합니다.

2단계 - 핵심 의미 추출: 발언의 핵심 메시지가 무엇인지 
        1-2문장으로 요약합니다.

3단계 - 코드 후보 선정: 코드북을 참조하여 해당 발언에 
        적용 가능한 코드 2-3개를 후보로 선정합니다.

4단계 - 최적 코드 결정: 후보 중 가장 적합한 코드를 선택하고, 
        선택 근거를 간략히 기술합니다.

5단계 - 맥락 코드 부여: 발언의 성격(문제 제기, 원인 분석, 
        해결책 제안 등)에 맞는 맥락 코드를 부여합니다."
```

이러한 단계적 접근은 LLM이 즉각적 판단(snap judgment)이 아닌 숙고된 판단(deliberate judgment)을 내리도록 유도하며, 코딩 정확도를 향상시킨다.

---

## 10.3 한국어 데이터 처리 팁

### 10.3.1 한국어 인터뷰의 구어체 특성

한국어 인터뷰 전사본에는 영어와 다른 고유한 특성이 있다:

- **비완결 문장**: "그래서 그게... 좀... 뭐라 그러까..." 같은 발언은 하나의 코딩 단위로 다루기 어렵다. 앞뒤 발언과 결합하여 완결된 의미를 구성해야 한다.
- **존칭 및 완곡 표현**: "좀 어려운 점이 있는 것 같습니다"는 실제로는 "매우 심각한 문제"를 의미할 수 있다. LLM에게 한국어 완곡 표현의 실제 강도를 고려하도록 지시한다.
- **반복 및 추임새**: "네, 네, 그렇죠, 그렇죠"와 같은 추임새는 코딩 대상에서 제외해야 한다.
- **두괄식 vs 미괄식**: 한국어 인터뷰에서는 핵심 메시지가 발언 후반부에 등장하는 경우가 많다.

**프롬프트 보완 예시**:
```
"한국어 인터뷰 전사본을 분석할 때 다음 사항에 유의하세요:

1. 비완결 문장은 앞뒤 맥락과 결합하여 완결된 의미 단위로 
   구성한 후 코딩하세요.
2. '좀 어려운', '그런 부분이 있다'와 같은 완곡 표현은 
   실제 의미를 파악하여 코딩하세요.
3. 추임새, 단순 동의 표현('네, 그렇죠')은 
   독립적 코딩 대상에서 제외하세요.
4. 핵심 메시지가 발언 후반부에 나오는 경우가 많으므로, 
   전체 발언을 끝까지 읽은 후 코딩하세요."
```

### 10.3.2 기관 고유 약어 및 전문 용어 사전

한국 출연연 관련 인터뷰에는 다수의 약어와 전문 용어가 등장한다. LLM이 이를 정확히 이해하도록 **용어 사전(Glossary)**을 프롬프트에 포함하거나 별도 파일로 제공한다:

```markdown
# 용어 사전 (프롬프트 포함용)

# 기관 약어
- KAERI: 한국원자력연구원 (Korea Atomic Energy Research Institute)
- ETRI: 한국전자통신연구원 (Electronics and Telecommunications Research Institute)
- KIGAM: 한국지질자원연구원 (Korea Institute of Geoscience and Mineral Resources)
- NST: 국가과학기술연구회 (National Research Council of Science and Technology)
- KFE: 한국핵융합에너지연구원 (Korea Institute of Fusion Energy)

# 정책/제도 용어
- 출연연: 정부출연연구기관 (Government-funded Research Institute, GRI)
- 과기부/과기정통부: 과학기술정보통신부
- PBS: 과제기반시스템 (Project-Based System)
- TRL: 기술성숙도 (Technology Readiness Level)
- CET: 핵심신흥기술 (Critical and Emerging Technology)

# 국제협력 관련
- MOU: 양해각서 (Memorandum of Understanding)
- ODA: 공적개발원조 (Official Development Assistance)
- KOICA: 한국국제협력단
- IAEA: 국제원자력기구
- SMR: 소형모듈원자로 (Small Modular Reactor)
```

### 10.3.3 한영 혼용 텍스트 처리

한국어 인터뷰에서는 영어 전문 용어가 빈번하게 혼용된다. 예를 들어:

> "우리가 IPR 문제에서 winning point를 찾으려면 top-down으로 예산을 때리는 게 아니라 bottom-up으로 접근해야 합니다."

이러한 한영 혼용 텍스트를 처리할 때의 팁:

- LLM에게 한영 혼용을 자연스러운 것으로 인지하고, 코드 부여 시 영어 용어의 한국어 맥락을 고려하도록 지시한다.
- 영어 용어가 한국어 맥락에서 다른 의미로 사용되는 경우(예: "blessing" = 정치적 승인)를 용어 사전에 명시한다.

---

## 10.4 비용 관리

### 10.4.1 API 사용량 추적 및 예산 관리

Claude Code는 API 호출 기반으로 작동하므로, 대규모 데이터를 처리할 때 비용이 발생한다. 비용을 효율적으로 관리하기 위한 전략은 다음과 같다:

- **월간 예산 한도 설정**: 프로젝트 시작 전 월간 API 예산을 설정하고, 사용량을 주기적으로 확인한다.
- **사용량 모니터링**: Claude Code의 `/cost` 명령어를 활용하여 세션별 사용량을 추적한다.
- **단계적 처리**: 전체 데이터를 한 번에 처리하지 않고, 소규모 파일럿부터 시작하여 프롬프트를 최적화한 후 대규모 처리에 돌입한다.

### 10.4.2 효율적인 작업 순서

비용 효율성을 극대화하기 위한 권장 작업 순서:

```
[1단계] 소규모 파일럿 (1개 기관, 비용: 전체의 ~5%)
   → 프롬프트 설계 검증
   → 코드북 적합성 확인
   → 출력 형식 최적화
        ↓
[2단계] 중규모 검증 (3개 기관, 비용: 전체의 ~15%)
   → 기관 간 일관성 확인
   → 코드북 보완
   → 처리 시간 추정
        ↓
[3단계] 대규모 처리 (전체 15개 기관, 비용: 전체의 ~80%)
   → 최적화된 프롬프트로 전체 처리
   → 결과 병합 및 1차 품질 확인
        ↓
[4단계] 검증 및 보완 (비용: 전체의 ~10%)
   → Cohen's Kappa 검증
   → 불일치 항목 재처리
   → 최종 데이터셋 확정
```

이 순서를 따르면, 1-2단계에서 프롬프트와 코드북을 충분히 검증한 후 3단계에서 대규모 처리를 수행하므로, 재작업으로 인한 비용 낭비를 최소화할 수 있다.

---

## 10.5 흔한 실수와 해결 방법

### 실수 1: 코드북 없이 코딩 시작하기

**문제**: 코드북을 사전에 정의하지 않고 LLM에게 "알아서 분류해 달라"고 요청하면, 분류 기준이 일관되지 않고, 코드의 입도(granularity)가 불균일해진다.

**해결**: 반드시 사전에 코드북을 작성한다. 본 프로젝트의 `codebook.md`처럼 각 코드에 대해 **정의**, **포함 기준**, **제외 기준**, **예시**를 명시한다.

```
[잘못된 예]
"이 인터뷰를 분석해서 적절한 카테고리로 분류해 주세요."

[올바른 예]  
"codebook.md의 분류 체계에 따라 각 발언을 코딩해 주세요.
판단이 어려운 경우 코드북의 포함/제외 기준을 참조하세요."
```

### 실수 2: 검증 없이 결과 수용하기

**문제**: LLM의 코딩 결과를 무비판적으로 수용하면, 체계적 오류가 전체 분석에 전파될 수 있다. 특히 "기타(Miscellaneous)" 범주가 과도하게 많아지는 현상(본 프로젝트 초기 13.1%)은 분류 체계의 불완전성을 나타내는 신호이다.

**해결**: 반드시 교차 검증(Cohen's Kappa)을 수행하고, 기타 항목의 비율이 5%를 초과하면 코드북을 보완한다.

### 실수 3: Git 미사용

**문제**: 버전 관리 없이 작업하면, 코드북 수정 이력, 프롬프트 변경 이력, 피드백 반영 이력을 추적할 수 없다. 학위논문 심사 과정에서 "이 분류 기준은 언제 바뀌었나요?"라는 질문에 답할 수 없게 된다.

**해결**: 프로젝트 시작과 동시에 Git 리포지토리를 초기화하고, 모든 변경 사항을 의미 있는 커밋 메시지와 함께 기록한다.

```bash
# 주요 마일스톤마다 태그 부여
git tag -a v1.0-codebook -m "초기 코드북 완성"
git tag -a v2.0-pilot -m "파일럿 코딩 완료 및 코드북 1차 수정"
git tag -a v3.0-full-coding -m "전체 코딩 완료"
git tag -a v4.0-validated -m "Cohen's Kappa 검증 완료 (κ=0.85)"
git tag -a v5.0-final -m "전문가 피드백 반영 최종본"
```

### 실수 4: 한 번에 모든 데이터 처리 시도

**문제**: 전체 21,011줄을 한 번에 처리하려고 시도하면, 컨텍스트 윈도우 초과, 처리 중 오류, 결과의 품질 저하 등의 문제가 발생한다. 또한 중간에 오류를 발견하더라도 전체를 재처리해야 하는 비효율이 생긴다.

**해결**: 10.4.2에서 설명한 단계적 처리 순서(소규모 파일럿 → 중규모 검증 → 대규모 처리 → 검증 보완)를 따른다. 각 단계의 결과를 Git에 커밋하여, 문제 발생 시 해당 단계만 재처리할 수 있도록 한다.

### 실수 5: 코드북을 고정불변으로 취급

**문제**: 초기 코드북을 수정하지 않고 끝까지 사용하면, 데이터에서 새롭게 발견되는 패턴이나 기존 코드의 모호성을 해결하지 못한다.

**해결**: 코드북은 **살아있는 문서(Living Document)**로 취급한다. 파일럿 코딩 후 1차 수정, 전체 코딩 중 2차 수정, 검증 후 3차 수정 등 반복적으로 개선한다. 단, 코드북이 변경될 때마다 이전에 코딩된 항목도 새로운 기준으로 재검토해야 한다. Git을 통해 코드북의 변경 이력을 추적한다.

---

# 11. 부록 (Appendix)

## 부록 A: 리포지토리 전체 파일 목록 및 설명

| 경로 | 파일명 | 크기 | 설명 |
|------|--------|------|------|
| `/` | `codebook.md` | 11KB | 코드북 정의서. 8개 주제 코드 범주(50여 개 세부 코드)와 10개 맥락/뉘앙스 코드 정의. 각 코드별 포함/제외 기준 및 예시 포함 |
| **`raw/`** | | | **원본 데이터 및 초기 분석** |
| `raw/` | `policy_250924.md` | 968KB | 인터뷰 전사본 원본 (2025년 9월 24일 기준). 15개 기관 인터뷰의 최초 전사 버전 |
| `raw/` | `정책연구1-1 인터뷰 기록_250924.md` | 968KB | 한국어 인터뷰 기록 원본. policy_250924.md와 동일 내용의 한국어 파일명 버전 |
| `raw/` | `policy_analysis_251024.md` | 1.4MB | 1차 분석 보고서 (2025년 10월 24일). LLM 기반 구조화 분석 결과 |
| `raw/` | `policy_1027_revised.md` | 1.4MB | 수정된 분석 (2025년 10월 27일). 21,011줄 전문을 포함하는 수정 버전 |
| `raw/` | `Policy_merged.csv` | 482KB | 구조화된 발췌문 데이터. 1,527개 행의 코딩 결과 (기관명, 원문, 코드, 맥락 등) |
| `raw/` | `llm_analysis_1003.md` | 11KB | 초기 LLM 분석 결과. 5대 테마, 7대 핵심 발견사항, 기관별 빈도 분석 포함 |
| **`qualitative/`** | | | **질적 분석 결과물** |
| `qualitative/` | `comprehensive_policy_analysis.md` | 35KB | 종합 정책 분석 보고서 (영문). 5대 테마별 심층 분석 및 정책 함의 |
| `qualitative/` | `comprehensive_policy_analysis_kor.md` | 34KB | 종합 정책 분석 한국어 번역본 |
| `qualitative/` | `policy_1027_all_coding.md` | 59KB | 전체 코딩 결과 문서. 1,527개 발췌문의 상세 코딩 내역 |
| `qualitative/` | `policy_recommendations.md` | 23KB | 정책 제언 보고서. 현장 목소리 기반 실행 가능한 정책 대안 |
| **`deliverables/`** | | | **최종 산출물 및 검증 문서** |
| `deliverables/` | `final_report.md` | 142KB | 최종 보고서 (초판). 연구 전체를 포괄하는 종합 보고서 |
| `deliverables/` | `final_report_revised.md` | 102KB | 최종 보고서 수정본. 전문가 피드백 반영 후 개정 |
| `deliverables/` | `final_report_revised_part1.md` | 24KB | 수정 보고서 Part 1. 연구 배경, 방법론, 분석 프레임워크 |
| `deliverables/` | `final_report_revised_part2.md` | 79KB | 수정 보고서 Part 2. 분석 결과, 정책 제언, 결론 |
| `deliverables/` | `policy_memo.md` | 14KB | 정책 메모. A4 기준 요약본, 정책 담당자용 |
| `deliverables/` | `policy_memo_update.md` | 1.9KB | 정책 메모 보완 사항 |
| `deliverables/` | `feedback.md` | 28KB | 1차 전문가 피드백. 분류 체계 강점 및 보완점 |
| `deliverables/` | `feedback_v2.md` | 22KB | 2차 전문가 피드백 (개정판). 전문 분석 결과 반영 |
| `deliverables/` | `feedback_addition.md` | 31KB | 피드백 보완 의견서. LLM 분석 결과와 피드백의 통합 평가 |
| `deliverables/` | `final_feedback.md` | 21KB | 최종 전문가 평가. 모든 피드백 통합본, 최종 ★★★★★ 평가 |
| `deliverables/` | `llm_analysis_evaluation.md` | 25KB | LLM 분석 방법론 평가. 5개 항목별 점수 및 개선 방향 |
| `deliverables/` | `viz_policy_roadmap.html` | 11KB | 정책 로드맵 Gantt 차트 시각화 |
| `deliverables/` | `viz_causal_network.html` | 9KB | 인과 네트워크 시각화. 테마 간 관계 다이어그램 |
| `deliverables/` | `viz_institutional_heatmap.html` | 6KB | 기관별 히트맵 시각화. 기관-테마 교차 분석 |

---

## 부록 B: 유용한 Claude Code 명령어 모음 (질적 연구 특화)

### B.1 프로젝트 초기 설정

```bash
# 리포지토리 초기화 및 구조 생성
"현재 디렉토리를 Git 리포지토리로 초기화하고, 
다음 디렉토리 구조를 생성해 주세요:
raw/ (원본 데이터), qualitative/ (분석 결과), 
deliverables/ (산출물)"
```

### B.2 코드북 관련

```bash
# 코드북 초안 생성
"인터뷰 전사본 policy_250924.md를 읽고, 
질적 연구의 주제 분석(Thematic Analysis) 방법론에 따라 
초기 코드북을 생성해 주세요. 
위계적 구조(대분류-중분류-소분류)로 구성하고, 
각 코드에 정의, 포함 기준, 제외 기준, 예시를 포함해 주세요."

# 코드북 검토 및 보완
"codebook.md를 검토하고, 상호배타성(Mutual Exclusivity)과 
포괄성(Exhaustiveness) 관점에서 보완이 필요한 부분을 
지적해 주세요."
```

### B.3 코딩 수행

```bash
# 기관별 코딩 수행
"policy_250924.md에서 KAERI(한국원자력연구원) 부분을 추출하고, 
codebook.md에 따라 의미 단위로 분절하여 코딩해 주세요.
결과는 CSV 형식으로 출력해 주세요."

# 기존 코딩 결과 검토
"Policy_merged.csv에서 코드 'OBS_POLICY_GOVERNANCE'로 
분류된 모든 항목을 추출하고, 코딩의 적절성을 검토해 주세요.
오분류 가능성이 있는 항목이 있으면 지적해 주세요."
```

### B.4 빈도 분석

```bash
# 전체 빈도 분석
"Policy_merged.csv를 분석하여 다음을 산출해 주세요:
1. 대분류 테마별 빈도 및 비율
2. 중분류별 빈도 및 비율 (상위 20개)
3. 기관별 빈도 분포
4. 기관-테마 교차 분석표"

# 기관별 특성 분석
"Policy_merged.csv에서 각 기관별로 가장 많이 언급된 
상위 3개 테마를 추출하고, 
기관 간 유사성/차별성을 분석해 주세요."
```

### B.5 검증 관련

```bash
# 무작위 표본 추출
"Policy_merged.csv에서 200개 항목을 무작위 추출해 주세요.
층화 추출(Stratified Sampling)을 적용하여 
대분류 테마별 비율이 전체 분포와 유사하도록 해 주세요."

# Cohen's Kappa 계산
"validation_sample.csv의 'llm_code' 열과 'human_code' 열을 비교하여
Cohen's Kappa를 계산해 주세요. 
혼동 행렬(Confusion Matrix)도 함께 산출해 주세요."

# 불일치 패턴 분석
"검증 결과에서 불일치 항목만 추출하고, 
어떤 코드 쌍에서 혼동이 자주 발생하는지 패턴을 분석해 주세요."
```

### B.6 보고서 작성

```bash
# 종합 분석 보고서 생성
"Policy_merged.csv의 전체 분석 결과를 바탕으로, 
다음 구조의 종합 보고서를 작성해 주세요:
1. 분석 개요 (데이터 규모, 방법론)
2. 주요 발견사항 (정량적 근거 포함)
3. 기관별 특성 분석
4. 정책 함의 및 제언"

# 시각화 생성
"Policy_merged.csv를 기반으로 기관별-테마별 히트맵을 
HTML 파일로 생성해 주세요."
```

### B.7 Git 관리

```bash
# 현재 상태 확인 및 커밋
"현재 변경된 파일을 확인하고, 
적절한 커밋 메시지와 함께 커밋해 주세요."

# 작업 이력 확인
"최근 10개의 커밋 이력을 보여 주세요."
```

---

## 부록 C: 참고 문헌 및 추가 자료

### C.1 Claude Code 관련

- Anthropic. (2025). *Claude Code Documentation*. https://docs.anthropic.com/en/docs/claude-code
- Anthropic. (2025). *Claude Model Card and Evaluations*. https://www.anthropic.com

### C.2 질적 연구 방법론

- Braun, V., & Clarke, V. (2006). Using thematic analysis in psychology. *Qualitative Research in Psychology*, 3(2), 77-101.
  - 주제 분석(Thematic Analysis)의 6단계 절차를 정립한 기초 문헌. 코드북 기반 분석의 이론적 토대.
- Saldana, J. (2021). *The Coding Manual for Qualitative Researchers* (4th ed.). SAGE Publications.
  - 질적 연구 코딩의 포괄적 안내서. 32가지 코딩 방법 소개. 코드북 설계 시 필수 참고.
- Miles, M. B., Huberman, A. M., & Saldana, J. (2020). *Qualitative Data Analysis: A Methods Sourcebook* (4th ed.). SAGE Publications.
  - 질적 데이터의 축약, 디스플레이, 결론 도출에 대한 체계적 안내.
- Creswell, J. W., & Poth, C. N. (2018). *Qualitative Inquiry and Research Design: Choosing Among Five Approaches* (4th ed.). SAGE Publications.
  - 질적 연구의 5대 접근법(내러티브, 현상학, 근거이론, 문화기술지, 사례연구) 비교.
- 김영천. (2016). *질적연구방법론 I: Bricoleur* (3판). 아카데미프레스.
  - 한국어로 된 질적 연구 방법론의 대표적 교재. 한국 연구 맥락에 맞는 사례 포함.

### C.3 LLM 활용 연구

- Tai, R. H., Bentley, L. J., Xia, X., Sitt, J. M., Fankhauser, S. C., Chicas, A. M., & Turkheimer, E. (2024). An examination of the use of large language models to aid analysis of textual data. *International Journal of Qualitative Methods*, 23.
  - LLM을 질적 연구의 코딩 도구로 활용한 선구적 연구. 인간 코더와의 일치도 비교 결과 포함.
- Xiao, Z., Yuan, X., Liao, Q. V., Abdelghani, R., & Oudeyer, P. Y. (2023). Supporting qualitative analysis with large language models: Combining coded segments with LLM annotations. *arXiv preprint arXiv:2306.14924*.
  - LLM 주석(annotation)과 인간 코딩의 결합 방법론 제안.
- Gao, J., Guo, Y., Lim, G., Zhang, T., Zhang, Z., Li, T. J. J., & Perrault, S. T. (2024). CollabCoder: A lower-barrier, rigorous approach to inductive qualitative analysis with multi-LLM collaboration. *Proceedings of the CHI Conference on Human Factors in Computing Systems*.
  - 다중 LLM 협업을 통한 귀납적 질적 분석 프레임워크.
- Ashwin, T. S., Guddala, R., & Ganapathy, S. (2023). Using ChatGPT for qualitative coding: A critical examination of its reliability for coding qualitative data. *British Journal of Educational Technology*.
  - ChatGPT의 질적 코딩 신뢰도에 대한 비판적 검토.

### C.4 Cohen's Kappa 및 코더 간 일치도

- Cohen, J. (1960). A coefficient of agreement for nominal scales. *Educational and Psychological Measurement*, 20(1), 37-46.
  - Cohen's Kappa의 원 논문. 우연의 일치를 보정한 일치도 계수의 수학적 정의.
- Landis, J. R., & Koch, G. G. (1977). The measurement of observer agreement for categorical data. *Biometrics*, 33(1), 159-174.
  - Kappa 해석 기준(Slight, Fair, Moderate, Substantial, Almost Perfect)을 제시한 논문.
- McHugh, M. L. (2012). Interrater reliability: The kappa statistic. *Biochemia Medica*, 22(3), 276-282.
  - Kappa 통계의 실용적 안내서. 계산 방법과 해석을 쉽게 설명.
- Krippendorff, K. (2018). *Content Analysis: An Introduction to Its Methodology* (4th ed.). SAGE Publications.
  - 내용 분석의 포괄적 방법론. Krippendorff's Alpha (Cohen's Kappa의 대안 지표) 설명 포함.

### C.5 과학기술정책 및 국제협력

- OECD. (2024). *Science, Technology and Innovation Outlook 2024*. OECD Publishing.
  - 글로벌 과학기술 혁신 동향 및 국제협력 현황.
- 과학기술정보통신부. (2024). *2024년도 과학기술 국제화 촉진 시행계획*.
  - 한국 정부의 과학기술 국제협력 정책 기본 계획.
- 국가과학기술연구회(NST). (2024). *출연연구기관 연구역량 강화 방안*.
  - 출연연 체계 및 운영에 대한 공식 자료.

---

## 부록 D: 용어 사전 (Glossary)

| 용어 (한국어) | 영문 | 정의 |
|-------------|------|------|
| 의미 단위 분절 | Semantic Unit Decomposition | 인터뷰 전사본을 하나의 완결된 의미를 가진 최소 단위로 분절하는 과정. 본 프로젝트에서 21,011줄을 1,527개 단위로 분절 |
| 위계적 주제 코딩 | Hierarchical Thematic Coding | 사전에 정의된 코드북의 위계적 구조(대분류-중분류-소분류)에 따라 각 의미 단위에 코드를 부여하는 과정 |
| 코드북 | Codebook | 코딩에 사용되는 코드의 목록, 정의, 적용 기준, 예시를 체계적으로 정리한 문서. 코딩의 일관성과 재현성을 보장하는 핵심 도구 |
| 코더 간 일치도 | Inter-coder Reliability (ICR) | 두 명 이상의 코더가 동일한 데이터를 독립적으로 코딩했을 때의 일치 정도. Cohen's Kappa 또는 Krippendorff's Alpha로 측정 |
| 코헨의 카파 | Cohen's Kappa ($\kappa$) | 두 코더 간 일치도를 측정하는 통계 지표. 우연의 일치를 보정하여 실질적 일치도만을 반영 |
| 컨텍스트 윈도우 | Context Window | LLM이 한 번에 처리할 수 있는 텍스트의 최대 길이 (토큰 수 기준) |
| 토큰 | Token | LLM의 텍스트 처리 기본 단위. 한국어의 경우 1글자가 약 1.5-2토큰에 해당 |
| 청크 | Chunk | 대용량 데이터를 처리하기 위해 분할한 데이터 조각. 청크 크기는 정확도와 맥락 유지의 균형점에서 결정 |
| 주제 분석 | Thematic Analysis | 질적 데이터에서 반복되는 패턴(주제, Theme)을 식별하고, 이를 체계적으로 분류 및 해석하는 연구 방법 |
| 반구조화 인터뷰 | Semi-structured Interview | 사전에 준비된 질문 목록을 기반으로 하되, 인터뷰 진행 중 유연하게 추가 질문을 하는 인터뷰 형식 |
| 삼각검증 | Triangulation | 연구의 타당성을 높이기 위해 다양한 데이터 출처, 방법론, 연구자 관점을 교차 검증하는 전략 |
| 포화 | Saturation | 추가적인 데이터 수집이 더 이상 새로운 주제나 코드를 생성하지 않는 상태. 질적 연구에서 표본 크기의 적절성을 판단하는 기준 |
| 층화 추출 | Stratified Sampling | 모집단을 하위 집단(층)으로 나누고, 각 층에서 비례적으로 표본을 추출하는 방법. 표본의 대표성을 보장 |
| 혼동 행렬 | Confusion Matrix | 분류 결과의 정확도를 평가하기 위한 표. 실제 분류와 예측 분류의 교차표로 구성 |
| 프롬프트 엔지니어링 | Prompt Engineering | LLM에게 최적의 결과를 도출하기 위해 입력 지시문(프롬프트)을 설계하고 최적화하는 과정 |
| 단계적 추론 | Chain-of-Thought (CoT) | LLM이 최종 답변에 도달하기 전에 중간 추론 과정을 단계별로 명시하도록 유도하는 프롬프트 기법 |
| 출연연 / 출연연구기관 | Government-funded Research Institute (GRI) | 정부가 출연금을 지원하여 운영하는 공공 연구기관. 한국에는 25개 기관이 국가과학기술연구회(NST) 산하에 있음 |
| 양적 규모의 질적 연구 | Qualitative Research at Scale | 대규모 질적 데이터(수만 줄 이상)를 LLM 등 기술을 활용하여 체계적으로 분석하는 연구 접근법. 본 프로젝트의 핵심 방법론 |
| 피드백 루프 | Feedback Loop | 전문가 평가 결과를 분석에 반영하고, 재평가를 받는 반복적 과정. 분석의 품질을 점진적으로 향상 |
| 재현 가능성 | Reproducibility | 다른 연구자가 동일한 방법과 데이터를 사용하여 유사한 결과를 얻을 수 있는 정도. 학술 연구의 핵심 요건 |
| 블라인드 코딩 | Blind Coding | 검증 과정에서 인간 코더가 LLM의 코딩 결과를 사전에 열람하지 않은 상태로 독립적으로 코딩하는 것 |
| Git | Git | 분산형 버전 관리 시스템. 파일의 변경 이력을 추적하고, 여러 버전을 관리할 수 있게 해 주는 도구 |
| GitHub | GitHub | Git 리포지토리를 호스팅하는 웹 기반 플랫폼. 협업, 코드 리뷰, 이슈 관리 등의 기능을 제공 |
| CLI | Command Line Interface | 텍스트 기반 명령어로 컴퓨터와 상호작용하는 인터페이스. Claude Code는 CLI 환경에서 동작 |
| API | Application Programming Interface | 소프트웨어 간 상호작용을 위한 인터페이스. Claude Code는 Anthropic API를 통해 LLM과 통신 |
| CSV | Comma-Separated Values | 쉼표로 구분된 값으로 구성된 텍스트 파일 형식. 코딩 결과를 정형 데이터로 저장하는 데 사용 |
| 공적개발원조 | Official Development Assistance (ODA) | 선진국이 개발도상국의 경제 발전과 복지 향상을 위해 제공하는 원조. 출연연의 주요 국제협력 유형 중 하나 |
| 범부처 컨트롤타워 | Cross-ministerial Control Tower | 여러 정부 부처에 걸친 정책을 통합 조정하는 중앙 기구. 본 연구에서 14개 기관, 46회 언급된 핵심 정책 제언 |
| 소형모듈원자로 | Small Modular Reactor (SMR) | 기존 대형 원전보다 소규모로 제작되는 차세대 원자로. KAERI의 국제협력 핵심 사안 |
| 코헨의 카파 해석 기준 | Landis & Koch Interpretation | κ < 0: Poor, 0.00-0.20: Slight, 0.21-0.40: Fair, 0.41-0.60: Moderate, 0.61-0.80: Substantial, 0.81-1.00: Almost Perfect |

---

## 부록 E: 프로젝트 데이터 흐름도

```
[원본 데이터]
정책연구1-1 인터뷰 기록_250924.md (968KB, 21,011줄)
                    │
                    ▼
[1단계: 데이터 정제 및 구조화]
Claude Code + codebook.md ──→ policy_analysis_251024.md (1.4MB)
                    │
                    ▼
[2단계: 의미 단위 분절 및 코딩]
LLM 기반 Semantic Unit Decomposition
+ Hierarchical Thematic Coding
                    │
                    ▼
[3단계: 구조화 데이터 생성]
Policy_merged.csv (482KB, 1,527 행)
                    │
            ┌───────┼───────┐
            ▼       ▼       ▼
[4단계: 다면 분석]
빈도 분석    기관별 분석    교차 분석
            │       │       │
            └───────┼───────┘
                    ▼
[5단계: 핵심 발견 도출]
llm_analysis_1003.md (7대 발견사항)
                    │
                    ▼
[6단계: 품질 검증]
Cohen's Kappa = 0.85 (200개 표본)
전문가 피드백 4단계 루프
                    │
                    ▼
[7단계: 최종 산출물]
├── final_report_revised.md (최종 보고서)
├── policy_memo.md (정책 메모)
├── viz_*.html (시각화 3종)
└── final_feedback.md (최종 평가: ★★★★★)
```

---

## 부록 F: 연구 윤리 체크리스트

질적 연구에서 LLM을 활용할 때 확인해야 할 윤리적 사항:

| 항목 | 확인 내용 | 상태 |
|------|----------|------|
| IRB 승인 | 연구 계획이 IRB 승인을 받았는가? LLM 활용이 승인 범위에 포함되는가? | [ ] 완료 |
| 사전 동의 | 인터뷰 대상자에게 AI 도구 활용 사실을 고지하고 동의를 받았는가? | [ ] 완료 |
| 익명화 | 전사본에서 개인 식별 정보(이름, 직책 등)를 제거 또는 가명 처리했는가? | [ ] 완료 |
| 데이터 보안 | 인터뷰 데이터가 적절한 보안 수준으로 저장 및 전송되는가? | [ ] 완료 |
| API 데이터 정책 | 사용하는 LLM 서비스의 데이터 처리 정책(저장, 학습 활용 여부)을 확인했는가? | [ ] 완료 |
| 기관 민감 정보 | 기관별 민감 정보(미공개 정책, 내부 갈등 등)가 적절히 처리되었는가? | [ ] 완료 |
| 저작권 | LLM 생성 결과물의 저작권 귀속이 명확한가? | [ ] 완료 |
| 투명성 | 논문/보고서에 LLM 활용 사실과 방법을 투명하게 서술했는가? | [ ] 완료 |
| 편향 검토 | LLM의 체계적 편향 가능성을 검토하고 대응 방안을 마련했는가? | [ ] 완료 |
| 인간 감독 | 최종 분석 결과에 대한 인간 연구자의 검토 및 판단이 이루어졌는가? | [ ] 완료 |
