# 技术SEO清单与模板

> 适用范围：todonot.com 工具站矩阵（calculator-tools、image-compress-tools、excalidraw-hub、temp-mail-tools）
> 目标：确保所有工具站在技术层面对搜索引擎100%友好，支撑1个月内上Google首页的目标
> 更新日期：2026-05-28

---

## 目录

1. [Core Web Vitals 优化清单](#1-core-web-vitals-优化清单)
2. [Schema.org 结构化数据模板](#2-schemaorg-结构化数据模板)
3. [sitemap.xml 生成规范](#3-sitemapxml-生成规范)
4. [robots.txt 最佳实践](#4-robotstxt-最佳实践)
5. [多语言 hreflang 配置](#5-多语言-hreflang-配置)
6. [页面速度优化](#6-页面速度优化)
7. [Meta标签模板](#7-meta标签模板)
8. [移动端优化清单](#8-移动端优化清单)

---

## 1. Core Web Vitals 优化清单

### 目标指标

| 指标 | 目标值 | 含义 |
|------|--------|------|
| LCP (Largest Contentful Paint) | < 2.5s | 最大内容绘制时间 |
| INP (Interaction to Next Paint) | < 200ms | 交互到下一次绘制延迟 |
| CLS (Cumulative Layout Shift) | < 0.1 | 累积布局偏移 |

### 1.1 LCP 优化（< 2.5s）

**关键实现方法：**

```html
<!-- 1. 预加载关键资源 -->
<link rel="preload" href="/fonts/inter.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="/hero-image.webp" as="image">
<link rel="preconnect" href="https://cdn.todonot.com">
<link rel="dns-prefetch" href="https://cdn.todonot.com">

<!-- 2. 关键CSS内联 -->
<style>
  /* 首屏关键CSS直接内联，不超过14KB */
  body { font-family: 'Inter', sans-serif; margin: 0; }
  .hero { min-height: 100vh; display: flex; align-items: center; }
  .tool-container { max-width: 1200px; margin: 0 auto; padding: 2rem; }
</style>

<!-- 3. 非关键CSS异步加载 -->
<link rel="preload" href="/styles/main.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="/styles/main.css"></noscript>
```

**服务端优化：**

```nginx
# Nginx 配置 - 启用压缩和缓存
gzip on;
gzip_types text/plain text/css application/json application/javascript text/xml;
gzip_min_length 1000;

# Brotli 压缩（优先）
brotli on;
brotli_types text/plain text/css application/json application/javascript;

# 静态资源长缓存
location ~* \.(js|css|png|webp|woff2)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
}
```

**图片优化：**

```html
<!-- 使用现代格式 + 响应式图片 -->
<picture>
  <source srcset="/images/tool-hero.avif" type="image/avif">
  <source srcset="/images/tool-hero.webp" type="image/webp">
  <img src="/images/tool-hero.jpg" alt="在线计算器工具" 
       width="1200" height="630" loading="eager" fetchpriority="high">
</picture>
```

### 1.2 INP 优化（< 200ms）

```javascript
// 1. 长任务拆分 - 使用 scheduler.yield()
async function processLargeData(items) {
  for (let i = 0; i < items.length; i++) {
    processItem(items[i]);
    // 每处理50项让出主线程
    if (i % 50 === 0) {
      await scheduler.yield();
    }
  }
}

// 2. 使用 Web Worker 处理计算密集型任务
// worker.js
self.onmessage = function(e) {
  const result = heavyCalculation(e.data);
  self.postMessage(result);
};

// main.js
const worker = new Worker('/worker.js');
worker.postMessage(inputData);
worker.onmessage = (e) => updateUI(e.data);

// 3. 事件处理防抖
function debounce(fn, delay = 150) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

// 4. 使用 requestAnimationFrame 处理视觉更新
button.addEventListener('click', () => {
  requestAnimationFrame(() => {
    updateDOM();
  });
});
```

### 1.3 CLS 优化（< 0.1）

```html
<!-- 1. 图片/视频始终设置尺寸 -->
<img src="tool-screenshot.webp" width="800" height="450" alt="工具截图">
<video width="1280" height="720" poster="poster.webp"></video>

<!-- 2. 广告/嵌入内容预留空间 -->
<div style="min-height: 250px; aspect-ratio: 300/250;">
  <!-- 广告位 -->
</div>

<!-- 3. 字体加载防抖动 -->
<style>
  @font-face {
    font-family: 'Inter';
    src: url('/fonts/inter.woff2') format('woff2');
    font-display: swap;
    size-adjust: 100%;
    ascent-override: 90%;
    descent-override: 20%;
    line-gap-override: 0%;
  }
</style>

<!-- 4. 动态内容插入使用 contain -->
<style>
  .dynamic-content {
    contain: layout;
    content-visibility: auto;
    contain-intrinsic-size: 0 500px;
  }
</style>
```

---

## 2. Schema.org 结构化数据模板

### 2.1 WebApplication 模板（适用于在线工具）

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebApplication",
  "name": "在线科学计算器",
  "url": "https://calculator-tools.todonot.com/scientific",
  "description": "免费在线科学计算器，支持三角函数、对数、指数等高级运算",
  "applicationCategory": "UtilitiesApplication",
  "operatingSystem": "All",
  "browserRequirements": "Requires JavaScript",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "ratingCount": "1250"
  },
  "author": {
    "@type": "Organization",
    "name": "TodoNot Tools",
    "url": "https://todonot.com"
  },
  "datePublished": "2026-01-01",
  "dateModified": "2026-05-28",
  "inLanguage": ["en", "zh-CN", "ja", "ko"],
  "screenshot": "https://cdn.todonot.com/screenshots/calculator.webp"
}
</script>
```

### 2.2 SoftwareApplication 模板（适用于可下载工具）

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Image Compress Tool",
  "url": "https://image-compress-tools.todonot.com",
  "description": "批量图片压缩工具，支持PNG、JPEG、WebP格式，本地处理保护隐私",
  "applicationCategory": "MultimediaApplication",
  "operatingSystem": "Windows, macOS, Linux",
  "softwareVersion": "2.1.0",
  "fileSize": "5MB",
  "downloadUrl": "https://image-compress-tools.todonot.com/download",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.7",
    "ratingCount": "890",
    "bestRating": "5",
    "worstRating": "1"
  },
  "author": {
    "@type": "Organization",
    "name": "TodoNot Tools",
    "url": "https://todonot.com",
    "logo": "https://todonot.com/logo.png"
  },
  "datePublished": "2026-01-15",
  "dateModified": "2026-05-28"
}
</script>
```

### 2.3 BreadcrumbList 模板

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "首页",
      "item": "https://calculator-tools.todonot.com"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "科学计算器",
      "item": "https://calculator-tools.todonot.com/scientific"
    }
  ]
}
</script>
```

### 2.4 FAQPage 模板（提升搜索结果展示面积）

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "这个在线计算器是免费的吗？",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "是的，我们的在线计算器完全免费，无需注册即可使用。"
      }
    },
    {
      "@type": "Question",
      "name": "计算数据会被保存吗？",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "不会。所有计算都在您的浏览器本地完成，我们不会收集或存储任何计算数据。"
      }
    }
  ]
}
</script>
```

---

## 3. sitemap.xml 生成规范

### 3.1 标准 sitemap.xml 模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:xhtml="http://www.w3.org/1999/xhtml"
        xmlns:image="http://www.google.com/schemas/sitemap-image/1.1">
  
  <url>
    <loc>https://calculator-tools.todonot.com/scientific</loc>
    <lastmod>2026-05-28</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.9</priority>
    <!-- 多语言替代页面 -->
    <xhtml:link rel="alternate" hreflang="en" href="https://calculator-tools.todonot.com/en/scientific"/>
    <xhtml:link rel="alternate" hreflang="zh" href="https://calculator-tools.todonot.com/zh/scientific"/>
    <xhtml:link rel="alternate" hreflang="ja" href="https://calculator-tools.todonot.com/ja/scientific"/>
    <xhtml:link rel="alternate" hreflang="x-default" href="https://calculator-tools.todonot.com/scientific"/>
    <!-- 图片信息 -->
    <image:image>
      <image:loc>https://cdn.todonot.com/screenshots/scientific-calculator.webp</image:loc>
      <image:title>Scientific Calculator Online</image:title>
    </image:image>
  </url>

</urlset>
```

### 3.2 Sitemap Index（多站点索引）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap>
    <loc>https://calculator-tools.todonot.com/sitemap-pages.xml</loc>
    <lastmod>2026-05-28</lastmod>
  </sitemap>
  <sitemap>
    <loc>https://calculator-tools.todonot.com/sitemap-tools.xml</loc>
    <lastmod>2026-05-28</lastmod>
  </sitemap>
  <sitemap>
    <loc>https://calculator-tools.todonot.com/sitemap-blog.xml</loc>
    <lastmod>2026-05-25</lastmod>
  </sitemap>
</sitemapindex>
```

### 3.3 动态 Sitemap 生成（Next.js 示例）

```typescript
// app/sitemap.ts - Next.js App Router 动态生成
import { MetadataRoute } from 'next';

const SUPPORTED_LOCALES = ['en', 'zh', 'ja', 'ko', 'es', 'fr', 'de'];
const BASE_URL = 'https://calculator-tools.todonot.com';

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  // 从数据库或CMS获取所有工具页面
  const tools = await getAllTools();
  
  const toolUrls = tools.flatMap(tool => 
    SUPPORTED_LOCALES.map(locale => ({
      url: `${BASE_URL}/${locale}/${tool.slug}`,
      lastModified: tool.updatedAt,
      changeFrequency: 'weekly' as const,
      priority: tool.isPrimary ? 0.9 : 0.7,
      alternates: {
        languages: Object.fromEntries(
          SUPPORTED_LOCALES.map(l => [l, `${BASE_URL}/${l}/${tool.slug}`])
        ),
      },
    }))
  );

  return [
    {
      url: BASE_URL,
      lastModified: new Date(),
      changeFrequency: 'daily',
      priority: 1.0,
    },
    ...toolUrls,
  ];
}
```

### 3.4 提交规范

| 操作 | 频率 | 方式 |
|------|------|------|
| Google Search Console 提交 | 新站首次 + 大更新时 | 手动提交 |
| IndexNow 推送 | 每次内容更新 | API自动推送 |
| Ping 搜索引擎 | 每次sitemap更新 | 自动化脚本 |

```bash
# IndexNow 推送脚本
curl -X POST "https://api.indexnow.org/indexnow" \
  -H "Content-Type: application/json" \
  -d '{
    "host": "calculator-tools.todonot.com",
    "key": "your-indexnow-key",
    "urlList": [
      "https://calculator-tools.todonot.com/scientific",
      "https://calculator-tools.todonot.com/mortgage"
    ]
  }'

# Ping Google
curl "https://www.google.com/ping?sitemap=https://calculator-tools.todonot.com/sitemap.xml"
```

---

## 4. robots.txt 最佳实践

### 4.1 推荐配置模板

```txt
# robots.txt for calculator-tools.todonot.com
# 更新日期: 2026-05-28

# 所有搜索引擎通用规则
User-agent: *
Allow: /
Disallow: /api/
Disallow: /admin/
Disallow: /private/
Disallow: /_next/static/
Disallow: /tmp/
Disallow: /user/settings
Disallow: /*?ref=*
Disallow: /*?utm_*

# Google 特定规则
User-agent: Googlebot
Allow: /
Disallow: /api/internal/
Crawl-delay: 0

# Bing 特定规则
User-agent: Bingbot
Allow: /
Crawl-delay: 1

# 禁止AI爬虫抓取（保护内容）
User-agent: GPTBot
Disallow: /

User-agent: ChatGPT-User
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: anthropic-ai
Disallow: /

# Sitemap 位置
Sitemap: https://calculator-tools.todonot.com/sitemap.xml
Sitemap: https://calculator-tools.todonot.com/sitemap-index.xml
```

### 4.2 各工具站 robots.txt 差异配置

| 站点 | 特殊禁止规则 | 说明 |
|------|-------------|------|
| calculator-tools | `/api/calculate/` | 防止爬虫触发计算API |
| image-compress-tools | `/api/compress/`, `/uploads/` | 保护用户上传文件 |
| excalidraw-hub | `/api/drawings/`, `/shared/private/` | 保护私有画板 |
| temp-mail-tools | `/api/mail/`, `/inbox/` | 保护邮件隐私 |

### 4.3 注意事项

- **不要**在 robots.txt 中屏蔽 CSS/JS 文件，Google 需要渲染页面
- **不要**设置过高的 Crawl-delay，新站需要被快速抓取
- 定期检查 Google Search Console 的"抓取统计信息"
- robots.txt 文件大小不超过 500KB

---

## 5. 多语言 hreflang 配置

### 5.1 支持语言列表

| 语言代码 | 语言 | URL前缀 | 目标市场 |
|----------|------|---------|----------|
| en | English | /en/ | 全球（默认） |
| zh-CN | 简体中文 | /zh/ | 中国大陆 |
| zh-TW | 繁体中文 | /zh-tw/ | 台湾、香港 |
| ja | 日本語 | /ja/ | 日本 |
| ko | 한국어 | /ko/ | 韩国 |
| es | Español | /es/ | 西班牙语地区 |
| fr | Français | /fr/ | 法语地区 |
| de | Deutsch | /de/ | 德语地区 |
| pt | Português | /pt/ | 葡萄牙语地区 |
| x-default | - | / | 未匹配语言的默认页 |

### 5.2 HTML Head 完整模板

```html
<head>
  <!-- 当前页面语言声明 -->
  <html lang="en">
  
  <!-- hreflang 标签 - 每个页面都必须包含所有语言版本的引用 -->
  <link rel="alternate" hreflang="en" href="https://calculator-tools.todonot.com/en/scientific" />
  <link rel="alternate" hreflang="zh-CN" href="https://calculator-tools.todonot.com/zh/scientific" />
  <link rel="alternate" hreflang="zh-TW" href="https://calculator-tools.todonot.com/zh-tw/scientific" />
  <link rel="alternate" hreflang="ja" href="https://calculator-tools.todonot.com/ja/scientific" />
  <link rel="alternate" hreflang="ko" href="https://calculator-tools.todonot.com/ko/scientific" />
  <link rel="alternate" hreflang="es" href="https://calculator-tools.todonot.com/es/scientific" />
  <link rel="alternate" hreflang="fr" href="https://calculator-tools.todonot.com/fr/scientific" />
  <link rel="alternate" hreflang="de" href="https://calculator-tools.todonot.com/de/scientific" />
  <link rel="alternate" hreflang="pt" href="https://calculator-tools.todonot.com/pt/scientific" />
  <link rel="alternate" hreflang="x-default" href="https://calculator-tools.todonot.com/scientific" />
  
  <!-- canonical 标签 - 指向当前语言版本 -->
  <link rel="canonical" href="https://calculator-tools.todonot.com/en/scientific" />
</head>
```

### 5.3 Next.js 动态生成 hreflang（App Router）

```typescript
// app/[locale]/[tool]/layout.tsx
import { Metadata } from 'next';

const LOCALES = ['en', 'zh', 'zh-tw', 'ja', 'ko', 'es', 'fr', 'de', 'pt'];
const HREFLANG_MAP: Record<string, string> = {
  'en': 'en',
  'zh': 'zh-CN',
  'zh-tw': 'zh-TW',
  'ja': 'ja',
  'ko': 'ko',
  'es': 'es',
  'fr': 'fr',
  'de': 'de',
  'pt': 'pt',
};

export async function generateMetadata({ params }: { params: { locale: string; tool: string } }): Promise<Metadata> {
  const { locale, tool } = params;
  const baseUrl = 'https://calculator-tools.todonot.com';

  return {
    alternates: {
      canonical: `${baseUrl}/${locale}/${tool}`,
      languages: Object.fromEntries(
        LOCALES.map(l => [HREFLANG_MAP[l], `${baseUrl}/${l}/${tool}`])
      ),
    },
  };
}
```

### 5.4 hreflang 常见错误检查清单

- [ ] 每个页面的 hreflang 标签包含自身引用
- [ ] 所有语言版本之间互相引用（双向确认）
- [ ] x-default 指向最通用的版本
- [ ] hreflang 值使用 ISO 639-1 语言代码
- [ ] 地区代码使用 ISO 3166-1 Alpha-2（如 zh-CN 而非 zh-Hans）
- [ ] URL 使用绝对路径（包含 https://）
- [ ] canonical 标签与 hreflang 不冲突

---

## 6. 页面速度优化

### 6.1 图片懒加载

```html
<!-- 首屏图片：eager加载 + fetchpriority -->
<img src="/hero.webp" alt="工具首页" loading="eager" fetchpriority="high" width="1200" height="630">

<!-- 非首屏图片：懒加载 -->
<img src="/feature.webp" alt="功能介绍" loading="lazy" width="600" height="400" decoding="async">
```

```typescript
// 基于 Intersection Observer 的高级懒加载
const lazyImages = document.querySelectorAll('img[data-src]');
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target as HTMLImageElement;
      img.src = img.dataset.src!;
      img.removeAttribute('data-src');
      imageObserver.unobserve(img);
    }
  });
}, { rootMargin: '200px' }); // 提前200px开始加载

lazyImages.forEach(img => imageObserver.observe(img));
```

### 6.2 代码分割（Next.js）

```typescript
// 1. 动态导入 - 工具组件按需加载
import dynamic from 'next/dynamic';

const ScientificCalculator = dynamic(
  () => import('@/components/tools/ScientificCalculator'),
  { 
    loading: () => <CalculatorSkeleton />,
    ssr: true // 工具页保持SSR以利于SEO
  }
);

const MortgageChart = dynamic(
  () => import('@/components/charts/MortgageChart'),
  { ssr: false } // 图表组件不需要SSR
);

// 2. Route-based 代码分割（自动）
// Next.js App Router 自动按路由分割，无需额外配置

// 3. 第三方库按需加载
const loadMathJS = () => import('mathjs').then(m => m.default);

async function calculate(expression: string) {
  const math = await loadMathJS();
  return math.evaluate(expression);
}
```

### 6.3 CDN 配置

```typescript
// next.config.js - CDN 资源配置
const nextConfig = {
  // 静态资源使用CDN
  assetPrefix: 'https://cdn.todonot.com',
  
  images: {
    // 图片CDN域名
    domains: ['cdn.todonot.com'],
    // 图片优化格式
    formats: ['image/avif', 'image/webp'],
    // 设备尺寸断点
    deviceSizes: [640, 750, 828, 1080, 1200, 1920],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
  },

  // HTTP头配置
  async headers() {
    return [
      {
        source: '/:all*(svg|jpg|png|webp|avif|woff2)',
        headers: [
          { key: 'Cache-Control', value: 'public, max-age=31536000, immutable' },
        ],
      },
      {
        source: '/:path*',
        headers: [
          { key: 'X-Content-Type-Options', value: 'nosniff' },
          { key: 'X-Frame-Options', value: 'DENY' },
          { key: 'Strict-Transport-Security', value: 'max-age=63072000; includeSubDomains' },
        ],
      },
    ];
  },
};
```

**CDN 选择建议：**

| CDN | 适用场景 | 优势 |
|-----|---------|------|
| Cloudflare | 全站加速 | 免费计划、自动优化、边缘缓存 |
| Vercel Edge | Next.js部署 | 零配置、自动CDN |
| AWS CloudFront | 大流量站 | 全球节点、精细控制 |

### 6.4 资源优化清单

```bash
# 构建时自动优化
# package.json scripts
{
  "build": "next build",
  "postbuild": "npm run optimize",
  "optimize": "npm run optimize:images && npm run optimize:fonts",
  "optimize:images": "sharp-cli --input public/images --output public/images --webp --quality 80",
  "optimize:fonts": "subfont public/**/*.html --inline-css"
}
```

**关键优化项：**

- [ ] 所有图片转换为 WebP/AVIF 格式
- [ ] 字体文件使用 woff2 格式，子集化处理
- [ ] JavaScript bundle < 200KB（gzipped）
- [ ] CSS bundle < 50KB（gzipped）
- [ ] 启用 HTTP/2 或 HTTP/3
- [ ] 使用 Service Worker 缓存静态资源

---

## 7. Meta标签模板

### 7.1 完整 HTML Head 模板

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- 基础SEO Meta -->
  <title>免费在线科学计算器 - 三角函数/对数/指数运算 | TodoNot Tools</title>
  <meta name="description" content="免费在线科学计算器，支持三角函数、对数、指数、阶乘等高级数学运算。无需下载，打开即用，支持手机和电脑。">
  <meta name="keywords" content="科学计算器,在线计算器,三角函数计算,对数计算器,免费计算器">
  <meta name="author" content="TodoNot Tools">
  <meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1">
  
  <!-- Canonical & Language -->
  <link rel="canonical" href="https://calculator-tools.todonot.com/en/scientific">
  
  <!-- Open Graph (Facebook/LinkedIn) -->
  <meta property="og:type" content="website">
  <meta property="og:site_name" content="TodoNot Calculator Tools">
  <meta property="og:title" content="免费在线科学计算器 - 高级数学运算">
  <meta property="og:description" content="免费在线科学计算器，支持三角函数、对数、指数等高级运算。无需下载，打开即用。">
  <meta property="og:url" content="https://calculator-tools.todonot.com/en/scientific">
  <meta property="og:image" content="https://cdn.todonot.com/og/scientific-calculator-1200x630.png">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:image:alt" content="TodoNot Scientific Calculator Interface">
  <meta property="og:locale" content="en_US">
  <meta property="og:locale:alternate" content="zh_CN">
  <meta property="og:locale:alternate" content="ja_JP">
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="免费在线科学计算器">
  <meta name="twitter:description" content="支持三角函数、对数、指数等高级运算，打开即用">
  <meta name="twitter:image" content="https://cdn.todonot.com/og/scientific-calculator-1200x630.png">
  
  <!-- PWA & 移动端 -->
  <meta name="theme-color" content="#4F46E5">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <link rel="manifest" href="/manifest.json">
  <link rel="icon" type="image/svg+xml" href="/favicon.svg">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">
</head>
```

### 7.2 Title 标签最佳格式

**公式：** `[核心关键词] - [补充描述/功能] | [品牌名]`

| 页面类型 | Title 格式 | 示例 |
|----------|-----------|------|
| 工具首页 | 工具名 - 核心功能 \| 品牌 | `在线图片压缩 - 免费批量压缩PNG/JPG | TodoNot` |
| 工具详情 | 具体功能 - 使用场景 \| 品牌 | `PNG转WebP - 在线免费图片格式转换 | TodoNot` |
| 博客文章 | 文章标题 - 分类 \| 品牌 | `2026年最好用的5款在线计算器 | TodoNot Blog` |
| 分类页 | 分类名 - 工具列表 \| 品牌 | `数学计算器合集 - 20+免费在线工具 | TodoNot` |

**规则：**
- Title 长度：30-60字符（英文）/ 15-30字符（中文）
- 核心关键词放最前面
- 品牌名放最后，用 `|` 或 `-` 分隔
- 每个页面 Title 唯一，不重复

### 7.3 Description 最佳格式

**规则：**
- 长度：120-160字符（英文）/ 60-80字符（中文）
- 包含核心关键词（自然融入）
- 包含行动号召（CTA）
- 突出差异化卖点

```
✅ 好的: "免费在线科学计算器，支持三角函数、对数、指数运算。无需注册，打开即用，支持手机电脑。"
❌ 差的: "这是一个计算器网站，可以做各种计算。"
```

### 7.4 Next.js Metadata 生成函数

```typescript
// lib/metadata.ts
import { Metadata } from 'next';

interface ToolMetaParams {
  title: string;
  description: string;
  slug: string;
  locale: string;
  site: string; // e.g., 'calculator-tools'
  image?: string;
}

export function generateToolMetadata({ title, description, slug, locale, site, image }: ToolMetaParams): Metadata {
  const baseUrl = `https://${site}.todonot.com`;
  const url = `${baseUrl}/${locale}/${slug}`;
  const ogImage = image || `${baseUrl}/api/og?title=${encodeURIComponent(title)}&locale=${locale}`;

  return {
    title: `${title} | TodoNot Tools`,
    description,
    robots: { index: true, follow: true, 'max-image-preview': 'large' },
    alternates: { canonical: url },
    openGraph: {
      type: 'website',
      title,
      description,
      url,
      siteName: 'TodoNot Tools',
      images: [{ url: ogImage, width: 1200, height: 630, alt: title }],
      locale: locale === 'zh' ? 'zh_CN' : locale,
    },
    twitter: {
      card: 'summary_large_image',
      title,
      description,
      images: [ogImage],
    },
  };
}
```

---

## 8. 移动端优化清单

### 8.1 响应式设计基础

```html
<!-- viewport 必须配置 -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

```css
/* 移动优先的断点设计 */
/* 基础样式 = 移动端 */
.tool-container {
  padding: 1rem;
  width: 100%;
}

/* 平板 */
@media (min-width: 768px) {
  .tool-container {
    padding: 2rem;
    max-width: 720px;
    margin: 0 auto;
  }
}

/* 桌面 */
@media (min-width: 1024px) {
  .tool-container {
    max-width: 1200px;
  }
}
```

### 8.2 触摸友好设计

```css
/* 触摸目标最小 48x48px */
.btn, .link, input, select, textarea {
  min-height: 48px;
  min-width: 48px;
  padding: 12px 16px;
}

/* 按钮间距至少 8px */
.btn + .btn {
  margin-left: 8px;
}

/* 禁止双击缩放延迟 */
html {
  touch-action: manipulation;
}

/* 输入框字体至少16px防止iOS缩放 */
input, select, textarea {
  font-size: 16px;
}
```

### 8.3 移动端性能优化

```html
<!-- 条件加载：移动端不加载重型组件 -->
<script>
  if (window.innerWidth >= 1024) {
    import('/components/DesktopSidebar.js');
  }
</script>
```

```css
/* 使用 content-visibility 优化长页面渲染 */
.below-fold-section {
  content-visibility: auto;
  contain-intrinsic-size: 0 500px;
}
```

### 8.4 移动端SEO检查清单

| 检查项 | 要求 | 检测工具 |
|--------|------|----------|
| 移动端可用性 | 无移动端错误 | Google Search Console |
| 触摸元素间距 | ≥ 8px | Lighthouse |
| 字体大小 | ≥ 12px | Lighthouse |
| 视口配置 | 正确设置 viewport | PageSpeed Insights |
| 内容宽度 | 不超出视口 | Chrome DevTools |
| 可点击区域 | ≥ 48x48px | Lighthouse |
| 页面加载速度 | 3G网络 < 5s | WebPageTest |
| 无水平滚动 | 100vw内完成布局 | 手动测试 |
| 弹窗/插页广告 | 不遮挡主要内容 | 手动测试 |

### 8.5 PWA 配置（提升移动体验）

```json
// manifest.json
{
  "name": "TodoNot Calculator Tools",
  "short_name": "Calculator",
  "description": "免费在线计算器工具集",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#4F46E5",
  "icons": [
    { "src": "/icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icons/icon-512.png", "sizes": "512x512", "type": "image/png" },
    { "src": "/icons/icon-maskable.png", "sizes": "512x512", "type": "image/png", "purpose": "maskable" }
  ]
}
```

---

## 附录：技术SEO实施优先级

### 第一周（立即执行）

1. ✅ 配置 robots.txt（所有站点）
2. ✅ 生成并提交 sitemap.xml
3. ✅ 添加基础 Meta 标签模板
4. ✅ 配置 hreflang 标签
5. ✅ 确保所有页面有 canonical 标签

### 第二周（性能优化）

1. ✅ 图片全部转 WebP/AVIF
2. ✅ 实施代码分割和懒加载
3. ✅ 配置 CDN 和缓存策略
4. ✅ Core Web Vitals 达标

### 第三周（结构化数据）

1. ✅ 添加 WebApplication Schema
2. ✅ 添加 BreadcrumbList Schema
3. ✅ 添加 FAQPage Schema
4. ✅ 使用 Google Rich Results Test 验证

### 第四周（监控与迭代）

1. ✅ 设置 Core Web Vitals 监控
2. ✅ 检查 Search Console 覆盖率报告
3. ✅ 修复所有抓取错误
4. ✅ 移动端可用性测试通过

---

## 附录：推荐检测工具

| 工具 | 用途 | URL |
|------|------|-----|
| Google PageSpeed Insights | CWV检测 | pagespeed.web.dev |
| Google Search Console | 索引/覆盖率 | search.google.com/search-console |
| Google Rich Results Test | 结构化数据验证 | search.google.com/test/rich-results |
| Schema Markup Validator | Schema验证 | validator.schema.org |
| Ahrefs Site Audit | 全站技术SEO | ahrefs.com |
| Screaming Frog | 爬虫模拟 | screamingfrog.co.uk |
| WebPageTest | 性能详细分析 | webpagetest.org |
| Mobile-Friendly Test | 移动端友好度 | search.google.com/test/mobile-friendly |
