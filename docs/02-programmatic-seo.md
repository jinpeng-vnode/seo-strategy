# 程序化SEO（Programmatic SEO）方案设计

> 适用对象：todonot.com 工具站矩阵（calculator-tools / image-compress-tools / excalidraw-hub / temp-mail-tools）
> 目标：通过批量生成针对长尾关键词的落地页，1个月内快速获取 Google 自然流量
> 撰写日期：2026-05-28

---

## 一、成功案例分析

### 1.1 Zapier — 集成页面矩阵（月访问 9M+）

**策略核心：**
- 为每一对 App 集成组合创建独立落地页，如 `/apps/slack/integrations/google-sheets`
- 页面模板统一：标题公式 = `Connect {App A} + {App B}`，内容包含集成步骤、用例、CTA
- 数据驱动：9000+ 应用的排列组合，自动生成数十万页面
- 每个页面都有独特的用户价值（具体的集成教程和触发器列表）

**关键数据：**
- 排名关键词数：数百万
- 月自然流量：6.3M - 9M
- 页面类型：App 页面 + Integration 页面 + Category 页面

**可借鉴点：**
- URL 模式清晰：`/apps/{app-name}` + `/apps/{a}/integrations/{b}`
- 每页有独特数据点（CTA按钮、连接应用列表）
- Hub-Spoke 内链：Category → App → Integration 三层结构

### 1.2 Nomad List — 城市数据页面矩阵

**策略核心：**
- 为全球 1000+ 城市创建数字游民评分页面
- 每个城市页面包含：生活成本、网速、安全指数、气候等结构化数据
- 数据来源：API 抓取 + 用户贡献 + 公开数据集

**关键数据：**
- 页面数量：1000+ 城市页
- 独特数据：用户评分、实时价格、社区反馈
- 内链模型：大洲 → 国家 → 城市 三级导航

**可借鉴点：**
- 结构化数据（Schema.org）提升富摘要展示
- 用户生成内容（UGC）增加页面独特性
- 筛选器页面创造更多长尾入口

### 1.3 Wise（原 TransferWise）— 汇率计算器页面

**策略核心：**
- 为每一对货币组合创建汇率转换页面，如 `/us/currency-converter/usd-to-eur`
- 实时汇率数据 + 历史走势图 + 费用对比
- 170+ 货币的排列组合 = 数万页面

**关键数据：**
- 目标关键词模式：`{currency A} to {currency B}`
- 每页独特内容：实时汇率、30天走势、银行对比表
- 转化路径：查汇率 → 注册 → 转账

**可借鉴点：**
- 工具型页面天然满足搜索意图
- 实时数据保证页面新鲜度
- 强 CTA 与工具功能无缝结合

### 1.4 对 todonot.com 的启示总结

| 维度 | Zapier | Nomad List | Wise | todonot.com 应用 |
|------|--------|-----------|------|-----------------|
| 数据源 | App API 数据库 | 城市公开数据 | 汇率 API | 计算公式/图片格式/模板库/邮箱域名 |
| 页面模式 | A+B 组合 | 实体属性 | A→B 转换 | 工具+修饰词 |
| 独特性 | 集成步骤 | 用户评分 | 实时数据 | 计算结果/转换预览 |
| 内链 | 三层分类 | 地理层级 | 货币分类 | 工具类型→具体工具→变体 |

---

## 二、页面模板设计

### 2.1 通用模板结构（适用所有工具站）

```
┌─────────────────────────────────────────────┐
│  [面包屑导航] Home > Category > Tool Name   │
├─────────────────────────────────────────────┤
│  H1: {动作词} + {具体对象} + {修饰词}        │
│  例: "在线计算BMI指数 - 免费BMI计算器"       │
├─────────────────────────────────────────────┤
│  [工具交互区] ← 核心功能，首屏可见           │
│  - 输入框/上传区/操作面板                    │
│  - 即时结果展示                              │
├─────────────────────────────────────────────┤
│  H2: 什么是{工具对象}？                      │
│  - 简短定义（2-3句）                         │
│  - 关键参数/公式说明                         │
├─────────────────────────────────────────────┤
│  H2: 如何使用本{工具名}                      │
│  - 步骤1/2/3（有序列表）                     │
│  - 配图或GIF演示                             │
├─────────────────────────────────────────────┤
│  H2: {对象}常见问题 (FAQ)                    │
│  - 3-5个FAQ（Schema标记）                    │
│  - 每个问答针对一个长尾关键词                │
├─────────────────────────────────────────────┤
│  H2: 相关工具推荐                            │
│  - 内链卡片网格（3-6个相关工具）             │
├─────────────────────────────────────────────┤
│  [CTA区域]                                   │
│  - 主CTA: "立即使用" / "免费试用"            │
│  - 副CTA: "收藏本工具" / "分享给朋友"       │
├─────────────────────────────────────────────┤
│  [Footer] 站点地图 | 工具分类 | 关于我们     │
└─────────────────────────────────────────────┘
```

### 2.2 各工具站模板差异化

#### calculator-tools 计算器模板
```html
<h1>在线{计算类型}计算器 - 免费{修饰词}</h1>

<!-- 工具区 -->
<section class="tool-widget">
  <form>输入参数 → 计算按钮 → 结果展示</form>
  <div class="result-display">计算结果 + 公式展示</div>
</section>

<h2>{计算类型}计算公式</h2>
<p>公式说明 + LaTeX渲染</p>

<h2>计算示例</h2>
<table>示例输入 → 示例输出</table>

<h2>常见问题</h2>
<div itemscope itemtype="https://schema.org/FAQPage">
  FAQ 1-5
</div>

<h2>相关计算器</h2>
<nav class="related-tools">内链卡片</nav>
```

#### image-compress-tools 图片工具模板
```html
<h1>在线{格式}转{目标格式} - 免费图片{操作}工具</h1>

<section class="tool-widget">
  <div class="upload-zone">拖拽上传区</div>
  <div class="preview">转换前后对比预览</div>
  <button>下载结果</button>
</section>

<h2>{源格式} vs {目标格式} 对比</h2>
<table>格式特性对比表</table>

<h2>批量{操作}指南</h2>
<ol>操作步骤</ol>

<h2>常见问题</h2>
<div>FAQ with Schema</div>
```

#### excalidraw-hub 绘图模板库模板
```html
<h1>{图表类型}模板 - 免费在线{场景}绘图</h1>

<section class="template-preview">
  <img>模板预览图</img>
  <button>使用此模板</button>
</section>

<h2>模板说明</h2>
<p>适用场景、包含元素</p>

<h2>如何自定义此{图表类型}</h2>
<ol>编辑步骤</ol>

<h2>相关模板</h2>
<nav>同类模板内链</nav>
```

#### temp-mail-tools 临时邮箱模板
```html
<h1>{用途场景}临时邮箱 - 免费一次性邮箱地址</h1>

<section class="tool-widget">
  <div class="email-display">生成的临时邮箱地址</div>
  <div class="inbox">收件箱实时刷新</div>
</section>

<h2>什么是临时邮箱</h2>
<p>定义 + 使用场景</p>

<h2>{场景}为什么需要临时邮箱</h2>
<p>场景化说明</p>

<h2>安全须知</h2>
<p>隐私保护说明</p>
```

### 2.3 SEO 元素清单（每页必备）

| 元素 | 规则 | 示例 |
|------|------|------|
| Title | `{核心关键词} - 免费在线工具 \| todonot.com` | `BMI计算器 - 免费在线BMI指数计算 \| todonot.com` |
| Meta Description | 包含关键词 + 功能描述 + CTA，120-155字符 | `使用免费在线BMI计算器，输入身高体重即刻获取BMI指数。支持公制英制，含健康建议。` |
| H1 | 每页唯一，包含主关键词 | `在线BMI计算器` |
| Canonical | 自引用 canonical | `<link rel="canonical" href="当前URL">` |
| Schema | FAQPage + WebApplication + BreadcrumbList | JSON-LD 结构化数据 |
| Open Graph | og:title, og:description, og:image | 社交分享优化 |
| hreflang | 多语言页面互指（如有） | `<link rel="alternate" hreflang="en">` |

---

## 三、URL 结构规划

### 3.1 总体原则

- 使用英文小写 + 连字符（kebab-case）
- URL 层级不超过 3 层（域名后最多 /a/b/c）
- 包含目标关键词
- 避免参数化 URL（?id=123），全部静态化
- 每个子域名独立 sitemap.xml

### 3.2 各工具站 URL Pattern

#### calculator-tools.todonot.com

```
首页:       /
分类页:     /{category}/
工具页:     /{category}/{calculator-name}/
变体页:     /{category}/{calculator-name}/{modifier}/

示例:
/                                    → 计算器工具首页
/health/                             → 健康计算器分类
/health/bmi-calculator/              → BMI计算器
/health/bmi-calculator/metric/       → 公制BMI计算器
/finance/                            → 金融计算器分类
/finance/compound-interest/          → 复利计算器
/finance/mortgage-calculator/        → 房贷计算器
/math/                               → 数学计算器分类
/math/percentage-calculator/         → 百分比计算器
```

**关键词模式：**
- `{type} calculator` → 主页面
- `{type} calculator {modifier}` → 变体页面
- `how to calculate {type}` → FAQ 锚定

#### image-compress-tools.todonot.com

```
首页:       /
操作页:     /{action}/
格式页:     /{action}/{format}/
转换页:     /convert/{source}-to-{target}/

示例:
/                                    → 图片工具首页
/compress/                           → 图片压缩分类
/compress/jpeg/                      → JPEG压缩
/compress/png/                       → PNG压缩
/resize/                             → 图片缩放分类
/resize/instagram/                   → Instagram尺寸调整
/convert/png-to-jpg/                 → PNG转JPG
/convert/webp-to-png/                → WebP转PNG
/convert/heic-to-jpg/                → HEIC转JPG
```

**关键词模式：**
- `compress {format} online` → 压缩页
- `{format A} to {format B} converter` → 转换页
- `resize image for {platform}` → 平台适配页

#### excalidraw-hub.todonot.com

```
首页:       /
分类页:     /templates/{category}/
模板页:     /templates/{category}/{template-name}/
场景页:     /use-cases/{scenario}/

示例:
/                                    → Excalidraw Hub 首页
/templates/flowchart/                → 流程图模板分类
/templates/flowchart/basic-flowchart/        → 基础流程图模板
/templates/flowchart/decision-tree/          → 决策树模板
/templates/wireframe/                → 线框图模板分类
/templates/wireframe/mobile-app/     → 移动App线框图
/templates/diagram/                  → 图表模板分类
/templates/diagram/er-diagram/       → ER图模板
/use-cases/project-planning/         → 项目规划场景页
```

**关键词模式：**
- `{diagram type} template` → 模板页
- `{diagram type} example` → 示例页
- `how to draw {diagram type}` → 教程页

#### temp-mail-tools.todonot.com

```
首页:       /
场景页:     /use-case/{scenario}/
域名页:     /domains/{domain}/
指南页:     /guide/{topic}/

示例:
/                                    → 临时邮箱首页（含工具）
/use-case/sign-up/                   → 注册场景临时邮箱
/use-case/testing/                   → 测试场景临时邮箱
/use-case/avoid-spam/                → 防垃圾邮件场景
/domains/gmail-alternative/          → Gmail替代方案
/guide/disposable-email-safety/      → 一次性邮箱安全指南
/guide/temporary-email-vs-alias/     → 临时邮箱 vs 邮箱别名
```

**关键词模式：**
- `temporary email for {use case}` → 场景页
- `disposable email {feature}` → 功能页
- `{provider} alternative temp mail` → 对比页

### 3.3 URL 规范化规则

```
规则1: 所有URL以 / 结尾（trailing slash）
规则2: 301重定向非规范URL到规范URL
规则3: 大写字母 → 301到小写
规则4: 带www → 301到不带www
规则5: HTTP → 301到HTTPS
规则6: 重复斜杠 // → 301到单斜杠
```

---

## 四、批量页面生成策略

### 4.1 数据源规划

| 工具站 | 数据源 | 数据量预估 | 更新频率 |
|--------|--------|-----------|---------|
| calculator-tools | 计算公式数据库（自建JSON）| 200+ 计算器 × 3-5变体 = 600-1000页 | 月更 |
| image-compress-tools | 图片格式规格表 + 平台尺寸数据库 | 20格式 × 20格式转换 + 30平台适配 = 430页 | 季更 |
| excalidraw-hub | 模板元数据库（分类/标签/描述）| 15分类 × 20模板 = 300页 | 周更 |
| temp-mail-tools | 使用场景库 + 域名特性数据 | 50场景 + 30域名 + 20指南 = 100页 | 月更 |

**总计首批目标：1500-2000 个落地页**

### 4.2 数据结构设计

每个工具站维护一个核心 JSON/CSV 数据文件：

```json
// calculator-tools 数据示例: data/calculators.json
{
  "calculators": [
    {
      "id": "bmi-calculator",
      "category": "health",
      "name_en": "BMI Calculator",
      "name_zh": "BMI计算器",
      "title": "在线BMI计算器 - 免费计算身体质量指数",
      "description": "输入身高体重，即刻计算BMI指数，了解体重是否健康。",
      "formula": "BMI = weight(kg) / height(m)²",
      "inputs": ["height", "weight"],
      "output_unit": "kg/m²",
      "related": ["body-fat-calculator", "ideal-weight-calculator"],
      "faq": [
        {"q": "BMI正常范围是多少？", "a": "18.5-24.9为正常范围。"},
        {"q": "BMI计算器准确吗？", "a": "BMI是初步筛查工具，不考虑肌肉量等因素。"}
      ],
      "modifiers": ["metric", "imperial", "for-kids", "for-athletes"]
    }
  ]
}
```

```json
// image-compress-tools 数据示例: data/conversions.json
{
  "conversions": [
    {
      "id": "png-to-jpg",
      "source_format": "PNG",
      "target_format": "JPG",
      "title": "PNG转JPG - 免费在线图片格式转换",
      "description": "一键将PNG图片转换为JPG格式，支持批量转换，无需安装软件。",
      "source_features": ["无损压缩", "支持透明", "文件较大"],
      "target_features": ["有损压缩", "不支持透明", "文件较小"],
      "use_cases": ["网页优化", "邮件附件", "社交媒体"],
      "related": ["jpg-to-png", "png-to-webp", "compress-png"],
      "faq": [...]
    }
  ]
}
```

### 4.3 模板引擎选型

**推荐方案：Next.js SSG（Static Site Generation）**

```
技术栈:
├── 框架: Next.js 14+ (App Router)
├── 渲染: SSG (getStaticPaths + getStaticProps) 或 ISR
├── 数据: JSON/CSV 文件 → 构建时读取
├── 样式: Tailwind CSS
├── 部署: Vercel / Cloudflare Pages
└── 构建: 增量构建（ISR fallback: 'blocking'）
```

**核心生成逻辑：**

```typescript
// app/[category]/[calculator]/page.tsx
import { getCalculatorData, getAllCalculatorPaths } from '@/lib/data';

export async function generateStaticParams() {
  const paths = getAllCalculatorPaths();
  return paths.map(({ category, calculator }) => ({
    category,
    calculator,
  }));
}

export default async function CalculatorPage({ params }) {
  const data = getCalculatorData(params.category, params.calculator);
  return <CalculatorTemplate data={data} />;
}
```

### 4.4 自动化生成流程

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  数据源更新   │────▶│  CI/CD 触发   │────▶│  SSG 构建    │
│  (JSON/CSV)  │     │  (GitHub     │     │  (Next.js    │
│              │     │   Actions)   │     │   build)     │
└──────────────┘     └──────────────┘     └──────────────┘
                                                  │
                                                  ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  SEO 验证    │◀────│  部署上线     │◀────│  Sitemap     │
│  (Lighthouse │     │  (Vercel/CF) │     │  自动生成    │
│   + Schema)  │     │              │     │              │
└──────────────┘     └──────────────┘     └──────────────┘
```

**具体步骤：**

1. **数据准备**：维护 `data/` 目录下的 JSON 文件
2. **模板开发**：为每种页面类型开发 React 组件模板
3. **构建触发**：数据文件变更 → GitHub Actions 自动触发构建
4. **静态生成**：Next.js `generateStaticParams` 遍历所有数据生成页面
5. **Sitemap 生成**：构建后自动生成 `sitemap.xml`（使用 `next-sitemap`）
6. **部署**：自动部署到 Vercel/Cloudflare Pages
7. **索引提交**：通过 Google Search Console API 批量提交新 URL
8. **质量检查**：Lighthouse CI 检查每批新页面的性能和 SEO 分数

### 4.5 内容独特性保障（避免薄内容惩罚）

每个页面必须有至少 3 个独特性向量：

| 独特性向量 | 实现方式 |
|-----------|---------|
| 动态计算结果 | 用户输入后生成唯一结果，搜索引擎可见默认示例 |
| 结构化数据差异 | 每页的公式、参数、单位各不相同 |
| FAQ 内容 | 每个工具 3-5 个独特 FAQ，针对该工具的长尾词 |
| 相关工具推荐 | 基于数据关联的不同推荐组合 |
| 用户评价/使用统计 | 展示该工具的使用次数、评分（可后期添加） |

---

## 五、内部链接架构（Hub-Spoke 模型）

### 5.1 架构总览

```
                    todonot.com (主站)
                         │
          ┌──────────────┼──────────────┐──────────────┐
          ▼              ▼              ▼              ▼
   calculator-tools  image-compress  excalidraw-hub  temp-mail
   (子域名Hub)       (子域名Hub)     (子域名Hub)    (子域名Hub)
          │              │              │              │
    ┌─────┼─────┐  ┌────┼────┐   ┌────┼────┐   ┌────┼────┐
    ▼     ▼     ▼  ▼    ▼    ▼   ▼    ▼    ▼   ▼    ▼    ▼
  分类A  分类B  分类C  操作A 操作B  分类A 分类B  场景A 场景B
  (Spoke) ...        (Spoke)...   (Spoke)...   (Spoke)...
    │                   │              │              │
    ▼                   ▼              ▼              ▼
  具体工具页          具体格式页      具体模板页      具体指南页
  (Leaf)             (Leaf)          (Leaf)         (Leaf)
```

### 5.2 三层链接策略

#### 第一层：Hub 页面（首页/分类首页）

- **角色**：权重集中页，承接外链和站内权重
- **链接规则**：
  - 链接到所有直属 Spoke 页面
  - 展示"热门工具"、"最新工具"模块
  - 包含完整分类导航

```html
<!-- Hub 页面示例：calculator-tools 首页 -->
<nav class="category-nav">
  <a href="/health/">健康计算器</a>
  <a href="/finance/">金融计算器</a>
  <a href="/math/">数学计算器</a>
  <a href="/conversion/">单位换算</a>
</nav>

<section class="popular-tools">
  <h2>热门计算器</h2>
  <!-- 链接到高流量 Leaf 页面 -->
  <a href="/health/bmi-calculator/">BMI计算器</a>
  <a href="/finance/mortgage-calculator/">房贷计算器</a>
  ...
</section>
```

#### 第二层：Spoke 页面（分类页）

- **角色**：中间层，连接 Hub 和 Leaf
- **链接规则**：
  - 向上链接到 Hub（面包屑）
  - 向下链接到所有该分类的 Leaf 页面
  - 横向链接到 2-3 个相关分类

```html
<!-- Spoke 页面示例：/health/ 分类页 -->
<nav class="breadcrumb">
  <a href="/">首页</a> > <span>健康计算器</span>
</nav>

<h1>健康计算器合集</h1>

<div class="tool-grid">
  <!-- 所有健康类 Leaf 页面 -->
  <a href="/health/bmi-calculator/">BMI计算器</a>
  <a href="/health/body-fat-calculator/">体脂率计算器</a>
  <a href="/health/calorie-calculator/">卡路里计算器</a>
  ...
</div>

<aside class="related-categories">
  <h3>相关分类</h3>
  <a href="/fitness/">健身计算器</a>
  <a href="/nutrition/">营养计算器</a>
</aside>
```

#### 第三层：Leaf 页面（具体工具页）

- **角色**：流量入口页，承接长尾搜索
- **链接规则**：
  - 向上链接到 Spoke（面包屑）
  - 横向链接到 3-6 个相关 Leaf 页面（"相关工具"模块）
  - 可选：跨子域名链接到相关工具

```html
<!-- Leaf 页面示例：/health/bmi-calculator/ -->
<nav class="breadcrumb">
  <a href="/">首页</a> > <a href="/health/">健康计算器</a> > <span>BMI计算器</span>
</nav>

<!-- 页面底部 -->
<section class="related-tools">
  <h2>相关工具</h2>
  <a href="/health/body-fat-calculator/">体脂率计算器</a>
  <a href="/health/ideal-weight-calculator/">理想体重计算器</a>
  <a href="/health/bmr-calculator/">基础代谢率计算器</a>
  <!-- 跨站链接 -->
  <a href="https://image-compress-tools.todonot.com/resize/health-app/">
    健康App图片尺寸调整
  </a>
</section>
```

### 5.3 跨子域名链接策略

```
calculator-tools ←──→ image-compress-tools
       ↕                      ↕
excalidraw-hub  ←──→  temp-mail-tools
```

**链接场景：**
- calculator-tools 的"图表计算器" → excalidraw-hub 的"数据图表模板"
- image-compress-tools 的"社交媒体尺寸" → excalidraw-hub 的"社交媒体模板"
- 所有工具站 Footer → 主站 todonot.com 工具导航页

**实现方式：**
- 每个 Leaf 页面底部"更多工具"区域放置 1-2 个跨站链接
- 所有子站共享统一 Footer，包含矩阵导航
- 主站 todonot.com 作为权重分发中心，链接到所有子站首页

### 5.4 Sitemap 与索引管理

```xml
<!-- 每个子站独立 sitemap -->
<!-- calculator-tools.todonot.com/sitemap.xml -->
<sitemapindex>
  <sitemap><loc>/sitemap-categories.xml</loc></sitemap>
  <sitemap><loc>/sitemap-tools-health.xml</loc></sitemap>
  <sitemap><loc>/sitemap-tools-finance.xml</loc></sitemap>
  <sitemap><loc>/sitemap-tools-math.xml</loc></sitemap>
</sitemapindex>
```

**索引控制规则：**
- 所有 Hub + Spoke + Leaf 页面：`index, follow`
- 搜索结果页/筛选页：`noindex, follow`
- 分页第2页起：`noindex, follow` + `rel="canonical"` 指向第1页
- 参数页面：robots.txt 屏蔽或 canonical 到无参数版本

---

## 六、实施路线图（4周计划）

### 第1周：数据准备 + 模板开发

| 天数 | 任务 | 产出 | 负责 |
|------|------|------|------|
| Day 1-2 | 关键词调研：为每个工具站挖掘 100+ 长尾词 | 关键词数据库 CSV | SEO |
| Day 2-3 | 数据结构设计：定义 JSON Schema | `data/schema/` 目录 | 开发 |
| Day 3-4 | 填充首批数据：每站 30-50 条核心数据 | `data/*.json` 文件 | SEO+开发 |
| Day 4-5 | 模板组件开发：通用组件（面包屑、FAQ、相关工具） | React 组件库 | 前端 |
| Day 5-7 | 各站专属模板开发：4个工具站各1个主模板 | 4套页面模板 | 前端 |

**本周里程碑：** 每个工具站可以用模板 + 数据生成 1 个示例页面

### 第2周：批量生成 + 技术SEO

| 天数 | 任务 | 产出 | 负责 |
|------|------|------|------|
| Day 8-9 | 完善数据：扩充到每站 100+ 条数据 | 完整数据文件 | SEO |
| Day 9-10 | SSG 构建流程：配置 generateStaticParams | 可构建的完整站点 | 开发 |
| Day 10-11 | Sitemap 自动生成 + robots.txt 配置 | sitemap.xml × 4 | 开发 |
| Day 11-12 | Schema 标记：FAQPage + WebApplication | 结构化数据验证通过 | 前端 |
| Day 12-13 | 内链系统实现：面包屑 + 相关工具 + 分类导航 | 内链组件完成 | 前端 |
| Day 13-14 | 性能优化：Core Web Vitals 达标 | Lighthouse 90+ | 开发 |

**本周里程碑：** 4个工具站各生成 100+ 页面，技术SEO全部就绪

### 第3周：上线部署 + 索引提交

| 天数 | 任务 | 产出 | 负责 |
|------|------|------|------|
| Day 15-16 | 部署到生产环境（Vercel/CF Pages） | 4站上线 | DevOps |
| Day 16-17 | Google Search Console 验证 + Sitemap 提交 | GSC 配置完成 | SEO |
| Day 17-18 | IndexNow / Google Indexing API 批量提交 | URL 提交完成 | 开发 |
| Day 18-19 | 质量抽检：随机检查 50 页的内容质量 | 质量报告 | SEO |
| Day 19-20 | 修复问题页面 + 补充薄内容页的 FAQ | 修复完成 | 全员 |
| Day 20-21 | 第二批数据扩充：每站再增 50-100 条 | 数据更新 | SEO |

**本周里程碑：** 所有页面上线并提交索引，开始被 Google 抓取

### 第4周：监控优化 + 扩量

| 天数 | 任务 | 产出 | 负责 |
|------|------|------|------|
| Day 22-23 | 监控索引状态：GSC 覆盖率报告 | 索引率报告 | SEO |
| Day 23-24 | 分析首批排名数据，识别快速上升的页面 | 排名分析报告 | SEO |
| Day 24-25 | 优化表现好的页面：增加内容深度 | 页面更新 | SEO |
| Day 25-26 | 第三批数据扩充 + 新变体页面生成 | 新增 200+ 页面 | 全员 |
| Day 26-27 | A/B 测试：标题公式、CTA 位置 | 测试方案 | 前端 |
| Day 27-28 | 月度复盘 + 下月计划制定 | 复盘文档 | 全员 |

**本周里程碑：** 首批页面开始获得排名，数据驱动优化启动

### 关键指标（KPI）

| 指标 | 第2周末 | 第4周末 | 第8周末（远期） |
|------|---------|---------|---------------|
| 总页面数 | 400+ | 800+ | 1500+ |
| 索引率 | - | >60% | >85% |
| 有排名关键词数 | - | 200+ | 1000+ |
| 日自然流量 | - | 50+ | 500+ |
| 首页关键词数 | - | 10+ | 100+ |

---

## 七、风险控制与注意事项

### 7.1 避免 Google 薄内容惩罚

- ❌ 不要：仅替换关键词的重复模板页
- ✅ 要做：每页至少 3 个独特内容区块（工具功能、FAQ、数据表）
- ✅ 要做：渐进式发布（每天不超过 50 个新页面提交索引）
- ✅ 要做：定期审计低质量页面，noindex 或合并

### 7.2 索引预算管理

- 分批提交 URL，优先提交高价值页面
- 监控 crawl stats，确保爬虫预算不被浪费在低价值页面
- 使用 `lastmod` 引导爬虫优先抓取更新页面

### 7.3 内容质量底线

- 每个页面正文不少于 300 字（不含代码/表格）
- FAQ 至少 3 个，每个回答不少于 50 字
- 工具功能必须真实可用（不是纯静态展示页）
- 定期更新数据，保持页面新鲜度

### 7.4 技术风险

- SSG 构建时间随页面增长：使用 ISR（增量静态再生）解决
- 数据源错误导致批量错误页面：构建前加数据校验脚本
- 子域名 vs 子目录 SEO 权重：子域名需要各自积累权重，通过主站外链分发

---

## 附录：快速启动清单

```
□ 创建 4 个子域名的 DNS 记录
□ 初始化 4 个 Next.js 项目（可用 monorepo）
□ 设计并填充首批数据文件（每站 30 条）
□ 开发通用组件：ToolTemplate, FAQ, Breadcrumb, RelatedTools
□ 配置 next-sitemap 自动生成 sitemap
□ 添加 Google Analytics 4 + Search Console
□ 配置 CI/CD（GitHub Actions → Vercel）
□ 编写数据校验脚本（JSON Schema validation）
□ 设置 IndexNow 自动提交
□ 创建内容质量检查 checklist
```
