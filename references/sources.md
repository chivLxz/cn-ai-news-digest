# RSS 数据源配置

## 数据源状态（v1.2.0）

共 10 个源，全部走 RSS，无第三方抓取库、无浏览器渲染。

### 媒体站

| 媒体 | 优先级 | RSS 地址 | 状态 |
|------|--------|----------|------|
| 量子位 | P0 | `https://www.qbitai.com/feed` | ✅ 稳定 |
| 36氪 | P0 | `https://36kr.com/feed` | ✅ 稳定 |
| 机器之心 | P0 | `https://www.jiqizhixin.com/rss` | ⚠️ XML 偶有格式异常，已做容错处理 |
| InfoQ 中文 | P0 | `https://www.infoq.cn/feed` | ✅ 稳定 |
| 爱范儿 | P1 | `https://www.ifanr.com/feed` | ✅ 稳定 |
| 钛媒体 | P1 | `https://www.tmtpost.com/rss` | ✅ 稳定 |
| 雷锋网 | P1 | `https://www.leiphone.com/feed` | ✅ 稳定 |
| 智东西 | P1 | `https://zhidx.com/rss.xml` | ✅ 稳定 |

### 微信公众号（经 supsub.net 转出 feed）

公众号本身不提供 RSS，这两个地址由第三方转出，可用性依赖该服务，失效时需重新获取。

| 媒体 | 优先级 | RSS 地址 | 状态 |
|------|--------|----------|------|
| 机器之心公众号 | P1 | `https://supsub.net/feed/public/58f382c9/rss` | ✅ 稳定 |
| 智能涌现公众号 | P1 | `https://supsub.net/feed/public/bb93f651/rss` | ✅ 稳定 |

## 容错机制

解析按三级依次降级，任一级成功即返回：

1. 标准 XML 解析（`xml.etree.ElementTree`）
2. `html.parser.HTMLParser` 逐标签拾取（结构不严格但可读的 feed）
3. 正则提取 `<item>` 块（彻底破损的 XML）

另外两项兜底：

- 机器之心 RSS 偶尔出现 XML 格式错误（mismatched tag），会落到第 2、3 级
- 日期过滤后当天不足 20 篇时，自动扩展至近 3 天，避免板块空着

## 数据来源说明

RSS 地址参考自 [tech-news-digest](https://github.com/draco-agent/tech-news-digest) 的中文源配置。
