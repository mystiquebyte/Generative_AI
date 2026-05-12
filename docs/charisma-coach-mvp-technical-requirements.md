# Charisma Coach MVP Technical Requirements Document

## 1. Product Overview

Charisma Coach is an AI-powered mobile app that helps users improve confidence, communication skills, voice clarity, body language, public speaking, and social charisma through a personalized 6-week coaching plan.

The MVP should allow users to:

- Create an account.
- Take a communication assessment.
- Receive a communication score out of 100.
- Get a personalized 6-week coaching plan.
- Complete daily lessons.
- Record voice practice.
- Receive AI feedback.
- Track progress.
- Upgrade to a paid subscription.

## 2. Recommended Tech Stack

| Area | Technology |
| --- | --- |
| Mobile App | React Native with Expo |
| Web Landing Page | Next.js |
| Backend | Supabase Edge Functions |
| Database | Supabase PostgreSQL |
| Authentication | Supabase Auth |
| Storage | Supabase Storage |
| AI | OpenAI API |
| Payments | Stripe |
| Deployment | Vercel for web, Expo/EAS for mobile |

## 3. Core MVP User Flow

1. User opens the app.
2. User signs up or logs in.
3. User selects gender, primary goal, and experience level.
4. User completes the communication assessment quiz.
5. App calculates the Communication Score.
6. App assigns a user archetype.
7. App generates a personalized 6-week plan.
8. User views the dashboard.
9. User completes daily lessons.
10. User records voice practice.
11. AI gives feedback.
12. User tracks progress.
13. User upgrades to Pro or VIP.

## 4. MVP Screens

### Onboarding

| Screen | Purpose | Required Inputs / Actions |
| --- | --- | --- |
| Splash screen | Show brand and initialize session state. | Auto-route to welcome, dashboard, or auth. |
| Welcome screen | Communicate app value proposition. | Continue CTA. |
| Benefits screen | Explain confidence, voice, body language, and public speaking benefits. | Continue CTA. |
| Select gender | Capture profile personalization attribute. | Gender selection and optional skip. |
| Select goal | Identify user's primary communication goal. | Goal selection such as confidence, dating, leadership, public speaking, or networking. |
| Select experience level | Capture current communication skill level. | Beginner, intermediate, or advanced. |

### Assessment

| Screen | Purpose | Required Inputs / Actions |
| --- | --- | --- |
| Assessment intro | Explain the quiz and scoring. | Start assessment CTA. |
| Assessment questions | Capture scored answers by category. | One answer per question. |
| Score processing screen | Show loading state while backend calculates score. | No user input. |
| Score result screen | Display total score and archetype. | Continue to plan CTA. |
| Score breakdown screen | Display strongest and weakest categories. | Category details and recommended focus areas. |

### Program

| Screen | Purpose | Required Inputs / Actions |
| --- | --- | --- |
| 6-week plan screen | Show personalized plan summary. | Select week or today's lesson. |
| Week overview screen | Show lessons for the selected week. | Select lesson. |
| Daily lesson screen | Deliver lesson content and task. | Mark complete and optionally add notes. |
| Voice recording screen | Capture practice audio. | Record, playback, retry, and submit. |
| AI feedback screen | Display voice scores, summary, tips, and next exercise. | Save feedback and continue. |

### App Core

| Screen | Purpose |
| --- | --- |
| Dashboard | Show today's lesson, current score, streak, and plan progress. |
| Progress tracker | Show completed lessons, score changes, and category trends. |
| Challenges | List free and premium challenges with XP rewards. |
| Achievements | Show earned milestones and badges. |
| Certificate | Unlock certificate after program completion. |
| Profile/settings | Manage profile, subscription status, notifications, and logout. |

### Monetization

| Screen | Purpose | Required Behavior |
| --- | --- | --- |
| Pricing screen | Compare Free, Pro, and VIP plans. | Show price, feature list, and selected plan CTA. |
| Upgrade screen | Confirm plan selection and explain entitlement changes. | Start Stripe checkout for Pro or VIP. |
| Stripe checkout redirect | Send user to Stripe-hosted checkout and return to app/web success or cancel URL. | Refresh subscription status after return. |

## 5. Assessment Scoring System

Total score: 100 points.

Categories:

| Category | Maximum Points | Measurement Focus |
| --- | ---: | --- |
| Confidence | 25 | Self-belief, assertiveness, and comfort initiating interactions. |
| Voice Clarity | 20 | Pacing, articulation, volume, and filler-word control. |
| Body Language | 15 | Posture, gestures, eye contact, and physical presence. |
| Conversation Skills | 15 | Listening, questions, flow, and social adaptability. |
| Public Speaking | 15 | Prepared speaking, storytelling, and audience comfort. |
| Social Anxiety | 10 | Avoidance, nervousness, and recovery after awkward moments. |

Each quiz answer maps to a numeric value.

Example answer values:

| Answer Label | Value |
| --- | ---: |
| Very confident | 10 |
| Confident | 8 |
| Neutral | 5 |
| Not confident | 2 |
| Very uncomfortable | 0 |

### Scoring Requirements

- Every assessment question must include a category, answer label, and numeric answer value.
- The backend should normalize each category's raw answer total to the category's maximum point value.
- The total communication score should be the sum of all normalized category scores, rounded to the nearest whole number, with a maximum of 100.
- Social Anxiety should be scored so that lower anxiety produces a higher category score.
- The backend should persist both category scores and individual answers.

The backend should calculate:

- Category scores.
- Total communication score.
- Weakest category.
- Strongest category.
- Archetype.
- Recommended plan.

## 6. User Archetypes

| Archetype | Definition | Suggested Plan Emphasis |
| --- | --- | --- |
| Quiet Thinker | Low confidence, high thoughtfulness. | Confidence building, conversation starters, and speaking reps. |
| Fast Talker | High energy, weak pacing/clarity. | Breath control, pacing, articulation, and concise answers. |
| Social Avoider | High anxiety, avoids conversations. | Low-pressure exposure challenges and anxiety reduction exercises. |
| Emerging Leader | Moderate/high score, needs polish. | Presence, storytelling, public speaking, and leadership communication. |
| Natural Charmer | High score, needs advanced mastery. | Advanced influence, roleplay, charisma refinement, and high-stakes speaking. |

### Archetype Assignment Guidance

- Assign **Social Avoider** when Social Anxiety is the weakest category or anxiety-related answers indicate frequent avoidance.
- Assign **Fast Talker** when Voice Clarity is weak while confidence or energy-related answers are moderate/high.
- Assign **Quiet Thinker** when Confidence is weak but Conversation Skills or reflection-oriented responses are moderate/high.
- Assign **Emerging Leader** when total score is moderate/high and no single category is critically weak.
- Assign **Natural Charmer** when total score is high and category scores are consistently strong.

## 7. Database Tables

All user-owned tables should enforce Supabase Row Level Security so users can only read and mutate their own records. Server-only operations such as Stripe webhooks and AI analysis should use service-role access inside trusted backend functions.

### `profiles`

Stores user profile data.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `user_id` | `uuid` | References Supabase Auth user. |
| `full_name` | `text` | Optional display name. |
| `email` | `text` | User email. |
| `gender` | `text` | Onboarding selection. |
| `primary_goal` | `text` | Main coaching goal. |
| `experience_level` | `text` | Beginner, intermediate, or advanced. |
| `subscription_plan` | `text` | `free`, `pro`, or `vip`. |
| `subscription_status` | `text` | Stripe/subscription state. |
| `created_at` | `timestamptz` | Creation timestamp. |
| `updated_at` | `timestamptz` | Last update timestamp. |

### `assessments`

Stores assessment results.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `user_id` | `uuid` | Assessment owner. |
| `confidence_score` | `integer` | 0-25. |
| `voice_score` | `integer` | 0-20. |
| `body_language_score` | `integer` | 0-15. |
| `conversation_score` | `integer` | 0-15. |
| `public_speaking_score` | `integer` | 0-15. |
| `social_anxiety_score` | `integer` | 0-10. |
| `total_score` | `integer` | 0-100. |
| `archetype` | `text` | Assigned archetype. |
| `weakest_category` | `text` | Lowest normalized category. |
| `strongest_category` | `text` | Highest normalized category. |
| `created_at` | `timestamptz` | Creation timestamp. |

### `assessment_answers`

Stores individual answers.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `assessment_id` | `uuid` | References `assessments.id`. |
| `user_id` | `uuid` | Answer owner. |
| `question_id` | `text` | Stable question identifier. |
| `question_text` | `text` | Question prompt shown to user. |
| `answer_value` | `integer` | Numeric score value. |
| `answer_label` | `text` | Human-readable answer. |
| `category` | `text` | Scoring category. |
| `created_at` | `timestamptz` | Creation timestamp. |

### `lessons`

Stores lesson content.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `week_number` | `integer` | 1-6. |
| `day_number` | `integer` | Day within week. |
| `title` | `text` | Lesson title. |
| `description` | `text` | Short summary. |
| `lesson_type` | `text` | Reading, exercise, voice practice, challenge, or reflection. |
| `duration_minutes` | `integer` | Estimated duration. |
| `content` | `text` | Lesson body. |
| `task` | `text` | Completion task. |
| `premium_required` | `boolean` | Whether lesson is gated. |
| `created_at` | `timestamptz` | Creation timestamp. |

### `user_progress`

Tracks completed lessons.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `user_id` | `uuid` | Progress owner. |
| `lesson_id` | `uuid` | References `lessons.id`. |
| `completed` | `boolean` | Completion state. |
| `completed_at` | `timestamptz` | Completion timestamp. |
| `score_before` | `integer` | Optional pre-lesson self-rating. |
| `score_after` | `integer` | Optional post-lesson self-rating. |
| `notes` | `text` | User notes. |

### `voice_recordings`

Stores audio practice records.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `user_id` | `uuid` | Recording owner. |
| `lesson_id` | `uuid` | Optional related lesson. |
| `audio_url` | `text` | Supabase Storage URL or path. |
| `transcript` | `text` | Transcribed speech. |
| `pace_score` | `integer` | 0-100. |
| `clarity_score` | `integer` | 0-100. |
| `confidence_score` | `integer` | 0-100. |
| `filler_words_count` | `integer` | Count of filler words. |
| `ai_feedback` | `jsonb` | Structured feedback JSON. |
| `created_at` | `timestamptz` | Creation timestamp. |

### `challenges`

Stores user challenges.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `title` | `text` | Challenge title. |
| `description` | `text` | Challenge instructions. |
| `category` | `text` | Related skill category. |
| `xp_reward` | `integer` | XP awarded on completion. |
| `premium_required` | `boolean` | Whether challenge is gated. |

### `user_challenges`

Tracks challenge completion.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `user_id` | `uuid` | Challenge owner. |
| `challenge_id` | `uuid` | References `challenges.id`. |
| `status` | `text` | Not started, in progress, completed, or skipped. |
| `completed_at` | `timestamptz` | Completion timestamp. |

### `subscriptions`

Stores Stripe subscription data.

| Field | Suggested Type | Notes |
| --- | --- | --- |
| `id` | `uuid` | Primary key. |
| `user_id` | `uuid` | Subscription owner. |
| `stripe_customer_id` | `text` | Stripe customer ID. |
| `stripe_subscription_id` | `text` | Stripe subscription ID. |
| `plan` | `text` | `pro` or `vip`; free users may only live in `profiles`. |
| `status` | `text` | Stripe status such as active, trialing, canceled, or past_due. |
| `current_period_end` | `timestamptz` | Paid-through timestamp. |
| `created_at` | `timestamptz` | Creation timestamp. |

## 8. AI Voice Feedback Flow

1. User records audio in app.
2. App uploads audio to Supabase Storage.
3. Backend creates a `voice_recordings` row.
4. Backend sends audio to a transcription API.
5. Transcript is analyzed by AI.
6. AI returns feedback JSON.
7. Backend stores scores and feedback.
8. App displays the feedback result.

AI feedback should return:

- Pace score.
- Clarity score.
- Confidence score.
- Filler word count.
- Summary.
- Improvement tips.
- Next exercise.

### Voice Feedback Requirements

- Voice recording upload should be restricted to authenticated users.
- The storage path should include the user ID and recording ID to simplify access control.
- The backend should create the `voice_recordings` row before analysis so failed analysis can be retried.
- The feedback JSON should be validated before being stored in `ai_feedback`.
- Free users should be blocked from AI voice feedback after free limits are reached; Pro and VIP users should have access according to subscription status.

## 9. Example AI Feedback JSON

```json
{
  "pace_score": 75,
  "clarity_score": 72,
  "confidence_score": 68,
  "filler_words_count": 6,
  "summary": "Your speech was clear, but you used several filler words.",
  "tips": [
    "Pause instead of saying 'um'.",
    "Slow down slightly.",
    "End sentences with stronger tone."
  ],
  "next_exercise": "Record a 60-second introduction using fewer filler words."
}
```

## 10. Subscription Rules

| Plan | Price | Included Features | Gating Rules |
| --- | ---: | --- | --- |
| Free | $0/month | Assessment, communication score, 3 free lessons, limited progress view. | Lock premium lessons, AI voice feedback beyond allowed trial limits, full analytics, challenges, and certificate. |
| Pro | $7/month | Full 6-week plan, all lessons, AI voice feedback, progress analytics, challenges, certificate. | Unlock core premium features except VIP-only roleplay and advanced AI feedback. |
| VIP | $15/month | Everything in Pro, roleplay practice, advanced AI feedback, premium challenges, priority support. | Unlock all MVP paid features. |

### Stripe Requirements

- Checkout should be created server-side with the authenticated user's ID and selected plan.
- Stripe webhook handling should update `subscriptions`, `profiles.subscription_plan`, and `profiles.subscription_status`.
- Subscription status should be refreshed after checkout success and app launch.
- Canceled or past-due subscriptions should remove paid entitlements when access expires.

## 11. API / Backend Functions

Required backend functions:

| Function | Purpose | Expected Result |
| --- | --- | --- |
| `calculateAssessmentScore()` | Normalize answers, calculate category scores, total score, strongest/weakest categories, and archetype. | Persist `assessments` and `assessment_answers`; return score result. |
| `generatePersonalizedPlan()` | Select or adapt the 6-week plan based on assessment, goal, experience level, and archetype. | Return ordered lessons and recommended focus areas. |
| `uploadVoiceRecording()` | Accept audio metadata and storage path after authenticated upload. | Create or update `voice_recordings` row. |
| `transcribeAudio()` | Send stored audio to transcription service. | Persist transcript. |
| `analyzeVoiceFeedback()` | Analyze transcript/audio for pace, clarity, confidence, fillers, and coaching tips. | Persist AI feedback JSON and scores. |
| `updateProgress()` | Mark lessons/challenges complete and update progress metrics. | Persist `user_progress` or `user_challenges`. |
| `createStripeCheckout()` | Create Stripe checkout session for Pro or VIP. | Return hosted checkout URL. |
| `handleStripeWebhook()` | Process Stripe subscription events. | Update subscription records and profile entitlements. |
| `generateCertificate()` | Generate completion certificate after eligibility checks. | Return certificate record or asset URL. |

## 12. MVP Acceptance Criteria

MVP is complete when:

- User can sign up and log in.
- User can complete onboarding selections for gender, goal, and experience level.
- User can complete the assessment.
- User receives a total communication score and category breakdown.
- User receives an archetype.
- User gets a 6-week plan.
- User can view dashboard and weekly lesson plan.
- User can complete lessons.
- User can record voice practice.
- AI feedback is generated and saved.
- Progress updates correctly after lessons and challenges.
- Subscription screen works.
- Stripe checkout can be launched for paid plans.
- Stripe payment or subscription webhook updates user plan.
- Premium gating reflects Free, Pro, and VIP entitlements.
- Certificate unlocks after completion.

## 13. Build Priority

| Sprint | Scope | Primary Deliverables |
| --- | --- | --- |
| Sprint 1 | Auth, onboarding, assessment, score result. | Supabase Auth, profile creation, onboarding screens, assessment questions, score calculation, archetype result. |
| Sprint 2 | 6-week plan, lessons, dashboard, progress. | Plan generation, lessons table/content seed, dashboard, lesson completion, progress tracker. |
| Sprint 3 | Voice recording, AI feedback, storage. | Audio recording UI, Supabase Storage upload, transcription, AI feedback analysis, feedback screen. |
| Sprint 4 | Subscriptions, Stripe, premium gating. | Pricing/upgrade screens, checkout creation, webhook processing, entitlement checks. |
| Sprint 5 | Certificate, polish, testing, deployment. | Certificate unlock, QA fixes, app/web polish, deployment setup, release checklist. |
