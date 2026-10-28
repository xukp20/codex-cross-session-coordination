<h1 align="center">面向 Codex 的跨会话协作</h1>

<p align="center">
  <strong>让相关信息到达能够采取行动的负责人。</strong>
</p>

<p align="center">
  <a href="skills/cross-session-coordination/SKILL.md">
    <img alt="Codex Skill" src="https://img.shields.io/badge/Codex-Skill-2563eb?style=flat-square">
  </a>
  <a href="LICENSE">
    <img alt="MIT License" src="https://img.shields.io/badge/license-MIT-0f8f88?style=flat-square">
  </a>
</p>

<p align="center">
  <a href="#适用场景">适用场景</a>
  &middot;
  <a href="#安装">安装</a>
  &middot;
  <a href="README.md">English</a>
  &middot;
  <a href="skills/cross-session-coordination/SKILL.md">Skill 原文</a>
</p>

本仓库提供 `cross-session-coordination` Codex skill，用于在各有负责人的独立会话之间传递发现、解决依赖并明确移交工作。协调会话可以处理共同决策与冲突，各负责人仍然直接沟通。

## 适用场景

- 生产者修改了另一个会话依赖的行为。
- 消费者缺少由其他负责人提供的接口或依赖。
- 检查器发现语义差异，或生产者发现检查覆盖不足。
- 一个会话找到了解法，能解除另一个会话正在面对的阻塞。
- 当前负责人需要把未完成的工作交给另一位负责人。
- 阶段工作完成，需要决定下一个目标。

消息用几句可读的话说明问题、影响、需要谁做什么以及证据入口。详细日志和技术标识在不影响当前决策时保留在链接指向的证据中。

## 协作原则

| 原则 | 期望行为 |
| --- | --- |
| 各条工作线保留负责人 | 工作责任留在本地；移交需要接任者确认接受 |
| 向需要行动的人沟通 | 直接相关的负责人互通信息，不例行广播 |
| 区分确认的含义 | 消息送达、接受责任、交付工作与验证结果各有含义 |
| 适度跟进 | 避免重复催办和确认循环；无法解决的边界问题才升级 |
| 证据可以找回 | 复用已有会话和项目记录，不强制存储格式 |

## 安装

示例使用本机支持的 `~/.codex/skills`（或 `CODEX_HOME`）。若你的客户端使用其他用户技能目录，请相应调整安装目标。

```bash
git clone https://github.com/xukp20/codex-cross-session-coordination.git
cd codex-cross-session-coordination
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cross_session_skill_target="${CODEX_HOME:-$HOME/.codex}/skills/cross-session-coordination"
if [ -e "$cross_session_skill_target" ] || [ -L "$cross_session_skill_target" ]; then
  echo "Destination already exists; inspect it before changing the installation."
else
  ln -s "$PWD/skills/cross-session-coordination" "$cross_session_skill_target"
fi
```

安装后重新加载 Codex，以发现此 skill。符号链接跟随当前仓库目录；后续可在仓库中运行 `git pull --ff-only` 更新。安装示例会保留已经存在的目标。

## 使用

```text
使用 $cross-session-coordination 梳理这几个会话之间的依赖。
明确需要哪位负责人采取行动，并准备一条包含影响和证据链接的短消息。
发送前由我审阅。
```

```text
对这几条相关工作线使用 $cross-session-coordination。你可以向它们现有的
负责人发送依赖请求，以及会改变对方工作的发现。共同决策升级给我，
日常进度留在各自会话。
```

此 skill 对相关任务启用正常自动发现，也支持点名调用。选中 skill 不等于授权发送消息、创建任务或修改另一个仓库；具体可执行动作由用户已经给予的授权决定。

## 与单会话委派的区别

单会话委派由父会话向内部 subagent 分配有界工作。此 skill 面向独立会话：每个负责人保留自己的目标、上下文和权限。它不选择模型、不启动工作者、不管理内部 agent 树，也不要求所有发现都经过顶层协调者。

## 边界

此 skill 提供沟通原则，不是邮箱服务或任务管理器。它不要求特定 App 或消息 API，也不提供后台监听、送达保证、计时器或自动唤醒。存在合适且已获授权的渠道时使用该渠道；否则准备移交内容并说明哪些尚未发送。

它不规定共享目录、数据库、事件 schema、状态流程或全局待办表。记录和证据仍遵循现有项目约定。

## 验证

使用 Codex `skill-creator` 系统 skill 附带的校验器：

```bash
python /path/to/skill-creator/scripts/quick_validate.py \
  skills/cross-session-coordination
```

校验器检查 skill 结构与元数据，行为审阅需另行完成。

## 来源与参考

本 skill 为原创编写。以下一手来源帮助明确了沟通边界，均不是运行依赖：

- [MCP Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust)：持久身份、定向消息、确认与可找回的证据。
- [Claude Code 跨会话通信](https://code.claude.com/docs/en/cross-session-messaging)：独立会话通信与接收方权限边界。
- [OpenAI Agents SDK handoffs](https://github.com/openai/openai-agents-python/blob/main/docs/handoffs.md)：明确移交目标与控制传递上下文。

采用 [MIT 许可证](LICENSE)。
