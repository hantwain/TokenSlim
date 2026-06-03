# TokenSlim · Make Every Token Count

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Discord](https://img.shields.io/badge/Community-Discord-5865F2)](https://discord.gg/your-link)
[![中文版](https://img.shields.io/badge/中文版-README.zh--CN.md-red)](./README.zh_CN.md)

> **Not zero token, but minimal token.**  
> TokenSlim is an open-source community and collection of techniques dedicated to **drastically reducing LLM API token consumption**.  
> We believe good AI apps should be both smart and cost-effective.

## 📌 Why TokenSlim?

| Scenario | Token Waste | Saving Potential |
|----------|-------------|------------------|
| Customer service bot | Repeating full history + system prompt every time | 30%~60% |
| Code generation | Returning excessive comments and explanations | 40%~70% |
| RAG retrieval | Returning whole chunks instead of the most relevant sentences | 50%+ |
| Chained calls | Intermediate steps outputting redundant JSON | 20%~80% |

**TokenSlim's goal**: Push token usage to the theoretical minimum while preserving output quality.

## 🚀 Core Directions (5 Technical Tracks)

1. **Prompt Compression**  
   - Semantic-preserving instruction distillation (LLM-in-the-loop)  
   - Replace natural language descriptions with templated parameters  
2. **Semantic Caching**  
   - Request-response caching based on embeddings (Sentence Transformers + similarity threshold)  
   - Dynamic TTL and cross-user reuse  
3. **Model Routing**  
   - Automatically select model based on query complexity: local small model → cheap API → powerful API  
4. **Output Slimming**  
   - Enforced schema output (JSON mode + only necessary fields)  
   - Strip redundant whitespace, newlines, and natural language explanations  
5. **Retrieval Dimension Reduction**  
   - Summarize retrieved documents via a small LLM before feeding to the main model  
   - Use embeddings + MMR (Maximal Marginal Relevance) for deduplication

## 🧰 Existing Tools (Examples / Contributions Welcome)

| Name | Function | Language |
|------|----------|----------|
| `slim-prompt` | Automatically compress prompts to a target token budget | Python |
| `semantic-cache` | Embedding cache with eviction policies | Python / Redis |
| `router-lite` | Complexity‑based model selector | Python |
| `token-budget-profiler` | Analyze your API logs to find waste | Python |

> See the `/tools` directory for more. PRs that add your own token‑saving tools are welcome.

## 📊 Benchmarks (Community Maintained)

We regularly test **compression ratio / cost savings / quality loss** on mainstream tasks.  
Latest results in [BENCHMARK.md](BENCHMARK.md):

- Short QA (HotpotQA): **54%** token reduction, ROUGE‑L drop < 2%  
- Summarization (CNN/DM): **68%** token reduction, human ratings nearly unchanged  
- Code generation (HumanEval): **41%** token reduction, pass@1 drop 3% (acceptable)

## 🧪 Quick Start (Python Example)

### Install the core compression library

```bash
pip install tokenslim
```

### Use the smart prompt compressor

```python
from tokenslim import compress_prompt

original = """You are a professional customer service assistant. Follow these steps:
Step 1: Confirm the user's question.
Step 2: Search the knowledge base for relevant terms.
Step 3: Answer in a friendly tone. User question: When will order #1234 be shipped?"""

compressed = compress_prompt(original, target_tokens=50)
print(compressed)
# Output: "Order #1234 shipping date?"  (saved ~70% of tokens)
```

### Enable semantic caching

```python
from tokenslim import SemanticCache

cache = SemanticCache(threshold=0.92)
response = cache("Why is the sky blue?", llm_call)   # first call – uses LLM
response2 = cache("Why does the sky appear blue", llm_call)  # cache hit – 0 tokens
```

For full documentation, see [docs/](docs/).

## 🤝 How to Join & Contribute

This community is not one person's project – it belongs to everyone who cares about token efficiency.

- 💬 **Discussions**: GitHub Issues / Discussions, or join our Discord for real‑time chats  
- 📝 **Share your tips**: Even non‑code articles (“How I cut token usage in half for task X”) can be PR’ed into `/awesome-tips`  
- 🔧 **Submit tools**: Any script, wrapper, or server that saves tokens can go into `/tools`  
- 🧪 **Run benchmarks**: Test on your own task and submit results to [BENCHMARK.md](BENCHMARK.md)

### Code of Conduct
We welcome constructive skepticism and method comparisons. However, we discourage suggestions like “just add more prompt” that ignore cost. **Every PR should ideally include an estimate of tokens saved.**

## 📄 License

MIT – you can freely use, modify, and commercialize, but please retain a reference to this community.

## 🌟 Finally

**TokenSlim is not anti‑AI, it’s smarter AI.**  
If you’ve ever winced at your monthly API bill, come join us in driving down token usage.

[![Star History Chart](https://api.star-history.com/svg?repos=hantwain/TokenSlim&type=Date)](https://star-history.com/#hantwain/TokenSlim&Date)

---
**Next step**: Star this repo ⭐, then head to Issues to share the token‑wasting scenario you want to solve first.

