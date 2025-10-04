# Strands SDK로 도구 만들기

이 가이드는 Strands Agents용 도구를 만드는 다양한 방법을 설명합니다.

## 도구 생성 방법

### 1. `@tool` 데코레이터 사용

도구를 만드는 가장 간단한 방법은 Python 함수에 `@tool` 데코레이터를 사용하는 것입니다:

```python
from strands import tool

@tool
def my_tool(param1: str, param2: int) -> str:
    """
    내 도구가 수행하는 작업에 대한 설명입니다.

    Args:
        param1: 첫 번째 매개변수 설명
        param2: 두 번째 매개변수 설명

    Returns:
        반환되는 내용에 대한 설명
    """
    # 더미 구현
    return f"Result: {param1}, {param2}"
```

참고: 이 방법은 Python 독스트링을 사용하여 도구를 문서화하고 타입 힌트를 사용하여 매개변수 유효성을 검사합니다

### 2. TOOL_SPEC 딕셔너리 사용

도구 정의를 더 세밀하게 제어하려면 TOOL_SPEC 딕셔너리 방식을 사용할 수 있습니다:

```python
from strands.types.tools import ToolResult, ToolUse
from typing import Any

TOOL_SPEC = {
    "name": "my_tool",
    "description": "이 도구가 수행하는 작업에 대한 설명",
    "inputSchema": {
        "json": {
            "type": "object",
            "properties": {
                "param1": {
                    "type": "string",
                    "description": "첫 번째 매개변수 설명"
                },
                "param2": {
                    "type": "integer",
                    "description": "두 번째 매개변수 설명",
                    "default": 2
                }
            },
            "required": ["param1"]
        }
    }
}

# 함수 이름은 도구 이름과 일치해야 합니다
def my_tool(tool: ToolUse, **kwargs: Any) -> ToolResult:
    tool_use_id = tool["toolUseId"]
    param1 = tool["input"].get("param1")
    param2 = tool["input"].get("param2", 2)

    # 도구 구현
    result = f"Result: {param1}, {param2}"

    return {
        "toolUseId": tool_use_id,
        "status": "success",
        "content": [{"text": result}]
    }
```

이 방법은 입력 스키마 정의에 대한 더 많은 제어를 제공합니다. 여기서는 성공 및 오류 상태를 명시적으로 처리할 수 있습니다.

참고: 이것은 Amazon Bedrock Converse API 형식을 따릅니다

#### 사용법

다음과 같이 함수를 통하거나 다른 파일에서 도구를 가져올 수 있습니다:

```python
agent = Agent(tools=[my_tool])
# 또는
agent = Agent(tools=["./my_tool.py"])
```

### 3. MCP (Model Context Protocol) 도구 사용

Model Context Protocol을 사용하여 외부 도구를 통합할 수도 있습니다:

```python
from mcp import StdioServerParameters, stdio_client
from strands.tools.mcp import MCPClient

# MCP 서버에 연결
mcp_client = MCPClient(
    lambda: stdio_client(
        StdioServerParameters(
            command="uvx", args=["awslabs.aws-documentation-mcp-server@latest"]
        )
    )
)

# 에이전트에서 도구 사용
with mcp_client:
    tools = mcp_client.list_tools_sync()
    agent = Agent(tools=tools)
```

이 방법은 MCP를 통해 외부 도구 제공자에 연결하여 다양한 서버의 도구를 사용할 수 있게 합니다. stdio 및 HTTP 전송을 모두 지원합니다

## 모범 사례

1. **도구 이름 지정**: 도구에 명확하고 설명적인 이름을 사용하세요
2. **문서화**: 도구가 수행하는 작업과 매개변수에 대한 자세한 설명을 제공하세요
3. **오류 처리**: 도구에 적절한 오류 처리를 포함하세요
4. **매개변수 유효성 검사**: 처리하기 전에 입력의 유효성을 검사하세요
5. **반환 값**: 에이전트가 이해하기 쉬운 구조화된 데이터를 반환하세요

## 예제

이 디렉토리의 예제 노트북을 확인하세요:
- [MCP 도구 사용](01-using-mcp-tools/mcp-agent.ipynb): MCP 도구를 에이전트와 통합하는 방법 학습
- [사용자 정의 도구](02-custom-tools/custom-tools-with-strands-agents.ipynb): 사용자 정의 도구를 만들고 사용하는 방법 학습

자세한 내용은 [Strands 도구 문서](https://strandsagents.com/0.1.x/user-guide/concepts/tools/python-tools/)를 참조하세요.
