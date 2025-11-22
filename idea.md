# Circles: GPS-Based Agent Networking

Tagline: "Your AI agent, quietly matching you with people nearby who can help you reach your goals."

---

## 1. Core Idea

- Each person has a personal AI agent that represents them in a network of nearby agents.
- The only sensor in the MVP is GPS: agents can know "who is within X meters of whom" and nothing else about the physical environment.
- People define **objectives** (things they want to achieve) and a **radius** in which it's relevant to meet others; these are called **Circles**.
- When two people come into each other's Circles, their agents talk first. Only if that conversation looks promising does anything reach the humans.

This flips social networking: instead of browsing profiles and feeds, your agent quietly scouts for people who could help you achieve specific goals in the real world.

---

## 2. Key Concepts

### Profiles
- Built during onboarding by connecting one or more social networks and/or answering a short questionnaire.
- Contain:
  - High-level interests (e.g., tennis, salsa, AI research, startups).
  - Skills and experience (e.g., "intermediate tennis", "senior backend engineer").
  - Personality & preferences (e.g., "introvert", "prefers 1:1", "open to spontaneous plans").
  - Availability pattern (e.g., "evenings & weekends", "weekdays after 6pm").

### Circles
- A Circle is: `(objective, radius, constraints)` defined by the user.
- Examples:
  - "I'd like to play tennis 1–2 times per week" with radius = 400m.
  - "I want to meet founders building AI products" with radius = 2km.
  - "I’d like to practice conversational Spanish" with radius = 800m.
- Constraints can include:
  - Time window (now, next 2 hours, next 2 days, recurring).
  - Skill level match (same level, slightly above, mentor/mentee).
  - Social preferences (1:1, small group, same gender, etc.).

### Agent-to-Agent Interviews
- When person B enters person A's Circle (and symmetry conditions are met), A’s agent **challenges** B’s agent.
- This is a structured, private conversation between agents that:
  - Clarifies each person’s objective and expectations.
  - Checks compatibility (skills, interests, availability, social preferences).
  - Assesses "vibe" based on profile, history, and previous interactions.
- No raw personal data is shared with the humans at this stage; only high-level summaries and a recommendation.

### The Judge
- A third agent (the "Judge") scores the potential interaction based on:
  - Goal alignment (are they trying to achieve similar things?).
  - Context fit (are they free within the desired time window?).
  - Social fit (preferences, introversion/extroversion, prior feedback).
  - Novelty vs. redundancy (is this a new interesting match or similar to past ones?).
- Output: a simple decision like:
  - "Not worth it."
  - "Promising, but better later."
  - "Strong match: suggest contact now/soon."

---

## 3. User Experience Flow

### Onboarding
1. Install app and connect one or more social accounts.
2. The app builds an initial profile from your public data and a short quiz.
3. You define 1–3 initial Circles:
   - Objective
   - Radius
   - Time window
   - Constraints

### Matching (Background)
1. Your phone shares approximate GPS coordinates with the backend at a low frequency.
2. When two users come within intersecting Circles, their agents:
   - Exchange high-level, anonymized representations of profiles and objectives.
   - Run a short "interview" to refine understanding.
   - Submit their notes to the Judge agent.
3. The Judge decides if the humans should be prompted.

### Push Notification
- If the Judge decides the interaction is worth it, **the challenged person** receives a clear, contextual push notification, for example:
  - "Are you interested in playing tennis today or this weekend with an intermediate, friendly player who lives within 300m?"
  - "There’s another AI founder nearby looking to meet co-founders this week. Interested in connecting?"
- The user can:
  - Accept
  - Snooze ("not now, ask later")
  - Decline ("not interested in this type of match")

### Chat & In-Person
- If accepted:
  - A private chat opens inside the app between the two users.
  - The agents can propose:
    - Time suggestions and locations.
    - Conversation starters or mini-agenda (e.g., "start with a 30-min hit on the nearby courts").
  - The app encourages in-person interaction when nearby:
    - "You’re 200m apart; want directions to meet at the public courts?"

---

## 4. MVP Technical Architecture (GPS-Only)

- **Client (mobile app / web wrapper)**:
  - Auth & onboarding.
  - Profile and Circle configuration UI.
  - Background GPS updates (coarse location, battery-friendly).
  - Push notifications and in-app chat.

- **Backend**:
  - User profiles and Circles storage.
  - Location index for "who is inside whose Circle?" queries.
  - Agent runtime (LLM or rules-based) to:
    - Represent each profile as an embedding.
    - Run agent-to-agent interviews using system + few-shot prompts.
    - Run the Judge agent to score match quality.
  - Match state machine:
    - Candidate detection → interview → judge → notify → accept/decline → chat.

- **Data/Privacy**:
  - Location stored at low precision (e.g., geohash) and short retention.
  - Agents operate on abstracted profile features, not raw social content whenever possible.
  - Users can pause all Circles or individual Circles at any time.

---

## 5. Example Circles & Flows

### Example 1 – Tennis Circle
- Alice defines: "Play tennis with intermediate players" (radius 400m, evenings).
- Bob walks into Alice’s Circle; his profile says "beginner–intermediate tennis, nearby courts, free after 6pm."
- Agents interview: confirm level, schedule, willingness to play soon.
- Judge: "Strong match for this week."
- Bob receives: "Interested in playing tennis this week with someone at your level, 5–10 min away?"
- Bob accepts → chat opens → agents propose time and nearby courts.

### Example 2 – Founder Circle
- Carla: "Meet AI founders building early-stage products" (radius 2km, next 7 days).
- Diego: "Looking for technical co-founder, AI/ML" (radius 3km).
- Agents interview and discover strong overlap in domain and stage.
- Judge triggers a push to Diego: "Nearby founder working on similar AI problems and open to meeting this week. Interested?"

---

## 6. Hackathon Scope

- Focus on **one mobile client** (simple React Native or web + PWA).
- GPS-only matching; no other sensors.
- Single-region deployment with:
  - Basic profile import (mocked social data or simple form).
  - Manual creation of 1–2 Circles per demo user.
  - Simple matching pipeline:
    - Distance filter → embedding similarity → short LLM-based "interview" → Judge decision → push → chat.
- Prepare 2–3 scripted demo stories (tennis, founders, language exchange) running on 4–6 test users to showcase end-to-end flow.

The result: a clear, focused prototype that proves the magic of agents networking on your behalf using nothing more than GPS and your goals.

