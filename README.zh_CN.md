# TokenSlim · 让每个 token 都花在刀刃上

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Discord](https://img.shields.io/badge/Community-Discord-5865F2)](https://discord.gg/your-link)

> **不是零 token，而是最少 token。**  
> TokenSlim 是一个致力于 **极致减少 LLM API token 消耗** 的开源社区与技术方案集。  
> 我们相信：好的 AI 应用应该既聪明，又省钱。

## 📌 为什么需要 TokenSlim？

| 场景 | token 浪费现象 | 省 token 潜力 |
|------|----------------|----------------|
| 客服机器人 | 每次重复发送完整历史 + 系统提示 | 30%~60% |
| 代码生成 | 返回大量无用的注释与解释 | 40%~70% |
| RAG 检索 | 返回整段原文而非最相关句子 | 50%+ |
| 链式调用 | 中间步骤输出冗余 JSON | 20%~80% |

**TokenSlim 的目标**：在保证输出质量的前提下，把 token 用量压到理论最低值。

## 🚀 核心方向（社区目前聚焦的 5 个技术赛道）

1. **Prompt 压缩**  
   - 保留语义的指令蒸馏（LLM-in-the-loop 压缩）  
   - 模板化参数取代自然语言描述  
2. **智能缓存**  
   - 语义级别的请求-响应缓存（Sentence Transformers + 相似度阈值）  
   - 动态 TTL & 跨用户复用  
3. **模型路由**  
   - 根据 query 复杂度自动选择：本地小模型 → 便宜 API → 强大 API  
4. **输出瘦身**  
   - 强制 schema 输出（JSON mode + 只返回必要字段）  
   - 去冗余换行 / 空格 / 自然语言解释  
5. **检索降维**  
   - 对检索到的文档先做 LLM 摘要再喂给主模型  
   - 用嵌入 + 最大边际相关性 (MMR) 去重

## 🧰 已有工具（示例 / 可贡献）

| 名称 | 功能 | 语言 |
|------|------|------|
| `slim-prompt` | 自动压缩 prompt 至指定 token 预算 | Python |
| `semantic-cache` | 嵌入缓存 + 过期策略 | Python / Redis |
| `router-lite` | 基于复杂度预测的模型选择器 | Python |
| `token-budget-profiler` | 分析你的 API 日志，找到浪费点 | Python |

> 更多工具请查看 `/tools` 目录，欢迎 PR 加入你自己的省 token 工具。

## 📊 效果基准（社区维护）

我们会定期测试主流任务上的 **压缩率 / 成本节省 / 质量损失**。  
最新结果见 [BENCHMARK.md](BENCHMARK.md)：

- 短问答（HotpotQA）: token 减少 **54%**，ROUGE-L 下降 < 2%  
- 摘要任务（CNN/DM）: token 减少 **68%**，人工评分基本持平  
- 代码生成（HumanEval）: token 减少 **41%**，pass@1 下降 3% (可接受)

## 🧪 快速开始（以 Python 示例）

### 安装核心压缩库

```bash
pip install tokenslim
```

### 使用智能 prompt 压缩器

```python
from tokenslim import compress_prompt

original = """你是一个专业的客服助手。请按照以下步骤回答用户问题：
第一步，确认用户问题；
第二步，检索知识库中的相关条款；
第三步，用友好亲切的语气回答。用户的问题是：订单号 #1234 什么时候发货？"""

compressed = compress_prompt(original, target_tokens=50)
print(compressed)
# 输出: "订单 #1234 何时发货？"  （省掉了 70% 的 tokens）
```

### 启用语义缓存

```python
from tokenslim import SemanticCache

cache = SemanticCache(threshold=0.92)
response = cache("为什么天空是蓝色的？", llm_call)  # 第一次调用 LLM
response2 = cache("天空为何呈现蓝色", llm_call)      # 命中缓存，0 token
```

更详细文档请查看 [docs/](docs/)。

## 🤝 如何加入社区 & 贡献

这个社区不是一个人的项目，而是所有在乎 token 效率的人的共同作品。

- 💬 **讨论**：GitHub Issues / Discussions，或者加入 Discord 实时聊方案  
- 📝 **分享你的技巧**：即使不是代码，一篇“如何把某任务 token 减半”的文章也可以 PR 到 `/awesome-tips`  
- 🔧 **提交工具**：任何能节省 token 的脚本、wrapper、server 都可以放进 `/tools`  
- 🧪 **参与评测**：在 [BENCHMARK.md](BENCHMARK.md) 下面跑你的任务并提交结果  

### 行为准则
我们欢迎所有建设性的质疑和方法比较，但杜绝“加更多 prompt 试试”这类不关心成本的提议。**每个 PR 最好附带 token 节省的估算**。

## 📄 许可证

MIT – 你可以随意使用、修改、商用，但请保留对本社区的引用。

## 🌟 最后

**TokenSlim 不是反 AI，而是更聪明的 AI。**  
如果你也曾为月底的 API 账单皱眉，欢迎来这里一起把 token 用量打下来。

[![Star History Chart](https://api.star-history.com/svg?repos=hantwain/TokenSlim&type=Date)](https://star-history.com/#hantwain/TokenSlim&Date)

---
**下一步**：给这个仓库点个 Star ⭐，然后来 Issue 里聊聊你最想解决的 token 浪费场景。
