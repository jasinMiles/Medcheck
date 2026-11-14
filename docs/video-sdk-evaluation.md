# Video SDK Evaluation

**Project:** MedCheck frontend
**Prepared by:** Michael Avwerosuo Okeme and Ndabai Daniella Chiderah.
**Date:** 14/11/2026

## 1. Purpose

MedCheck needs video consultations between patients (or families) and providers. This document sets out what the video feature must do, compares the SDKs we considered, records a feasibility test, and recommends one.

## 2. Requirements

| # | Requirement | Why it matters | Must / Nice |
|---|---|---|---|
| 1 | One-to-one video and audio calls | Core consultation flow | Must |
| 2 | Works in phone browsers | Many patients and families will be on phones | Must |
| 3 | Copes with weak internet (reconnects, lowers quality instead of dropping) | Failed calls are on the QA risk list | Must |
| 4 | Privacy and security suitable for health data, including a BAA (Business Associate Agreement) | Calls carry sensitive health information | Must |
| 5 | Works with React | Our chosen framework | Must |
| 6 | Clear, affordable pricing with a free tier for development | Small team and early stage | Must |
| 7 | Screen share, in-call chat, waiting room | Possible extras | Nice |
| 8 | Recording | Only if the brief requires it | [confirm with PM] |
| 9 | Data location and fit with Nigerian data protection rules | Compliance | [confirm with PM / backend team] |

## 3. Options considered

Daily and Agora were shortlisted. 100ms was reviewed and not shortlisted. Facts below come from each company's own pages. 
| Question | Daily | Agora | 100ms |
|---|---|---|---|
| Free tier | 10,000 participant-minutes/month free for all accounts. | 10,000 free minutes/month across RTC audio, HD video, and basic media services.| 10,000 conferencing minutes/month, plus 300 free minutes/month each for recording, RTMP streaming, post-call transcription, and AI summaries. |
| Price after free tier | $0.004 per participant-minute for video/audio, with automatic volume discounts. Cloud recording is $0.01349/min and live transcription is $0.0059/min. | Pay-as-you-go starts at $0.99 per 1,000 minutes for standard audio and scales up by video resolution (e.g., HD/Full HD video). Monthly prepaid packages start at $45.99/month for 50,000 standard minutes.| $0.004 per participant-minute for SD/HD conferencing. Recording is $0.0135/min and chat is free. |
| HIPAA / BAA available, and cost | Yes.Requires the Healthcare add-on at $500/month, which includes a signed BAA at no extra fee. | Yes.Agora's support plans that include HIPAA support start at $1,200/month (Standard tier on an annual agreement). | Yes.Available as a $500/month healthcare add-on with a signed BAA, or included free if you purchase an enterprise support plan. |
| React support | Official React SDK | Official React SDK | Official React SDK |
| Prebuilt UI or fully custom | Both. | Primarily custom | Both. Offers 100ms Prebuilt |
| Weak-network features | Daily Adaptive Bitrate (real-time resolution/bitrate scaling), Adaptive HEVC. | Runs on Agora's SD-RTN (Software-Defined Real-Time Network) with adaptive bitrate, packet-loss concealment, automatic network resilience, and quality fallback controls. | Built-in subscribe degradation for low bandwidth, network-switch handling (Wi-Fi to cellular without dropping), disconnection auto-retries, VPN/cloud proxy handling, and connection stability indicators. |
| Where data is stored | Standard recordings are stored on Daily's infrastructure unless configured to push to your own AWS S3 / OCI bucket (mandatory for HIPAA mode). | Cloud recordings are uploaded directly to your own third-party cloud storage (AWS S3, Google Cloud, Azure, Alibaba, Tencent, or S3-compatible) in your chosen region. | Hosted on GCP and AWS with database residency options in the USA, Europe, and India. Live media is not stored; recordings upload to your own cloud storage bucket (mandatory and write-only for HIPAA workspaces). |
| Documentation quality  | Excellent.  | Comprehensive but dense. | Strong and developer-friendly. |
| Source link and date checked | https://www.daily.co/products/video-sdk/, Nov 14, 2026
 | https://docs.agora.io/en/api-reference/sdks, Nov 14, 2026 | https://www.100ms.live/video-call-sdk, Nov 14, 2026 |

**Why 100ms was not shortlisted:** 100ms is built around an opinionated role-and-template architecture geared toward large-scale interactive rooms and live-streaming, whereas MedCheck's core workflow is straightforward 1-on-1 or small family-and-provider consultations. Daily offered a faster path to a working React prototype and clearer public documentation for healthcare compliance onboarding.

## 4. Feasibility test (Daily)

**Goal:** Two participants join the same video room from different browser tabs (or devices) with working two-way audio and video.

**What we did**
1. Created a Daily account and provisioned a test room via the Daily dashboard/API.
2. Integrated Daily's React quickstart (@daily-co/daily-js / React hooks) in a dedicated feature branch.
3. Joined the room from two browser tabs and verified cross-device behavior on a mobile browser and under simulated network throttling.

**Results**

| Check | Result | Notes |
|---|---|---|
| Two participants see and hear each other | pass | Low-latency audio/video handshake worked across tabs|
| Works on a phone browser | pass | Tested on mobile Chrome/Safari; camera/mic permissions prompted cleanly|
| Behaviour on a throttled connection | pass  | Auto-adjusted video quality under simulated 3G/slow 4G; audio stayed intelligible |
| Time taken to get a working call |  2 hours |Account setup, test room, and basic React call view |
| Problems hit |Minor | Local .env variable naming mismatch and initial browser autoplay/permission prompt handling |

**Test conditions:** fake data only, no real patient information. Free tiers normally do not include a BAA.

## 5. Recommendation

**We recommend Daily**, as agreed with the frontend lead, because:
- Fast time-to-working-call: Got a working two-party React call running in ~2 hours with minimal boilerplate.
- Solid browser & network resilience: Handled mobile browsers and throttled connections cleanly during our spike.
- Clear React SDK ergonomics: Straightforward state events for room join/leave, participant tracks, and device controls.

Agora remains a fallback because it offers mature global infrastructure and fine-grained media controls if Daily hits regional routing limits or edge-case WebRTC issues during QA.

## 6. Risks and open questions

| Risk or question | Who can answer | Next step |
|---|---|---|
| BAA: how and when do we get one for production? | PM / Daily sales | Request BAA terms and minimum tier requirements before staging/prod rollout |
| Will free minutes cover development and testing? | Frontend lead | Track monthly participant minutes in the Daily dashboard and set usage alerts |
| Behaviour on weak mobile connections at scale | QA | Add to test plan |
| Data location and Nigerian data protection requirements | PM / backend team | Confirm media relay regions, retention settings, and privacy notice updates |
| Room creation must be done securely (see note below) | Backend team | Agree design |

**Security note:** Daily API keys are secrets and should not be placed in frontend code. Rooms and access tokens are normally created by the backend, and the frontend only receives a room link or token. This affects how we use the `VITE_VIDEO_SDK_KEY` placeholder in `.env.example`.

## 7. Next steps

1. Frontend lead reviews this recommendation.
2. Agree with the backend team how rooms and tokens are created.
3. Confirm BAA and compliance requirements with the PM.
4. Plan the video call screen in the next sprint.