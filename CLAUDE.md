# CatTI — Project Context for Claude

## 项目概述

**产品名：** CatTI
**核心概念：** 12题人格测试，MBTI恶搞版，结果是各种"有点不太正常的猫"。
**目标感觉：** 搞笑、有梗、离谱但说中了、互联网感强，像会在朋友圈刷屏的小测试。
**系列关系：** DogTI 的猫版续作，结构完全一致。

---

## 设计原则（不要改动）

- **视觉风格：** 宠物像素风（pixel art pets）
- **配色：** 黑字白底，极简 UI
- **禁止：** 深色主题、复杂配色、严肃的 MBTI 风格
- **字体：** 标题用 `Press Start 2P`（base64 内嵌），正文用系统字体
- **按钮：** 黑色胶囊形（`border-radius: 100px`），白字，带播放圆圈图标

---

## 文件结构

```
/Users/junli/Desktop/catti-designs/
├── index.html         ← 生产主文件（Cloudflare Pages 入口）
├── catti-main.html    ← 开发主文件（内容与 index.html 完全相同）
├── CLAUDE.md          ← 本文件
├── QUIZ-LOGIC.md      ← 题目与结果逻辑文档
└── assets/            ← 静态资源目录
    ├── CATTI标题.png
    ├── 猫文案.png
    └── 16只猫咪.png × 16
```

主文件是**单文件 HTML**，所有 CSS 和 JS 内联，无构建工具，无外部依赖。

---

## 页面结构（catti-main.html）

三个页面，通过 `.page` / `.page.active` 切换显示：

| 页面 | ID | 内容 |
|------|-----|------|
| 封面页 | `#page-home` | 像素大标题 CATTI + 三行滚动像素猫 + 文案 + 开始按钮 |
| 题目页 | `#page-quiz` | 进度条 + 像素猫提示气泡 + 问题文本 + 3个选项(A/B/C) |
| 结果页 | `#page-result` | 猫格名称 + 副标题 + 金句 + 性格标签 + 描述 + 分享/再测按钮 |

### 首页关键实现
- 动物滚动带：CSS `@keyframes`，第1、3行向左，第2行向右
- 像素猫：`<img src="assets/猫种.png">` PNG 文件，`image-rendering: pixelated`
- 标题：`assets/CATTI标题.png`，文案：`assets/猫文案.png`

---

## 测试逻辑

- **12道题**，每个选项标记 MBTI 维度（E/I、S/N、T/F、J/P）
- 每题3个选项（A/B/C），选项覆盖不同维度
- 题目视角：**"我"是一只猫**，全程第一视角
- `calcResult()` 统计各维度计数，4个维度各取多数，组合成16种 MBTI 类型
- 详细题目、维度映射、结果数据见 → `QUIZ-LOGIC.md`

### 16种结果（格式：XX的XX猫）

| MBTI | 结果名 | cat key |
|------|--------|---------|
| ENTJ | 已经替你想好了的暹罗猫 | siamese |
| ENTP | 把规则都踢翻了的孟加拉猫 | bengal |
| ENFJ | 比你更知道你要什么的布偶猫 | ragdoll |
| ENFP | 看到什么都要凑过去的橘猫 | orange |
| ESTJ | 把作息表刻进基因的阿比西尼亚猫 | abyssinian |
| ESTP | 说好午睡结果跳上冰箱的美短 | americanshorthair |
| ESFJ | 随时监控全家动态的波斯猫 | persian |
| ESFP | 必须坐在镜头正中间的金渐层 | goldengradient |
| INTJ | 假装不在乎的英短 | britishshorthair |
| INTP | 盯着墙发呆发了半小时的蓝猫 | russianblue |
| INFJ | 一直在微笑但没人懂为什么的缅因猫 | mainecoon |
| INFP | 你不懂的无毛猫 | sphynx |
| ISTJ | 饭点差一秒都不行的苏格兰折耳猫 | scottishfold |
| ISTP | 不需要你懂的挪威森林猫 | norwegianforest |
| ISFJ | 记住你昨天心情不好的加菲猫 | garfield |
| ISFP | 急不起来的土耳其安哥拉猫 | angora |

---

## 结果页文案规范

- 标签文字：**「你的猫格是」**
- 结果格式：`XX的XX猫`（共16种猫）

---

## 部署

- **平台：** Cloudflare Pages（主用）→ cattti.pages.dev
- **GitHub 仓库：** diggtoli-stack/catti-designs
- **流程：** 用户确认OK → 推送 GitHub → Cloudflare 自动重新部署

---

## 用户偏好（重要）

- **只改文案时，不动设计和结构**
- 修改某一项时，只改被指定的那一项，其余保持原样
- 不要主动"顺手"优化代码、重构结构、添加功能
