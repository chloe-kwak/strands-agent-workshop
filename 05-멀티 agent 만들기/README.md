# 멀티 Agent 시스템

이 튜토리얼 섹션에서는 Strands Agents SDK를 사용하여 멀티 에이전트 시스템을 구축하는 다양한 접근 방식을 탐색합니다.

## 멀티 Agent 시스템 접근 방식

### 1. Agents as Tools
[문서 링크](https://strandsagents.com/latest/user-guide/concepts/multi-agent/agents-as-tools/)

"Agents as Tools" 패턴은 특수화된 AI 에이전트를 다른 에이전트가 사용할 수 있는 호출 가능한 함수(도구)로 래핑하여 계층적 구조를 만듭니다:

- **Orchestrator Agent**: 사용자 상호 작용을 처리하고 특수화된 에이전트에 작업을 위임합니다
- **Specialized Tool Agents**: 오케스트레이터가 호출할 때 도메인별 작업을 수행합니다
- **주요 이점**: 관심사 분리, 계층적 위임, 모듈식 아키텍처

구현은 `@tool` 데코레이터를 사용하여 특수화된 에이전트를 호출 가능한 함수로 변환하는 것을 포함합니다:

```python
@tool
def research_assistant(query: str) -> str:
    """연구 관련 쿼리를 처리하고 응답합니다."""
    research_agent = Agent(system_prompt=RESEARCH_ASSISTANT_PROMPT)
    return str(research_agent(query))
```


### 2. Agent Swarms
[문서 링크](https://strandsagents.com/latest/user-guide/concepts/multi-agent/swarm/)

에이전트 스웜은 함께 작동하는 자율 AI 에이전트 컬렉션을 통해 집단 지능을 활용합니다:

- **분산 제어**: 전체 시스템을 지시하는 단일 에이전트가 없습니다
- **공유 메모리**: 에이전트가 통찰력을 교환하여 집단 지식을 구축합니다
- **조정 메커니즘**: 협력적, 경쟁적 또는 하이브리드 접근 방식
- **통신 패턴**: 에이전트가 서로 통신할 수 있는 메시 네트워크

내장된 `swarm` 도구가 구현을 단순화합니다:

```python
from strands import Agent
from strands_tools import swarm

agent = Agent(tools=[swarm])
result = agent.tool.swarm(
    task="이 데이터셋을 분석하고 시장 트렌드를 식별하세요",
    swarm_size=4,
    coordination_pattern="collaborative"
)
```

### 3. Agent Graphs
[문서 링크](https://strandsagents.com/latest/user-guide/concepts/multi-agent/graph/)

에이전트 그래프는 명시적인 통신 경로를 가진 상호 연결된 AI 에이전트의 구조화된 네트워크를 제공합니다:

- **노드 (Agents)**: 특수화된 역할을 가진 개별 AI 에이전트
- **엣지 (Connections)**: 에이전트 간의 통신 경로를 정의합니다
- **토폴로지 패턴**: 스타, 메시 또는 계층적 구조

`agent_graph` 도구는 정교한 에이전트 네트워크 생성을 가능하게 합니다:

```python
from strands import Agent
from strands_tools import agent_graph

agent = Agent(tools=[agent_graph])
agent.tool.agent_graph(
    action="create",
    graph_id="research_team",
    topology={
        "type": "star",
        "nodes": [
            {"id": "coordinator", "role": "team_lead"},
            {"id": "data_analyst", "role": "analyst"},
            {"id": "domain_expert", "role": "expert"}
        ],
        "edges": [
            {"from": "coordinator", "to": "data_analyst"},
            {"from": "coordinator", "to": "domain_expert"}
        ]
    }
)
```

### 4. Agent Workflows
[문서 링크](https://strandsagents.com/latest/user-guide/concepts/multi-agent/workflow/)

에이전트 워크플로는 명확한 종속성을 가진 정의된 시퀀스로 여러 AI 에이전트에 걸쳐 작업을 조정합니다:

- **작업 정의**: 각 에이전트가 수행해야 하는 작업에 대한 명확한 설명
- **종속성 관리**: 순차 종속성, 병렬 실행, 조인 포인트
- **정보 흐름**: 한 에이전트의 출력을 다른 에이전트의 입력에 연결

`workflow` 도구는 작업 생성, 종속성 해결 및 실행을 처리합니다:

```python
from strands import Agent
from strands_tools import workflow

agent = Agent(tools=[workflow])
agent.tool.workflow(
    action="create",
    workflow_id="data_analysis",
    tasks=[
        {
            "task_id": "data_extraction",
            "description": "보고서에서 주요 데이터를 추출합니다"
        },
        {
            "task_id": "analysis",
            "description": "추출된 데이터를 분석합니다",
            "dependencies": ["data_extraction"]
        }
    ]
)
```

## 올바른 접근 방식 선택

- **Agents as Tools**: 특수화된 전문 지식을 가진 명확한 계층적 구조에 가장 적합
- **Agent Swarms**: 창발적 지능을 통한 협업 문제 해결에 이상적
- **Agent Graphs**: 통신 패턴에 대한 정밀한 제어에 완벽
- **Agent Workflows**: 명확한 종속성을 가진 순차적 프로세스에 최적

각 접근 방식은 복잡성, 제어 및 협업 패턴 측면에서 서로 다른 트레이드오프를 제공합니다. 올바른 선택은 특정 사용 사례 및 요구 사항에 따라 달라집니다.
