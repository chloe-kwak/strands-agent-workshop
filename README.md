# Strands Agent Workshop (한국어)

이 저장소는 [Strands Agents Samples](https://github.com/strands-agents/samples)에서 선별된 예제들을 한국어로 번역한 워크샵 자료입니다.

## 원본 출처

**Original Repository:** https://github.com/strands-agents/samples

이 워크샵은 원본 저장소의 일부 예제를 선별하여 번역하고 재구성한 것입니다. 데모 및 교육 목적으로만 사용되며, 프로덕션 환경에 직접 사용하기 전에 적절한 보안 및 테스트 절차를 적용해야 합니다.

## 목차

- [워크샵 구성](#워크샵-구성)
- [시작하기](#시작하기)
- [참고 자료](#참고-자료)

## 워크샵 소개 영상 (강추!) 

![Video](https://github.com/user-attachments/assets/2e5c8b49-10c1-492f-aa9d-36788f06c83b)

## 워크샵 구성

### [01-strands를 이용한 첫 agent 만들기](./01-strands를%20이용한%20첫%20agent%20만들기/)
Strands Agents의 기본 개념과 첫 번째 에이전트를 만드는 방법을 학습합니다.
- Agent, Model, Tools의 기본 개념
- 시스템 프롬프트 설정
- 내장 도구 및 커스텀 도구 추가
- 간단한 대화형 레시피 에이전트 구현

### [02-mcp tool을 사용하는 agent 만들기](./02-mcp%20tool을%20사용하는%20agent%20만들기/)
다양한 방식으로 도구를 생성하고 MCP(Model Context Protocol)를 활용하는 방법을 배웁니다.
- `@tool` 데코레이터를 사용한 도구 생성
- TOOL_SPEC 딕셔너리를 활용한 도구 정의
- MCP를 통한 외부 도구 통합

### [03-aws-service와 연결하는 agent 만들기](./03-aws-service와%20연결하는%20agent%20만들기/)
AWS 서비스와 연동하는 에이전트를 구축하는 방법을 학습합니다.
- AWS 서비스 연결 및 활용
- 실제 비즈니스 로직과 통합

### [04-메모리가 유지되는 agent 만들기](./04-메모리가%20유지되는%20agent%20만들기/)
대화 간 정보를 기억하고 유지하는 에이전트를 만드는 방법을 배웁니다.
- 지속적 메모리를 통한 컨텍스트 유지
- Vector Storage를 활용한 정보 저장 및 검색
- OpenSearch Serverless, FAISS, Mem0 플랫폼 활용
- 개인화된 대화 경험 구현

### [05-멀티 agent 만들기](./05-멀티%20agent%20만들기/)
여러 에이전트가 협력하는 시스템을 구축하는 다양한 패턴을 학습합니다.
- **Agents as Tools**: 계층적 구조의 에이전트 시스템
- **Agent Swarms**: 집단 지능을 활용한 협업
- **Agent Graphs**: 명시적 통신 경로를 가진 에이전트 네트워크
- **Agent Workflows**: 의존성 관리가 있는 순차적 태스크 조정

### [06-agent를 운영환경에 배포하기](./06-agent를%20운영환경에%20배포하기/)
프로덕션 환경에 에이전트를 배포하는 방법을 학습합니다.
- AWS Lambda를 통한 서버리스 배포
- AWS Fargate를 통한 컨테이너 배포

### [07-langfuse로 agent 모니터링하기](./07-langfuse로%20agent%20모니터링하기/)
운영 중인 에이전트를 모니터링하고 평가하는 방법을 배웁니다.
- Langfuse를 활용한 관찰성 확보
- 에이전트 성능 평가 및 개선

## 시작하기

### 사전 요구사항
- **Python 3.10 이상**
- **AWS 계정** (일부 예제에서 필요)
- **pip 패키지 매니저**
- 기본적인 Python 프로그래밍 지식

### 설치

1. **가상 환경 생성**
```bash
# 가상 환경 생성
python -m venv venv

# 가상 환경 활성화
# macOS/Linux:
source venv/bin/activate
# Windows:
venv\Scripts\activate
```

2. **필수 패키지 설치**
```bash
pip install strands-agents strands-agents-tools
```

3. **모델 프로바이더 설정**

[Strands 공식 문서](https://strandsagents.com/latest/user-guide/quickstart/#model-providers)를 참고하여 모델 프로바이더를 설정하세요.

### 학습 방법

각 폴더의 순서대로 진행하시면 Strands Agents의 기초부터 고급 기능까지 체계적으로 학습할 수 있습니다. 각 폴더 내 README.md와 노트북 파일을 참고하세요.

## 참고 자료

- [Strands Agents 공식 문서](https://strandsagents.com/)
- [Strands Agents 원본 샘플 저장소](https://github.com/strands-agents/samples)
- [Strands Agents Python SDK](https://github.com/strands-agents/sdk-python)
- [Strands Agents Tools](https://github.com/strands-agents/tools)

## 라이선스

원본 프로젝트의 라이선스(Apache License 2.0)를 따릅니다.
