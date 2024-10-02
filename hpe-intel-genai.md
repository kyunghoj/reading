--
Date: 2024-10-02
--
# HPE-Intel Gen AI 세미나

* Date: 2024-10-02
* Place: [페어몬트 앰배서더 서울](https://naver.me/x0ahMtPm)

## 환영사

이승국 전무, HPE

2023년은 Gen AI에 대해 묻지마 투자가 이루어졌다면, 2024년부터는 ROI와 TCO를
고려한 투자가 이루어지고 있다. 2025년에는 24년의 추세가 가속될 것이며, 점점 더
소위 "가성비"를 따지는 솔루션과 하드웨어를 고객들이 찾을 것이다.
HPE는 N사와 협력해 왔으나, Intel 과 협력을 시작했다.
AI 플랫폼 구축에는 Public Cloud 를 사용하거나 Open Source 로 (on-premises)에
구축하는 것 외에 선택지가 없었다. HPE는 Ezmeral 이라는 소프트웨어를 제공한다.
또한 한국 고객들이 한국화 (한국어 및 커스터마이즈)를 많이 원한다. 따라서
Teddysum 이나 DeepNatural 같은 국내 회사와 협업하고 있다.

## HPE GenAI Solution with Intel

유충근 상무, HPE

HPE와 Intel이 협력한 Aurora Supercomputer 를 보여줌. Argonne National Lab.
TOP500 에서 2위를 차지하고 있다. NVIDIA GPU를 사용하지 않고, Intel Data Center GPU Max Xe를 사용한다.
AuroraGPT 는 Aurora를 이용한 AGI for Science. 1T 개의 parameter 를 가진 모델.

Gaudi 3에는 128GB의 HBM2e 가 있으며, Vector Engine과 Matrix Engine이 달려 있다.
네트워크는 Ethernet 을 기반으로 한 HPE Slingshot 을 사용했다.
Dragonfly, RDMA, Ethernet 표준. Native IP 사용. Encapsulation 필요 없음.
Switch, NIC 를 위한 ASIC 을 개발하고 있다.
Gaudi 3 는 H100 대비 약간 (10%) 나은 성능. 가성비로는 1.8배.
850W라고 하는데, H100 PCIe 랑 비교한 것 같다.
UXL 은 SYCL, oneAPI 를 표준화 하는 것.
SYCLomatic 은 CUDA 소스코드를 SYCL로 변환하는 툴.

HPE New Hope 라는 비공개 아키텍처 코드명. Gaudi 3를 위한 것. (사진)

## Power AI Everywhere  with Intel AI Solutions

나승주 상무, Intel
APAC - 중국 제외 D/C 영업 담당.

왜 고객들이 Intel을 고려하는가?
* 더 많은 선택
* 종속성 X
* 비용 절감

Inference 하는데에는 최고 성능이 필요하지 않다. 적당한 성능, 비용, 안정성, 전력이 중요.
Software 최적화를 통해 성능 개선이 지속적으로 이루어지고 있다.

## HPE ProLiant Compute Portfolio for AI inferencing

서유덕 매니저, HPE

* Gen12 소개. Xeon 6 기반. Modularity 에 초점.
* iLO 7. Sustainability 관련 telemetry 데이터 제공.
* PCIe lane 이 88개 / Socket
* NVMe, EDSFF 라는 form factor - CXL의 기본 ff
* DC-MHS: Data Center Modular Hardware System 이라는 규격 (OCP에서 나온 것)
  * M-FLW: 표준 풀사이즈 메인보드
  * M-DNO: density. CPU 하나 혹은 두개더라도 1U 서버.
  * DC-SCM: BMC 부분.
    * DC-SCM is critical to DC-MHS ecosystem enablement!
* 다음 세대에는 고객 맞춤형... 이면서도 기존 ProLiant 의 장점을...
* Intel Gaudi 관련 얘기는 없고, GH200 NVL 을 활용한 부분이 인상적
  * NVL은 NVLink 로 GPU 두 개를 붙인 것

## HPE Ezmeral을 활용한 AI 구축 전략
김일근 매니저, HPE

마케팅의 4P
* Product
* Price
* Place
* Promotion

제품Product 자체의 혁신이 중요한 상황이 되었음
* 드러커: 큰 혁신 없이는 선도 기업 뛰어넘을 수 없음
* 브라이언 아서: 성공한 기업은 계속 더 큰 성공을 거둠.

## HPE GenAI Value Pack & Portfolio

* 프로젝트 경험 기반으로 소개해주심. SI 하시던 분.
* 필요한 모든 솔루션을 가진 회사는 없음
* 그걸 꿰매서 e2e를 만들어야 함
* 최근 OCR은 성능이 좋아짐. 90% 이상.
* 잘 안된 것은 failure 처리.
* Foundation Models
  * Solar 나 Llama3 가 10B 이하에서 잘 됨
  * 70B는 bllosomm (Llama3 기반 한국어 튜닝)
* Hybrid Search
  * 의미검색 잘 안되기도 함
  * keyword search도 한다...
* CI/CD
  * FM이 큰 파일이므로... vLLM을 쓴다고?
  * Serving fw: KServe + Triton + vLLM?
    * KServe 역할이 궁금하네...
* Inference Orchestrator
  * AppBuilder - DeepNatural
* RAG - Milvus 를 default 로 하고 있음
* Model을 HF에서 다운로드 받아야 함. 별도로 받아서 업로드 하는 것 가능.

## 최신 AI 활용 고객사례

염승명 전무, 한국정보공학

KBO 데이터 RAG 파일럿

* MariDB 를 사용 - Milvus 와 함께
* 예상 질문을 가지고 SQL query 몇 개 만들고 Few-Shot example로 사용
* txt-to-sql
  * Input text 에 대해 sql query 를 매칭
* Upstage Document AI 많이 쓰는 듯

## HPE Private LLM Architecture

김기웅 매니저, HPE

LLMOps 와 Infrastructure 부분에 대해 이야기하겠다. 

Ezmeral 기반
* EZUA: Orchestrator 부분인 듯? K8s 기반이라고.
  * Unified Analytics 라고...
  * VM 사용. KVM. RedHat Openstack이 아닐까 싶음.
* EZUA Worker: 추론에는 L40s GPU로 충분. L4 를 구매해서 테스트 하기도 함.
* Ezmeral Data Fabric: Storage 인데, object storage 인가? Ceph 같은?
  * https://www.hpe.com/psnow/doc/a00110846kop
  * File, Object, Event Stream, NoSQL 을 다 가지고 있는 듯
* PoC 에는 10Gbps 네트워크로 들어가고 있음. 10G SFP+.
* GPU Worker Node 의 경우 CPU 의 로드는 높지 않음

## Seamless Model Management: updatable LLM with Blang

함영균 대표, Teddysum
* KAIST 박사 (최기선 교수 연구실?)

* 고유 언어모델 BLLOSSOM 보유.
  * http://chat.bllossom.ai/
  * (https 를 안 하다니...)
  * https://huggingface.co/MLP-KTLim/llama-3-Korean-Bllossom-8B
* Private sLLM
* 오픈 LLM 의 문제: 비영어권 언어 소외 현상
* 한국어 모델은 학계에 의존
* HyperCLOVA 보다 낫다고 자부. GPT와 경쟁한다?
* 기존 방법은 2-3억원이 들지만 여기는 5일만에 0.5억원.
* Bilingual PP, Bilingual Code-switch Pre-training
* 250GB 정도의 한국어 데이터로 pre-training 을 진행 중
* Fine-tuning 을 하면 지식이 채워지냐? 그렇지 않음. Pre-training이 지식이 채워짐.
* Fine-tuning은 공부를 열심히 한 친구가 말을 잘 못한다? 아나운서 교육을 듣는 것과 같음.
  * PEFT 를 얘기하는 것 같음
* 우리는 Pretraining을 하는 업체임. KoAlpaca, Lllama2-70B 대비 더 나음.
* GPT4 외에는 더 나음.
* GPT는 표나 그래프를 잘 못함.
  * 한국 문서에 표가 많고 처리하게 어려움.
  * workflow 같은 그림도 해석 못함 (한국 문서에 많음)
* Bllossom-V: Language-Vision Alignment
  * X-LLaVA: LLaVA 기반인 듯?
* bLang - Seamless Model Management Platform
  * Ubuntu 상용 버전과 비슷한 모델
* 학습은 어려우니 Prompt Engineering으로 많이 하려고 함
  * 실제 문제는 학습이 필요한 경우가 많음
  * 하지만 데이터 양이 적다
  * 가지고 있는 데이터도 적고 변환하는 비용이 크다
  * Prompt Tuning 이라는 개념
  * PEFT - Parameter-Efficient Transfer Learning for NLP
    * LoRA: Adapter 를 붙이는 것
* 국방: 공군 기계번역.
* Code assistant
  * RAG for Code Assistant
    * [BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3), Milvus
    * [Nori Tokenizer](https://esbook.kimjmin.net/06-text-analysis/6.7-stemming/6.7.2-nori), OpenSearch
    * 장애문서 활용
* 금융: 신뢰성. 근거. 최신성 문제.
* 대화기록 저장 안함

## AI Assistant & Enterprise Search

박상원 대표, DeepNatural

* LLM: In-context Learning
  * Few-shot learning
  * 이것을 극대화 한 것이 RAG 임
  * 지식은 검색해서 넣어주고, 추론을 LLM이 맡음
* AI Agent 는 무엇인가?
  * 환경에서 관찰, 액션, 경험 등을 쌓아가면서...
  * Matrix의 Agent Smith: Matrix 라는 환경에서 ...
* Agentic Workflow
  * 대화가 가능한 agent 가 서로 대화, 혹은 중재자가 있고 다른 agent 가 소통.
  * 장점
    * 각 에이전트가 특정 역할을 담당, 복잡한 문제 해결 가능
    * 새로운 기능, 에이전트를 추가하기 용이
    * 병렬 프로세싱 및 다양한 에이전트같의 협업을 통해 더 복잡하고 동적인 작업을 수행가능
  * 단점
    * 상호작용 고려, 신경쓸 게 많고 개발 및 관리가 복잡해짐
    * (Byzantine Fault?)
  * Case Study #1
    * 영어 문법 문제 출제
      * subtask 를 정의
      * Examiner, Critic, Reviewers (Vocabulary, Grammer, Question Length Reviewer, Meta Reviewer)
  * Case Study #2: LLM 파인튜닝 학습 데이터 구축
    * 보험 약관, 판매 예규
    * Agentic Workflow 가 Vector Store 에 저장된 문서를 보고 학습데이터 셋을 만들어 봄
    * 비용: 1500만원, 3개월 -> 500만원, 3일
  * "전체는 그 부분의 합보다 크다" - 아리스토텔레스
* Enterprise Search
  * 사내 지식 정보 검색이 어려움. 평균 103개의 SaaS를 사용.
  * 업무 시간의 25% 를 정보 검색에 할애하고 있음.
* AI Agent + Enterprise Search
  * Friday 라는 제품을 만드는 중
  * Gen AI 기반 사내 검색 AI Agent
  * AI Agent가 사용자 질문을 분석하여 검색 계획을 세움
  * 관련 검색/질문 추천. 
  * 검색 결과는 thread 로 관리됨.

## 맺음말

김일근 매니저, HPE

