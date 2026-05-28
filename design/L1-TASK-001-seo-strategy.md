# L1-TASK-001 SEO策略中枢 文档结构设计

## 项目概述
为 todonot.com 工具站矩阵提供统一 SEO 策略支撑，目标1个月内上 Google 首页。

## 工具站矩阵
| 站点 | 域名 | 竞品对标 |
|------|------|----------|
| 计算器工具 | calculator-tools.todonot.com | Calculator.net |
| 图片压缩 | image-compress-tools.todonot.com | TinyPNG |
| 白板绘图 | excalidraw-hub.todonot.com | Excalidraw.com |
| 临时邮箱 | temp-mail-tools.todonot.com | Temp-Mail.org |

## 文档目录结构

```
docs/
├── 01-keyword-research.md        # 关键词调研与竞品分析
├── 02-programmatic-seo.md        # Programmatic SEO 方案
├── 03-new-domain-ranking.md      # 新域名快速排名策略
├── 04-backlink-building.md       # 外链建设计划
├── 05-technical-seo.md           # 技术SEO清单
└── 06-monitoring.md              # 监控方案
```

## 各文档大纲

### 01-keyword-research.md
1. 竞品关键词分析（TinyPNG/Calculator.net/Excalidraw/Temp-Mail）
2. 长尾关键词挖掘方法论
3. 按工具站分类的关键词数据库
4. 关键词优先级排序（搜索量 × 竞争度）
5. 内容映射：关键词 → 落地页

### 02-programmatic-seo.md
1. 成功案例分析
2. 页面模板设计（每个关键词一个落地页）
3. URL 结构规划
4. 批量生成策略
5. 内部链接架构

### 03-new-domain-ranking.md
1. Google 沙盒期分析与应对
2. Google Indexing API 配置与使用
3. 子域名 vs 子目录 SEO 权重对比
4. todonot.com 子域名权重继承分析
5. 快速建立域名权威的方法

### 04-backlink-building.md
1. 工具目录站清单（50+站点）
2. 各目录站提交流程与模板
3. 社区推广渠道（Reddit/HN/V2EX/IndieHackers）
4. 内容营销外链策略
5. 执行时间表

### 05-technical-seo.md
1. Core Web Vitals 优化清单
2. Schema.org 结构化数据模板（WebApplication/SoftwareApplication）
3. sitemap.xml 生成规范
4. robots.txt 最佳实践
5. 多语言 hreflang 配置
6. 页面速度优化

### 06-monitoring.md
1. Google Search Console 配置步骤
2. 排名追踪工具选型与对比
3. 流量监控方案
4. 关键指标 Dashboard 设计
5. 周报/月报模板

## 执行顺序
1. 关键词调研（基础，其他文档依赖此数据）
2. Programmatic SEO（依赖关键词数据）
3. 新域名排名策略（独立）
4. 外链建设（独立）
5. 技术SEO（独立）
6. 监控方案（最后，需要知道监控什么指标）
