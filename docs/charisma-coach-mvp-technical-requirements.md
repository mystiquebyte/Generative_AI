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
- Complete fun speaking games and mini challenges.
- Share achievement cards.
- Invite friends through a referral program.
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
3. User answers onboarding psychology prompts about why they are here.
4. User selects gender, goal, and skill level.
5. User completes assessment quiz.
6. App calculates Communication Score.
7. App assigns user archetype.
8. App generates 6-week plan.
9. User views dashboard.
10. User completes daily lessons.
11. User records voice practice.
12. AI gives feedback.
13. User completes speaking games and mini challenges.
14. User tracks progress.
15. User shares achievement cards or invites friends.
16. User upgrades to Pro/VIP.

## 4. MVP Screens

### Onboarding

- Splash screen
- Welcome screen
- Benefits screen
- Select gender
- Select goal
- Select experience level
- Why are you here? prompt
- Motivation examples: fear of public speaking, job interviews, dating confidence, sales, and leadership

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
- Speaking games
- Achievements
- Achievement share cards
- Referral invite screen
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
| `motivation_reason` |
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

### `speaking_games`

Stores reusable speaking game definitions for fun practice and mini challenges.

| Field |
| --- |
| `id` |
| `title` |
| `description` |
| `category` |
| `success_metric` |
| `duration_seconds` |
| `xp_reward` |
| `premium_required` |
| `created_at` |

### `user_speaking_games`

Tracks user attempts and results for speaking games.

| Field |
| --- |
| `id` |
| `user_id` |
| `speaking_game_id` |
| `voice_recording_id` |
| `score` |
| `status` |
| `completed_at` |

### `referrals`

Tracks invite links, successful referrals, and VIP day rewards.

| Field |
| --- |
| `id` |
| `referrer_user_id` |
| `referred_user_id` |
| `invite_code` |
| `reward_days` |
| `status` |
| `created_at` |
| `completed_at` |

### `achievement_share_cards`

Stores generated social sharing card metadata.

| Field |
| --- |
| `id` |
| `user_id` |
| `achievement_type` |
| `headline` |
| `image_url` |
| `share_text` |
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

### `community_posts`

Stores optional later-stage community content. This table is not required for the first MVP release unless the community feature is pulled forward.

| Field |
| --- |
| `id` |
| `user_id` |
| `post_type` |
| `content` |
| `weekly_prompt_id` |
| `created_at` |

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

## 10. Speaking Games and Fun Learning

Speaking games should make practice feel lightweight, repeatable, and rewarding. Games can be attached to lessons, daily challenges, or the free-tier mini challenge.

Examples:

| Game | Objective | Primary Metric |
| --- | --- | --- |
| Filler Word Challenge | Avoid saying “um” and other filler words. | Filler word count below target |
| Slow Talk Challenge | Maintain steady pacing. | Words per minute within target range |
| Storytelling Challenge | Tell a complete story within a timer. | Completion, structure, and timing score |
| Confidence Meter Challenge | Keep confidence above a target threshold. | Confidence score above threshold |

## 11. Growth and Business Improvements

### Referral Program

The app should include a referral growth loop. Users can invite friends and receive 7 VIP days after a successful qualifying referral. Referral rewards should update subscription entitlements or temporary VIP access without requiring a manual support process.

### Achievement Share Cards

The app should generate branded social cards for shareable milestones, such as:

> Confidence score improved 18 points.

Cards should include the Charisma Coach brand, the achievement headline, and a clear call to action for new users to take the free assessment.

### Community Feature (Later)

A community area can be added after the core MVP is stable. It should remain optional and allow users to:

- Share wins.
- Support others.
- Respond to weekly prompts.

## 12. Conversion Improvements

### Stronger Free Hook

The free tier should give users a real taste of the product before asking them to upgrade. The free experience should include:

- Free assessment.
- 1 AI voice analysis.
- 3 daily lessons.
- 1 mini challenge.
- Teaser progress dashboard.

### Onboarding Psychology

Onboarding should increase emotional investment by asking why the user is here. Example answer options include:

- Fear of public speaking.
- Job interviews.
- Dating confidence.
- Sales.
- Leadership.

The selected reason should be stored on the profile and used to personalize assessment copy, the 6-week plan, lesson examples, and upgrade messaging.

## 13. Subscription Rules

### Free Plan

- Free assessment
- Communication score
- 1 AI voice analysis
- 3 daily lessons
- 1 mini challenge
- Teaser progress dashboard

### Pro Plan — $7/month

- Full 6-week plan
- All lessons
- AI voice feedback
- Progress analytics
- Challenges
- Speaking games
- Achievement share cards
- Certificate

### VIP Plan — $15/month

- Everything in Pro
- Roleplay practice
- Advanced AI feedback
- Premium challenges
- Advanced speaking games
- Priority support

## 14. API / Backend Functions

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
- `completeSpeakingGame()`
- `createReferralInvite()`
- `applyReferralReward()`
- `generateAchievementShareCard()`

## 15. MVP Acceptance Criteria

MVP is complete when:

- User can sign up and log in.
- User can complete assessment.
- User receives score and archetype.
- User gets a 6-week plan.
- User can complete lessons.
- User can record voice practice.
- AI feedback is generated and saved.
- User can complete at least one speaking game or mini challenge.
- Progress updates correctly.
- Referral invite flow can grant 7 VIP days after a qualifying referral.
- Achievement share cards can be generated for score improvements.
- Subscription screen works.
- Stripe payment updates user plan.
- Certificate unlocks after completion.

## 16. Build Priority

### Sprint 1

Auth, onboarding psychology prompts, assessment, score result.

### Sprint 2

6-week plan, lessons, dashboard, progress, free-tier teaser dashboard.

### Sprint 3

Voice recording, AI feedback, storage, speaking games, mini challenge.

### Sprint 4

Subscriptions, Stripe, premium gating, referral rewards.

### Sprint 5

Certificate, achievement share cards, polish, testing, deployment. Optional community discovery can start after MVP validation.
