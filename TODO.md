# PPTFlux · 已知改进点

> 本项目已停在 **v1.0**(2026-04 Round 4 收官状态),作为超级个体作品集保留。
> 后续不计划主动开发,但欢迎 issue 讨论。
>
> 以下为已知未完成项,作为技术档案保留,**不承诺修复**。

---

## 内容生成

- [ ] **Agent 4 prompt 硬约束**:让 Agent 4 严格遵守 variant 字段并停止"自由发挥"装饰元素。当前依赖 `sanitizeAgentSlide` 后处理兜底,治标不治本
- [ ] **Agent 3 内容连贯性约束**:加"编号概念首次出现必须定义"硬规则(防止只讲阶段 2/3 不讲阶段 1)
- [ ] **本地兜底排版优雅降级**:`buildLocalSlide` 在 bullet 内容过短时所有 variant 都需要 graceful fallback,目前 P4/P8/P11 这种 Agent 4 失败页本地兜底排版稀疏
- [ ] **本地兜底触发频率监控**:收集真 API 数据决定是否要增强 `retrySinglePage`(目前 20s 超时 + 4000 tokens 非流式可能不够)
- [ ] **`pickProcessVariant` 关键词表扩展**:收集更多真实场景触发词

## 视觉系统

- [ ] **扩展到 12 种风格**:已有 6 种(Bold Signal / Dark Botanical / Creative Voltage / Swiss Modern / Electric Studio / Notebook Tabs),计划补 Neon Cyber / Paper & Ink / Pastel Geometry 等
- [ ] **process-v3 形态扩展**:stair / circular 形态的 6 步压力测试
- [ ] **Notebook Tabs 署名融入纸卡**:把 `.slide-signature` 从卡外 margin 移入 `.paper-card` 内部紧挨 `tab-strip` 上方,作为"笔记本纸张水印"的设计语言一部分
- [ ] **Bold Signal 环境粒子(可选)**:当前 `initParticleTrail` 是鼠标轨迹粒子,鼠标不动就无粒子。待评估是否需要加一套不依赖鼠标的环境粒子/星光(类似 Dark Botanical `.slide-orb` 静态浮光)

## 交互层

- [ ] **nav-dots tooltip + 点击跳页**:对标 Zara frontend-slides 的右侧导航
- [ ] **insight 权重分布验证**:v1/v2/v3 = 50/30/20 在长 PPT 真实分布需要验证

## 工具链

- [ ] **`SHARED_CSS` 抽取**:把 Round 2/3/4 内联的 variant 样式抽到 `SHARED_CSS` 做统一管理
- [ ] **`verify-variants.js` 分类器重写**:从 eyebrow 字符串匹配改为基于 section innerHTML 结构特征匹配(`.insight-wrap` / `.hook-stat` / `.data-cards` / 双色背景 flex / SVG 弧形 `<path>` 等),脱离文本依赖
- [ ] **CSS 孤儿规则清理**:Round 4 删除后留下的无 HTML 引用规则——`.hook-eyebrow` / `.data-eyebrow` / `.insight-eyebrow` / `.action-eyebrow` / `.slide-bg-num`(`buildTitleSlide` 的 `✦` 装饰仍在用最后一条)
- [ ] **`retrySinglePage` 超时**:可能需要从 20s 调到 25s(Opus 4.7 边界)

## UI / 品牌

- [ ] **UI brand 锁头改造**:`index.html:484-486` 顶部 `PPT·FLUX / Studio` 三层 typographic 处理需要随品牌改名(PPTFlux)一起重做视觉。改造完成后 `.brand-serif` / `.brand-sep` / `.brand-sub` 三条 CSS 规则可能全变孤儿
- [ ] **UI 主界面整体视觉优化**(详细方案待整体规划):主色 / 主背景 / 风格卡片预览缩略图 / 右侧面板 living preview / 衬线字体 / 输入框占位符等
