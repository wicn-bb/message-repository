# 大型代码库中的 Agent：广泛探索，维护者负责做减法

- 收录日期：2026-10-23
- 原文日期：2026-10-13
- 作者：George Weale、Bo Yang、Andrew Biernat
- 来源：Google Developers Blog
- 原文：[Optimizing a 200,000-line Python codebase overnight with Gemini 4 Argon](https://developers.googleblog.com/en/optimizing-a-200000-line-python-codebase-overnight-with-gemini-4-argon/)
- 主题：代码库优化、AI 重构、性能工程、Agent Skills、人工评审

## 核心观点

Gemini 在约 20.5 万行、近 800 个文件和约 1.7 万个测试的 ADK Python 仓库中运行 11 小时，提出六项优化。人工评审最终只接受两项：长会话的内存会话准备开销下降约 40%，`google.adk` 冷导入时间下降 21%。

另外四项分别因陈旧缓存风险、依赖私有 API、增加核心路径复杂度或只改善合成基准而被拒绝。案例体现的合理分工是：Agent 扩大探索范围，维护者删除不可靠或收益不真实的方案。

## 值得保留的工程启示

1. 让 Agent 在修改前建立基线，并为每项改动提供前后数据。
2. 将维护者约定拆成 `.agents/skills` 和按需读取的参考文件，避免把全部规则塞入常驻上下文。
3. 要求完整测试、独立保留集 benchmark 和明确的公开 API 不变约束。
4. 性能改进不能只看合成基准，还应检查真实调用路径、生命周期和维护成本。
5. Agent 的探索结果是候选集合，不是可以直接合并的最终答案。

## 证据与限制

文章公开了代码库规模、运行时长、测试数量、接受与拒绝的方案及拒绝理由，比只报告成功结果更可信。但它同时承担 Gemini 4 Argon 的产品展示功能，实验由 Google 自己设计和报告；40% 的收益只适用于内存会话服务，21% 是导入耗时，并不等于端到端 Agent 延迟同比下降。

## 收录理由

它提供了一个比“AI 写了多少代码”更成熟的生产案例：先建立基准，让 Agent 广泛搜索，再通过完整测试、独立指标和维护者判断缩小结果集合。
