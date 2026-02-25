# KICT 인터뷰 코딩 (추가 분석)

본 문서는 `policy_1027_all_coding.md`에 수록된 15개 기관 인터뷰 코딩의 보완 자료로, 한국건설기술연구원(KICT) 인터뷰를 동일한 절차에 따라 분석한 결과입니다.

- **분석 대상:** `raw/kict_250905.md` (2025년 9월 5일 인터뷰)
- **코드북:** GRI-IC-Codebook v1.0 (`codebook.md`)
- **분석 방법:** 의미 단위 분해 → 맥락 재구성 → 이중 코딩 → 검증 및 통합

---

## KICT 한국건설기술연구원

### 1. 인터뷰 핵심 요약
- 이찬주 실장님은 글로벌사업실이 ODA 사업을 주 임무로 하면서, 2018년부터 잉여금을 활용한 국제매칭연구 사업(연간 6~7건, 건당 약 1억 원, 1:1 매칭)을 운영해 약 40여 건의 과제를 수행했다고 설명했습니다.
- 최준석 박사님은 혁신법 시행으로 해외 기관 R&D 비용 지급이 한때 금지되었다가 최근 재개되는 등 정책 혼선이 있었고, 과기부의 국제협력 확대 기조에 따라 R&D 기획 단계에서 국제협력을 포함해야 하는 분위기가 형성되었다고 밝혔습니다.
- 양 인터뷰이 모두 공동 IP의 법적 복잡성과 비용(2천만 원 과제에 변호사·변리사 비용이 2천만 원 소요)을 핵심 장애로 지목하며, 연구진이 국제 공동 IP를 사실상 포기하고 논문 중심 협력으로 전환했다고 토로했습니다.
- 이찬주 실장님은 호주 CSIRO(7년 이상), 네덜란드 Deltares(5년), 핀란드 Aalto대학(7년), 일본 교토대학(6년) 등 장기 신뢰 기반 협력 성공 사례를 소개하며, 안동 하천실험센터(세계 1~2개) 등 고유 인프라가 해외 파트너 유치에 강점이 된다고 강조했습니다.
- 최준석 박사님은 호라이즌 EU 준회원국 가입 이후 과제 참여를 추진 중이나, 유럽의 폐쇄적 네트워크와 인맥 의존 구조로 진입이 극히 어렵다고 진단했습니다.

### 2. 주요 장애요인 및 도전과제
- 국제 공동 IP 협상의 법적 복잡성과 비용이 과제 규모를 초과해 연구진이 IP를 포기하고, 미국의 수출통제 강화로 계약 조건이 더욱 까다로워지고 있습니다.
- 혁신법 개정에 따른 해외 기관 비용 지급 금지·재개, 정권별 기조 변화 등 정책 비일관성이 현장에 혼란을 야기하며, 단년 과제 구조와 단기 성과 요구가 국제협력의 장기적 특성과 괴리됩니다.
- 호라이즌 EU의 기존 컨소시엄 진입이 유럽 내 인맥 중심으로 폐쇄적이고, 과제 예산 대부분이 인건비여서 직접 연구비 인센티브가 미약합니다.
- RCMS·E-JIBARO 등 국내 연구관리 플랫폼의 복잡성으로 외국 연구자 참여가 사실상 불가능하며, 해외 기관 정보가 체계화되어 있지 않아 파트너 탐색이 인맥에 의존합니다.

### 3. 정책적 시사점
- 국제 공동연구 계약·IP 협상을 전담 지원하는 별도 조직·시스템 구축이 필요하며, 나라·기관별로 상이한 법적 기준을 대응할 수 있는 전문 인력 확보가 시급합니다.
- 국제협력 과제의 평가 기간과 성과 기준을 장기 시간 프레임에 맞게 조정하고, 단년 예산 구조를 개선하여 네트워크 구축과 신뢰 형성에 필요한 시간을 보장해야 합니다.
- 건설·SOC 분야의 현장 실증 역량과 세계적 수준의 연구 인프라(안동 하천실험센터 등)를 국제협력 유치의 전략적 자산으로 체계적으로 활용하고, 해외 기관 정보 플랫폼 구축을 통해 파트너 발굴을 지원할 필요가 있습니다.

### 4. 코드화 테이블

| Institution | Speaker | Source_Text | Thematic_Code(s) | Context_Code | Location_Ref |
| :--- | :--- | :--- | :--- | :--- | :--- |
| KICT | 이찬주 실장님 | 글로벌사업실은 ODA 국제개발협력 사업을 주 임무로 하며, 잉여금(적립금)을 활용해 2018년부터 국제매칭연구 사업을 운영하고 있고, 연간 6~7개 과제(건당 약 1억 원)를 1:1 매칭 원칙으로 지원한다고 설명했습니다. | `CTX_INST_MISSION_MANDATE`, `CTX_ORG_STRUCTURE_CULTURE`, `SUC_INTERNAL_SUPPORT_SYSTEM` | `FACTUAL_REPORTING` | (250905_KICT_~00:51) |
| KICT | 최준석 박사님 | 해외 거점기관을 선정해 연간 1명씩 파견을 보내 국제협력 아이템을 발굴하도록 하고 있으며, KIST 유럽·교토대학·미국 대학 등에 연구자를 파견 중이라고 소개했습니다. | `CT_PERSONNEL_EXCHANGE`, `CD_NETWORKING_INFO` | `FACTUAL_REPORTING` | (250905_KICT_~05:27) |
| KICT | 이찬주 실장님 | 호라이즌 EU 준회원국 가입에 따라 과제 정보 제공, NCP 초청 행사 개최, KIST 유럽 파견 등을 통해 연구진의 유럽 과제 참여를 지원하고 있다고 밝혔습니다. | `CT_MULTILATERAL_ENGAGEMENT`, `CD_NETWORKING_INFO` | `FACTUAL_REPORTING` | (250905_KICT_~04:04) |
| KICT | 이찬주 실장님 | 과기부의 국제협력 확대 기조에 따라 과제 예산 중 일부를 국제공동과제로 편성해야 접수가 가능한 상황이며, 공무원들도 총액 목표를 달성해야 하는 구조라고 지적했습니다. | `OBS_POL_TOP_DOWN_APPROACH`, `CD_REQUIREMENT_DRIVEN`, `PER_RESPONSE_TO_POLICY_MANDATE` | `CAUSE_ANALYSIS` | (250905_KICT_~07:14) |
| KICT | 최준석 박사님 | 혁신법 시행으로 해외 기관에 대한 국가 R&D 비용 지급이 한때 금지되어 국제협력이 대폭 위축되었고, 이후 정책이 재개되었으나 현장에 혼선을 야기했다고 설명했습니다. | `OBS_POL_INCONSISTENCY_SHORT_TERMISM`, `FAIL_EXTERNAL_SHOCKS` | `NEGATIVE_EXAMPLE/FACTOR` | (250905_KICT_~07:40) |
| KICT | 최준석 박사님 | 전 정부에서 국가 R&D 예산이 줄어든 상황에서 국제협력 확대를 요구하며 부처별로 가점 제도를 급조했고, 연구진은 필수적으로 국제협력을 과제에 포함시키기 시작했다고 밝혔습니다. | `PER_IMPACT_BUDGET_CHANGES_2023`, `OBS_POL_TOP_DOWN_APPROACH` | `CAUSE_ANALYSIS` | (250905_KICT_~08:55) |
| KICT | 이찬주 실장님 | 미국 대학에 2천만 원짜리 위탁과제를 주려 했으나 IP·보안 관련 계약 검토에 변호사·변리사 비용이 과제비 이상 소요되어, 결국 계약도 못 하고 종료된 사례를 소개했습니다. | `OBS_IPR_LACK_LEGAL_SUPPORT`, `OBS_CONTRACT_COMPLEXITY_DELAY` | `NEGATIVE_EXAMPLE/FACTOR` | (250905_KICT_~04:45) |
| KICT | 최준석 박사님 | 해외 기관과 공동 IP를 시도했으나 양측 법무 부서의 검토 과정에서 극도의 행정적 스트레스가 발생해, 이후 연구진 사이에서 국제 공동 IP는 사실상 포기하고 논문 위주로만 협력한다고 토로했습니다. | `OBS_IPR_DISPUTES_NEGOTIATION`, `OBS_HR_WORKLOAD_BURDEN` | `PROBLEM_STATED` | (250905_KICT_~18:52) |
| KICT | 이찬주 실장님 | 건설·수자원 분야는 반도체·배터리처럼 세계 시장에서 경쟁하는 것이 아니라 지역 기반으로 운영되기 때문에 IP 갈등이 상대적으로 덜 첨예하며, 각자 자국에서 특허를 내는 방식으로 처리한다고 설명했습니다. | `CTX_SECTOR_CHARACTERISTICS`, `OBS_IPR_DISPUTES_NEGOTIATION` | `INST_SECTOR_SPECIFIC` | (250905_KICT_~20:21) |
| KICT | 최준석 박사님 | IP 공동 출원의 행정적 복잡성을 경험한 뒤 연구진 간에 'IP는 아예 언급도 하지 말자'는 합의가 형성되었고, 계약서에도 IP 관련 조항을 넣지 않는 것이 관행이 되었다고 밝혔습니다. | `OBS_IPR_DISPUTES_NEGOTIATION`, `OBS_CONTRACT_COMPLEXITY_DELAY` | `NEGATIVE_EXAMPLE/FACTOR` | (250905_KICT_~24:35) |
| KICT | 이찬주 실장님 | 미국에서 최근 보안·수출통제 규정이 강화되면서 기존에 없던 규제가 신설되고, 연구 기관 법무팀이 정보 유출 소송 리스크를 극도로 경계해 글자 한두 개 차이로 밀당이 반복된다고 지적했습니다. | `OBS_GEO_RESTRICTIONS_CONTROLS`, `OBS_CONTRACT_COMPLEXITY_DELAY` | `CAUSE_ANALYSIS` | (250905_KICT_~26:50) |
| KICT | 최준석 박사님 | IP 협상을 전담해 주는 별도 조직이나 기관이 있으면 연구자가 던지기만 하면 되어 매우 도움이 될 것이라고 제안했으나, 나라·기관마다 규정이 달라 표준 가이드라인 마련은 어렵다는 한계도 인식하고 있었습니다. | `REC_SUPPORT_SYSTEMS_IPR_LEGAL_ADMIN` | `SOLUTION_PROPOSED` | (250905_KICT_~25:45) |
| KICT | 최준석 박사님 | 호라이즌 EU 참여를 추진하고 있으나 유럽 기존 컨소시엄의 이너서클에 진입하기가 극히 어렵고, 한국 기관의 인지도가 낮아 콜드 컨택이 통하지 않는다고 진단했습니다. | `OBS_PARTNER_RELATION_DIFFICULTY`, `CT_MULTILATERAL_ENGAGEMENT` | `PROBLEM_STATED` | (250905_KICT_~33:49) |
| KICT | 이찬주 실장님 | 유럽은 인맥 중심으로 운영되며, 코디네이터(주관기관 책임자)의 승인이 필수인데 이를 모르는 상태에서 접근하면 불가능에 가깝다고 강조하며, 수년간의 신뢰 구축이 선행돼야 한다고 설명했습니다. | `SUC_TRUST_RELATIONSHIP`, `OBS_PARTNER_RELATION_DIFFICULTY` | `CAUSE_ANALYSIS` | (250905_KICT_~35:52) |
| KICT | 이찬주 실장님 | 호라이즌 EU 사업은 본래 유럽 27개국 간 인건비 분배 메커니즘이며, 직접비 비중이 매우 낮고 최종 성과도 유럽 사회에 대한 혜택으로 귀결되어야 하는 구조라서, 한국 참여자의 재정적 인센티브가 미약하다고 분석했습니다. | `CT_MULTILATERAL_ENGAGEMENT`, `OBS_FUND_ALLOCATION_RIGIDITY` | `CAUSE_ANALYSIS` | (250905_KICT_~37:56) |
| KICT | 최준석 박사님 | 국제매칭연구 사업이 단년 과제라 연속성이 부족하고 인건비를 잡을 수 없어 메리트가 떨어지며, ODA 사업도 간접비가 너무 적어 기관 차원에서 기피하는 측면이 있다고 지적했습니다. | `OBS_FUND_ALLOCATION_RIGIDITY`, `OBS_HR_LACK_OF_INCENTIVES_RECOGNITION` | `PROBLEM_STATED` | (250905_KICT_~45:41) |
| KICT | 이찬주 실장님 | 정부가 단기 성과를 요구하지만 국제협력은 네트워크 구축과 원거리 협업으로 진행 속도가 본질적으로 느리며, 과제 기간을 타이트하게 잡으면 눈에 보이는 성과를 위해 답을 미리 정해놓고 수행하게 된다고 비판했습니다. | `OBS_POL_INCONSISTENCY_SHORT_TERMISM`, `REC_STRATEGY_LONG_TERM_CONSISTENCY` | `OPINION_EXPRESSED` | (250905_KICT_~1:03:19) |
| KICT | 최준석 박사님 | 국제협력 과제가 다양한 부처에서 상이한 형태로 발주되어 혼란스럽고, 상대 기관에 대한 정보가 한정되어 있어 항상 알음알음 인맥으로 파트너를 찾는 실정이라고 토로했습니다. | `OBS_POL_FRAGMENTATION_LACK_COORDINATION`, `OBS_INST_LACK_INFO_SHARING_KM` | `PROBLEM_STATED` | (250905_KICT_~1:01:45) |
| KICT | 최준석 박사님 | 해외 기관 정보가 국가·분야별로 체계화되어 제공된다면 큰 도움이 될 것이며, 현재는 외국 기관도 한국과 협업하고 싶어 하지만 서로 정보가 없어 알음알음 연락하는 상황이라고 밝혔습니다. | `REC_INFORMATION_PLATFORM_KM`, `OBS_PARTNER_RELATION_DIFFICULTY` | `SOLUTION_PROPOSED` | (250905_KICT_~1:11:06) |
| KICT | 최준석 박사님 | RCMS·E-JIBARO 등 국내 연구관리 플랫폼이 외국 연구자에게는 인증·등록 과정이 지나치게 복잡해 사실상 사용이 불가능하며, 해외 파트너에게는 lump sum 방식이 현실적이라고 지적했습니다. | `OBS_INST_INTERNAL_SYSTEMS_INEFFICIENCY`, `REC_STREAMLINE_PROCESSES_REGULATIONS` | `PROBLEM_STATED` | (250905_KICT_~1:13:08) |
| KICT | 최준석 박사님 | ODA 사업에서 수원국이 오랜 기간 원조에 익숙해져 이를 역으로 레버리지로 활용하는 경우가 빈번하고, 현지 협조가 안 되면 과제 수행 자체가 어려운 상황이 발생한다고 설명했습니다. | `CT_ODA_CAPACITY_BUILDING`, `OBS_GEO_PARTNER_COUNTRY_RISKS` | `PROBLEM_STATED` | (250905_KICT_~52:47) |
| KICT | 이찬주 실장님 | ODA 현장 실증 시 계측 장비·특수 소재를 한국에서 가져가야 하는데 통관·운반·전기 규격 맞춤 등에 6개월이 소요되기도 하며, 일반 R&D 과제의 정해진 기간 안에 이를 처리하기 극히 어렵다고 토로했습니다. | `OBS_INST_INFRA_SUPPORT_LACK`, `OBS_CONTRACT_COMPLEXITY_DELAY`, `CT_ODA_CAPACITY_BUILDING` | `NEGATIVE_EXAMPLE/FACTOR` | (250905_KICT_~56:55) |
| KICT | 이찬주 실장님 | CSIRO(호주)와의 우연한 MOU에서 시작된 협력이 NST 차원의 MOU 확대로 이어졌고, 지금도 후속 과제가 계속 나오는 대표적 성공 사례라고 소개했습니다. | `SUC_TRUST_RELATIONSHIP`, `CT_BILATERAL_ENGAGEMENT`, `CT_JOINT_RD` | `POSITIVE_EXAMPLE/FACTOR` | (250905_KICT_~1:06:28) |
| KICT | 이찬주 실장님 | 안동 하천실험센터(세계 1~2개 수준)를 기반으로 Deltares와 5년, Aalto대학과 7년간(2018~현재) 장기 공동연구를 수행하며 다수의 SCI 논문을 발표했고, 교토대학 파견도 6년째 계속되고 있다고 밝혔습니다. | `CT_INFRA_ACCESS_SHARING`, `SUC_TRUST_RELATIONSHIP`, `CT_JOINT_RD` | `POSITIVE_EXAMPLE/FACTOR` | (250905_KICT_~1:07:57) |
| KICT | 이찬주 실장님 | 45억 원 규모의 달 환경 모사 진공체임버를 활용해 2019년 NASA와 공동연구를 추진했으나 코로나로 중단되었다며, 세계적 수준의 연구 인프라가 국제협력 유치의 강점이 된다고 강조했습니다. | `CT_INFRA_ACCESS_SHARING`, `FAIL_EXTERNAL_SHOCKS` | `NEGATIVE_EXAMPLE/FACTOR` | (250905_KICT_~1:09:10) |
| KICT | 최준석 박사님 | 12대 국가전략기술에 건설·토목이 포함되지 않아 기관 차원에서 소외감과 위기감을 느끼고 있으며, 기후·재해·도시 환경 등 중요한 이슈가 전략기술에 반영되지 않는 현실을 지적했습니다. | `CTX_INST_MISSION_MANDATE`, `OBS_POL_LACK_OF_STRATEGY`, `PER_CRITIQUE_OF_POLICY_PROCESS` | `OPINION_EXPRESSED` | (250905_KICT_~1:21:16) |
