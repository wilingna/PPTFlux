# Changelog

## v1.0 — 2026-04-25

**Round 4 · variant 视觉统一 + PPTFlux 品牌收敛** · [`3edbb1b`](https://github.com/wilingna/pptflux/commit/3edbb1b)

把 18 个 variant 的差异从「内容排布 + 装饰元素」收敛到「内容排布」单一维度,统一品牌署名和页码格式。真 API 验收暴露 Agent 4 自由发挥的装饰不一致,通过 post-processing 层兜底解决。

- 页码格式统一为 `NN/MM`(无空格)
- 18 个 variant 的左上 eyebrow 全部删除(共 16 处实际渲染 + 1 hookV1 例外)
- Bold Signal 右下 `.slide-bg-num` 超大水印删除
- 全站隐蔽署名 `.slide-signature`(Notebook Tabs 左下避让 + 纸色对比)
- 品牌名从 `Made with PPT-FLUX Studio` 收敛为 `PPTFlux`(11 处替换)
- 新增 `sanitizeAgentSlide` Agent 4 输出后处理(强制标准化页码 + 署名,DOM 操作幂等)
- 主 pipeline 和 reRenderOnly 两条路径都接入 sanitize,保证全站统一

---

## v0.3 — 2026-04-23

**Round 3 · slot-map 寻址 + 验证器 + 标题页清爽** · [`3735230`](https://github.com/wilingna/pptflux/commit/3735230)

修真 API 漏页根因。Agent 4 返回空骨架时 first-N 假设导致页码错位(P3 位置显示 "05/14"),换成 slot-map 内容寻址。

- 新增 `isValidSlideHtml` 内容长度 + 页码交叉验证(100 字符阈值)
- 用 `extractClaimedAbsNum` 从 section 自述里提取页码,替换"first-N"假设
- `retrySinglePage` 拒绝空骨架 section
- `processV3Horizontal` 4 节点 nowrap 强约束(`flex:1 1 0` + `min-width:0` + `flex-wrap:nowrap`)
- 标题页模板重写:删除 `PRESENTATION · 高管/CEO` eyebrow + `14 slides` 元信息,改为居中大号标题 + 右下隐蔽品牌

---

## v0.2 — 2026-04-22

**Round 2 · buildLocalSlide variant 重构 + processV3 智能派发** · [`cb31c83`](https://github.com/wilingna/pptflux/commit/cb31c83)

把 6 个 layout 各拆出 3 个 variant,共 18 个本地兜底渲染器。processV3 从"2x2 编号网格"重新设计为真正的流程图。

- 6 layout × 3 variant = 18 个本地渲染器(hook / data / process / compare / insight / action)
- `pickVariant` 按页位 + 风格做轮转,保证相邻页排版差异
- `processV3` 升级为智能派发器,按内容语义在 3 形态间切换:
  - 横向流程(默认):圆圈 + 横向箭头 + 标题描述
  - 阶梯进化(`进化/阶段` 关键词):缩进 + opacity 渐变
  - 环形循环(`循环/闭环/迭代` 关键词):SVG 弧形箭头 + 中心 ↻ 标识
- 步数 3-6 自适应(圆圈尺寸 80→72→64→56)
- < 3 步回落 v1,> 6 步回落 v2

---

## v0.1 — 2026-04-21

**Round 1 · 漏页基础修复 + 单页补救** · [`c26212f`](https://github.com/wilingna/pptflux/commit/c26212f)

修一份 14 页 deck 经常缺 2-3 页的根本性 bug。Agent 4 批次返回不全时,新增 per-page recovery 路径。

- 批次返回页数不足时,缺失页走 `retrySinglePage` 单独 API 调用
- 单页补救失败时,本地 `buildLocalSlide` 兜底
- 最终页数校验,允许少 / 多返回的容错处理
- 完整页码闭环检查

---

## 早期(pre-Round 1)

- [`4b1fc9a`](https://github.com/wilingna/pptflux/commit/4b1fc9a) — PPTX 解析、风格预览、演讲者备注、图片嵌入、PDF 导出、键盘快捷键
- [`6811b33`](https://github.com/wilingna/pptflux/commit/6811b33) — 基线 v6 部署(粒子动效、4 Agent 流水线、6 风格框架)
