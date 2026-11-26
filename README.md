# date-tool-mcp

一个基于 Python 和 [Model Context Protocol（MCP）](https://modelcontextprotocol.io/) 的日期工具服务。MCP 客户端连接后，可以调用 `get_today` 获取运行该服务的服务器当前日期。

## 功能

- 提供 `get_today` 工具，无需传入参数。
- 返回服务器本地日期，格式为 `YYYY-MM-DD`，例如 `2026-11-26`。
- 使用 FastMCP 实现，默认通过 stdio 与 MCP 客户端通信。

## 项目结构

```text
MCP_开源/
├── server.py          # MCP 服务及日期工具实现
├── chatmcp.yaml       # ChatMCP 服务配置示例
├── requirements.txt   # Python 依赖
└── README.md          # 项目说明
```

## 环境要求

- 推荐使用 Python 3.10 或更高版本
- `mcp>=1.28.1`

## 安装

在项目目录中执行：

```bash
python -m venv .venv
```

激活虚拟环境：

```powershell
# Windows PowerShell
.venv\Scripts\Activate.ps1
```

```bash
# macOS / Linux
source .venv/bin/activate
```

安装依赖：

```bash
python -m pip install -r requirements.txt
```

## 启动服务

```bash
python server.py
```

服务默认使用 stdio 传输，启动后会等待 MCP 客户端连接，通常不会输出交互式提示或 HTTP 访问地址。实际使用时一般由 MCP 客户端自动启动该进程，无需单独保持终端运行。

## 配置 MCP 客户端

在支持 MCP 的客户端配置中添加以下服务：

```json
{
  "mcpServers": {
    "date-tool-mcp": {
      "command": "python",
      "args": [
        "D:\\PythonProject_stage2\\上课\\p17_1125\\MCP_开源\\server.py"
      ]
    }
  }
}
```

请根据实际部署位置修改 `server.py` 的绝对路径，并确保客户端使用的 `python` 环境已安装项目依赖。如果客户端直接使用虚拟环境，也可以将 `command` 改为虚拟环境中 Python 解释器的绝对路径。

仓库中的 `chatmcp.yaml` 也提供了相对路径配置示例；使用相对路径时，需要确保客户端的工作目录为本项目目录。

## 工具说明

### `get_today`

获取服务器当前日期。

- **参数**：无
- **返回值**：字符串，格式为 `YYYY-MM-DD`
- **返回示例**：`2026-11-26`

核心实现：

```python
@mcp.tool()
def get_today() -> str:
    """返回当前的年月日，格式 YYYY-MM-DD。"""
    return datetime.now().strftime("%Y-%m-%d")
```

日期由 `datetime.now()` 获取，因此结果取决于运行 MCP 服务的系统本地时间和时区，而不是客户端或大语言模型的时间。

## 许可证

MIT

## 项目地址

[https://github.com/zxk0311/date-tool-mcp](https://github.com/zxk0311/date-tool-mcp)
