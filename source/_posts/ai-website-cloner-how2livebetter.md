---
title: 用 AI Coding Agent 像素级复刻一个网站：how2livebetter.net 实录
date: 2026-10-13 09:30:00
tags:
  - 网站复刻
  - AI Agent
  - Next.js
  - 前端工程
categories:
  - 技术实践
---

最近用开源项目 [ai-website-cloner-template](https://github.com/JCodesMore/ai-website-cloner-template)（MIT，⭐36.6K，Next.js + Tailwind）完整复刻了一个中文内容站 [how2livebetter.net](https://how2livebetter.net/)（「高性价比人生指南」）。这篇记录整个过程、验证数据和三个值得记下的坑。

## 工具是什么

ai-website-cloner-template 的思路是：给 AI coding agent 一个 URL，它把目标网站重建成一个干净的 Next.js 代码库。官方推荐 Claude Code + Opus，也支持 Codex、Cursor、OpenCode，核心工作流就一条命令：

```text
/clone-website https://example.com
```

我的 fork 在 [Hua2026-ai/ai-website-cloner-template](https://github.com/Hua2026-ai/ai-website-cloner-template)，复刻成果已落在 `src/components/sites/how2livebetter/` 下，共 13 个组件（header、hero、stages、moments、editorial、about、chapters、colophon、footer、consent 等），完整对应原站结构。

## 验证：不只是"看起来像"

复刻网站最怕自欺欺人。这次的验收标准全部量化：

| 验收项 | 结果 |
|---|---|
| `npm run check`（lint + typecheck + build） | 全绿，3 个静态路由 |
| 像素 diff（桌面 / 桌面暗色） | 0.0120% / 0.0128% |
| 像素 diff（移动 / 移动暗色） | 0.0301% / 0.0321% |
| 页高一致性 | 桌面 3733px、移动 6841px，与原站完全一致 |
| 文本节点 | 243/243 位置、宽度、字体一致 |
| 字体文件 | woff2 SHA-256 逐字节一致 |
| 交互冒烟测试 | 8/8 通过（consent、主题切换含 meta theme-color、burger 菜单、无横向溢出、无 JS 错误） |

残留的 diff 全部来自正文无衬线文本的抗锯齿微差——衬线标题零 diff，3 倍放大目检确认不可见。到这个精度，基本可以认为复刻完成。

## 三个值得记录的坑

**1. dev server 的陈旧编译会骗人**

`site-footer.tsx` 里一处 `{" "}` 修复写入磁盘后，运行中的 `next dev` 没有重新编译：served HTML 里缺空格，但 RSC payload 里又是对的。重启重建后才生效。教训：**验证修复必须重新 build 或重启服务，不能信 Fast Refresh**。

**2. React 19 SSR 的 `{" "}` 行为**

React 19 服务端渲染时，JSX 里的 `{" "}` 会输出为 `<!-- -->` 注释标记加一个空格（如 `数据版本<!-- --> <a>`）。写像素级审计脚本时如果按纯字符串匹配预期输出，这里会产生大量误报。用 `renderToStaticMarkup` 做最小复现是有效的排查手段。另外审计时要保留纯空白文本节点参与比对——flex/grid 容器内被 JSX 裁掉的元素间空白虽然不影响渲染，但walker 跳过它们会漏判。

**3. standalone 输出下 `next start` 不可用**

模板配置了 `output: "standalone"`，生产运行需要手动拼装静态资源：

```bash
cp -r .next/static .next/standalone/.next/ && cp -r public .next/standalone/ && PORT=3100 node .next/standalone/server.js
```

## 小结

AI agent 复刻网站这件事，真正的门槛不在"生成代码"，而在**建立可量化的验收闭环**：像素 diff、文本节点审计、字体哈希比对、交互冒烟，每一项都能给出通过/失败的数字。有了这套闭环，agent 的每一轮改动是否有效，立刻可见。

复刻仓库：[Hua2026-ai/ai-website-cloner-template](https://github.com/Hua2026-ai/ai-website-cloner-template)
