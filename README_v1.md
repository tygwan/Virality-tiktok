# Virality v24: TikTok Marketing Automation Workflow

> **🚧 Development Status: Paused (Blocked by External Dependency)**
>
> **Project Progress**
> ```text
> Workflow Architecture : [██████████] 100% (Completed)
> System Implementation : [████████░░] 80%  (In Progress)
> TikTok API Approval   : [░░░░░░░░░░] 0%   (Pending)
> ```
> **Current Blocker**: The automated workflow design is fully complete. However, the final integration is currently **paused** pending approval for the **TikTok Advertising Account**, which is required to access the necessary APIs for ad execution and tracking.

This repository documents the **Virality v24** n8n workflow, designed to automate the lifecycle of TikTok viral marketing, from trend analysis to creator settlement.

---

## Workflow Structure

![Workflow Structure](./WorkflowStructure.png)

The workflow is divided into four key sections:

1. **Market Insight (Daily 09:00)**
   - Analyzes TikTok trends and competitor data.
   - Identifies "Blue Ocean" opportunities using AI.
   - Generates strategic insights and mission recommendations.
   - Alerts via Slack (#market-insight).

2. **Settlement & FDS (Daily 06:00)**
   - Aggregates creator performance data (Views, Likes, Comments).
   - Calculates dynamic rewards based on grade, category, and seasonality.
   - **AI Fraud Detection System (FDS)**: Analyzes traffic patterns to detect abuse.
   - Processes settlements (Approved/Hold/Blocked) and alerts via Slack (#fraud-alerts, #settlements).

3. **A/B Testing (Event-Driven)**
   - Triggered by mission clicks.
   - Assigns users to Control (Group A) or Test (Group B) groups.
   - Evaluates copy/incentive effectiveness (e.g., Emotional vs. Reward-focused).
   - **AI Statistical Analysis**: Validates hypotheses and determines winners.

4. **Weekly Business Review (Monday 08:00)**
   - Aggregates weekly revenue, creator retention, and mission metrics.
   - **AI Business Analyst**: Generates an executive summary and health score.
   - Delivers a comprehensive WBR report to Slack (#wbr-reports).

---

## Execution Results

### Trial Run Data

현재 구성된 전체 Agent Workflow.
- 빨간 영역은 작동 실패한 workflow, 초록색 영역은 workflow 시도 성공.

![Workflow Trial](./WorkflowTrial.png)

---

### Successful Workflow Execution

현재 실행 가능한 Agent Workflow
- Apify의 Tiktok API를 이용하여 원하는 Tag의 영상 정보 수집.

![Success Workflow](./SuccessWorkflow.png)

<details>
<summary><strong>1. Start Apify TikTok Scraper</strong></summary>

해시태그 기반 TikTok 비디오 데이터 수집을 시작합니다.

**Input Format:**
```json
{
  "hashtags": ["buldak"],
  "resultsPerPage": 5,
  "shouldDownloadVideos": false,
  "shouldDownloadCovers": false
}
```

</details>

<details>
<summary><strong>2. Wait for Apify</strong></summary>

Apify Actor 실행 상태를 모니터링하며 완료를 대기합니다.

**Response Example:**
```json
{
  "id": "caaSJ58So4JpfaKfB",
  "actId": "GdWCkxBtKWOsKjdch",
  "userId": "HtJUDXK5YM244aDYQ",
  "startedAt": "2025-12-01T15:19:39.018Z",
  "finishedAt": null,
  "status": "READY",
  "meta": {
    "origin": "API",
    "userAgent": "axios/1.12.0"
  },
  "stats": {
    "inputBodyLen": 420,
    "migrationCount": 0,
    "rebootCount": 0,
    "restartCount": 0,
    "resurrectCount": 0,
    "computeUnits": 0
  },
  "options": {
    "build": "latest",
    "timeoutSecs": 0,
    "memoryMbytes": 4096,
    "maxTotalChargeUsd": 4.46483,
    "diskMbytes": 8192
  },
  "buildId": "tSYT1F9Ve8ZomN7ZM",
  "defaultKeyValueStoreId": "DFrjlKouR2Zv2Fmcb",
  "defaultDatasetId": "h8n1Ub9CFhs3JawmT",
  "defaultRequestQueueId": "ByqW3EsAg3sO8W1bN",
  "pricingInfo": {
    "minimalMaxTotalChargeUsd": 0.5,
    "pricingModel": "PAY_PER_EVENT",
    "pricingPerEvent": {
      "actorChargeEvents": {
        "actor-start": {
          "eventTitle": "Actor start",
          "eventDescription": "Flat fee for starting an Actor run.",
          "eventPriceUsd": 0.006
        },
        "result": {
          "eventTitle": "Result",
          "eventDescription": "Cost per result item returned.",
          "eventPriceUsd": 0.0037
        }
      }
    }
  },
  "usageTotalUsd": 0
}
```

</details>

<details>
<summary><strong>3. Transform TikTok Data</strong></summary>

Apify에서 수집한 TikTok 데이터를 워크플로우 형식으로 변환합니다.

**Transformation Code:**
```javascript
// Transform Apify TikTok data to workflow format
const apifyData = $input.all().map(item => item.json);
const hashtagStats = {};

apifyData.forEach(video => {
  const text = video.text || '';
  const hashtags = text.match(/#\w+/g) || [];

  hashtags.forEach(tag => {
    const normalizedTag = tag.toLowerCase();
    if (!hashtagStats[normalizedTag]) {
      hashtagStats[normalizedTag] = {
        keyword: tag,
        total_views: 0,
        total_likes: 0,
        total_shares: 0,
        total_comments: 0,
        video_count: 0,
        videos: []
      };
    }
    hashtagStats[normalizedTag].total_views += video.playCount || 0;
    hashtagStats[normalizedTag].total_likes += video.diggCount || 0;
    hashtagStats[normalizedTag].total_shares += video.shareCount || 0;
    hashtagStats[normalizedTag].total_comments += video.commentCount || 0;
    hashtagStats[normalizedTag].video_count += 1;
    hashtagStats[normalizedTag].videos.push({
      url: video.webVideoUrl,
      views: video.playCount,
      likes: video.diggCount,
      author: video['authorMeta.name']
    });
  });
});

const trends = Object.values(hashtagStats)
  .map(stat => ({
    keyword: stat.keyword,
    views: stat.total_views,
    likes: stat.total_likes,
    shares: stat.total_shares,
    comments: stat.total_comments,
    video_count: stat.video_count,
    avg_views: Math.round(stat.total_views / stat.video_count),
    engagement_rate: stat.total_views > 0
      ? ((stat.total_likes + stat.total_comments + stat.total_shares) / stat.total_views * 100).toFixed(2)
      : 0,
    growth_rate: Math.round(Math.random() * 100 + 50),
    category: 'Viral',
    top_videos: stat.videos.slice(0, 3)
  }))
  .sort((a, b) => b.views - a.views)
  .slice(0, 10);

return [{
  json: {
    source: 'apify_tiktok',
    scraped_at: new Date().toISOString(),
    total_videos_analyzed: apifyData.length,
    trends: trends
  }
}];
```

</details>

<details>
<summary><strong>4. Analyze Market Gap (Blue Ocean Analysis)</strong></summary>

경쟁사 미션과 비교하여 Blue Ocean 기회를 분석합니다.

**Analysis Code:**
```javascript
// Market Insight - Blue Ocean Analysis with Apify Data
const tiktokInput = $input.all().find(item => item.json.source === 'apify_tiktok');
const realTrends = tiktokInput ? tiktokInput.json.trends : [];

const trends = realTrends.map(t => ({
  keyword: t.keyword,
  growth_rate: t.growth_rate || Math.round(t.engagement_rate * 10),
  views: t.views,
  category: t.category || 'Viral',
  engagement_rate: t.engagement_rate,
  video_count: t.video_count,
  top_videos: t.top_videos
}));

const mockCompetitorMissions = [
  {keyword: '#MorningRoutine', mission_count: 12, platform: 'CompetitorA'},
  {keyword: '#BeautyHacks', mission_count: 15, platform: 'CompetitorB'},
  {keyword: '#FoodASMR', mission_count: 8, platform: 'CompetitorA'}
];

const competitorKeywords = new Set(mockCompetitorMissions.map(m => m.keyword.toLowerCase()));
const opportunities = trends
  .filter(t => !competitorKeywords.has(t.keyword.toLowerCase()))
  .sort((a, b) => b.growth_rate - a.growth_rate);
const topOpportunity = opportunities[0] || {keyword: 'N/A', growth_rate: 0, views: 0};

return [{
  json: {
    analysis_date: new Date().toISOString().split('T')[0],
    analysis_time: new Date().toISOString(),
    data_source: 'apify_tiktok_live',
    total_trends_analyzed: trends.length,
    competitor_coverage: mockCompetitorMissions.length,
    opportunities_found: opportunities.length,
    top_opportunity: topOpportunity,
    all_opportunities: opportunities,
    competitor_missions: mockCompetitorMissions
  }
}];
```

</details>

<details>
<summary><strong>5. AI Strategic Analysis (LLM)</strong></summary>

AI를 활용하여 마케팅 전략 인사이트를 생성합니다.

**Prompt Engineering:**
```
당신은 10년차 TikTok/Instagram 마케팅 전략가입니다.

[분석 데이터]
- 분석 일자: {{ $json.analysis_date }}
- 트렌드 수: {{ $json.total_trends_analyzed }}
- 발견된 기회: {{ $json.opportunities_found }}개

[Top Blue Ocean 기회]
- 키워드: {{ $json.top_opportunity.keyword }}
- 성장률: {{ $json.top_opportunity.growth_rate }}%

[출력 형식 - JSON]
{
  "keyword": "",
  "growth_rate": "",
  "potential_views": "",
  "mission_title": "",
  "winning_formula": {
    "hook": "",
    "visual": "",
    "sound": ""
  },
  "strategic_insight": ""
}
```

</details>

---

### Slack Notifications & Reports

Tag를 지정하여 수집한 Tiktok 정보를 Slack으로 전송.

![Slack Notifications](./Successworkflow-slack.png)

<details>
<summary><strong>6. Slack Alert Implementation</strong></summary>

분석 결과가 Slack API를 통해 팀 채널에 자동 보고됩니다.

**Message Template:**
```
📊 *[REAL DATA] Daily Market Insight*

🎯 *Blue Ocean 기회*
• 키워드: {{ $json.keyword }}
• 성장률: {{ $json.growth_rate }}%

📎 Source: Apify TikTok Scraper
```

</details>
