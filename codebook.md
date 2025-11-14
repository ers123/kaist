# 출연연 국제협력 인터뷰 분석 코드북 (GRI-IC-Codebook v1.0)
(Opal 에이전트용 필터 기준 정의서)

이 코드북은 인터뷰 텍스트 세그먼트를 분류하기 위한 두 가지 차원(주제 코드, 맥락/뉘앙스 코드)의 코드를 정의합니다. 에이전트는 각 텍스트 세그먼트에 대해 **가장 구체적인 주제 코드 1개 이상**과 **맥락 코드 1개**를 부여해야 합니다.

## 1. 주제 코드 (Hierarchical Thematic Codes)

### 1.1. 국제협력 동인 및 목표 (COLLABORATION_DRIVERS)
* `CD_TECH_ACCESS`: (선진/보완 기술 접근) 국내에 없거나 부족한 선진 기술, 장비, 노하우 확보 목적의 발언.
* `CD_MARKET_ACCESS` / `CD_COMMERCIALIZATION`: (시장 접근/상용화) 해외 시장 진출, 기술 수출, 사업화 연계 목적.
* `CD_RESOURCE_ACCESS`: (자원/데이터 접근) 특정 생물자원, 광물자원, 거대 데이터, 인프라(연구망 등) 접근/활용 목적.
* `CD_STANDARDS_NORMS`: (표준/규범 선점) 국제 표준, 규범, 룰 세팅에 참여하거나 영향력을 행사하려는 목적.
* `CD_NETWORKING_INFO`: (네트워킹/정보 교류) 인적 네트워크 구축, 최신 동향 파악, 정보 교류 자체를 목적으로 하는 활동.
* `CD_CAPACITY_BUILDING`: (파트너 역량 강화) 주로 개도국 대상 기술 전수, 교육, 시스템 구축 지원 등 ODA와 연관된 목적.
* `CD_POLICY_INFLUENCE` / `CD_SCIENCE_DIPLOMACY`: (정책 영향력/과학 외교) 국가적 위상 제고, 외교적 관계 강화 등 정책적/외교적 목적.
* `CD_GLOBAL_CHALLENGE`: (글로벌 난제 해결) 기후 변화, 감염병 등 특정 국가가 아닌 전 지구적 문제 해결에 기여하려는 목적.
* `CD_REQUIREMENT_DRIVEN`: (과제 요건 충족) 정부 과제나 상위 기관의 요구사항(예: 국제협력 필수)을 충족시키기 위한 동기.

### 1.2. 국제협력 유형 및 방식 (COLLABORATION_TYPES_MODALITIES)
* `CT_JOINT_RD`: (공동 연구) 특정 R&D 목표를 위해 파트너와 공식적인 공동 연구 과제를 수행.
* `CT_PERSONNEL_EXCHANGE`: (인력 교류) 훈련, 파견, 초청, 단기 방문, 학위 과정(UST 등) 등 인력 교류.
* `CT_INFRA_ACCESS_SHARING`: (인프라 공유/활용) 연구 시설, 장비, 연구망, 슈퍼컴 등 인프라를 공동 활용.
* `CT_COMMERCIAL_ORIENTED`: (상업화 연계) 기술 수출, 라이선싱, 기술 이전, 합작 투자(JV) 등 상업적 활동.
* `CT_MULTILATERAL_ENGAGEMENT`: (다자 협력) IAEA, GIF, CCOP, 국제 표준화 기구 등 국제기구/컨소시엄 활동 참여.
* `CT_BILATERAL_ENGAGEMENT`: (양자 협력) 특정 해외 기관 또는 정부와 1:1로 맺는 협력 (MOU, RA 등).
* `CT_ODA_CAPACITY_BUILDING`: (ODA 사업) KOICA, EDCF 등 공식 ODA 재원을 활용한 개도국 지원 사업 수행.
* `CT_NETWORKING_EVENTS`: (네트워킹 행사) 워크숍, 컨퍼런스, 세미나 등 네트워킹 및 정보 교류 목적의 행사.

### 1.3. 장애물 및 도전과제 (OBSTACLES_CHALLENGES)
* `OBS_FUNDING`: (예산 문제)
    * `OBS_FUND_INSUFFICIENT_CORE`: (핵심 예산 부족) 기관 고유 임무나 기본 인프라 운영 예산 부족/삭감.
    * `OBS_FUND_LACK_NEW_PROJECT`: (신규 예산 부족) 국제협력 전용 신규 사업 예산 확보의 어려움.
    * `OBS_FUND_INSTABILITY`: (예산 불안정성/삭감) 정부 정책 변화에 따른 갑작스러운 예산 삭감 또는 예측 불가능성.
    * `OBS_FUND_CYCLE_MISMATCH`: (예산 주기 불일치) 파트너 국가와의 회계연도 또는 예산 확보 시기 불일치.
    * `OBS_FUND_ALLOCATION_RIGIDITY`: (예산 집행 경직성) 예산 항목 변경 불가, 불용 처리 등 경직된 집행 규정.
* `OBS_IPR_LEGAL_CONTRACTUAL`: (IPR/법률/계약 문제)
    * `OBS_IPR_DISPUTES_NEGOTIATION`: (IPR 분쟁/협상) 지식재산권 소유, 활용 관련 협상 어려움 또는 발생한 분쟁.
    * `OBS_IPR_LACK_LEGAL_SUPPORT`: (법률 지원 부족) IPR, 국제 계약 검토를 위한 내부/외부 전문 법률 지원 부족.
    * `OBS_CONTRACT_COMPLEXITY_DELAY`: (계약 절차) 계약서 검토, 합의 과정의 복잡성 및 시간 소요.
* `OBS_POLICY_GOVERNANCE`: (정책/거버넌스 문제)
    * `OBS_POL_INCONSISTENCY_SHORT_TERMISM`: (정책 비일관성/단기성) 정권 교체, 장관 교체 등에 따른 정책 방향 급변, 장기 비전 부재.
    * `OBS_POL_LACK_OF_STRATEGY`: (국가/기관 전략 부재) 명확한 국가/기관 차원의 국제협력 전략 부재.
    * `OBS_POL_FRAGMENTATION_LACK_COORDINATION`: (부처 간 칸막이/조정 부재) 과기부, 산업부, 외교부 등 부처 간 협력/조정 미흡, 파편화.
    * `OBS_POL_TOP_DOWN_APPROACH`: (하향식 정책) 현장 의견 수렴 없는 일방적, 급조된 정책 지시 (예: 2023년 국제협력 확대).
    * `OBS_POL_BUREAUCRACY_JOB_ROTATION`: (관료주의/순환보직) 정부 부처의 비효율적 행정, 담당자의 잦은 순환보직으로 인한 전문성/지속성 결여.
* `OBS_GEOPOLITICAL_PARTNER`: (지정학/파트너 문제)
    * `OBS_GEO_RESTRICTIONS_CONTROLS`: (지정학적 제약) 수출 통제(E/C), 기술보호, 민감 기술, 특정 국가(예: 미국, 중국, 러시아)와의 관계에 따른 협력 제한.
    * `OBS_GEO_PARTNER_COUNTRY_RISKS`: (파트너 국가 리스크) 파트너 국가의 정치/경제적 불안정성, 정책 변화.
    * `OBS_GEO_TECH_COMPETITION_NATIONALISM`: (기술 패권 경쟁) 협력 대상이 아닌 경쟁 대상으로 인식됨, 기술 유출 우려.
    * `OBS_PARTNER_RELATION_DIFFICULTY`: (파트너 관계 어려움) 적절한 파트너 탐색의 어려움, 신뢰 부족, 기대치 불일치, 문화/언어 장벽.
* `OBS_HUMAN_RESOURCES`: (인력 문제)
    * `OBS_HR_LACK_OF_EXPERTISE`: (전문 인력 부족) 국제 협상, 계약, 법률, 외교, 언어 등 전문 인력 부족.
    * `OBS_HR_LACK_OF_INCENTIVES_RECOGNITION`: (인센티브/평가 부족) 국제협력 활동에 대한 평가 반영 미흡, 보상/인정 부족.
    * `OBS_HR_WORKLOAD_BURDEN`: (업무 부담) 기존 업무 외 추가 부담, 시차, 언어 장벽, 행정 부담 가중.
* `OBS_INSTITUTIONAL_SYSTEMIC`: (기관/시스템 문제)
    * `OBS_INST_MANDATE_CULTURE_MISMATCH`: (임무/문화 충돌) 비영리 기관 임무와 상업화 활동 간 충돌, 위험 회피 문화.
    * `OBS_INST_INTERNAL_SYSTEMS_INEFFICIENCY`: (내부 시스템 비효율) 복잡한 내부 행정 절차, 연구관리시스템(IRIS, NTIS 등) 사용 불편.
    * `OBS_INST_LACK_INFO_SHARING_KM`: (내부 정보 공유 부족) 부서 간 협력 정보 공유 미흡, 지식 관리(KM) 부족.
    * `OBS_INST_INFRA_SUPPORT_LACK`: (기반 지원 부족) 국제협력을 위한 기초적인 행정/운영 지원 부족.

### 1.4. 성공 요인 및 촉진제 (SUCCESS_FACTORS_ENABLERS)
* `SUC_TRUST_RELATIONSHIP`: (신뢰 관계 구축) 장기적 교류, 개인적 네트워크 등 인간적 신뢰 관계.
* `SUC_CLEAR_GOALS_MUTUAL_BENEFIT`: (명확한 목표/상호 이익) 양측의 목표가 명확하고 상호 이익이 됨.
* `SUC_STRATEGIC_PLANNING_PREPARATION`: (전략적 기획/준비) 사전에 철저한 기획과 준비.
* `SUC_LEADERSHIP_SUPPORT`: (리더십 지원) 기관장 또는 정부 고위급의 관심과 지원.
* `SUC_ADEQUATE_RESOURCES_STABILITY`: (안정적 자원) 지속적이고 안정적인 예산 및 인력 지원.
* `SUC_EFFECTIVE_COMMUNICATION_COORDINATION`: (효과적 소통/조정) 내부 및 파트너와의 원활한 소통.
* `SUC_INTERNAL_SUPPORT_SYSTEM`: (내부 지원 시스템) KIMM의 MIS, KRICT의 KGRC 프로그램 등 내부 지원 제도.

### 1.5. 실패 요인 및 교훈 (FAILURE_FACTORS_LESSONS)
* `FAIL_POOR_PLANNING_PREPARATION`: (기획/준비 부족)
* `FAIL_LACK_COMMITMENT_FOLLOWUP`: (의지/후속 조치 부족)
* `FAIL_EXTERNAL_SHOCKS`: (외부 충격) 갑작스러운 정책 변경, 지정학적 이슈 등.
* `FAIL_MISCOMMUNICATION_CULTURAL_DIFF`: (소통 오류/문화 차이)

### 1.6. 정책 요구 및 제언 (POLICY_NEEDS_RECOMMENDATIONS)
* `REC_FUNDING_STABILITY_INCREASE_FLEXIBILITY`: (재원 안정성/확대/유연성)
* `REC_STRATEGY_LONG_TERM_CONSISTENCY`: (장기적/일관된 전략)
* `REC_SUPPORT_SYSTEMS_IPR_LEGAL_ADMIN`: (IPR/법률/행정 지원 시스템)
* `REC_INCENTIVES_EVALUATION_REFORM`: (인센티브/평가 제도 개선)
* `REC_HR_DEVELOPMENT_SPECIALIZATION`: (전문 인력 양성/활용)
* `REC_GOVERNANCE_COORDINATION_IMPROVEMENT`: (거버넌스/조정 기능 강화)
* `REC_STREAMLINE_PROCESSES_REGULATIONS`: (절차/규제 간소화)
* `REC_SUPPORT_BOTTOM_UP_INITIATIVES`: (상향식/풀뿌리 협력 지원)
* `REC_INFORMATION_PLATFORM_KM`: (정보 플랫폼/지식 관리)

### 1.7. 기관/분야 맥락 (INSTITUTIONAL_SECTORAL_CONTEXT)
* `CTX_INST_MISSION_MANDATE`: (기관 고유 임무) 기관의 핵심 임무와 관련된 맥락 (예: 표준, 연구망, 자원).
* `CTX_SECTOR_CHARACTERISTICS`: (분야 특수성) 원자력, 우주항공, ICT, 지질 등 해당 분야 고유의 특성.
* `CTX_ORG_STRUCTURE_CULTURE`: (조직 구조/문화) 내부 조직 구성, 의사결정 방식, 조직 문화.
* `CTX_RELATIONS_STAKEHOLDERS`: (이해관계자 관계) 특정 정부 부처(과기부, 산업부 등), 산업계, 학계와의 관계.

### 1.8. 특정 정책/사건 반응 (POLICY_EVENT_RESPONSE)
* `PER_IMPACT_BUDGET_CHANGES_2023`: (2023년 예산 조정 영향) 2023년 예산 삭감 또는 국제협력 확대 지침의 구체적인 영향.
* `PER_RESPONSE_TO_POLICY_MANDATE`: (정책 지침 대응) 특정 정책(예: 2023년 국제협력 확대)에 대한 기관의 대응 방식.
* `PER_CRITIQUE_OF_POLICY_PROCESS`: (정책 과정 비판) 정책이 결정되고 집행되는 과정 자체에 대한 비판이나 평가.

---

## 2. 맥락/뉘앙스 코드 (Context/Nuance Codes)
*(각 텍스트 세그먼트에 대해 아래 코드 중 **하나**를 부여하여 해당 발언의 성격을 명시)*

* `PROBLEM_STATED`: (문제점 제기) 문제, 어려움, 도전과제, 부정적 측면을 명시적으로 언급.
* `CAUSE_ANALYSIS`: (원인 분석) 특정 문제나 상황의 배경, 이유, 근본 원인을 설명.
* `EFFECT_DESCRIBED`: (결과/영향 기술) 특정 행동, 정책, 상황으로 인한 결과나 영향을 기술 (긍정/부정 포함).
* `SOLUTION_PROPOSED`: (해결책 제안) 개선 방안, 정책 제언, 필요 사항, 희망 사항을 제시.
* `POSITIVE_EXAMPLE/FACTOR`: (긍정 사례/요인) 성공 사례, 긍정적 요인, 장점, 이점을 구체적 사례를 들어 설명.
* `NEGATIVE_EXAMPLE/FACTOR`: (부정 사례/요인) 실패 사례, 부정적 요인, 단점, 한계를 구체적 사례를 들어 설명.
* `FACTUAL_REPORTING`: (사실/현황 전달) 객관적인 사실, 배경 정보, 현재 상태, 프로세스를 설명.
* `OPINION_EXPRESSED`: (의견/평가 제시) 발언자의 주관적인 견해, 평가, 판단, 신념을 표현.
* `INST_SECTOR_SPECIFIC`: (기관/분야 특수성 강조) 해당 기관이나 분야(예: 원자력) 고유의 특수성을 강조/설명.
* `COMPARISON`: (비교) 다른 기관, 국가, 정책, 사례 등과 비교하여 설명.