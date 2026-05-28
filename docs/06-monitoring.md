# 排名追踪与流量监控方案

> 适用对象：todonot.com 工具站矩阵（calculator-tools、image-compress-tools、excalidraw-hub、temp-mail-tools）
> 更新日期：2026-05-28
> 目标：建立完整的SEO监控体系，实时追踪关键词排名变化和流量增长

---

## 1. Google Search Console 配置

### 1.1 Property 类型选择

| 类型 | 适用场景 | 推荐 |
|------|----------|------|
| Domain Property | 覆盖所有子域名+协议 | ✅ 首选 |
| URL Prefix | 单个子域名精确监控 | 补充使用 |

### 1.2 Domain Property 验证（推荐）

**步骤：**

1. 登录 [Google Search Console](https://search.google.com/search-console)
2. 点击左上角下拉 → "添加资源"
3. 选择 "网域" 类型，输入 `todonot.com`
4. 选择 DNS 验证方式
5. 在域名 DNS 管理面板添加 TXT 记录：
   ```
   类型: TXT
   主机: @
   值: google-site-verification=xxxxxxxxxxxxxxxx
   TTL: 3600
   ```
6. 等待 DNS 生效（通常 5-30 分钟），点击验证

### 1.3 子域名单独配置（URL Prefix）

为每个工具站单独添加 URL Prefix Property：

```
https://calculator-tools.todonot.com
https://image-compress-tools.todonot.com
https://excalidraw-hub.todonot.com
https://temp-mail-tools.todonot.com
```

**验证方法优先级：**
1. HTML 标签（最快）：在 `<head>` 中添加 `<meta name="google-site-verification" content="xxx" />`
2. HTML 文件：上传 `googlexxxxxxxx.html` 到根目录
3. Google Analytics：如已安装 GA4 可自动验证

### 1.4 配置完成后设置

- [ ] 提交 sitemap：`https://todonot.com/sitemap.xml`
- [ ] 各子域名分别提交 sitemap
- [ ] 设置国际定位（如有多语言）
- [ ] 关联 Google Analytics 4 账户
- [ ] 添加团队成员权限（完整/受限）

---

## 2. 排名追踪工具选型对比

### 2.1 免费工具

| 工具 | 价格 | 关键词数 | 功能特点 | 推荐理由 |
|------|------|----------|----------|----------|
| Google Search Console | 免费 | 无限 | 真实排名数据、点击率、展示量 | 必装，Google官方数据源 |
| Bing Webmaster Tools | 免费 | 无限 | Bing排名数据 | 补充监控 |
| Ubersuggest（免费版） | 免费 | 3次/天 | 关键词建议、排名检查 | 快速验证 |
| Google Keyword Planner | 免费 | 无限 | 搜索量趋势 | 关键词研究 |
| Whatsmyserp | 免费 | 25个 | 每日排名检查 | 小规模监控 |

### 2.2 付费工具

| 工具 | 月费 | 关键词数 | 功能特点 | 推荐理由 |
|------|------|----------|----------|----------|
| Ahrefs | $99+ | 750+ | 外链分析、排名追踪、竞品分析 | 综合最强，外链数据最全 |
| SEMrush | $119+ | 500+ | 排名追踪、广告分析、内容审计 | 功能最全面 |
| SERPRobot | $4.99+ | 100+ | 纯排名追踪、每日更新 | 性价比高 |
| AccuRanker | $116+ | 1000 | 实时排名、API支持 | 大规模追踪 |
| SE Ranking | $44+ | 250+ | 排名+审计+外链 | 中小站首选 |
| Mangools (SERPWatcher) | $29+ | 200+ | 排名追踪、关键词难度 | 界面友好，入门首选 |

### 2.3 推荐组合（todonot.com）

**启动阶段（预算有限）：**
- Google Search Console（免费，必装）
- SERPRobot（$4.99/月，100个关键词日检）
- 自建 Python 脚本（免费，补充监控）

**成长阶段（有预算）：**
- Google Search Console + Ahrefs（$99/月）
- 或 SE Ranking（$44/月，性价比最优）

---

## 3. 流量监控方案（Google Analytics 4）

### 3.1 GA4 基础配置

**步骤：**

1. 访问 [analytics.google.com](https://analytics.google.com)
2. 创建账号 → 创建媒体资源（Property）
3. 媒体资源名称：`todonot.com 工具站矩阵`
4. 创建数据流：选择 "网站"，输入 `todonot.com`
5. 获取衡量 ID（格式：`G-XXXXXXXXXX`）

**安装代码（所有子域名统一）：**

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX', {
    cookie_domain: '.todonot.com',
    cookie_flags: 'SameSite=None;Secure'
  });
</script>
```

### 3.2 跨子域名追踪配置

在 GA4 管理面板中：
1. 数据流 → 配置标记设置 → 配置网域
2. 添加条件：`todonot.com`（包含匹配）
3. 这样所有 `*.todonot.com` 子域名共享同一会话

### 3.3 自定义事件追踪

```html
<script>
// 工具使用事件
function trackToolUsage(toolName, action) {
  gtag('event', 'tool_usage', {
    tool_name: toolName,
    action: action,
    page_location: window.location.href
  });
}

// 转化事件：用户完成工具操作
function trackConversion(toolName) {
  gtag('event', 'conversion_complete', {
    tool_name: toolName,
    value: 1
  });
}

// SEO相关事件
gtag('event', 'page_view_extended', {
  page_referrer: document.referrer,
  is_organic: document.referrer.includes('google.com') || document.referrer.includes('bing.com'),
  landing_page: window.location.pathname
});
</script>
```

### 3.4 GA4 关键报告配置

| 报告 | 用途 | 配置路径 |
|------|------|----------|
| 流量获取 | 自然搜索流量趋势 | 报告 → 流量获取 → 流量获取概览 |
| 着陆页 | 各页面SEO表现 | 报告 → 互动 → 着陆页 |
| 搜索控制台 | GSC数据整合 | 管理 → Search Console关联 |
| 自定义探索 | 深度分析 | 探索 → 自由形式 |

---

## 4. 关键指标定义（KPI）

### 4.1 核心 KPI 列表

| 指标 | 定义 | 数据源 | 监控频率 | 目标值（1个月） |
|------|------|--------|----------|----------------|
| 关键词排名 | 目标关键词在 Google SERP 的位置 | GSC / 排名工具 | 每日 | Top 10 进入 20+ 个词 |
| 自然搜索点击量 | 来自搜索引擎的总点击数 | GSC | 每日 | 周增长 15%+ |
| 平均点击率（CTR） | 点击数 / 展示数 | GSC | 每周 | >3% |
| 索引页面数 | Google 已收录的页面数量 | GSC Coverage | 每周 | 100% 目标页收录 |
| 外链数量 | 指向站点的外部链接总数 | Ahrefs / GSC | 每周 | 月增 50+ |
| 引荐域名数 | 不同域名的外链来源数 | Ahrefs | 每周 | 月增 20+ |
| 页面加载速度 | Core Web Vitals (LCP/FID/CLS) | PageSpeed Insights | 每周 | LCP<2.5s |
| 跳出率 | 单页会话占比 | GA4 | 每周 | <60% |
| 平均会话时长 | 用户停留时间 | GA4 | 每周 | >1分钟 |
| 工具使用转化率 | 使用工具的用户占比 | GA4 自定义事件 | 每周 | >40% |

### 4.2 各子站独立 KPI

| 子站 | 核心关键词（示例） | 排名目标 | 月流量目标 |
|------|-------------------|----------|-----------|
| calculator-tools | online calculator, unit converter | Top 10 | 5,000 |
| image-compress-tools | compress image online, image optimizer | Top 10 | 8,000 |
| excalidraw-hub | excalidraw alternative, whiteboard online | Top 20 | 3,000 |
| temp-mail-tools | temporary email, disposable email | Top 10 | 10,000 |

### 4.3 指标健康度评级

| 等级 | 颜色 | 条件 |
|------|------|------|
| 优秀 | 🟢 | 达到或超过目标值 |
| 正常 | 🟡 | 目标值的 60%-100% |
| 警告 | 🟠 | 目标值的 30%-60% |
| 危险 | 🔴 | 低于目标值的 30% |

---

## 5. 自动化监控脚本

### 5.1 排名检查 + 告警脚本

```python
#!/usr/bin/env python3
"""
SEO 排名监控脚本 - todonot.com
功能：定期检查关键词排名变化，异常时发送告警
依赖：pip install requests google-auth google-auth-oauthlib
"""

import json
import os
import smtplib
from datetime import datetime, timedelta
from email.mime.text import MIMEText
from pathlib import Path

import requests

# ============ 配置 ============
CONFIG = {
    "site_url": "sc-domain:todonot.com",
    "credentials_file": "credentials.json",
    "data_dir": "./ranking_data",
    "alert_email": "admin@todonot.com",
    "smtp_host": "smtp.gmail.com",
    "smtp_port": 587,
    "smtp_user": os.getenv("SMTP_USER", ""),
    "smtp_pass": os.getenv("SMTP_PASS", ""),
    # 告警阈值
    "rank_drop_threshold": 5,      # 排名下降超过5位告警
    "traffic_drop_threshold": 0.3,  # 流量下降超过30%告警
}

KEYWORDS = {
    "calculator-tools.todonot.com": [
        "online calculator", "unit converter", "percentage calculator",
    ],
    "image-compress-tools.todonot.com": [
        "compress image online", "image optimizer", "reduce image size",
    ],
    "excalidraw-hub.todonot.com": [
        "excalidraw", "online whiteboard", "drawing tool online",
    ],
    "temp-mail-tools.todonot.com": [
        "temporary email", "disposable email", "temp mail",
    ],
}


def get_gsc_data(service, site_url, start_date, end_date, keyword=None):
    """从 Google Search Console API 获取数据"""
    body = {
        "startDate": start_date,
        "endDate": end_date,
        "dimensions": ["query", "page"],
        "rowLimit": 1000,
    }
    if keyword:
        body["dimensionFilterGroups"] = [{
            "filters": [{"dimension": "query", "expression": keyword}]
        }]
    resp = service.searchanalytics().query(siteUrl=site_url, body=body).execute()
    return resp.get("rows", [])


def load_previous_data(keyword):
    """加载上次排名数据"""
    data_dir = Path(CONFIG["data_dir"])
    data_dir.mkdir(exist_ok=True)
    filepath = data_dir / f"{keyword.replace(' ', '_')}.json"
    if filepath.exists():
        return json.loads(filepath.read_text())
    return None


def save_current_data(keyword, data):
    """保存当前排名数据"""
    data_dir = Path(CONFIG["data_dir"])
    data_dir.mkdir(exist_ok=True)
    filepath = data_dir / f"{keyword.replace(' ', '_')}.json"
    filepath.write_text(json.dumps(data, indent=2))


def check_ranking_change(keyword, current_position, previous_data):
    """检查排名变化，返回告警信息"""
    if not previous_data:
        return None
    prev_position = previous_data.get("position", 0)
    change = current_position - prev_position  # 正数=下降
    if change > CONFIG["rank_drop_threshold"]:
        return {
            "keyword": keyword,
            "previous": prev_position,
            "current": current_position,
            "change": change,
            "severity": "critical" if change > 10 else "warning",
        }
    return None


def send_alert(alerts):
    """发送告警邮件"""
    if not alerts or not CONFIG["smtp_user"]:
        return
    subject = f"[SEO告警] todonot.com - {len(alerts)}个关键词排名异常"
    body = "排名异常告警：\n\n"
    for alert in alerts:
        body += (
            f"关键词: {alert['keyword']}\n"
            f"排名变化: {alert['previous']} → {alert['current']} (下降{alert['change']}位)\n"
            f"严重程度: {alert['severity']}\n\n"
        )
    msg = MIMEText(body, "plain", "utf-8")
    msg["Subject"] = subject
    msg["From"] = CONFIG["smtp_user"]
    msg["To"] = CONFIG["alert_email"]
    with smtplib.SMTP(CONFIG["smtp_host"], CONFIG["smtp_port"]) as server:
        server.starttls()
        server.login(CONFIG["smtp_user"], CONFIG["smtp_pass"])
        server.send_message(msg)
    print(f"[{datetime.now()}] 告警邮件已发送，共 {len(alerts)} 条告警")


def run_monitoring():
    """主监控流程"""
    from google.oauth2.credentials import Credentials
    from googleapiclient.discovery import build

    creds = Credentials.from_authorized_user_file(CONFIG["credentials_file"])
    service = build("searchconsole", "v1", credentials=creds)

    today = datetime.now().strftime("%Y-%m-%d")
    week_ago = (datetime.now() - timedelta(days=7)).strftime("%Y-%m-%d")

    alerts = []
    report = []

    for domain, keywords in KEYWORDS.items():
        for keyword in keywords:
            rows = get_gsc_data(service, CONFIG["site_url"], week_ago, today, keyword)
            if rows:
                current_position = rows[0].get("position", 100)
                clicks = rows[0].get("clicks", 0)
                impressions = rows[0].get("impressions", 0)
            else:
                current_position = 100
                clicks = 0
                impressions = 0

            previous = load_previous_data(keyword)
            alert = check_ranking_change(keyword, current_position, previous)
            if alert:
                alerts.append(alert)

            data = {
                "keyword": keyword,
                "domain": domain,
                "position": current_position,
                "clicks": clicks,
                "impressions": impressions,
                "date": today,
            }
            save_current_data(keyword, data)
            report.append(data)

    if alerts:
        send_alert(alerts)

    # 输出报告
    print(f"\n{'='*60}")
    print(f"SEO 监控报告 - {today}")
    print(f"{'='*60}")
    for item in report:
        status = "🔴" if item["position"] > 50 else "🟡" if item["position"] > 10 else "🟢"
        print(f"{status} [{item['domain']}] {item['keyword']}: 排名#{item['position']:.0f} | 点击:{item['clicks']} | 展示:{item['impressions']}")
    print(f"{'='*60}")
    print(f"告警数: {len(alerts)}")


if __name__ == "__main__":
    run_monitoring()
```

### 5.2 Cron 定时任务配置

```bash
# 每天早上 8:00 执行排名检查
0 8 * * * cd /opt/seo-monitor && python3 rank_monitor.py >> /var/log/seo-monitor.log 2>&1

# 每周一生成周报
0 9 * * 1 cd /opt/seo-monitor && python3 generate_report.py >> /var/log/seo-report.log 2>&1
```

### 5.3 GSC API 认证设置

```bash
# 1. 在 Google Cloud Console 创建项目
# 2. 启用 Search Console API
# 3. 创建 OAuth 2.0 凭据（桌面应用类型）
# 4. 下载 credentials.json
# 5. 首次运行授权：

pip install google-auth google-auth-oauthlib google-api-python-client requests

python3 -c "
from google_auth_oauthlib.flow import InstalledAppFlow
flow = InstalledAppFlow.from_client_secrets_file(
    'client_secret.json',
    scopes=['https://www.googleapis.com/auth/webmasters.readonly']
)
creds = flow.run_local_server(port=0)
import json
with open('credentials.json', 'w') as f:
    f.write(creds.to_json())
print('认证成功！')
"
```

---

## 6. 周报/月报模板

### 6.1 SEO 周报模板

```markdown
# SEO 周报 - todonot.com

**报告周期：** 2026-05-19 ~ 2026-05-25
**生成时间：** 2026-05-26

## 📊 核心指标概览

| 指标 | 本周 | 上周 | 变化 | 趋势 |
|------|------|------|------|------|
| 自然搜索点击 | 1,234 | 1,100 | +12.2% | 📈 |
| 搜索展示量 | 45,000 | 40,000 | +12.5% | 📈 |
| 平均点击率 | 2.7% | 2.75% | -0.05% | ➡️ |
| 平均排名 | 18.5 | 20.2 | +1.7 | 📈 |
| 索引页面数 | 156 | 150 | +6 | 📈 |
| 新增外链 | 12 | 8 | +4 | 📈 |

## 🏆 排名变化 Top 10

| 关键词 | 当前排名 | 上周排名 | 变化 | 页面 |
|--------|----------|----------|------|------|
| online calculator | 8 | 12 | ↑4 | /calculator |
| compress image | 15 | 18 | ↑3 | /compress |
| temp mail | 6 | 7 | ↑1 | /temp-mail |
| ... | ... | ... | ... | ... |

## ⚠️ 排名下降关键词

| 关键词 | 当前排名 | 上周排名 | 变化 | 可能原因 |
|--------|----------|----------|------|----------|
| unit converter | 25 | 18 | ↓7 | 竞品更新内容 |
| ... | ... | ... | ... | ... |

## 📈 各子站流量

| 子站 | 本周点击 | 上周点击 | 变化 |
|------|----------|----------|------|
| calculator-tools | 450 | 400 | +12.5% |
| image-compress-tools | 380 | 350 | +8.6% |
| excalidraw-hub | 204 | 180 | +13.3% |
| temp-mail-tools | 200 | 170 | +17.6% |

## 🎯 本周行动项

- [ ] 优化 unit converter 页面内容
- [ ] 新增 3 篇博客文章
- [ ] 提交 2 个 guest post

## 📋 下周计划

1. 重点优化排名下降的关键词页面
2. 继续外链建设
3. 发布新工具页面
```

### 6.2 SEO 月报模板

```markdown
# SEO 月报 - todonot.com

**报告月份：** 2026年5月
**生成时间：** 2026-06-01

## 📊 月度核心指标

| 指标 | 本月 | 上月 | 环比 | 目标 | 达成率 |
|------|------|------|------|------|--------|
| 自然搜索点击 | 5,200 | 3,800 | +36.8% | 5,000 | 104% ✅ |
| 搜索展示量 | 180,000 | 140,000 | +28.6% | 150,000 | 120% ✅ |
| 平均 CTR | 2.9% | 2.7% | +0.2% | 3.0% | 97% 🟡 |
| Top 10 关键词数 | 25 | 15 | +10 | 20 | 125% ✅ |
| 索引页面数 | 180 | 120 | +60 | 150 | 120% ✅ |
| 外链总数 | 320 | 250 | +70 | 300 | 107% ✅ |
| 引荐域名数 | 45 | 30 | +15 | 40 | 113% ✅ |

## 📈 排名分布变化

| 排名区间 | 本月关键词数 | 上月关键词数 | 变化 |
|----------|-------------|-------------|------|
| Top 3 | 5 | 2 | +3 |
| Top 4-10 | 20 | 13 | +7 |
| Top 11-20 | 35 | 25 | +10 |
| Top 21-50 | 60 | 50 | +10 |
| 50+ | 80 | 110 | -30 |

## 🏆 本月最大进步关键词

| 关键词 | 月初排名 | 月末排名 | 提升 | 月点击量 |
|--------|----------|----------|------|----------|
| ... | ... | ... | ... | ... |

## 💡 分析与建议

### 成功因素
1. ...

### 需改进
1. ...

### 下月重点
1. ...
```

---

## 7. 异常告警规则

### 7.1 告警规则定义

| 告警类型 | 触发条件 | 严重程度 | 响应时间 | 处理方式 |
|----------|----------|----------|----------|----------|
| 排名暴跌 | 单个关键词排名下降 >10 位 | 🔴 严重 | 24h内 | 检查页面是否被惩罚、内容是否被抄袭 |
| 排名下降 | 单个关键词排名下降 5-10 位 | 🟠 警告 | 48h内 | 分析竞品变化、优化页面内容 |
| 流量骤降 | 日流量较7日均值下降 >30% | 🔴 严重 | 12h内 | 检查服务器状态、索引状态、算法更新 |
| 流量下降 | 周流量环比下降 >15% | 🟠 警告 | 72h内 | 分析流量来源变化 |
| 索引异常 | 索引页面数下降 >10% | 🔴 严重 | 24h内 | 检查 robots.txt、sitemap、服务器错误 |
| 索引减少 | 新页面 48h 未被收录 | 🟡 提示 | 72h内 | 手动请求索引、检查页面质量 |
| 外链丢失 | 单日丢失外链 >5 条 | 🟠 警告 | 48h内 | 确认链接页面状态、联系站长 |
| 服务器错误 | 5xx 错误率 >1% | 🔴 严重 | 1h内 | 检查服务器日志、修复错误 |
| Core Web Vitals | LCP >4s 或 CLS >0.25 | 🟠 警告 | 1周内 | 性能优化 |

### 7.2 告警通知渠道

| 严重程度 | 通知方式 | 接收人 |
|----------|----------|--------|
| 🔴 严重 | 邮件 + 即时通讯（Slack/飞书） | 全团队 |
| 🟠 警告 | 邮件 | SEO负责人 |
| 🟡 提示 | 周报汇总 | SEO负责人 |

### 7.3 告警处理 SOP

**排名暴跌处理流程：**

```
1. 确认数据准确性（排除工具误报）
2. 检查 GSC 是否有手动操作通知
3. 检查页面是否可正常访问（HTTP状态码）
4. 检查 robots.txt 是否误封
5. 查看是否有 Google 算法更新（搜索 "Google algorithm update"）
6. 分析竞品是否有重大变化
7. 检查页面内容是否被修改/删除
8. 制定恢复计划并执行
9. 持续监控 7 天确认恢复
```

**流量骤降处理流程：**

```
1. 确认服务器正常运行
2. 检查 GA4 代码是否正常触发
3. 分析流量下降的具体来源（自然搜索/直接/引荐）
4. 如为自然搜索下降 → 按排名暴跌流程处理
5. 如为全渠道下降 → 检查DNS/CDN/服务器问题
6. 检查是否有季节性因素
7. 记录事件并更新监控基线
```

### 7.4 Google 算法更新应对

| 更新类型 | 影响范围 | 应对策略 |
|----------|----------|----------|
| Core Update | 全站排名波动 | 等待2周稳定，分析受影响页面质量 |
| Spam Update | 低质量内容降权 | 审查内容质量，删除薄内容页面 |
| Link Spam | 外链相关 | 审查外链质量，disavow 垃圾链接 |
| Helpful Content | 内容有用性 | 确保内容满足用户搜索意图 |

---

## 8. 监控体系实施时间表

| 阶段 | 时间 | 任务 | 产出 |
|------|------|------|------|
| 第1天 | Day 1 | GSC 验证 + GA4 安装 | 基础数据采集开始 |
| 第2-3天 | Day 2-3 | 配置排名追踪工具 + 脚本部署 | 自动化监控上线 |
| 第1周末 | Day 7 | 产出第一份周报 | 基线数据建立 |
| 第2周 | Day 8-14 | 优化告警阈值、补充关键词 | 监控体系稳定运行 |
| 第1月末 | Day 30 | 产出第一份月报 | 完整数据对比 |

---

## 附录：常用命令速查

```bash
# 手动触发排名检查
python3 rank_monitor.py

# 查看最近告警日志
tail -50 /var/log/seo-monitor.log

# 快速检查某个关键词的 Google 排名（非API方式，仅供参考）
curl -s "https://www.google.com/search?q=online+calculator&num=100" | grep -o 'todonot.com[^"]*' | head -5

# 检查页面索引状态
curl -s "https://www.google.com/search?q=site:todonot.com" | grep -o "About [0-9,]* results"

# 检查 robots.txt
curl -s https://todonot.com/robots.txt

# 检查 sitemap
curl -s https://todonot.com/sitemap.xml | head -20
```
