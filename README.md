# news-volume-mcp

News mention volume as an MCP tool. Plug into Claude, Cursor, or any MCP-compatible AI host. Weekly series, growth percentages, and live Google News feed.

Powered by **[trendsmcp.ai](https://trendsmcp.ai)** — one API key, one client, **30+ data sources**: Google Search, YouTube, TikTok, Reddit, Amazon, Wikipedia, App Store, Steam, npm, news volume, news sentiment, live trending feeds, and more. No separate credentials per platform.

**[Get your free API key → trendsmcp.ai](https://trendsmcp.ai)** — 100 free requests/month, no credit card.

📖 **[Full API docs → trendsmcp.ai/docs](https://trendsmcp.ai/docs)**

Updated for 2026. Works with Python 3.8 through 3.13.

## Quick install

Same four clients as the site hero. [Get a free key](https://www.trendsmcp.ai/account) first (100 req/mo). Claude and ChatGPT sign you in with OAuth. Cursor and VS Code: click, then put your key from `/account` if the deeplink used a placeholder.

<p align="center">
  <a href="https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Trends%20MCP&connectorUrl=https%3A%2F%2Fwww.trendsmcp.ai%2Fmcp"><img src="https://img.shields.io/badge/Add_to-Claude-DA7756?style=for-the-badge&logo=claude&logoColor=white" alt="Add to Claude"></a>
  <a href="cursor://anysphere.cursor-deeplink/mcp/install?name=trends-mcp&config=eyJ1cmwiOiJodHRwczovL2FwaS50cmVuZHNtY3AuYWkvbWNwIiwidHJhbnNwb3J0IjoiaHR0cCIsImhlYWRlcnMiOnsiQXV0aG9yaXphdGlvbiI6IkJlYXJlciBZT1VSX0FQSV9LRVkifX0%3D"><img src="https://img.shields.io/badge/Add_to-Cursor-000000?style=for-the-badge" alt="Add to Cursor"></a>
  <a href="https://chatgpt.com/plugins#settings/Connectors?create-connector=true&redirectAfter=%2Fplugins"><img src="https://img.shields.io/badge/Add_to-ChatGPT-10A37F?style=for-the-badge" alt="Add to ChatGPT"></a>
  <a href="https://www.trendsmcp.ai/account"><img src="https://img.shields.io/badge/Add_to-VS_Code-007ACC?style=for-the-badge&logo=visualstudiocode&logoColor=white" alt="Add to VS Code"></a>
</p>

| Client | After you click |
|---|---|
| **Claude** | Connector name and URL are prefilled (`https://www.trendsmcp.ai/mcp`). Confirm, then authorize. |
| **Cursor** | Approve the MCP install. Replace `YOUR_API_KEY` if prompted. |
| **ChatGPT** | Enable Developer mode (Profile → Settings → Security). Name `Trends MCP`, URL `https://www.trendsmcp.ai/mcp`, then authorize. |
| **VS Code** | Sign in on the account page and use the **VS Code** button so the key is included. |

Then ask: `Using TrendsMCP, what's trending on Google right now?`

## Use as an MCP tool

Add to your `mcp.json` (Claude Desktop, Cursor, Windsurf, VS Code, or any MCP host):

```json
{
  "mcpServers": {
    "trends-mcp": {
      "url": "https://api.trendsmcp.ai/mcp",
      "transport": "http",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

Get your free key at **[trendsmcp.ai](https://trendsmcp.ai)**. Full setup instructions for Claude, Cursor, Windsurf, and VS Code at **[trendsmcp.ai/docs](https://trendsmcp.ai/docs)**.

---

## No scraping. No 429 errors. No proxies.

If you have used pytrends or similar scrapers before, you know the problems: random `429 Too Many Requests` blocks, broken pipelines at 2am, time.sleep() hacks, proxy rotation costs, and a library that is now **archived** because Google explicitly flags scrapers at the protocol level.

trendsmcp is the managed alternative. We run the data infrastructure. You call a REST endpoint.

### pytrends alternative for News Volume data

| | Scrapers / pytrends | trendsmcp |
|---|---|---|
| 429 rate limit errors | constant | never |
| Proxy required | often | never |
| Breaks on platform changes | yes, regularly | no |
| Data sources covered | 1 (Google only) | 30+ |
| Absolute volume estimates | no | yes |
| Cross-platform growth | no | yes |
| Async support | no | yes |
| Actively maintained | no (archived) | yes |
| Free tier | no | yes, 100 req/month |

---

## Install

```bash
pip install news-volume-mcp
```

Zero system dependencies. Python 3.8 or later. Uses `httpx` under the hood.

---

## Quick start

```python
from news_volume_mcp import TrendsMcpClient, SOURCE

client = TrendsMcpClient(api_key="YOUR_API_KEY")

# 5-year weekly time series — no sleep(), no proxies, no 429s
series = client.get_trends(source=SOURCE, keyword="climate change")
print(series[0])
# TrendsDataPoint(date='2026-03-28', value=72, keyword='climate change', source='news volume')

# Period-over-period growth
growth = client.get_growth(
    source=SOURCE,
    keyword="climate change",
    percent_growth=["3M", "1Y"],
)
print(growth.results[0])
# GrowthResult(period='3M', growth=14.5, direction='increase', ...)

# What's trending right now (across all live platforms)
trending = client.get_top_trends(limit=10)
print(trending.data)
# [[1, 'topic one'], [2, 'topic two'], ...]
```

---

## Async support

```python
import asyncio
from news_volume_mcp import AsyncTrendsMcpClient, SOURCE

async def main():
    client = AsyncTrendsMcpClient(api_key="YOUR_API_KEY")
    series = await client.get_trends(source=SOURCE, keyword="climate change")
    print(series[0])

asyncio.run(main())
```

Query multiple platforms concurrently with one key:

```python
google, youtube, reddit, amazon, tiktok = await asyncio.gather(
    client.get_trends(source="google search", keyword="climate change"),
    client.get_trends(source="youtube",       keyword="climate change"),
    client.get_trends(source="reddit",        keyword="climate change"),
    client.get_trends(source="amazon",        keyword="climate change"),
    client.get_trends(source="tiktok",        keyword="climate change"),
)
```

---

## Use cases

- **SEO research**: track keyword search volume trends across Google Search, Google News, and Google Images before publishing content
- **Market research**: measure consumer demand signals on Amazon and Google Shopping before entering a product category
- **Investment research**: monitor Reddit discussion volume, news sentiment, and Wikipedia page view spikes as leading indicators
- **Content strategy**: find what is growing on YouTube and TikTok before topics peak and competition saturates them
- **Competitor tracking**: compare brand search volume growth across platforms over custom date ranges
- **App analytics**: track App Store interest and app download estimates alongside Reddit and news buzz

---

## Works with

- **Claude** (via MCP — [trendsmcp.ai/docs](https://trendsmcp.ai/docs))
- **Cursor** (via MCP — [trendsmcp.ai/docs](https://trendsmcp.ai/docs))
- **ChatGPT** (via MCP — [trendsmcp.ai/docs](https://trendsmcp.ai/docs))
- **Windsurf** (via MCP — [trendsmcp.ai/docs](https://trendsmcp.ai/docs))
- **VS Code Copilot** (via MCP — [trendsmcp.ai/docs](https://trendsmcp.ai/docs))
- **LangChain**: pass `TrendsMcpClient` output directly as tool results or context
- **CrewAI**: wrap any method as a `Tool` and drop it into your crew
- **AutoGen**: register as a callable tool for any agent
- **LlamaIndex**: use trend series as structured data nodes for retrieval
- **Pandas**: each `get_trends()` response converts to a DataFrame in one line

---

## Methods

### `get_trends(source, keyword, data_mode=None)`

Returns a historical time series for a keyword. Defaults to 5 years of weekly data. Pass `data_mode="daily"` for the last 30 days at daily granularity.

### `get_growth(source, keyword, percent_growth, data_mode=None)`

Calculates percentage growth between two points in time. Pass preset strings or `CustomGrowthPeriod` objects.

**Growth presets:** `7D` `14D` `30D` `1M` `2M` `3M` `6M` `9M` `12M` `1Y` `18M` `24M` `2Y` `36M` `3Y` `48M` `60M` `5Y` `MTD` `QTD` `YTD`

### `get_top_trends(type=None, limit=None)`

Returns today's live trending items. Omit `type` to get all feeds at once.

**Available live feeds:** `Google Trends` `Google News Top News` `YouTube Trending` `TikTok Trending Hashtags` `X (Twitter) Trending` `Reddit Hot Posts` `Reddit World News` `Wikipedia Trending` `Amazon Best Sellers Top Rated` `Amazon Best Sellers by Category` `App Store Top Free` `App Store Top Paid` `Google Play` `Spotify Top Podcasts` `Top Websites`

---

## All 30+ data sources

One API key. One client. Every platform. No separate credentials for each.

| source | What it measures |
|---|---|
| `"google search"` | Google Search volume |
| `"google images"` | Google Images search volume |
| `"google news"` | Google News search volume |
| `"google shopping"` | Google Shopping purchase intent |
| `"youtube"` | YouTube search volume |
| `"tiktok"` | TikTok hashtag volume |
| `"reddit"` | Reddit subreddit subscribers over time |
| `"amazon"` | Amazon product search volume |
| `"wikipedia"` | Wikipedia page views |
| `"news volume"` | News article mention count |
| `"news sentiment"` | News sentiment score (positive/negative) |
| `"app downloads"` | Mobile app download/install estimates (Android) |
| `"npm"` | npm package weekly downloads |
| `"steam"` | Steam concurrent player count |

All values normalized 0–100 so you can compare across platforms directly.

---

## Error handling

```python
from news_volume_mcp import TrendsMcpClient, TrendsMcpError, SOURCE

client = TrendsMcpClient(api_key="YOUR_API_KEY")

try:
    series = client.get_trends(source=SOURCE, keyword="climate change")
except TrendsMcpError as e:
    print(e.status)   # e.g. 429 if you exceed your plan quota
    print(e.code)     # e.g. "rate_limited"
    print(e.message)
```

---

## Frequently asked questions

**Does this scrape News Volume?**
No. trendsmcp runs managed data infrastructure. Your Python code makes a single authenticated REST call. No scraping, no Selenium, no cookies, no proxies required.

**Do I need a News Volume developer account, OAuth token, or platform API key?**
No. One trendsmcp API key gives you access to all 30+ data sources.

**Will it break when News Volume changes its backend?**
No. API stability is our responsibility. If something changes upstream, we update the backend. Your code keeps working.

**Can I query multiple platforms with the same key?**
Yes. One key covers every data source. Switch `source` to any of the 30+ values listed above.

**Is there a free tier?**
Yes, 100 requests per month, no credit card required. [Get your key at trendsmcp.ai](https://trendsmcp.ai).

**Can I use this in production data pipelines?**
Yes. The client is stateless, thread-safe, and supports async for concurrent queries across multiple platforms.

---

## Related packages

- [trendsmcp](https://pypi.org/project/trendsmcp/) — core package, all 30+ data sources
- [youtube-trends-api](https://pypi.org/project/youtube-trends-api/) / [youtube-trends-mcp](https://pypi.org/project/youtube-trends-mcp/) / [youtube-trends-agent](https://pypi.org/project/youtube-trends-agent/)
- [reddit-trends-api](https://pypi.org/project/reddit-trends-api/) / [reddit-trends-mcp](https://pypi.org/project/reddit-trends-mcp/) / [reddit-trends-agent](https://pypi.org/project/reddit-trends-agent/)
- [google-search-trends-api](https://pypi.org/project/google-search-trends-api/) / [google-search-trends-mcp](https://pypi.org/project/google-search-trends-mcp/) / [google-search-trends-agent](https://pypi.org/project/google-search-trends-agent/)
- [amazon-trends-api](https://pypi.org/project/amazon-trends-api/) / [amazon-trends-mcp](https://pypi.org/project/amazon-trends-mcp/) / [amazon-trends-agent](https://pypi.org/project/amazon-trends-agent/)
- [tiktok-trends-api](https://pypi.org/project/tiktok-trends-api/) / [tiktok-trends-mcp](https://pypi.org/project/tiktok-trends-mcp/) / [tiktok-trends-agent](https://pypi.org/project/tiktok-trends-agent/)
- [wikipedia-trends-api](https://pypi.org/project/wikipedia-trends-api/) / [wikipedia-trends-mcp](https://pypi.org/project/wikipedia-trends-mcp/) / [wikipedia-trends-agent](https://pypi.org/project/wikipedia-trends-agent/)
- [npm-trends-api](https://pypi.org/project/npm-trends-api/) / [npm-trends-mcp](https://pypi.org/project/npm-trends-mcp/) / [npm-trends-agent](https://pypi.org/project/npm-trends-agent/)
- [steam-trends-api](https://pypi.org/project/steam-trends-api/) / [steam-trends-mcp](https://pypi.org/project/steam-trends-mcp/) / [steam-trends-agent](https://pypi.org/project/steam-trends-agent/)
- [app-store-trends-api](https://pypi.org/project/app-store-trends-api/) / [app-store-trends-mcp](https://pypi.org/project/app-store-trends-mcp/) / [app-store-trends-agent](https://pypi.org/project/app-store-trends-agent/)
- [news-volume-api](https://pypi.org/project/news-volume-api/) / [news-volume-mcp](https://pypi.org/project/news-volume-mcp/) / [news-volume-agent](https://pypi.org/project/news-volume-agent/)
- [news-sentiment-api](https://pypi.org/project/news-sentiment-api/) / [news-sentiment-mcp](https://pypi.org/project/news-sentiment-mcp/) / [news-sentiment-agent](https://pypi.org/project/news-sentiment-agent/)

---

## Links

- [API docs](https://trendsmcp.ai/docs)
- [Get a free API key](https://trendsmcp.ai)
- [pytrends alternative](https://trendsmcp.ai/pytrends-alternative)
- [All packages on PyPI](https://pypi.org/user/trendsmcp/)
- [GitHub](https://github.com/trendsmcp/trendsmcp-py)

---

## License

MIT
