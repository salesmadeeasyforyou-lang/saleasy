# Product Requirements Document

# SalesVoice
## Sales Call Recording & Analytics Platform

**Version:** 2.0
**Date:** January 2025
**Status:** Draft
**Classification:** Confidential

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Target Users](#3-target-users)
4. [Product Overview](#4-product-overview)
5. [Detailed Features & Requirements](#5-detailed-features--requirements)
6. [User Stories](#6-user-stories)
7. [Technical Architecture](#7-technical-architecture)
8. [Data Model](#8-data-model)
9. [User Flows](#9-user-flows)
10. [UI/UX Requirements](#10-uiux-requirements)
11. [Non-Functional Requirements](#11-non-functional-requirements)
12. [Security & Privacy](#12-security--privacy)
13. [Monitoring & Observability](#13-monitoring--observability)
14. [Testing Strategy](#14-testing-strategy)
15. [Metrics & Success Criteria](#15-metrics--success-criteria)
16. [Roadmap & Phases](#16-roadmap--phases)
17. [Cost Analysis](#17-cost-analysis)
18. [Risks & Mitigations](#18-risks--mitigations)
19. [Open Questions & Future Considerations](#19-open-questions--future-considerations)
20. [Appendix](#20-appendix)

---

## 1. Executive Summary

SalesVoice is a mobile-first sales call recording and analytics platform designed for businesses in India. The platform enables field sales agents to record in-person customer meetings, automatically transcribe the conversations (supporting Hindi-English code-switching), and generate AI-powered insights including summaries, key topics, sentiment analysis, and actionable next steps.

Business owners gain visibility into their sales team's activities through a comprehensive web dashboard, enabling accountability, performance monitoring, and data-driven coaching. The platform addresses a critical gap in the market for SMBs who need enterprise-grade sales intelligence at an accessible price point.

### Key Value Propositions

| Stakeholder | Value |
|-------------|-------|
| **Sales Agents** | Effortless call documentation, automatic transcription, personal performance insights, and AI feedback on call quality |
| **Business Owners** | Complete visibility into field sales activities, accountability, team performance metrics, and real-time dashboard updates |
| **The Business** | Reduced manual reporting overhead, better sales coaching, improved customer relationship tracking, and cost-optimized AI pipeline |

### MVP Scope

The Minimum Viable Product focuses on the core recording-to-insight loop: one-tap recording with pause/resume, offline-first architecture with chunked background sync, AI-powered transcription and analysis via cost-optimized providers, and a clean owner dashboard with real-time updates. Advanced features like coaching suggestions and industry-specific templates are deferred to post-MVP iterations.

---

## 2. Problem Statement

### Current Challenges

Field sales teams in India face several critical challenges that impact productivity and accountability:

#### For Sales Agents

- Manual documentation of customer conversations is time-consuming and often incomplete
- Important details, commitments, and next steps are frequently forgotten or misremembered
- No systematic way to improve sales techniques through self-review
- End-of-day reporting is tedious and often done hastily
- No personal performance analytics to track improvement over time

#### For Business Owners

- Limited visibility into what actually happens during customer meetings
- Reliance on self-reported data which may be inaccurate or embellished
- Difficulty identifying coaching opportunities and performance issues
- No standardized metrics for comparing agent performance
- No real-time awareness of field activity; reliance on end-of-day reports

### Market Gap

Enterprise solutions like Gong and Chorus exist but are priced for large organizations and primarily focused on phone/video calls rather than in-person meetings. There is no affordable, India-focused solution that handles Hindi-English code-switching and works reliably in low-connectivity field conditions.

---

## 3. Target Users

### Primary Personas

#### Persona 1: Sales Agent (Ramesh)

| Attribute | Details |
|-----------|---------|
| **Age** | 25-40 years |
| **Role** | Field Sales Executive / Medical Representative / Insurance Agent |
| **Tech Proficiency** | Moderate - comfortable with WhatsApp, basic Android apps |
| **Device** | Mid-range Android smartphone (₹10,000-25,000) |
| **Connectivity** | Variable - often in areas with poor network coverage |
| **Daily Routine** | 4-6 customer meetings per day, travels between locations |
| **Pain Points** | Manual reporting, forgetting meeting details, proving productivity |
| **Goals** | Minimize paperwork, improve sales performance, build customer relationships |

#### Persona 2: Business Owner (Priya)

| Attribute | Details |
|-----------|---------|
| **Age** | 35-55 years |
| **Role** | Business Owner / Sales Manager / Regional Head |
| **Tech Proficiency** | Moderate to High - uses laptop for business operations |
| **Team Size** | 5-50 sales agents |
| **Industry** | Pharma, Insurance, FMCG, B2B Sales, Real Estate |
| **Pain Points** | No visibility into field activities, inconsistent reporting, coaching at scale |
| **Goals** | Ensure accountability, identify top performers, improve team-wide results |

### Target Industries

The platform is designed as a general-purpose solution applicable across industries with field sales teams. Initial focus verticals include:

- **Pharmaceutical:** Medical representatives visiting doctors and hospitals
- **Insurance:** Agents meeting prospects for policy discussions
- **FMCG:** Sales executives visiting retail outlets and distributors
- **B2B Sales:** Account executives meeting business clients
- **Real Estate:** Agents meeting property buyers and sellers

---

## 4. Product Overview

### Product Components

| Component | Platform | Primary Users | Description |
|-----------|----------|---------------|-------------|
| Agent Mobile App | Android (Native Kotlin) | Sales Agents | Recording, viewing transcripts/insights, call history, personal analytics |
| Owner Dashboard | Next.js (Web, Responsive) | Business Owners/Admins | Team management, call review, analytics, reports, real-time updates |
| Backend Services | Supabase + Cloudflare R2 | System | Auth, database, audio storage, AI processing pipeline |

### Core Capabilities

#### Recording & Transcription

- One-tap recording initiation with pause/resume support
- Offline-first architecture — recordings stored locally until sync
- Support for calls up to 2 hours duration
- Hindi-English code-switching support in transcription
- On-device audio chunking and resumable background upload
- Audio quality gate (silence/noise detection) before upload
- Screen-off recording via Android foreground service

#### AI-Powered Insights

- Automatic call summarization
- Key topics and products discussed extraction
- Next steps identification
- Sentiment analysis (positive/neutral/negative)
- Thumbs up/down feedback on AI-generated insights
- Transcript correction mechanism for inaccurate words/phrases

#### Team Management

- Company profile creation with shareable invite codes
- Agent onboarding via deep-linked invite link
- Role-based access control: owner, admin, agent
- Agent removal with data retention and immediate token revocation

#### Reporting

- Daily summary reports (brief, WhatsApp-optimized)
- WhatsApp share functionality for reports
- Per-agent and team-wide metrics

---

## 5. Detailed Features & Requirements

### 5.1 Agent Mobile Application

#### 5.1.1 Authentication

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| AUTH-001 | Phone number OTP-based authentication | P0 |
| AUTH-002 | Automatic session persistence (stay logged in) | P0 |
| AUTH-003 | Join company via deep-linked invite link/code | P0 |
| AUTH-004 | Logout functionality | P1 |
| AUTH-005 | OTP rate limiting: max 5 requests/hour per number, 3 verification attempts per OTP, 10-minute OTP expiry | P0 |
| AUTH-006 | Show "You've been removed" screen if agent is removed from company | P0 |
| AUTH-007 | Minimum app version check on launch with force-update prompt | P0 |

#### 5.1.2 Call Recording

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| REC-001 | One-tap recording start from home screen | P0 |
| REC-002 | Visual recording indicator (timer, waveform) | P0 |
| REC-003 | One-tap recording stop | P0 |
| REC-004 | Audio saved locally in compressed format (M4A/AAC, mono, 64kbps) | P0 |
| REC-005 | Maximum recording duration: 2 hours | P0 |
| REC-006 | Recording continues when screen is off via Android foreground service with persistent notification and wakelock | P0 |
| REC-007 | Low storage warning before recording | P1 |
| REC-008 | Consent reminder display before recording starts | P1 |
| REC-009 | Pause/resume recording button | P1 |
| REC-010 | Minimum call duration threshold: 30 seconds. Below threshold, prompt "Discard this recording?" | P1 |
| REC-011 | Single active recording enforcement — cannot start new recording while one is in progress | P0 |
| REC-012 | Audio focus handling: gracefully handle phone call interruptions, microphone conflicts, and voice assistant activations during recording | P1 |
| REC-013 | Audio quality gate: detect silence ratio (>80% silence) and low audio energy before queuing upload. Flag poor-quality recordings for agent review. | P1 |

#### 5.1.3 Post-Call Metadata Entry

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| META-001 | Customer name field with autocomplete from existing customers (fuzzy matching) | P0 |
| META-002 | Customer phone number field (optional, used for deduplication) | P1 |
| META-003 | Company name field (optional) | P1 |
| META-004 | Call type selection: First Meeting / Follow-up / Closing | P0 |
| META-005 | Outcome selection: Positive / Neutral / Negative | P0 |
| META-006 | Create new customer if not found in autocomplete | P0 |
| META-007 | Skip/Save later option for metadata (call still syncs) | P2 |
| META-008 | Agent notes field (free-text, optional) for personal context | P1 |
| META-009 | Edit call metadata after saving (customer name, type, outcome, notes) | P1 |

#### 5.1.4 Offline & Sync

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| SYNC-001 | All recordings saved locally first (offline-first) | P0 |
| SYNC-002 | On-device audio chunking: split recording into 30-second chunks with 5-second overlap before upload | P0 |
| SYNC-003 | Resumable chunked upload — failed chunks resume from last successful chunk, not from zero | P0 |
| SYNC-004 | Background upload when network available (max 2 parallel chunk uploads) | P0 |
| SYNC-005 | Sync status indicator (X calls pending upload) | P0 |
| SYNC-006 | Automatic retry on upload failure (max 5 retries per chunk with exponential backoff) | P0 |
| SYNC-007 | After max retries exhausted: show "Upload failed — tap to retry" in call history | P0 |
| SYNC-008 | Upload order: smallest-first for quick progress indication | P1 |
| SYNC-009 | WiFi-preferred upload option in settings | P2 |
| SYNC-010 | Manual sync trigger button | P1 |
| SYNC-011 | Removed agent's pending uploads still sync (data belongs to company) | P0 |
| SYNC-012 | Cache last 50 call insights locally for offline viewing | P1 |
| SYNC-013 | File type validation: only M4A/AAC audio accepted. Server-side max file size: 250MB. MIME type verification. | P0 |

#### 5.1.5 Call History & Insights

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| HIST-001 | List view of agent's own calls (newest first) with cursor-based pagination (20 per page) | P0 |
| HIST-002 | Call detail view with full transcript | P0 |
| HIST-003 | AI-generated summary display | P0 |
| HIST-004 | Key topics/products as tags | P0 |
| HIST-005 | Next steps list | P0 |
| HIST-006 | Sentiment indicator | P1 |
| HIST-007 | Filter calls by date range (default: last 7 days) | P1 |
| HIST-008 | Filter calls by customer | P1 |
| HIST-009 | Search within transcripts | P2 |
| HIST-010 | Thumbs up/down feedback on AI-generated summary and insights | P1 |
| HIST-011 | Transcript correction: tap a word/phrase to suggest correction | P2 |

#### 5.1.6 Agent Personal Analytics

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| AGENT-ANALYTICS-001 | Total calls this week / this month | P1 |
| AGENT-ANALYTICS-002 | Outcome distribution (positive/neutral/negative) for the agent | P1 |
| AGENT-ANALYTICS-003 | Calls this week vs last week comparison | P2 |
| AGENT-ANALYTICS-004 | Most-visited customers list | P2 |

#### 5.1.7 Notifications

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| NOTIF-001 | Push notification when transcription complete | P1 |
| NOTIF-002 | Push notification when insights ready | P1 |
| NOTIF-003 | Notification for sync failures | P1 |

#### 5.1.8 First-Time Experience

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| FTX-001 | Welcome screen after joining company with brief tutorial (3 screens max) | P1 |
| FTX-002 | Empty state on home screen: "Record your first call" CTA with visual guidance | P1 |
| FTX-003 | Microphone permission request with explanation of why it's needed | P0 |
| FTX-004 | Battery optimization whitelisting prompt for OEM devices (Xiaomi, Vivo, Oppo, Samsung) | P1 |

---

### 5.2 Owner Web Dashboard

#### 5.2.1 Authentication & Onboarding

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| DASH-AUTH-001 | Phone number OTP-based authentication | P0 |
| DASH-AUTH-002 | Company profile creation (name, basic info) | P0 |
| DASH-AUTH-003 | Generate shareable invite link (deep-linked with Android App Links) | P0 |
| DASH-AUTH-004 | Generate invite codes | P1 |
| DASH-AUTH-005 | Regenerate/invalidate invite links | P2 |
| DASH-AUTH-006 | OTP rate limiting (same rules as agent: 5/hr, 3 attempts, 10-min expiry) | P0 |
| DASH-AUTH-007 | Empty state for new company: "Invite your first agent" prompt with copy-link CTA | P1 |

#### 5.2.2 Team Management

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| TEAM-001 | View list of all agents | P0 |
| TEAM-002 | View agent details (name, phone, join date, calls count) | P0 |
| TEAM-003 | Remove agent from company (with confirmation dialog) | P0 |
| TEAM-004 | Removed agent data retained for owner view | P0 |
| TEAM-005 | Agent status indicator (active/removed) | P1 |
| TEAM-006 | Removing agent immediately invalidates their auth tokens | P0 |
| TEAM-007 | Add admin role: owner can promote an agent to admin (admin sees all calls, manages agents) | P1 |
| TEAM-008 | Configure per-agent daily call limit (default: 15, configurable 1-50) | P1 |

#### 5.2.3 Call Review

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| REVIEW-001 | View all calls across all agents with cursor-based pagination (default: last 7 days) | P0 |
| REVIEW-002 | Filter by agent | P0 |
| REVIEW-003 | Filter by date range | P0 |
| REVIEW-004 | Filter by customer | P1 |
| REVIEW-005 | Filter by outcome (positive/neutral/negative) | P1 |
| REVIEW-006 | Call detail view with transcript | P0 |
| REVIEW-007 | View AI summary and insights | P0 |
| REVIEW-008 | View customer conversation history (all calls to same customer) | P1 |
| REVIEW-009 | View failed calls with option to manually trigger reprocessing | P1 |
| REVIEW-010 | Real-time call list updates via Supabase Realtime (new calls appear without page refresh) | P1 |

#### 5.2.4 Analytics & Metrics

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| ANALYTICS-001 | Total calls today/this week/this month | P0 |
| ANALYTICS-002 | Calls per agent breakdown | P0 |
| ANALYTICS-003 | Average call duration | P1 |
| ANALYTICS-004 | Outcome distribution (positive/neutral/negative) | P1 |
| ANALYTICS-005 | Active agents today indicator | P0 |

#### 5.2.5 Daily Reports

| Requirement ID | Requirement | Priority |
|----------------|-------------|----------|
| REPORT-001 | Auto-generated daily summary (brief format, optimized for WhatsApp sharing) | P0 |
| REPORT-002 | Total calls and active agents count | P0 |
| REPORT-003 | Per-agent highlights (calls, outcomes) | P0 |
| REPORT-004 | Share report via WhatsApp (share intent, max 500 characters for readability) | P0 |
| REPORT-005 | View historical daily reports | P2 |
| REPORT-006 | Customizable report time (default 8 PM) | P2 |

---

## 6. User Stories

### Agent User Stories

#### US-A01: Record a Customer Meeting

*As a sales agent, I want to record my customer meeting with one tap, so that I don't have to take manual notes during the conversation.*

**Acceptance Criteria:**
- Large record button on home screen
- Visual timer showing recording duration
- Recording continues even if I switch apps or turn screen off
- Can pause and resume recording during interruptions
- Cannot start a second recording while one is active

#### US-A02: Add Call Details After Recording

*As a sales agent, I want to add customer name and call outcome after recording, so that my calls are properly documented.*

**Acceptance Criteria:**
- Form appears after stopping recording (if recording > 30 seconds)
- Existing customers appear in autocomplete (fuzzy matching)
- Can select call type and outcome
- Can add optional notes
- Can edit metadata later from call detail screen

#### US-A03: Work Offline

*As a sales agent, I want to record calls even without internet, so that I can work in areas with poor connectivity.*

**Acceptance Criteria:**
- Recording works without internet
- Can see pending upload count
- Calls sync automatically when online (chunked, resumable)
- Can view cached insights from previously synced calls while offline
- If upload fails permanently, can tap to retry

#### US-A04: Review My Call Insights

*As a sales agent, I want to see AI-generated summaries of my calls, so that I can quickly recall what was discussed.*

**Acceptance Criteria:**
- Can see full transcript
- Summary highlights key points
- Next steps are clearly listed
- Can give thumbs up/down feedback on insight quality
- Can tap to correct inaccurate words in transcript

#### US-A05: Join My Company

*As a sales agent, I want to join my company using an invite link, so that my calls are associated with my team.*

**Acceptance Criteria:**
- Can click invite link to join (deep link opens app directly)
- If app not installed, redirected to Play Store
- Company name shown after joining
- Brief onboarding tutorial shown on first launch

#### US-A06: Track My Own Performance

*As a sales agent, I want to see my personal call stats, so that I can track my improvement over time.*

**Acceptance Criteria:**
- Can see total calls this week/month
- Can see outcome distribution trend
- Can see most-visited customers

---

### Owner User Stories

#### US-O01: Set Up My Company

*As a business owner, I want to create my company profile and invite my team, so that I can start tracking their activities.*

**Acceptance Criteria:**
- Can create company with name
- Get shareable invite link (with deep linking)
- Can see agents who have joined
- Empty state guides me to invite first agent

#### US-O02: Monitor Team Activity

*As a business owner, I want to see all calls made by my team, so that I can ensure accountability.*

**Acceptance Criteria:**
- Dashboard shows total calls today
- Can see which agents are active
- Can view any call's transcript and summary
- New calls appear in real-time without page refresh
- Can see and reprocess failed calls

#### US-O03: Receive Daily Report

*As a business owner, I want to receive a daily summary of team activities, so that I stay informed without checking the dashboard constantly.*

**Acceptance Criteria:**
- Daily report generated automatically
- Can share to WhatsApp with one click (brief, readable format)
- Report includes per-agent breakdown

#### US-O04: Review Specific Agent Performance

*As a business owner, I want to filter calls by agent, so that I can review individual performance.*

**Acceptance Criteria:**
- Can select agent from dropdown
- See only that agent's calls
- See agent's outcome distribution

#### US-O05: Manage Team Roles

*As a business owner, I want to promote agents to admin role, so that my managers can also review team performance.*

**Acceptance Criteria:**
- Can change an agent's role to admin
- Admins can view all calls and manage agents
- Admins cannot remove the owner or change owner's role

---

## 7. Technical Architecture

### 7.1 Technology Stack

| Layer | Technology | Rationale |
|-------|------------|-----------|
| Mobile App | Native Kotlin (Android) | Best performance on mid-range Android, native foreground service/wakelock support, optimal audio recording APIs, no cross-platform overhead for Android-only MVP |
| Owner Dashboard | Next.js | Fast development, good Supabase integration, SEO-friendly, responsive |
| Backend Database & Auth | Supabase | Auth, PostgreSQL database, Realtime subscriptions, Edge Functions for lightweight tasks |
| Audio Storage | Cloudflare R2 | S3-compatible, zero egress fees, ~$0.015/GB/month, ideal for audio files downloaded by processing pipeline |
| Transcription | Groq Whisper API (MVP) / Self-hosted Whisper large-v3 (scale) | Groq: ~$0.001/min (6x cheaper than OpenAI). Self-host at 500+ hours/day for further savings |
| LLM Processing | GPT-4o Mini / Claude Haiku | Summary generation, insight extraction, sentiment analysis. Use cheaper models for structured extraction |
| Push Notifications | Firebase Cloud Messaging | Industry standard for Android notifications, free |
| Async Job Queue | Inngest / Trigger.dev | Reliable async processing with retries, checkpointing, independent stage scaling. Replaces fragile Edge Function triggers |
| Crash Reporting | Firebase Crashlytics | Free, unlimited, production-grade crash reporting |
| Error Tracking | Sentry (free tier) | Backend/Edge Function error tracking |

### 7.2 System Architecture

The system uses Supabase as the core backend for auth, database, and realtime, with Cloudflare R2 as a dedicated audio storage layer. Processing is handled through an async job queue (Inngest/Trigger.dev) rather than database triggers, ensuring reliability, checkpointing, and independent retries per stage.

#### Component Overview

- **Mobile App (Kotlin):** Handles recording (foreground service), local storage (Room/SQLite), on-device chunking, resumable sync, displays insights with offline caching
- **Web Dashboard (Next.js):** Owner-facing interface for team management, analytics, and real-time call monitoring via Supabase Realtime
- **Supabase Auth:** Phone OTP authentication for agents, owners, and admins
- **Supabase Database (PostgreSQL):** Stores all structured data with Row Level Security
- **Cloudflare R2:** Stores audio file chunks with lifecycle policy (auto-delete after 7 days)
- **Async Job Queue (Inngest/Trigger.dev):** Handles transcription pipeline, LLM processing, report generation with per-stage checkpointing

### 7.3 Processing Pipeline

**Call Processing Flow (with checkpointing):**

```
Stage 1: UPLOAD
  1. Agent stops recording → Audio saved locally
  2. Agent enters metadata → Stored in local Room DB
  3. On-device: split audio into 30s chunks with 5s overlap
  4. Background service uploads chunks to Cloudflare R2 (resumable, 2 parallel max)
  5. All chunks uploaded → Call status set to 'uploaded'
  6. Job enqueued in async queue

Stage 2: TRANSCRIPTION (independent retry)
  7. Worker downloads audio chunks from R2
  8. Sends chunks to Groq Whisper API (parallel, max 5 concurrent)
  9. Stitches transcript chunks, removes overlap duplicates
  10. Saves transcript to call_insights table
  11. Call status set to 'transcribed'

Stage 3: ANALYSIS (independent retry)
  12. Worker sends transcript to LLM for summary, topics, next steps, sentiment
  13. Saves insights to call_insights table
  14. Call status set to 'completed'
  15. Sends push notification to agent

Error handling:
  - Each stage retries independently (max 5 retries with exponential backoff)
  - Stage 2 failure does NOT re-upload audio
  - Stage 3 failure does NOT re-transcribe
  - After max retries: call status set to 'failed', owner notified
  - Owner can manually trigger reprocessing from dashboard
```

### 7.4 Audio Chunking Strategy

```
On-device (before upload):
├── Input: single M4A/AAC file (mono, 64kbps)
├── Split into 30-second chunks with 5-second overlap
├── Each chunk: ~240KB (easily uploadable on slow networks)
├── Chunks named: {call_id}_chunk_{index}.m4a
└── Manifest file: {call_id}_manifest.json (chunk count, order, overlap)

Server-side (during transcription):
├── Download all chunks from R2
├── Transcribe each chunk via Groq Whisper API (parallel)
├── Stitch transcripts using overlap alignment
├── Remove duplicate text from overlap regions
└── Produce single continuous transcript
```

### 7.5 Offline Sync Architecture

```
Local Room DB tables:
├── pending_uploads (call_id, chunk_paths[], metadata, retry_count, status)
├── cached_calls (last 50 synced calls with insights for offline viewing)
└── cached_customers (for autocomplete while offline)

Sync logic:
├── On app open: check pending_uploads, attempt upload (smallest-first)
├── On network restore: trigger sync via connectivity broadcast receiver
├── Foreground service with 15-min periodic sync (when app alive)
├── Max 2 parallel chunk uploads to avoid saturating bandwidth
├── Show badge: "3 calls pending sync"
├── After max retries: show "Upload failed — tap to retry"
└── Removed agent's pending uploads still sync to company
```

### 7.6 Deep Linking Architecture

```
Invite link format: https://app.salesvoice.in/invite/{invite_code}

Android App Links setup:
├── assetlinks.json hosted at https://app.salesvoice.in/.well-known/assetlinks.json
├── Intent filter in AndroidManifest.xml for app.salesvoice.in/invite/*
├── If app installed → opens app directly, auto-joins company
├── If app not installed → web fallback page with Play Store redirect
└── Invite code extracted from URL and passed to join API

Web fallback page:
├── Shows company name and "Download SalesVoice" CTA
├── Play Store badge link
└── Manual invite code entry option
```

---

## 8. Data Model

### 8.1 Entity Relationship Overview

The data model is designed around six core entities: Companies, Users, Customers, Calls, Call Insights, and Daily Reports. A company has one owner, optional admins, and many agents. Agents make calls to customers. Each call generates one set of insights after processing.

```
companies
    │
    ├── 1:N → users (owner + admins + agents)
    │
    ├── 1:N → customers
    │
    ├── 1:N → calls
    │
    └── 1:N → daily_reports

calls
    │
    └── 1:1 → call_insights
```

### 8.2 Table Definitions

#### companies

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier |
| name | VARCHAR(255) | NOT NULL | Company display name |
| invite_code | VARCHAR(20) | UNIQUE, NOT NULL | Shareable code for agent onboarding |
| owner_id | UUID | FOREIGN KEY → users | Reference to owner user |
| max_daily_calls_per_agent | INTEGER | DEFAULT 15 | Configurable daily call limit per agent |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last modification timestamp |

#### users

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier (Supabase Auth ID) |
| phone_number | VARCHAR(15) | UNIQUE, NOT NULL | Phone number for auth |
| name | VARCHAR(255) | NOT NULL | User display name |
| role | ENUM | NOT NULL | Values: owner, admin, agent |
| company_id | UUID | FK → companies, NULLABLE | Company association |
| status | ENUM | DEFAULT 'active' | Values: active, removed |
| created_at | TIMESTAMP | DEFAULT NOW() | Registration timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last modification timestamp |

#### customers

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier |
| company_id | UUID | FOREIGN KEY → companies | Parent company |
| name | VARCHAR(255) | NOT NULL | Customer/prospect name |
| phone_number | VARCHAR(15) | NULLABLE | Customer phone (optional, for deduplication) |
| company_name | VARCHAR(255) | NULLABLE | Customer's company name |
| created_by | UUID | FOREIGN KEY → users | Agent who created this customer |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last modification timestamp |

#### calls

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier |
| agent_id | UUID | FOREIGN KEY → users | Agent who made the call |
| company_id | UUID | FOREIGN KEY → companies | Company association |
| customer_id | UUID | FK → customers, NULLABLE | Customer called (nullable if metadata skipped) |
| call_type | ENUM | NULLABLE | first_meeting, follow_up, closing |
| outcome | ENUM | NULLABLE | positive, neutral, negative |
| agent_notes | TEXT | NULLABLE | Agent's personal notes about the call |
| duration_seconds | INTEGER | NOT NULL | Call duration |
| recorded_at | TIMESTAMP | NOT NULL | When recording started |
| uploaded_at | TIMESTAMP | NULLABLE | When upload completed |
| status | ENUM | NOT NULL | recording, uploading, uploaded, transcribing, transcribed, analyzing, completed, failed |
| retry_count | INTEGER | DEFAULT 0 | Processing retry attempts per current stage |
| audio_path | VARCHAR(500) | NULLABLE | R2 storage path for audio chunks |
| chunk_count | INTEGER | DEFAULT 0 | Number of audio chunks |
| audio_quality_flag | BOOLEAN | DEFAULT false | True if audio quality gate detected issues |
| created_at | TIMESTAMP | DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last modification timestamp |

#### call_insights

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier |
| call_id | UUID | FK → calls, UNIQUE | Associated call |
| transcript | TEXT | NOT NULL | Full transcription text |
| summary | TEXT | NOT NULL | AI-generated summary |
| key_topics | JSONB | NOT NULL | Array of topics discussed |
| next_steps | JSONB | NOT NULL | Array of action items |
| sentiment | ENUM | NOT NULL | positive, neutral, negative |
| coaching_notes | TEXT | NULLABLE | Future: AI coaching suggestions |
| feedback_rating | SMALLINT | NULLABLE | User feedback: 1 (thumbs down) or 5 (thumbs up) |
| transcript_corrections | JSONB | NULLABLE | Array of {original, corrected, position} |
| processing_cost_cents | INTEGER | NULLABLE | Total processing cost in paisa (for cost tracking) |
| created_at | TIMESTAMP | DEFAULT NOW() | Processing completion timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last modification timestamp |

#### daily_reports

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier |
| company_id | UUID | FOREIGN KEY → companies | Company association |
| report_date | DATE | NOT NULL | Date of the report |
| report_content | TEXT | NOT NULL | Generated report text (brief, <500 chars for WhatsApp) |
| total_calls | INTEGER | NOT NULL | Calls that day |
| active_agents | INTEGER | NOT NULL | Agents who made calls |
| created_at | TIMESTAMP | DEFAULT NOW() | Generation timestamp |

#### subscriptions (skeleton for future billing)

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier |
| company_id | UUID | FK → companies, UNIQUE | Company association |
| plan | ENUM | NOT NULL | starter, growth, business, enterprise |
| status | ENUM | NOT NULL | active, trialing, cancelled, past_due |
| max_agents | INTEGER | NOT NULL | Agent limit for plan |
| max_minutes_monthly | INTEGER | NOT NULL | Transcription minutes limit |
| current_period_start | TIMESTAMP | NOT NULL | Billing period start |
| current_period_end | TIMESTAMP | NOT NULL | Billing period end |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last modification timestamp |

### 8.3 Row Level Security Policies

Supabase Row Level Security ensures data isolation between companies and appropriate access control:

- **Agents:** Can read/write their own calls and customers within their company. Can read (not write) company details.
- **Admins:** Can read all calls, customers, and users within their company. Can remove agents.
- **Owners:** Full read/write on all company data. Can manage roles, remove agents/admins.
- **Cross-company:** No user can access data from other companies.
- **Removed agents:** RLS policies deny all access. Token revocation provides defense in depth.

### 8.4 File Storage Structure (Cloudflare R2)

Audio chunks are stored in Cloudflare R2 with the following structure:

```
salesvoice-audio/
└── {company_id}/
    └── {call_id}/
        ├── manifest.json
        ├── chunk_000.m4a
        ├── chunk_001.m4a
        └── ...
```

- R2 lifecycle rule: auto-delete objects older than 7 days
- All access via presigned URLs (generated server-side, 1-hour expiry)
- Zero egress fees for processing pipeline downloads

---

## 9. User Flows

### 9.1 Agent Onboarding Flow

```
1. Agent receives invite link from owner (via WhatsApp/SMS)
2. Clicks link → Deep link opens app (or redirects to Play Store if not installed)
3. If app not installed: web fallback shows Play Store download CTA
4. Opens app → Invite code auto-extracted from deep link
5. Enters phone number → Receives OTP (rate limited: 5/hr)
6. Verifies OTP (max 3 attempts, 10-min expiry) → Creates account
7. Enters name
8. Automatically joined to company (from invite code)
9. Brief tutorial: 3 screens explaining record → insights → history
10. Sees home screen with "Record your first call" CTA
11. Grants microphone permission on first recording attempt
12. Prompted to whitelist battery optimization (OEM-specific guidance)
```

### 9.2 Recording Flow

```
1. Agent taps large 'Record' button on home screen
2. Check: is another recording in progress? If yes, show error. If no, continue.
3. Check: has agent exceeded daily call limit? If yes, show warning.
4. Consent reminder displayed: "Please ensure the other party is aware of recording"
5. Recording starts via foreground service → Timer visible, persistent notification shown
6. Agent conducts meeting (can minimize app, turn off screen — recording continues)
7. If phone call received: audio focus handler pauses recording, shows "Recording paused" notification
8. Agent can manually pause/resume via notification or in-app button
9. Agent taps 'Stop' button
10. If duration < 30 seconds → Prompt: "Discard this recording?"
11. Audio quality gate runs: check silence ratio and energy levels
12. If quality poor → Warning: "Audio quality may be too low for accurate transcription. Save anyway?"
13. Post-call form appears
14. Agent enters/selects customer name (with fuzzy autocomplete)
15. Optionally enters customer phone number
16. Agent selects call type
17. Agent selects outcome
18. Optionally adds notes
19. Taps 'Save'
20. Audio split into 30s chunks on-device
21. Call saved locally → Sync status shows 'Uploading'
22. Background upload begins (or queues if offline)
```

### 9.3 Processing Flow (Backend — Async Job Queue)

```
Stage 1: UPLOAD COMPLETE → Enqueue job
  1. All chunks received in R2 → Call status set to 'uploaded'
  2. Job enqueued in Inngest/Trigger.dev with call_id

Stage 2: TRANSCRIPTION (idempotent, independently retryable)
  3. Call status set to 'transcribing'
  4. Worker downloads chunks from R2 via presigned URLs
  5. Sends chunks to Groq Whisper API (parallel, max 5 concurrent per call)
  6. Stitches transcripts, removes overlap duplicates
  7. Saves transcript to call_insights table
  8. Call status set to 'transcribed'
  9. Processing cost logged (transcription cost in paisa)

Stage 3: ANALYSIS (idempotent, independently retryable)
  10. Call status set to 'analyzing'
  11. Sends transcript to LLM with insight extraction prompt
  12. Saves summary, key_topics, next_steps, sentiment to call_insights
  13. Processing cost logged (LLM cost in paisa)
  14. Call status set to 'completed'
  15. Sends push notification to agent via FCM

Error handling:
  - Each stage retries independently (max 5, exponential backoff)
  - Failure in Stage 3 does NOT re-run Stage 2
  - After max retries → status set to 'failed'
  - Owner can view failed calls on dashboard and trigger reprocessing
```

### 9.4 Owner Company Setup Flow

```
1. Owner visits web dashboard
2. Enters phone number → Receives OTP (rate limited)
3. Verifies OTP → Account created with 'owner' role
4. Prompted to create company
5. Enters company name
6. Company created with unique invite code
7. Deep link generated: https://app.salesvoice.in/invite/{code}
8. Sees dashboard with empty state: "Invite your first agent" with copy-link CTA
9. Can copy link or share directly to WhatsApp
```

### 9.5 Daily Report Flow

```
1. Scheduled job runs at 8 PM daily (via async queue cron)
2. For each company: query day's calls
3. Aggregate metrics (total calls, per-agent breakdown, outcomes)
4. Generate brief summary text via LLM (constrained to <500 chars for WhatsApp readability)
5. Save to daily_reports table
6. Owner can view in dashboard
7. Owner taps 'Share to WhatsApp' → Opens WhatsApp with pre-filled text
```

### 9.6 Removed Agent Flow

```
1. Owner/admin clicks "Remove agent" → Confirmation dialog
2. Agent's status set to 'removed' in database
3. Agent's auth tokens immediately revoked (Supabase Auth admin API)
4. RLS policies deny all further access
5. Agent's pending uploads on their device continue to sync (data belongs to company)
6. On agent's next app open: "You've been removed from [Company Name]" screen
7. Agent can still create a new account and join a different company
8. All of agent's historical call data remains accessible to owner
```

---

## 10. UI/UX Requirements

### 10.1 Design Principles

- **Simplicity First:** Minimize taps to complete core actions. Recording should be one tap.
- **Offline-Aware:** Always show sync status. Never block the user due to connectivity.
- **Low-End Device Friendly:** Optimize for mid-range Android phones. Avoid heavy animations.
- **Bilingual Ready:** Design should accommodate both English and Hindi text lengths.
- **Trust Building:** Clear feedback on what's happening (uploading, processing, complete).
- **Accessible:** Minimum 48dp touch targets, sufficient color contrast (WCAG AA), colorblind-safe outcome indicators (use icons alongside colors).

### 10.2 Agent App Screens

#### Home / Recording Screen
- Large, prominent 'Record' button (primary action)
- Sync status badge ('3 calls pending')
- Quick access to recent calls
- Company name displayed in header
- Personal stats summary (calls this week)
- Empty state for new agents: "Record your first call" with illustration

#### Active Recording Screen
- Large timer showing duration
- Audio waveform visualization with real-time audio level indicator
- 'Pause' button and 'Stop' button (equally prominent)
- Minimal UI to avoid distraction
- Persistent notification with timer + pause/stop controls
- Continues working with screen off

#### Post-Call Metadata Screen
- Customer name input with fuzzy autocomplete dropdown
- Customer phone number input (optional, with explanation: "Helps avoid duplicate customers")
- Company name input (optional)
- Call type selector (pill buttons)
- Outcome selector (color-coded with icons: green checkmark/yellow dash/red x for colorblind accessibility)
- Notes field (optional, multiline)
- Save button
- Audio quality warning banner (if flagged)

#### Call History Screen
- List of calls sorted by date (newest first), paginated (20 per page)
- Each card shows: Customer name, date/time, duration, outcome badge, status
- Failed calls highlighted with "Tap to retry" action
- Filter chips (date range default: last 7 days, customer)
- Pull-to-refresh

#### Call Detail Screen
- Header: Customer name, call type badge, outcome badge
- Agent notes section (editable)
- Summary section (collapsible) with thumbs up/down
- Key topics as tags
- Next steps as checklist
- Full transcript (scrollable, tappable words for correction)
- Processing status if not yet complete
- Edit button for metadata (customer, type, outcome)

#### Personal Analytics Screen
- Calls this week / this month counters
- Outcome distribution chart
- Most-visited customers list
- Week-over-week comparison

### 10.3 Owner Dashboard Screens

#### Dashboard Home
- Key metrics cards (calls today, active agents, outcome breakdown)
- Recent calls list (real-time updates via Supabase Realtime)
- Agent activity summary
- Quick filters
- "New calls available" indicator for real-time updates
- Empty state for new companies

#### Team Management
- List of agents with status and role badge (owner/admin/agent)
- Invite link display with copy button
- Remove agent action (with confirmation)
- Promote to admin action
- Configure daily call limit
- Agent detail view

#### Calls View
- Filterable table/list of all calls with cursor-based pagination
- Default view: last 7 days
- Columns: Agent, Customer, Date, Duration, Type, Outcome, Status
- Click to expand/view details
- Failed calls tab with reprocess button

#### Reports
- Today's summary (brief format)
- Share to WhatsApp button
- Historical reports list (future)

---

## 11. Non-Functional Requirements

### 11.1 Performance

| Requirement | Target | Measurement |
|-------------|--------|-------------|
| App launch time | < 3 seconds | Time from tap to home screen ready |
| Recording start latency | < 500ms | Time from tap to recording active |
| Transcript processing | < 5 minutes for 1-hour call | End-to-end processing time |
| Dashboard page load | < 2 seconds | Time to interactive |
| Offline operation | 100% core features | Recording, viewing cached calls/insights |
| Chunk upload speed | < 5 seconds per chunk on 4G | Individual chunk upload time |

### 11.2 Scalability

| Metric | MVP Target | Year 1 Target |
|--------|------------|---------------|
| Concurrent companies | 100 | 1,000 |
| Total agents | 1,000 | 10,000 |
| Daily calls processed | 5,000 | 50,000 |
| Audio storage (R2, peak with 7-day retention) | 500 GB | 5 TB |

### 11.3 Reliability

- **System uptime:** 99.5% availability (excluding planned maintenance)
- **Data durability:** Zero data loss for completed uploads
- **Processing reliability:** < 1% permanent failure rate (after retries per stage)
- **Offline resilience:** Core recording and cached insight viewing works without internet
- **Pipeline recovery:** Each processing stage independently retryable; no wasted work on partial failures

### 11.4 Compatibility

- **Android version:** Android 8.0 (API 26) and above
- **Target devices:** Mid-range smartphones (2GB+ RAM)
- **OEM testing:** Xiaomi (MIUI), Samsung (OneUI), Vivo (FuntouchOS), Oppo (ColorOS), Realme (RealmeUI)
- **Web browsers:** Chrome 90+, Safari 14+, Firefox 90+, Edge 90+
- **Screen sizes:** Responsive design for mobile, tablet, desktop

### 11.5 Localization

- **MVP languages:** English (default)
- **Transcription languages:** English, Hindi, Hindi-English code-mixed
- **Future:** Hindi UI, regional language transcription

### 11.6 Accessibility

- Minimum touch target size: 48dp (Android Material guidelines)
- Color contrast: WCAG AA compliance (4.5:1 for text, 3:1 for large text)
- Outcome indicators: use icons alongside colors (not color-only) for colorblind users
- Screen reader compatibility for core flows (recording, call history)

---

## 12. Security & Privacy

### 12.1 Authentication & Authorization

- Phone OTP-based authentication (no passwords)
- OTP rate limiting: max 5 requests/hour per phone number, max 3 verification attempts per OTP, 10-minute OTP expiry
- Block disposable/VoIP numbers (use provider validation)
- JWT tokens for session management:
  - Access token expiry: 1 hour
  - Refresh token expiry: 30 days
  - Refresh token rotation on each use
- Immediate token revocation when agent is removed (Supabase Auth admin API)
- Row Level Security for data isolation
- Role-based access: owner > admin > agent
- Invite-only company membership
- Minimum app version enforcement on API calls (reject outdated clients)

### 12.2 Data Protection

- All data encrypted in transit (TLS 1.3)
- Data encrypted at rest (Supabase default for database, R2 default for storage)
- Audio files auto-deleted after 7 days via R2 lifecycle policy
- Audio access via presigned URLs only (1-hour expiry, generated server-side)
- No direct/public URLs to audio files

### 12.3 Upload Security

- Server-side file validation:
  - MIME type check (audio/mp4, audio/aac only)
  - Max file size per chunk: 5MB
  - Max total file size per call: 250MB
  - Audio header verification (reject non-audio files)
- Per-agent daily call limit (configurable, default 15) as cost guardrail
- Rate limiting on all API endpoints

### 12.4 Privacy Considerations

| Aspect | Approach |
|--------|----------|
| **Consent model** | Trust-based — agents verbally inform customers before recording |
| **In-app reminder** | Displayed before each recording starts |
| **Data ownership** | Company owns call data, retained even after agent removal |
| **Agent privacy** | Agents cannot see other agents' calls |

### 12.5 Compliance

- Privacy policy and terms of service required
- Data stored in India-region servers (Supabase Singapore/Mumbai, R2 Asia-Pacific)
- Future consideration: DPDP Act compliance as regulations evolve

---

## 13. Monitoring & Observability

### 13.1 Mobile App Monitoring

| Tool | Purpose | Cost |
|------|---------|------|
| Firebase Crashlytics | Crash reporting, ANR detection, device-specific issues | Free (unlimited) |
| Firebase Analytics | App usage, screen flows, retention | Free |
| Custom sync metrics | Upload success/failure rates, chunk retry counts, pending upload age | Built-in |

#### Key Mobile Metrics to Track
- Crash-free users rate (target: >99.5%)
- Recording start success rate
- Average sync time per call
- Sync failure rate by network type (WiFi vs 4G vs 3G)
- Pending upload age distribution (how long calls wait before syncing)
- Audio quality gate rejection rate
- App performance on target OEM devices

### 13.2 Backend Monitoring

| Tool | Purpose | Cost |
|------|---------|------|
| Sentry (free tier) | Error tracking for Edge Functions, job queue workers | Free (5K events/month) |
| Supabase Dashboard | Database metrics, API latency, connection pool usage | Included |
| Job queue dashboard (Inngest/Trigger.dev) | Processing pipeline visibility, retry tracking, stage latency | Included in plan |

#### Key Backend Metrics to Track
- Processing pipeline success rate per stage (target: >99%)
- Average processing latency by stage (upload → transcribed → completed)
- Transcription cost per call (tracked in `processing_cost_cents`)
- Failed calls count and age (how long until reprocessed)
- API response times (p50, p95, p99)
- Active database connections vs pool limit
- R2 storage usage and lifecycle deletion rate

### 13.3 Alerting

| Alert | Condition | Channel |
|-------|-----------|---------|
| Processing failure spike | >5% calls failing in 1 hour | Email/Slack |
| Transcription provider down | 3 consecutive API failures | Email/Slack |
| Database connection pool exhaustion | >80% connections used | Email/Slack |
| R2 storage approaching limit | >80% of budget threshold | Email |
| Agent with 0 synced calls for 48+ hours | Agent recorded locally but nothing synced | Dashboard warning for owner |

---

## 14. Testing Strategy

### 14.1 Device Testing Matrix

| Device | OS | Price Range | Why |
|--------|------|-------------|-----|
| Xiaomi Redmi Note 12 | MIUI 14 / Android 13 | ₹12,000 | Most popular mid-range, aggressive battery optimization |
| Samsung Galaxy M14 | OneUI 5 / Android 13 | ₹13,000 | Second most popular, different OEM behavior |
| Vivo Y56 | FuntouchOS / Android 13 | ₹14,000 | Vivo-specific background restrictions |
| Realme Narzo 60 | RealmeUI / Android 13 | ₹15,000 | Realme-specific auto-start restrictions |
| Low-end: Samsung Galaxy A04 | OneUI Core / Android 12 | ₹8,000 | 2GB RAM stress testing |

### 14.2 Test Categories

#### Unit Tests
- Audio chunking logic (correct chunk sizes, overlap, manifest generation)
- Sync queue ordering (smallest-first)
- Retry logic with exponential backoff
- OTP rate limiting logic
- Customer fuzzy matching
- Audio quality gate (silence detection, energy thresholds)
- Token expiry and refresh logic

#### Integration Tests
- End-to-end recording → chunking → upload → R2 storage flow
- Supabase Auth OTP flow with rate limiting
- RLS policy verification (agent can't see other agent's calls, cross-company isolation)
- Deep link invite flow (app installed vs not installed)
- Presigned URL generation and expiry
- Job queue: enqueue → process → checkpoint → complete
- Job queue: failure → retry → eventual success
- Job queue: max retries → permanent failure → owner notification

#### Recording Edge Case Tests
- Recording while screen is off (foreground service + wakelock)
- Recording during incoming phone call (audio focus loss/regain)
- Recording when battery drops below 5%
- Recording when storage is nearly full
- Starting recording while another is in progress (should block)
- Recording < 30 seconds (discard prompt)
- Recording exactly 2 hours (max duration cutoff)
- App force-killed during recording (recovery on next launch)
- Pause/resume across screen on/off cycles

#### Offline & Sync Tests
- Record 5 calls offline → come online → verify all sync in order
- Upload interrupted mid-chunk → verify resume from last successful chunk
- Network flapping (connect/disconnect rapidly) during sync
- Upload with only 2G connectivity (very slow)
- Max retries exhausted → verify "tap to retry" shown
- Removed agent's pending uploads still sync
- Verify cached insights available offline after sync

#### Processing Pipeline Tests
- Normal flow: 30-minute call → transcription → analysis → completed
- Transcription failure → retry → success (verify no duplicate transcripts)
- Analysis failure → retry (verify transcription not re-run)
- Corrupt audio chunk → graceful failure, skip chunk, note gap in transcript
- Concurrent processing of 50+ calls (load test)
- 2-hour call with 240 chunks (stress test)

#### Security Tests
- Verify RLS: agent A cannot access agent B's calls
- Verify RLS: company A cannot access company B's data
- Removed agent's token is rejected immediately
- Presigned URL expired → returns 403
- Upload non-audio file → rejected
- Upload oversized file → rejected
- OTP brute force (>3 attempts) → blocked
- OTP spam (>5 requests/hour) → rate limited

#### Dashboard Tests
- Empty state rendering (new company, zero calls)
- Pagination with 10,000+ calls
- Real-time updates (new call appears without refresh)
- Filter combinations (agent + date range + outcome)
- Failed call reprocessing from dashboard
- WhatsApp share generates correct format (<500 chars)

### 14.3 Performance Benchmarks

| Scenario | Target | Test Method |
|----------|--------|-------------|
| App cold start on 2GB RAM device | < 3 seconds | Automated via Firebase Test Lab |
| Recording start latency | < 500ms | Manual timing on test devices |
| Chunk upload on 4G | < 5 seconds per chunk | Network-conditioned test |
| Chunk upload on 3G | < 15 seconds per chunk | Network-conditioned test |
| Dashboard load with 1000 calls | < 2 seconds TTI | Lighthouse CI |
| 50 concurrent call processing | All complete within 10 minutes | Load test via k6/Artillery |

### 14.4 QA Process

- **Pre-merge:** Unit tests + integration tests run in CI (GitHub Actions)
- **Weekly:** Manual testing on all 5 devices in the device matrix
- **Pre-release:** Full regression on recording, sync, processing, and dashboard flows
- **Post-release:** Monitor Crashlytics for new crash clusters within 24 hours

---

## 15. Metrics & Success Criteria

### 15.1 Key Performance Indicators

#### Acquisition Metrics

| Metric | Definition | MVP Target (Month 3) |
|--------|------------|----------------------|
| Companies onboarded | Total companies with active subscription | 50 |
| Total agents | Total agents across all companies | 300 |
| Activation rate | % of signed-up agents who record first call within 7 days | 70% |

#### Engagement Metrics

| Metric | Definition | Target |
|--------|------------|--------|
| Daily Active Agents | Agents who record at least 1 call/day | 60% of total agents |
| Calls per agent per day | Average calls recorded | 3-4 calls |
| Dashboard daily visits | Unique owner logins per day | 80% of owners |
| Report share rate | % of owners who share daily report | 50% |

#### Quality Metrics

| Metric | Definition | Target |
|--------|------------|--------|
| Transcription accuracy | Word Error Rate (WER) | < 15% for English, < 25% for code-mixed |
| Processing success rate | % of calls fully processed | > 99% |
| Sync success rate | % of uploads completing within 24 hours | > 98% |
| AI insight feedback | % of thumbs-up ratings on insights | > 70% |

#### Business Metrics

| Metric | Definition | Target |
|--------|------------|--------|
| Monthly Recurring Revenue (MRR) | Total subscription revenue | ₹2,00,000 by Month 6 |
| Churn rate | % of companies canceling per month | < 5% |
| Net Promoter Score (NPS) | Customer satisfaction | > 40 |

### 15.2 MVP Success Criteria

The MVP will be considered successful if, within 3 months of launch:

- 50+ companies actively using the platform
- 300+ agents recording calls regularly
- 1,000+ calls processed weekly
- 70%+ of agents find the transcription accurate enough to be useful
- 5+ organic referrals from existing customers

---

## 16. Roadmap & Phases

### Phase 1: MVP (Weeks 1-10)

**Goal:** Core recording-to-insight loop working end-to-end with production-grade reliability

#### Week 1-2: Foundation
- Supabase project setup (auth, database, Realtime)
- Cloudflare R2 bucket setup with lifecycle rules
- Kotlin Android project setup with Room DB, foreground service scaffolding
- Next.js dashboard project setup
- Data model implementation with RLS policies
- Async job queue setup (Inngest/Trigger.dev)
- Deep linking setup (assetlinks.json, web fallback page)

#### Week 3-4: Agent App Core
- Phone OTP authentication with rate limiting
- Recording functionality with foreground service + wakelock (screen-off support)
- Pause/resume recording
- Audio quality gate (silence/energy detection)
- On-device audio chunking
- Resumable chunked upload to R2
- Post-call metadata entry with fuzzy customer autocomplete
- Local Room DB for offline storage + cached insights

#### Week 5-6: Agent App Polish
- Call history with pagination
- Call detail view with transcript display
- Agent personal analytics screen
- First-time experience (tutorial, empty states)
- Sync status UI, "tap to retry" for failed uploads
- Force-update version check
- OEM battery optimization whitelisting prompts

#### Week 7-8: Backend Processing
- Transcription pipeline via Groq Whisper API (chunked, parallel)
- Transcript stitching with overlap removal
- LLM insight generation (summary, topics, next steps, sentiment)
- Processing checkpointing (independent stage retries)
- Push notifications via FCM
- Processing cost tracking per call
- Failed call handling (owner reprocessing)

#### Week 9-10: Owner Dashboard & Launch Prep
- Dashboard authentication and company setup
- Team management (invite, remove, admin role)
- Call list with filters, pagination, and real-time updates
- Call detail view with insights
- Daily report generation (brief format)
- WhatsApp share integration
- Firebase Crashlytics + Sentry integration
- Bug fixes and performance optimization
- Device matrix testing

### Phase 2: Enhancement (Weeks 11-18)

- Improved Hindi-English transcription (Sarvam AI evaluation)
- Advanced analytics on dashboard
- Customer conversation history grouping
- Transcript search
- Thumbs up/down feedback on insights
- Transcript correction mechanism
- Payment integration (subscription billing)
- Customer phone number deduplication improvements

### Phase 3: Scale (Weeks 19-26)

- iOS app (evaluate Flutter or native Swift)
- AI coaching suggestions
- Industry-specific insight templates
- WhatsApp Business API for automated reports
- Data export (CSV)
- Hindi UI localization
- Self-hosted Whisper migration (if volume justifies)
- Location data capture (optional, with consent)

---

## 17. Cost Analysis

### 17.1 Per-Call Processing Costs

Estimated costs for processing a 1-hour call (using Groq Whisper):

| Component | Cost (INR) | Notes |
|-----------|------------|-------|
| Transcription (Groq Whisper) | ₹5 | ~$0.06 per hour (~$0.001/min) |
| LLM processing (GPT-4o Mini) | ₹3-5 | Summary, topics, sentiment |
| Storage (R2, temporary) | ₹0.15 | ~10MB chunks for 7 days, zero egress |
| **Total per 1-hour call** | **₹8-10** | ~75% cheaper than v1.0 estimate |

### 17.2 Monthly Infrastructure Costs (MVP Scale)

| Component | Monthly Cost (INR) | Notes |
|-----------|-------------------|-------|
| Supabase Pro | ₹2,100 | $25/month base |
| Additional database | ₹0-4,000 | Based on usage |
| Cloudflare R2 storage | ₹300-500 | ~250GB peak at $0.015/GB, zero egress |
| Async job queue (Inngest) | ₹0-2,000 | Free tier covers MVP, paid at scale |
| Firebase (FCM + Crashlytics) | ₹0 | Free |
| Sentry (free tier) | ₹0 | Free (5K events/month) |
| Domain & SSL | ₹500 | Annual amortized |
| OTP SMS provider (MSG91) | ₹1,000-3,000 | ~₹0.20/OTP, depends on auth volume |
| **Total infrastructure** | **₹3,900-12,100** | Variable based on usage |

### 17.3 Unit Economics (Revised)

For an agent doing 4 calls/day (average 30 minutes each):

- Daily processing cost: ~₹16-20 (down from ₹70-80 in v1.0)
- Monthly processing cost: ~₹350-440 per agent
- Suggested pricing: ₹500-800 per agent/month (now profitable per agent)

### 17.4 Pricing Recommendations

| Plan | Price | Includes |
|------|-------|----------|
| Starter | ₹3,000/month | Up to 5 agents, 500 minutes transcription |
| Growth | ₹7,500/month | Up to 15 agents, 1,500 minutes transcription |
| Business | ₹15,000/month | Up to 30 agents, 3,500 minutes transcription |
| Enterprise | Custom | Unlimited agents, volume discounts |

---

## 18. Risks & Mitigations

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| Hindi-English transcription quality poor | High | Medium | Evaluate Sarvam AI early; collect feedback via thumbs up/down; build transcript correction mechanism |
| Agents resist adoption (feel surveilled) | High | Medium | Position as productivity tool; show personal analytics; agent notes for their own use |
| Offline sync causes data loss | High | Low | Robust local Room DB; resumable chunked uploads; clear sync status UI; "tap to retry" for failures |
| Processing costs exceed revenue | Medium | Low | Groq Whisper reduces costs 75%; track cost per call; per-agent daily limits as guardrails |
| Low-end phones have performance issues | Medium | Medium | Native Kotlin (no Flutter overhead); test on ₹8k-15k device matrix; lazy loading |
| OEM background restrictions kill recording | High | High | Foreground service + wakelock; OEM-specific whitelisting prompts; test on 5 target OEM devices |
| Groq API rate limits or downtime | Medium | Low | Fallback to GPT-4o Mini Transcribe ($0.003/min); processing queue absorbs temporary outages |
| Privacy/legal concerns | Medium | Low | Consent reminder; data retention policy; legal review |
| Competition from enterprise players | Medium | Low | Focus on India-specific needs; price advantage; local language support |
| Audio quality too poor for transcription | Medium | Medium | On-device quality gate; flag poor recordings; agent guidance on microphone placement |

---

## 19. Open Questions & Future Considerations

### 19.1 Open Questions for MVP

- Which transcription provider (Groq Whisper vs. Sarvam) performs better for code-mixed Hindi-English? Need A/B testing.
- What is the acceptable transcription latency? 5 minutes? 15 minutes?
- What's the minimum viable insight set that delivers the 'aha moment'?
- What is the optimal audio bitrate for balancing file size vs transcription accuracy on mid-range phone microphones?

### 19.2 Future Feature Considerations

| Feature | Description |
|---------|-------------|
| **AI Coaching** | Post-call suggestions for improving sales techniques |
| **CRM Integration** | Sync call data to Salesforce, Zoho, etc. |
| **Competitive Intelligence** | Extract mentions of competitors from calls |
| **Voice Analytics** | Tone analysis, talk-to-listen ratio, interruption detection |
| **Phone Call Recording** | Extend to phone calls (not just in-person) |
| **Team Leaderboards** | Gamification elements for agent motivation |
| **Custom Insight Templates** | Industry-specific extraction (pharma: products mentioned, insurance: objections) |
| **Location Tracking** | Optional GPS-based location capture per call (with consent) for coverage maps |
| **Audio Playback** | Time-limited playback while audio exists in R2 (7 days) as transcription fallback |

### 19.3 Technical Debt Considerations

- Plan for migration path if Supabase limits become constraining
- Consider CDN for dashboard static assets as user base grows
- Evaluate on-device ML for basic processing to reduce cloud costs
- Plan migration to self-hosted Whisper when processing volume exceeds 500 hours/day
- API versioning from day one (`/v1/` prefix) to support backward compatibility

---

## 20. Appendix

### A. Glossary

| Term | Definition |
|------|------------|
| **Agent** | Sales representative who records calls using the mobile app |
| **Owner** | Business owner who created the company and has full access |
| **Admin** | Manager promoted by owner, can view all calls and manage agents |
| **Call** | A recorded customer meeting/conversation |
| **Insight** | AI-generated analysis including summary, topics, next steps |
| **Code-switching** | Mixing two languages (Hindi-English) in conversation |
| **OTP** | One-Time Password for phone authentication |
| **RLS** | Row Level Security — Supabase feature for data isolation |
| **R2** | Cloudflare R2 — S3-compatible object storage with zero egress fees |
| **Chunk** | A 30-second segment of audio, the unit of upload and transcription |
| **Foreground Service** | Android service that runs with a persistent notification, survives screen-off |

### B. Sample Daily Report Format

```
SalesVoice Daily Report — 24 Jan 2025
Company: ABC Pharma

Calls: 12 | Agents: 4/6 active

Ramesh: 4 calls, 2 positive
Priya: 3 calls, pricing objection noted
Amit: 3 calls, 1 closing scheduled
Neha: 2 calls, new customer

Details: app.salesvoice.in/dashboard
```

*Note: Report kept under 500 characters for WhatsApp readability.*

### C. LLM Prompt Template for Insights

```
You are analyzing a sales call transcript. Extract the following:

1. Summary (2-3 sentences capturing the key points)
2. Key topics discussed (list of 3-5 items)
3. Next steps/action items (list of concrete actions)
4. Overall sentiment: positive/neutral/negative

Respond in JSON format:
{
  "summary": "...",
  "key_topics": ["topic1", "topic2", ...],
  "next_steps": ["action1", "action2", ...],
  "sentiment": "positive|neutral|negative"
}

Transcript:
{transcript}
```

### D. API Endpoints Summary (v1)

All endpoints prefixed with `/v1/`.

#### Authentication
- `POST /v1/auth/otp/send` — Send OTP to phone number (rate limited: 5/hr per number)
- `POST /v1/auth/otp/verify` — Verify OTP and get session (max 3 attempts, 10-min expiry)

#### App Config
- `GET /v1/config/min-version` — Get minimum required app version

#### Companies
- `POST /v1/companies` — Create company (owner only)
- `GET /v1/companies/:id` — Get company details
- `GET /v1/companies/:id/invite-link` — Get/regenerate invite link
- `PATCH /v1/companies/:id/settings` — Update company settings (daily call limit, etc.)

#### Users
- `GET /v1/users/me` — Get current user profile
- `POST /v1/users/join/:invite_code` — Join company via invite
- `GET /v1/companies/:id/agents` — List agents (owner/admin only)
- `DELETE /v1/companies/:id/agents/:agent_id` — Remove agent (owner/admin only)
- `PATCH /v1/companies/:id/agents/:agent_id/role` — Change agent role (owner only)

#### Calls
- `POST /v1/calls` — Create call record (agent)
- `GET /v1/calls` — List calls (filtered by role, cursor-based pagination, default last 7 days)
- `GET /v1/calls/:id` — Get call details with insights
- `PATCH /v1/calls/:id` — Update call metadata (agent notes, customer, type, outcome)
- `POST /v1/calls/:id/reprocess` — Trigger reprocessing of failed call (owner/admin only)
- `GET /v1/calls/failed` — List failed calls (owner/admin only)

#### Upload
- `POST /v1/calls/:id/upload-url` — Get presigned R2 upload URL for a chunk
- `POST /v1/calls/:id/upload-complete` — Signal all chunks uploaded, trigger processing

#### Customers
- `GET /v1/customers` — List customers (with fuzzy search)
- `POST /v1/customers` — Create customer
- `PATCH /v1/customers/:id` — Update customer details

#### Insights Feedback
- `POST /v1/calls/:id/insights/feedback` — Submit thumbs up/down rating
- `POST /v1/calls/:id/insights/corrections` — Submit transcript corrections

#### Reports
- `GET /v1/reports/daily` — Get today's daily report
- `GET /v1/reports/daily/:date` — Get historical report

#### Agent Analytics
- `GET /v1/analytics/agent/me` — Get personal analytics (calls count, outcome distribution)

---

### E. Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | January 2025 | — | Initial PRD creation |
| 2.0 | January 2025 | — | Major revision: 44 gaps addressed. Architecture changes (Cloudflare R2, async job queue, Groq Whisper, native Kotlin). Added: pause/resume, audio chunking, resumable upload, admin role, agent notes/editing, AI feedback, audio quality gate, foreground service recording, OTP rate limiting, token revocation, pipeline checkpointing, offline insight caching, deep linking, force-update, API versioning, personal analytics, empty states, accessibility requirements. New sections: Monitoring & Observability, Testing Strategy. |

---

*End of Document*
