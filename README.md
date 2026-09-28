[Official Website](https://spacebunny.app) · [Live Demo on Hugging Face](https://huggingface.co/spaces/space-bunny/space-bunny-free)

---

<p align="center">
  <a href="https://spacebunny.app">
    <img src="https://spacebunny.app/og.png" alt="Space Bunny — 1M Context Multimodal AI Playground" width="100%" />
  </a>
</p>

<h1 align="center">Space Bunny</h1>

<p align="center"><strong>A million-token context for curious minds.</strong></p>

<p align="center">
  <a href="https://spacebunny.app"><strong>Website</strong></a> ·
  <a href="https://spacebunny.app/playground"><strong>Playground</strong></a> ·
  <a href="https://spacebunny.app/docs"><strong>Documentation</strong></a> ·
  <a href="mailto:support@spacebunny.app"><strong>Support</strong></a>
</p>

<p align="center"><a href="#english">English</a> · <a href="#简体中文">简体中文</a></p>

## English

### Long-context reasoning for real work

Welcome to the GitHub home of **Space Bunny**, an AI playground and developer API for **Space Bunny Alpha**, an anonymous preview model built for long-context reasoning, multimodal understanding, structured output, and agent workflows.

Space Bunny gives one request a very large working memory. With a one-million-token context window and support for up to 524,288 completion tokens, it can work across long documents, source repositories, screenshots, diagrams, video references, and extended conversations without forcing the task into many small fragments.

The product is designed to be useful before and after integration: test a real prompt in the browser playground, tune reasoning depth, then send the same familiar chat-completion request through the API from your own server.

### What Space Bunny provides

- **1M-token context window** — reason across large documents, codebases, research notes, and long-running sessions.
- **Multimodal input** — send text, public image URLs, base64 images, and compatible video URLs when the active provider route supports them.
- **Five reasoning levels** — choose `low`, `medium`, `high`, `xhigh`, or `max` to balance latency and depth.
- **Text and JSON output** — use ordinary assistant text or request a JSON object that your application can validate and consume.
- **Tool calling** — describe approved functions and let the model request them while your application keeps control of permissions and side effects.
- **OpenAI-compatible API** — use a familiar chat-completions shape with server-side API keys and standard client patterns.
- **Browser playground** — try prompts, multimodal context, reasoning settings, and output formats before committing to an integration.

### Model snapshot

| Capability | Space Bunny Alpha |
| --- | --- |
| Model ID | `stealth/space-bunny-alpha` |
| Context window | 1,000,000 tokens |
| Maximum completion | Up to 524,288 tokens |
| Input | Text, image, and video |
| Output | Text and JSON object responses |
| Reasoning | `low`, `medium`, `high`, `xhigh`, `max` |
| Tools | Function calling with application-side validation |
| Availability | Anonymous preview model served through Space Bunny |

### From a first prompt to a production call

Space Bunny is organized around a simple progression:

1. **Explore** — Open the [Playground](https://spacebunny.app/playground) and test a question, code review, long document, image, or compatible video URL.
2. **Choose depth** — Start with low reasoning effort for routine work and increase it when a task needs more analysis.
3. **Create a key** — Generate a Space Bunny API key and keep it in a server-side environment variable or secret manager.
4. **Send the request** — Call the OpenAI-compatible endpoint with the `stealth/space-bunny-alpha` model ID.
5. **Validate the result** — Parse text or JSON, inspect usage, and validate every tool call before any external side effect.

### Quickstart

Configure the endpoint and model in your server environment:

```bash
export OPENAI_BASE_URL="https://spacebunny.app/api/v1"
export SPACE_BUNNY_API_KEY="sk_your_key_here"
export OPENAI_MODEL="stealth/space-bunny-alpha"
```

Send a standard chat-completions request:

```bash
curl https://spacebunny.app/api/v1/chat/completions \\
  -H "Authorization: Bearer $SPACE_BUNNY_API_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "model": "stealth/space-bunny-alpha",
    "messages": [
      { "role": "user", "content": "Review this API design and identify the safest improvement." }
    ],
    "reasoning": { "effort": "low" },
    "max_completion_tokens": 2048
  }'
```

Keep API keys on the server. Never expose a real key in browser code, public prompts, source control, browser storage, or agent transcripts.

### Multimodal requests

A user message can contain a text instruction followed by image or video URL parts. This makes it possible to ask Space Bunny to review an interface, inspect a diagram, explain a screenshot, or analyze a compatible video alongside a written question.

```json
{
  "model": "stealth/space-bunny-alpha",
  "messages": [
    {
      "role": "user",
      "content": [
        { "type": "text", "text": "Review this interface hierarchy and identify the three highest-impact issues." },
        { "type": "image_url", "image_url": { "url": "https://example.com/interface.png" } }
      ]
    }
  ],
  "reasoning": { "effort": "medium" }
}
```

Supported image formats and video compatibility depend on the active provider route. Verify the exact route before using visual inputs in production.

### Reasoning, JSON, and tools

#### Tune reasoning to the task

Use `low` for routine questions and fast iteration. Move to `medium`, `high`, `xhigh`, or `max` when the task benefits from deeper analysis, such as migration planning, incident review, codebase reasoning, or risk assessment. Space Bunny Alpha currently defaults to low when no reasoning effort is supplied.

#### Request structured output

When the next step is handled by code, ask for one valid JSON object and set `response_format` to `json_object`. The response content is returned as a string, so parse and validate it in your application.

```json
{
  "model": "stealth/space-bunny-alpha",
  "messages": [
    { "role": "user", "content": "Return a JSON launch plan with summary, risks, steps, and verification." }
  ],
  "response_format": { "type": "json_object" }
}
```

#### Connect approved tools

Define functions in the `tools` array and let the model request a function when it needs external information. Tool names, arguments, permissions, and side effects must be validated by your application before execution. The model can propose or request an action; your product remains responsible for authorization and approval.

### Response handling

A successful non-streaming response normally includes:

- `choices[0].message.content` for the assistant's text or JSON string;
- `choices[0].message.tool_calls` when the model requests a function;
- `usage.prompt_tokens`, `usage.completion_tokens`, and `usage.total_tokens` for token accounting;
- `model`, `finish_reason`, and other standard response metadata;
- optional elapsed-time information for Playground responses.

Reasoning tokens may count toward completion usage even when internal reasoning is not shown in the final answer. Treat generated content and tool calls as model output that requires normal application validation.

### Practical use cases

- **Codebase review** — provide architecture notes, source files, and runtime evidence in one context so the model can connect issues across a repository.
- **Incident analysis** — summarize logs, review a production failure, separate evidence from assumptions, and propose a safe recovery plan.
- **Research and document analysis** — compare long documents, extract decisions, and return a structured brief.
- **Visual understanding** — inspect screenshots, diagrams, interface hierarchy, and other visual context.
- **Agent workflows** — combine reasoning with a controlled set of read-only or approved business functions.
- **Structured planning** — return JSON plans that can be validated and passed to another workflow.
- **Long-form conversations** — keep more of the relevant context available during extended analysis.

### Pricing and access

The Playground is free to try. When a workflow is ready for more usage, Space Bunny offers permanent, one-time credit packs with no subscription, no auto-renewal, and no credit expiry.

| Plan | Price | Credits | Intended use |
| --- | ---: | ---: | --- |
| Starter | $9.90 | 100,000 permanent credits | Experiments, prototypes, and first integrations |
| Pro | $99 | 1,000,000 permanent credits | Regular long-context, multimodal, and tool workflows |
| Enterprise | $999 | 11,000,000 permanent credits | Teams and higher-volume workloads |

See the [pricing page](https://spacebunny.app/pricing) for current workspace, concurrency, speed, API, and support details.

### Built for developers, with clear boundaries

Space Bunny keeps the application in charge of important decisions. API keys stay server-side, tools are approved by the integrating application, and external side effects should be validated before execution. The model is anonymous and in preview; its architecture, parameter count, training details, and knowledge cutoff are not disclosed.

Space Bunny is independently operated and is not affiliated with, operated by, or endorsed by the undisclosed model provider. Model behavior, output quality, latency, input compatibility, and availability can change as the preview evolves. Validate important results and keep human review for high-impact decisions.

### Explore Space Bunny

- [Website](https://spacebunny.app) — Learn about the product and its capabilities.
- [Playground](https://spacebunny.app/playground) — Run a real prompt and test model settings.
- [Documentation](https://spacebunny.app/docs) — Read the API, multimodal, JSON, tool-calling, agent, and error-handling guides.
- [Pricing](https://spacebunny.app/pricing) — Compare permanent credit packs and usage limits.
- [Blog](https://spacebunny.app/blog) — Read model notes, benchmarks, API guidance, and practical use cases.
- [About](https://spacebunny.app/about) — Learn more about Space Bunny and its developer focus.
- [Support](mailto:support@spacebunny.app) — Contact the team for product, technical, or partnership questions.

Start with a real prompt at [spacebunny.app](https://spacebunny.app), then connect the same workflow to your product when the use case is clear.

---

## 简体中文

### 面向真实工作的超长上下文推理

欢迎来到 **Space Bunny** 的 GitHub 组织主页。Space Bunny 是面向开发者的 AI Playground 与 API，围绕 **Space Bunny Alpha** 这一匿名预览模型构建，支持超长上下文推理、多模态理解、结构化输出和 Agent 工作流。

Space Bunny 让一次请求拥有更大的工作记忆：模型提供一百万 Token 上下文窗口，最多支持 524,288 个补全 Token，可以在同一次任务中处理长文档、源代码仓库、截图、图表、视频引用和长时间对话，减少把复杂任务拆成许多小片段的需要。

产品同时覆盖“先体验、后接入”的完整路径：先在浏览器 Playground 中测试真实问题，调整推理深度，再从自己的服务器发送熟悉的 Chat Completions 请求。

### Space Bunny 提供什么

- **一百万 Token 上下文窗口**：适合长文档、代码库、研究资料和长期会话。
- **多模态输入**：支持文本、公开图片 URL、Base64 图片，以及在当前模型路由支持时使用视频 URL。
- **五档推理强度**：可选择 `low`、`medium`、`high`、`xhigh` 或 `max`，在速度和深度之间进行平衡。
- **文本和 JSON 输出**：可以返回普通回答，也可以请求由应用校验和消费的 JSON 对象。
- **工具调用**：描述经过批准的函数，让模型在需要时请求工具，同时由应用掌控权限和副作用。
- **OpenAI 兼容 API**：使用熟悉的 Chat Completions 请求结构和服务端 API Key。
- **浏览器 Playground**：在正式集成前测试提示词、多模态上下文、推理设置和输出格式。

### 模型概览

| 能力 | Space Bunny Alpha |
| --- | --- |
| 模型 ID | `stealth/space-bunny-alpha` |
| 上下文窗口 | 1,000,000 Token |
| 最大补全长度 | 最多 524,288 Token |
| 输入 | 文本、图片和视频 |
| 输出 | 文本和 JSON 对象 |
| 推理强度 | `low`、`medium`、`high`、`xhigh`、`max` |
| 工具能力 | 函数调用，由应用侧负责校验 |
| 当前状态 | 通过 Space Bunny 提供的匿名预览模型 |

### 从第一次提问到生产调用

Space Bunny 的使用流程可以概括为：

1. **探索**：打开 [Playground](https://spacebunny.app/playground)，测试问题、代码审查、长文档、图片或兼容的视频 URL。
2. **选择深度**：日常任务从低推理强度开始，需要更深入分析时再提高等级。
3. **创建 API Key**：生成 Space Bunny API Key，并保存到服务端环境变量或密钥管理工具中。
4. **发送请求**：使用 `stealth/space-bunny-alpha` 模型 ID 调用 OpenAI 兼容 API。
5. **校验结果**：处理文本或 JSON，查看 Token 用量，并在执行外部副作用前校验每一次工具调用。

### 快速开始

在服务端环境中配置 API 地址、模型和密钥：

```bash
export OPENAI_BASE_URL="https://spacebunny.app/api/v1"
export SPACE_BUNNY_API_KEY="sk_your_key_here"
export OPENAI_MODEL="stealth/space-bunny-alpha"
```

发送标准 Chat Completions 请求：

```bash
curl https://spacebunny.app/api/v1/chat/completions \\
  -H "Authorization: Bearer $SPACE_BUNNY_API_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "model": "stealth/space-bunny-alpha",
    "messages": [
      { "role": "user", "content": "Review this API design and identify the safest improvement." }
    ],
    "reasoning": { "effort": "low" },
    "max_completion_tokens": 2048
  }'
```

API Key 必须保存在服务端。不要把真实密钥暴露在浏览器代码、公开提示词、源码仓库、浏览器存储或 Agent 对话记录中。

### 多模态请求

用户消息可以由文本指令和图片或视频 URL 组成。这样可以让 Space Bunny 分析界面、检查图表、解释截图，或结合文字问题分析兼容的视频。

```json
{
  "model": "stealth/space-bunny-alpha",
  "messages": [
    {
      "role": "user",
      "content": [
        { "type": "text", "text": "Review this interface hierarchy and identify the three highest-impact issues." },
        { "type": "image_url", "image_url": { "url": "https://example.com/interface.png" } }
      ]
    }
  ],
  "reasoning": { "effort": "medium" }
}
```

图片格式和视频兼容性取决于当前使用的模型路由。正式使用视觉输入前，请先确认具体路由的支持范围。

### 推理、JSON 与工具调用

#### 根据任务调整推理强度

日常问题和快速迭代可以使用 `low`。迁移规划、事故复盘、代码库推理和风险评估等需要深入分析的任务，可以提高到 `medium`、`high`、`xhigh` 或 `max`。如果 API 请求没有传入推理强度，Space Bunny Alpha 当前默认使用 `low`。

#### 请求结构化输出

当下一步由代码处理时，可以要求模型返回一个合法 JSON 对象，并设置 `response_format` 为 `json_object`。响应内容以字符串形式返回，应用需要自行解析和校验。

```json
{
  "model": "stealth/space-bunny-alpha",
  "messages": [
    { "role": "user", "content": "Return a JSON launch plan with summary, risks, steps, and verification." }
  ],
  "response_format": { "type": "json_object" }
}
```

#### 连接经过批准的工具

在 `tools` 数组中定义函数，模型需要外部信息时可以请求调用。应用必须在执行前校验工具名称、参数、权限和副作用。模型可以提出或请求动作，但授权与审批仍由接入产品负责。

### 响应处理

一次成功的非流式响应通常包含：

- `choices[0].message.content`：助手返回的文本或 JSON 字符串；
- `choices[0].message.tool_calls`：模型请求函数时的工具调用信息；
- `usage.prompt_tokens`、`usage.completion_tokens` 和 `usage.total_tokens`：Token 用量；
- `model`、`finish_reason` 等标准响应元数据；
- Playground 响应中可能包含端到端耗时信息。

即使内部推理过程不会显示在最终回答中，推理 Token 也可能计入补全用量。应用应像处理其他模型输出一样，对生成内容和工具调用进行校验。

### 实用场景

- **代码库审查**：在同一个上下文中提供架构说明、源文件和运行证据，让模型关联整个仓库中的问题。
- **事故分析**：汇总日志、复盘生产故障、区分证据和假设，并提出安全的恢复方案。
- **研究与文档分析**：比较长文档、提取决策，并返回结构化摘要。
- **视觉理解**：分析截图、图表、界面层级和其他视觉上下文。
- **Agent 工作流**：将推理能力与一组受控的只读或经过批准的业务函数结合。
- **结构化规划**：返回可以由代码校验并交给下一个工作流的 JSON 计划。
- **长对话**：在持续分析过程中保留更多相关上下文。

### 价格与使用方式

Playground 可以免费试用。需要更多用量时，Space Bunny 提供永久有效的一次性积分包：没有订阅、没有自动续费，积分不会过期。

| 方案 | 价格 | 积分 | 适用场景 |
| --- | ---: | ---: | --- |
| Starter | $9.90 | 100,000 永久积分 | 实验、原型和首次集成 |
| Pro | $99 | 1,000,000 永久积分 | 日常长上下文、多模态和工具工作流 |
| Enterprise | $999 | 11,000,000 永久积分 | 团队和更高用量场景 |

访问[价格页面](https://spacebunny.app/pricing)查看当前工作区、并发、速度、API 和支持详情。

### 面向开发者，同时保持清晰边界

Space Bunny 将重要决策留在应用侧：API Key 保存在服务端，工具由接入应用批准，外部副作用必须在执行前完成校验。该模型是匿名预览模型，架构、参数量、训练细节和知识截止时间尚未公开。

Space Bunny 独立运营，与未公开的模型提供方没有隶属、运营或背书关系。随着预览版本演进，模型行为、输出质量、延迟、输入兼容性和可用性都可能变化。重要结果应进行验证，高影响决策应保留人工审核。

### 探索 Space Bunny

- [官方网站](https://spacebunny.app) — 了解产品与能力。
- [Playground](https://spacebunny.app/playground) — 运行真实提示词并测试模型设置。
- [开发者文档](https://spacebunny.app/docs) — 查看 API、多模态、JSON、工具调用、Agent 和错误处理指南。
- [价格方案](https://spacebunny.app/pricing) — 比较永久积分包和使用限制。
- [官方博客](https://spacebunny.app/blog) — 阅读模型说明、基准测试、API 指南和实践场景。
- [关于 Space Bunny](https://spacebunny.app/about) — 了解产品定位和开发者方向。
- [联系支持](mailto:support@spacebunny.app) — 咨询产品、技术或合作问题。

欢迎先在 [spacebunny.app](https://spacebunny.app) 运行一个真实提示词，确认场景后，再将相同工作流接入你的产品。