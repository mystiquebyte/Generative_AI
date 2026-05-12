# Charisma Coach MVP Technical Requirements Document

## 1. Product Overview

Charisma Coach is an AI-powered mobile app that helps users improve confidence, communication skills, voice clarity, body language, public speaking, and social charisma through a personalized 6-week coaching plan.

The MVP should allow users to:

- Create an account.
- Take a communication assessment.
- Receive a score out of 100.
- Get a personalized 6-week plan.
- Complete daily lessons.
- Record voice practice.
- Receive AI feedback.
- Track progress.
- Upgrade to a paid subscription.

## 2. Recommended Tech Stack

- **Mobile App:** React Native with Expo
- **Web Landing Page:** Next.js
- **Backend:** Supabase
- **Database:** Supabase PostgreSQL
- **Authentication:** Supabase Auth
- **Storage:** Supabase Storage
- **AI:** OpenAI API
- **Payments:** Stripe
- **Deployment:** Vercel for web, Expo/EAS for mobile

## 3. Core MVP User Flow

1. User opens app.
2. User signs up or logs in.
3. User selects gender, goal, and skill level.
4. User completes assessment quiz.
5. App calculates Communication Score.
6. App assigns user archetype.
7. App generates 6-week plan.
8. User views dashboard.
9. User completes daily lessons.
10. User records voice practice.
11. AI gives feedback.
12. User tracks progress.
13. User upgrades to Pro/VIP.

## 4. MVP Screens

### Onboarding

- Splash screen
- Welcome screen
- Benefits screen
- Select gender
- Select goal
- Select experience level

### Assessment

- Assessment intro
- Assessment questions
- Score processing screen
- Score result screen
- Score breakdown screen

### Program

- 6-week plan screen
- Week overview screen
- Daily lesson screen
- Voice recording screen
- AI feedback screen

### App Core

- Dashboard
- Progress tracker
- Challenges
- Achievements
- Certificate
- Profile/settings

### Monetization

- Pricing screen
- Upgrade screen
- Stripe checkout redirect

## 5. Assessment Scoring System

Total score: 100 points.

Categories:

| Category | Maximum Points |
| --- | ---: |
| Confidence | 25 |
| Voice Clarity | 20 |
| Body Language | 15 |
| Conversation Skills | 15 |
| Public Speaking | 15 |
| Social Anxiety | 10 |

Each quiz answer maps to a numeric value.

Example answer values:

| Answer Label | Value |
| --- | ---: |
| Very confident | 10 |
| Confident | 8 |
| Neutral | 5 |
| Not confident | 2 |
| Very uncomfortable | 0 |

The backend should calculate:

- Category scores
- Total communication score
- Weakest category
- Strongest category
- Archetype
- Recommended plan

## 6. User Archetypes

### Quiet Thinker

Low confidence, high thoughtfulness.

### Fast Talker

High energy, weak pacing/clarity.

### Social Avoider

High anxiety, avoids conversations.

### Emerging Leader

Moderate/high score, needs polish.

### Natural Charmer

High score, needs advanced mastery.

## 7. Database Tables

### `profiles`

Stores user profile data.

| Field |
| --- |
| `id` |
| `user_id` |
| `full_name` |
| `email` |
| `gender` |
| `primary_goal` |
| `experience_level` |
| `subscription_plan` |
| `subscription_status` |
| `created_at` |
| `updated_at` |

### `assessments`

Stores assessment results.

| Field |
| --- |
| `id` |
| `user_id` |
| `confidence_score` |
| `voice_score` |
| `body_language_score` |
| `conversation_score` |
| `public_speaking_score` |
| `social_anxiety_score` |
| `total_score` |
| `archetype` |
| `weakest_category` |
| `strongest_category` |
| `created_at` |

### `assessment_answers`

Stores individual answers.

| Field |
| --- |
| `id` |
| `assessment_id` |
| `user_id` |
| `question_id` |
| `question_text` |
| `answer_value` |
| `answer_label` |
| `category` |
| `created_at` |

### `lessons`

Stores lesson content.

| Field |
| --- |
| `id` |
| `week_number` |
| `day_number` |
| `title` |
| `description` |
| `lesson_type` |
| `duration_minutes` |
| `content` |
| `task` |
| `premium_required` |
| `created_at` |

### `user_progress`

Tracks completed lessons.

| Field |
| --- |
| `id` |
| `user_id` |
| `lesson_id` |
| `completed` |
| `completed_at` |
| `score_before` |
| `score_after` |
| `notes` |

### `voice_recordings`

Stores audio practice records.

| Field |
| --- |
| `id` |
| `user_id` |
| `lesson_id` |
| `audio_url` |
| `transcript` |
| `pace_score` |
| `clarity_score` |
| `confidence_score` |
| `filler_words_count` |
| `ai_feedback` |
| `created_at` |

### `challenges`

Stores user challenges.

| Field |
| --- |
| `id` |
| `title` |
| `description` |
| `category` |
| `xp_reward` |
| `premium_required` |

### `user_challenges`

Tracks challenge completion.

| Field |
| --- |
| `id` |
| `user_id` |
| `challenge_id` |
| `status` |
| `completed_at` |

### `subscriptions`

Stores Stripe subscription data.

| Field |
| --- |
| `id` |
| `user_id` |
| `stripe_customer_id` |
| `stripe_subscription_id` |
| `plan` |
| `status` |
| `current_period_end` |
| `created_at` |

## 8. AI Voice Feedback Flow

1. User records audio in app.
2. App uploads audio to Supabase Storage.
3. Backend creates `voice_recordings` row.
4. Backend sends audio to transcription API.
5. Transcript is analyzed by AI.
6. AI returns feedback JSON.
7. Backend stores scores and feedback.
8. App displays feedback result.

AI feedback should return:

- Pace score
- Clarity score
- Confidence score
- Filler word count
- Summary
- Improvement tips
- Next exercise

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

### Free Plan

- Assessment
- Communication score
- 3 free lessons
- Limited progress view

### Pro Plan — $7/month

- Full 6-week plan
- All lessons
- AI voice feedback
- Progress analytics
- Challenges
- Certificate

### VIP Plan — $15/month

- Everything in Pro
- Roleplay practice
- Advanced AI feedback
- Premium challenges
- Priority support

## 11. API / Backend Functions

Required backend functions:

- `calculateAssessmentScore()`
- `generatePersonalizedPlan()`
- `uploadVoiceRecording()`
- `transcribeAudio()`
- `analyzeVoiceFeedback()`
- `updateProgress()`
- `createStripeCheckout()`
- `handleStripeWebhook()`
- `generateCertificate()`

## 12. MVP Acceptance Criteria

MVP is complete when:

- User can sign up and log in.
- User can complete assessment.
- User receives score and archetype.
- User gets a 6-week plan.
- User can complete lessons.
- User can record voice practice.
- AI feedback is generated and saved.
- Progress updates correctly.
- Subscription screen works.
- Stripe payment updates user plan.
- Certificate unlocks after completion.

## 13. Build Priority

### Sprint 1

Auth, onboarding, assessment, score result.

### Sprint 2

6-week plan, lessons, dashboard, progress.

### Sprint 3

Voice recording, AI feedback, storage.

### Sprint 4

Subscriptions, Stripe, premium gating.

### Sprint 5

Certificate, polish, testing, deployment.
