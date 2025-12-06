# Virality v24: TikTok Marketing Automation Workflow

<div align="center">

![Version](https://img.shields.io/badge/version-v24-blue?style=for-the-badge)
![Platform](https://img.shields.io/badge/platform-n8n-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/status-Development%20Paused-yellow?style=for-the-badge)
![TikTok API](https://img.shields.io/badge/TikTok%20API-Pending-red?style=for-the-badge)

**End-to-end TikTok viral marketing automation from trend discovery to creator settlement**

[Overview](#overview) | [Architecture](#workflow-architecture) | [Components](#workflow-components) | [Results](#execution-results) | [Roadmap](#roadmap)

</div>

---

## Project Status

```
┌─────────────────────────────────────────────────────────────────────────┐
│  DEVELOPMENT STATUS: PAUSED (External Dependency Blocker)               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Workflow Architecture    [██████████████████████████████] 100%  ✅     │
│  System Implementation    [████████████████████████░░░░░░]  80%  🔄     │
│  API Integration (Apify)  [██████████████████████████████] 100%  ✅     │
│  TikTok Ads API Approval  [░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]   0%  ⏳     │
│                                                                         │
│  ⚠️  BLOCKER: TikTok Advertising Account approval pending               │
│      Required for: Ad execution, Campaign tracking, Performance APIs    │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Overview

**Virality v24**는 TikTok 바이럴 마케팅의 전체 라이프사이클을 자동화하는 n8n 기반 워크플로우 시스템입니다.

### Key Features

| Feature | Description | Status |
|---------|-------------|--------|
| **Trend Discovery** | AI 기반 Blue Ocean 기회 발굴 | ✅ Active |
| **Market Analysis** | 실시간 경쟁사 분석 및 인사이트 | ✅ Active |
| **Settlement System** | 크리에이터 성과 기반 정산 | ⏳ Pending |
| **Fraud Detection** | AI FDS 기반 어뷰징 탐지 | ⏳ Pending |
| **A/B Testing** | 통계적 가설 검증 시스템 | ⏳ Pending |
| **Business Intelligence** | WBR 리포트 자동 생성 | ⏳ Pending |

### Tech Stack

```
n8n Workflow Engine    │  Orchestration & Automation
Apify TikTok Scraper   │  Data Collection
OpenAI GPT-4           │  AI Analysis & Strategy
Slack API              │  Notifications & Reports
PostgreSQL             │  Data Persistence (Planned)
```

---

## Workflow Architecture

### System Overview

![Workflow Structure](./WorkflowStructure.png)

### Component Diagram

```
                          ┌─────────────────────────────────────────┐
                          │           VIRALITY v24 SYSTEM           │
                          └────────────────────┬────────────────────┘
                                               │
         ┌─────────────────────────────────────┼─────────────────────────────────────┐
         │                                     │                                     │
         ▼                                     ▼                                     ▼
┌─────────────────┐              ┌──────────────────────┐              ┌─────────────────┐
│  DATA SOURCES   │              │   PROCESSING LAYER   │              │    OUTPUTS      │
├─────────────────┤              ├──────────────────────┤              ├─────────────────┤
│ • TikTok API    │──────────────│ • Market Analysis    │──────────────│ • Slack Alerts  │
│ • Apify Scraper │              │ • AI Blue Ocean      │              │ • Reports       │
│ • Competitor DB │              │ • FDS Engine         │              │ • Settlements   │
│ • Creator Data  │              │ • A/B Statistics     │              │ • Dashboards    │
└─────────────────┘              └──────────────────────┘              └─────────────────┘
```

---

## Workflow Components

### 1. Market Insight Workflow

> **Schedule**: Daily 09:00 KST | **Channel**: #market-insight

TikTok 트렌드 분석 및 Blue Ocean 기회 발굴을 위한 자동화 워크플로우

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ TikTok Data  │───▶│ Transform    │───▶│ AI Analysis  │───▶│ Slack Alert  │
│ Collection   │    │ & Aggregate  │    │ (Blue Ocean) │    │ (Insights)   │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
```

**Key Functions**:
- 해시태그 기반 트렌드 데이터 수집
- 경쟁사 미션 대비 기회 영역 분석
- AI 전략 인사이트 및 미션 추천 생성

---

### 2. Settlement & FDS Workflow

> **Schedule**: Daily 06:00 KST | **Channel**: #settlements, #fraud-alerts

크리에이터 성과 정산 및 AI 기반 부정행위 탐지 시스템

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ Performance  │───▶│ Calculate    │───▶│ AI FDS      │───▶│ Settlement   │
│ Data Fetch   │    │ Rewards      │    │ Analysis     │    │ Processing   │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
                                               │
                                               ▼
                                    ┌──────────────────────┐
                                    │ APPROVED │ HOLD │ BLOCKED │
                                    └──────────────────────┘
```

**Key Functions**:
- 크리에이터 성과 지표 집계 (Views, Likes, Comments)
- 등급/카테고리/시즈널리티 기반 동적 보상 계산
- 트래픽 패턴 분석을 통한 어뷰징 탐지

---

### 3. A/B Testing Workflow

> **Trigger**: Event-driven (Mission Click) | **Channel**: #ab-testing

카피/인센티브 효과 검증을 위한 통계적 A/B 테스트 시스템

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ User Click   │───▶│ Group        │───▶│ Track        │───▶│ Statistical  │
│ Event        │    │ Assignment   │    │ Performance  │    │ Analysis     │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
       ┌────────────┐           ┌────────────┐
       │  Group A   │           │  Group B   │
       │ (Control)  │           │  (Test)    │
       └────────────┘           └────────────┘
```

**Test Variables**:
- Copy: Emotional vs Reward-focused messaging
- Incentive: Reward amount & structure optimization

---

### 4. Weekly Business Review (WBR)

> **Schedule**: Monday 08:00 KST | **Channel**: #wbr-reports

주간 비즈니스 성과 분석 및 Executive Summary 자동 생성

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ Aggregate    │───▶│ Calculate    │───▶│ AI Business  │───▶│ WBR Report   │
│ Weekly Data  │    │ KPIs         │    │ Analysis     │    │ Generation   │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
```

**Report Metrics**:
- Weekly Revenue & Growth Rate
- Creator Retention & Churn
- Mission Performance & ROI
- Health Score & Recommendations

---

## Execution Results

### Workflow Trial Overview

전체 Agent Workflow 시험 실행 결과

![Workflow Trial](./WorkflowTrial.png)

| Area | Status | Description |
|------|--------|-------------|
| 🟢 Green | Success | 정상 실행된 워크플로우 |
| 🔴 Red | Failed | 외부 의존성으로 실패한 워크플로우 |

---

### Active Workflow: Market Insight Pipeline

현재 운영 중인 TikTok 데이터 수집 및 분석 파이프라인

![Success Workflow](./SuccessWorkflow.png)

#### Pipeline Stages

<details>
<summary><strong>Stage 1: Start Apify TikTok Scraper</strong></summary>

해시태그 기반 TikTok 비디오 데이터 수집 시작

**Configuration**:
```json
{
  "hashtags": ["buldak"],
  "resultsPerPage": 5,
  "shouldDownloadVideos": false,
  "shouldDownloadCovers": false
}
```

**Capabilities**:
- Multiple hashtag tracking
- Configurable result limits
- Video/cover download options

</details>

<details>
<summary><strong>Stage 2: Wait for Apify Completion</strong></summary>

Apify Actor 실행 상태 모니터링 및 완료 대기

**Status Response Example**:
```json
{
  "id": "caaSJ58So4JpfaKfB",
  "status": "READY",
  "startedAt": "2025-12-01T15:19:39.018Z",
  "options": {
    "memoryMbytes": 4096,
    "timeoutSecs": 0
  },
  "pricingInfo": {
    "pricingModel": "PAY_PER_EVENT",
    "pricingPerEvent": {
      "actorChargeEvents": {
        "actor-start": { "eventPriceUsd": 0.006 },
        "result": { "eventPriceUsd": 0.0037 }
      }
    }
  }
}
```

**Cost Optimization**:
- Pay-per-event pricing model
- Actor start: $0.006/run
- Per result: $0.0037/item

</details>

<details>
<summary><strong>Stage 3: Transform TikTok Data</strong></summary>

수집된 데이터를 워크플로우 분석 형식으로 변환

**Transformation Logic**:
```javascript
// Extract hashtag statistics from video data
const hashtagStats = {};

apifyData.forEach(video => {
  const hashtags = (video.text || '').match(/#\w+/g) || [];

  hashtags.forEach(tag => {
    const key = tag.toLowerCase();
    if (!hashtagStats[key]) {
      hashtagStats[key] = {
        keyword: tag,
        total_views: 0,
        total_likes: 0,
        total_shares: 0,
        total_comments: 0,
        video_count: 0,
        videos: []
      };
    }

    hashtagStats[key].total_views += video.playCount || 0;
    hashtagStats[key].total_likes += video.diggCount || 0;
    hashtagStats[key].total_shares += video.shareCount || 0;
    hashtagStats[key].total_comments += video.commentCount || 0;
    hashtagStats[key].video_count += 1;
  });
});

// Calculate engagement metrics
const trends = Object.values(hashtagStats)
  .map(stat => ({
    keyword: stat.keyword,
    views: stat.total_views,
    engagement_rate: ((stat.total_likes + stat.total_comments + stat.total_shares)
                      / stat.total_views * 100).toFixed(2),
    avg_views: Math.round(stat.total_views / stat.video_count)
  }))
  .sort((a, b) => b.views - a.views)
  .slice(0, 10);
```

**Output Metrics**:
| Metric | Description |
|--------|-------------|
| `views` | Total view count |
| `engagement_rate` | (Likes + Comments + Shares) / Views |
| `avg_views` | Average views per video |
| `video_count` | Number of videos analyzed |

</details>

<details>
<summary><strong>Stage 4: Blue Ocean Analysis</strong></summary>

경쟁사 미션 대비 기회 영역 분석

**Analysis Process**:
```javascript
// Compare trends with competitor missions
const competitorKeywords = new Set(
  competitorMissions.map(m => m.keyword.toLowerCase())
);

// Find untapped opportunities
const opportunities = trends
  .filter(t => !competitorKeywords.has(t.keyword.toLowerCase()))
  .sort((a, b) => b.growth_rate - a.growth_rate);

return {
  analysis_date: new Date().toISOString().split('T')[0],
  data_source: 'apify_tiktok_live',
  opportunities_found: opportunities.length,
  top_opportunity: opportunities[0],
  all_opportunities: opportunities
};
```

**Output Structure**:
```json
{
  "top_opportunity": {
    "keyword": "#buldak",
    "growth_rate": 127,
    "views": 15234567,
    "engagement_rate": 8.5
  },
  "opportunities_found": 7
}
```

</details>

<details>
<summary><strong>Stage 5: AI Strategic Analysis</strong></summary>

GPT-4 기반 마케팅 전략 인사이트 생성

**System Prompt**:
```
당신은 10년차 TikTok/Instagram 마케팅 전략가입니다.
주어진 트렌드 데이터를 분석하여 실행 가능한 전략을 제시하세요.
```

**Input Context**:
```
[분석 데이터]
- 분석 일자: {{ analysis_date }}
- 트렌드 수: {{ total_trends_analyzed }}
- 발견된 기회: {{ opportunities_found }}개

[Top Blue Ocean 기회]
- 키워드: {{ top_opportunity.keyword }}
- 성장률: {{ top_opportunity.growth_rate }}%
- 총 조회수: {{ top_opportunity.views }}
```

**Output Schema**:
```json
{
  "keyword": "string",
  "growth_rate": "number",
  "potential_views": "string",
  "mission_title": "string",
  "winning_formula": {
    "hook": "string (첫 3초 전략)",
    "visual": "string (비주얼 컨셉)",
    "sound": "string (음악/효과음 추천)"
  },
  "strategic_insight": "string (핵심 인사이트)"
}
```

</details>

<details>
<summary><strong>Stage 6: Slack Alert</strong></summary>

분석 결과를 Slack 채널로 자동 전송

**Message Template**:
```markdown
:bar_chart: *[REAL DATA] Daily Market Insight*

:dart: *Blue Ocean 기회*
• 키워드: {{ keyword }}
• 성장률: {{ growth_rate }}%
• 예상 조회수: {{ potential_views }}

:bulb: *추천 미션*
{{ mission_title }}

:clapper: *Winning Formula*
• Hook: {{ winning_formula.hook }}
• Visual: {{ winning_formula.visual }}
• Sound: {{ winning_formula.sound }}

:brain: *전략 인사이트*
{{ strategic_insight }}

---
:paperclip: Source: Apify TikTok Scraper
:clock1: Generated: {{ timestamp }}
```

</details>

---

### Slack Integration Results

수집된 TikTok 정보가 자동으로 Slack 채널에 리포팅됩니다.

![Slack Notifications](./Successworkflow-slack.png)

**Channel Configuration**:
| Channel | Purpose | Schedule |
|---------|---------|----------|
| `#market-insight` | 트렌드 분석 결과 | Daily 09:00 |
| `#fraud-alerts` | FDS 탐지 알림 | Real-time |
| `#settlements` | 정산 처리 결과 | Daily 06:00 |
| `#wbr-reports` | 주간 비즈니스 리포트 | Monday 08:00 |
| `#ab-testing` | A/B 테스트 결과 | On-demand |

---

## Roadmap

### Phase 1: Foundation (Completed)
- [x] n8n 워크플로우 아키텍처 설계
- [x] Apify TikTok Scraper 연동
- [x] 데이터 변환 파이프라인 구축
- [x] Blue Ocean 분석 알고리즘
- [x] AI 전략 분석 (GPT-4)
- [x] Slack 알림 시스템

### Phase 2: API Integration (Blocked)
- [ ] TikTok Advertising Account 승인
- [ ] TikTok Ads API 연동
- [ ] 캠페인 성과 트래킹
- [ ] Creator 데이터 수집 API

### Phase 3: Settlement System
- [ ] Creator 성과 집계 시스템
- [ ] 동적 보상 계산 엔진
- [ ] AI FDS (Fraud Detection System)
- [ ] 정산 승인/홀드/차단 워크플로우

### Phase 4: Analytics & Optimization
- [ ] A/B 테스트 프레임워크
- [ ] 통계적 유의성 검증 엔진
- [ ] WBR 자동 리포팅
- [ ] 실시간 대시보드

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v24 | 2025-12 | Market Insight 파이프라인 완성, Slack 연동 |
| v23 | 2025-11 | 초기 아키텍처 설계 |

---

<div align="center">

**Built with n8n + Apify + GPT-4 + Slack**

</div>
