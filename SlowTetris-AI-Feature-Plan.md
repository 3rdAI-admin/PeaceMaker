# Slow Tetris Therapeutic App — AI Feature Plan

**Project:** Th3rdAI Slow Tetris
**Author:** James (PM) via Cowork
**Date:** March 12, 2026
**Status:** 📋 Planning

---

## 1. Executive Summary

This plan defines how artificial intelligence integrates into a therapeutic slow-Tetris web application designed for patients with PTSD, anxiety, depression, and substance cravings. The app is grounded in peer-reviewed clinical research — most notably the February 2026 *Lancet Psychiatry* RCT showing 90% reduction in intrusive trauma memories through slow Tetris with mental rotation.

The AI layer transforms a simple block game into a **personalized therapeutic companion** that adapts to each patient's needs, coaches them through clinically validated techniques, provides safe pre-session check-ins, and generates meaningful progress insights.

**Platform:** Web-first (browser), mobile later
**User:** Self-guided patients (no therapist required for v1)
**AI approach:** Local-first via Ollama (privacy-preserving), with optional cloud fallback

---

## 2. Why AI Matters for This App

The clinical research reveals a gap: the therapeutic Tetris protocol works, but it requires guided instruction — trained clinicians walked patients through memory recall, mental rotation technique, and session pacing. No existing app bridges this gap.

AI fills the role of that guide. It makes the clinical protocol accessible to anyone without requiring a therapist in the loop, while respecting the safety boundaries the research demands.

Without AI, this is just another Tetris clone. With AI, it becomes the first self-guided implementation of the ICTI (Imagery Cognitive Task Interference) protocol.

---

## 3. AI Feature Architecture Overview

```
┌─────────────────────────────────────────────────┐
│                PATIENT EXPERIENCE                │
├─────────────┬──────────┬──────────┬──────────────┤
│ Pre-Session │ Gameplay │  Post-   │  Between     │
│ Check-In    │          │ Session  │  Sessions    │
│             │          │ Journal  │              │
├─────────────┼──────────┼──────────┼──────────────┤
│ AI          │ Adaptive │ Smart    │ Progress     │
│ Companion   │ Diff.    │ Journal  │ Insights &   │
│ + Memory    │ Engine + │ Prompts  │ Session      │
│ Recall      │ Rotation │          │ Reminders    │
│ Guide       │ Coach    │          │              │
├─────────────┴──────────┴──────────┴──────────────┤
│              LOCAL AI ENGINE (Ollama)             │
│         Patient profile + session history         │
├──────────────────────────────────────────────────┤
│           LOCAL DATA STORE (encrypted)           │
│    Mood logs, flashback diary, game metrics      │
└──────────────────────────────────────────────────┘
```

All four AI features operate across a single session lifecycle:

1. **Pre-Session Check-In** → Before the game starts
2. **Adaptive Difficulty + Mental Rotation Coach** → During gameplay
3. **Smart Journaling** → After the game ends
4. **Progress Insights** → Between sessions (dashboard + nudges)

---

## 4. Feature 1: Adaptive Difficulty Engine

### What It Does
The AI continuously adjusts block fall speed, piece complexity, and board state to keep the patient in a **flow state** — the psychological zone where therapeutic benefit is maximized (Sweeny et al., 2018).

### Why It Matters
The research is explicit: too easy = boredom (no benefit), too hard = frustration (no benefit, potentially harmful for anxious patients). A fixed difficulty slider can't adapt to a patient's changing state within a single session — some days they're sharper, some days they're struggling.

### How It Works

**Inputs the AI monitors (real-time):**
- Block placement speed (time from appearance to placement)
- Rotation count per block (more rotations = more engaged OR more confused)
- Placement accuracy (gaps created vs. lines cleared)
- Pause frequency (patient stepping away or hesitating)
- Session duration vs. typical session length

**Outputs the AI adjusts:**
- Fall speed (primary lever — slower for struggling, faster for bored)
- Piece preview count (show more upcoming pieces when player is overwhelmed)
- Board width (optional: narrower board = simpler spatial reasoning)
- Mental rotation prompt frequency (more coaching when accuracy drops)

**AI Model Approach:**
- A lightweight local model (or even a rule-based system initially) using a sliding window of the last 30 seconds of gameplay metrics
- No need for a large language model here — this is a **control system** with simple heuristics that can be refined with data over time
- Phase 1 can use basic rules (if placement_time > 8s, reduce speed by 10%)
- Phase 2 can introduce a trained model once player data exists

### Safety Guardrails
- Speed never exceeds a maximum that could feel stressful
- Minimum speed floor so the game remains engaging
- If the patient pauses for >60 seconds, gently prompt rather than auto-adjusting
- No sudden difficulty spikes

---

## 5. Feature 2: Mental Rotation Coach

### What It Does
An AI guide that teaches and reinforces the **mental rotation technique** — the clinically validated mechanism that makes therapeutic Tetris work. The coach adapts to each patient's skill level and fades as they master the technique.

### Why It Matters
The 2026 Lancet study specifically found that **slow Tetris with intentional mental rotation** outperformed regular Tetris. The mental rotation step — visualizing the block in different orientations before placing it — is what engages the frontal cortex and premotor cortex, competing with trauma imagery. Without this coaching, patients would just play Tetris normally and miss the therapeutic mechanism.

### How It Works

**Onboarding Phase (Sessions 1–3):**
- Full guided tutorial: "See this L-shaped block? Before you place it, close your eyes for a moment and picture it rotated 90 degrees. What does it look like now?"
- Visual overlay showing rotation options with gentle animation
- AI narrates each step in a calm, encouraging tone
- Positive reinforcement: "Great — you rotated that one in your mind before placing it. That's exactly the technique."

**Learning Phase (Sessions 4–10):**
- Prompts become shorter and less frequent
- AI notices when the player is naturally rotating before placing and acknowledges it
- Occasional reminders if placement speed suggests the player is rushing (reflexive play = less therapeutic)
- "Take your time with this one. Picture how it fits before you drop it."

**Mastery Phase (Sessions 10+):**
- Prompts fade to occasional gentle nudges
- AI intervenes only if gameplay pattern shifts back to rapid/reflexive play
- Progress acknowledgment: "You've been consistently using mental rotation. Your technique is strong."

**AI Model Approach:**
- Language model (Ollama) generates contextual coaching messages
- System prompt includes clinical protocol guidance and tone requirements
- Player profile tracks which prompts have been shown and their skill progression
- Rotation detection: measure if player rotates a piece AND pauses before placement vs. just dropping immediately

### Safety Guardrails
- Coaching tone is always gentle, never corrective or critical
- Player can disable coaching at any time
- No references to specific trauma content in coaching messages
- If a player repeatedly ignores prompts, AI backs off rather than escalating

---

## 6. Feature 3: Pre-Session AI Check-In

### What It Does
Before gameplay begins, a conversational AI companion offers a brief, gentle check-in that optionally guides the patient through the **memory recall step** from the clinical ICTI protocol.

### Why It Matters
The clinical studies found that briefly recalling an intrusive memory before playing Tetris opens the **reconsolidation window** — making the memory malleable so Tetris can interfere with its sensory components. Without this step, Tetris is still helpful for anxiety and flow, but the specific flashback-reduction effect is diminished.

However, this is the **most sensitive feature** in the entire app. Asking someone to recall trauma must be done with extreme care.

### How It Works

**Standard Check-In Flow:**
```
AI: "Welcome back. How are you feeling right now?"
   [😊 Good]  [😐 Okay]  [😔 Struggling]  [😰 Distressed]

AI (if "Okay"): "Thanks for sharing. Would you like to do a
   brief grounding exercise before we play, or jump straight in?"
   [Grounding first]  [Start playing]

AI (if "Struggling"): "I hear you. Some days are harder. Would
   you like to talk about what's on your mind for a moment,
   or would you prefer to go straight to playing?"
   [Talk briefly]  [Just play]

AI (if "Distressed"): "I'm glad you're here. If you're in
   crisis, please reach out to the 988 Suicide & Crisis Lifeline
   (call or text 988). Would you like to try a calming breathing
   exercise, or would you prefer to start a gentle game?"
   [Breathing exercise]  [Gentle game]  [Show resources]
```

**Optional Memory Recall Step (unlocked after 3+ sessions):**
```
AI: "Some people find it helpful to briefly bring to mind
   an image that's been bothering them before playing. This
   is completely optional. Would you like to try this today?"
   [Yes, guide me]  [Not today]  [What is this?]

AI (if "Yes"): "Okay. Take a moment to bring that image
   to mind — just for a few seconds. You don't need to
   describe it or dwell on it. Just let it appear briefly...
   Now let's play. The game will help your mind shift away
   from that image."
```

**AI Model Approach:**
- Ollama language model with a carefully crafted system prompt
- System prompt includes: clinical safety guidelines, tone requirements (warm, brief, never probing), crisis detection keywords, and escalation paths
- Conversation history is session-scoped (not retained long-term) for privacy
- Mood selection stored locally for trend tracking

### Safety Guardrails (Critical)
- **Memory recall is always optional** — never prompted by default, never pressured
- Memory recall step is **hidden for the first 3 sessions** until the patient is comfortable with the app
- AI never asks what the memory is about — no trauma details
- AI never attempts to process or discuss trauma — it redirects to professional resources
- Crisis keywords (self-harm, suicide, hopelessness) immediately surface helpline resources
- All check-in data stored locally, encrypted, never transmitted
- Disclaimer shown on first use: "This app is a wellness tool, not a replacement for professional mental health care."

---

## 7. Feature 4: Smart Journaling & Progress Insights

### What It Does
After each Tetris session, the AI generates personalized journal prompts and, over time, surfaces meaningful progress insights drawn from mood logs, flashback diary entries, and gameplay patterns.

### Why It Matters
The clinical studies tracked flashback frequency weekly and showed clear improvement trends. Patients who tracked their symptoms saw their progress objectively, which itself is therapeutic (builds self-efficacy and hope). AI makes this tracking effortless and the insights personal.

### How It Works

**Post-Session Journal Prompts:**
After a session ends, the AI offers a brief journaling moment:

```
AI: "Nice session — 18 minutes of focused play.
   How do you feel compared to before you started?"
   [Better]  [About the same]  [Worse]  [Skip]

AI (if "Better"): "That's great to hear. If you'd like,
   jot down a quick thought about what helped today."
   [Text input]  [Skip]

AI: "One more thing — how many intrusive memories or
   flashbacks have you noticed since your last session?"
   [0]  [1-2]  [3-5]  [More than 5]  [Skip]
```

**Progress Insights (weekly/monthly):**
After sufficient data (2+ weeks), the AI generates natural-language insights:

- "Over the past two weeks, your flashback count dropped from about 8 per week to 3. That's meaningful progress."
- "You tend to report feeling better after sessions longer than 15 minutes. The research suggests 20-minute sessions are optimal — try extending a bit next time?"
- "You've been consistent — 5 sessions this week. Consistency is one of the strongest predictors of improvement in the studies."
- "Your mental rotation speed has improved by 40% since you started. Your brain is building new pathways."

**AI Model Approach:**
- Ollama generates personalized prompts and insights based on structured patient data
- Input: mood logs (pre/post), flashback counts, session durations, gameplay metrics, journal entries
- System prompt instructs the AI to be encouraging, factual, and never diagnostic
- Trend detection uses simple statistical analysis (rolling averages, week-over-week comparison)
- Journal entries processed locally — AI can reference themes but never stores raw text long-term

### Safety Guardrails
- AI never diagnoses or claims medical outcomes ("you're getting better" → "your reported flashbacks have decreased")
- Negative trends handled gently: "Your flashback count went up this week. That can happen — it doesn't mean you're going backward. If it continues, consider reaching out to a professional."
- Journaling is always optional and skippable
- All data stored locally, encrypted

---

## 8. Technical Architecture

### AI Stack

| Component | Technology | Rationale |
|-----------|-----------|-----------|
| Language Model | Ollama (local) | Privacy-first — mental health data never leaves the device |
| Recommended Models | Llama 3.1 8B, Mistral 7B, or Phi-3 | Small enough for consumer hardware, capable enough for coaching |
| Adaptive Engine | Rule-based → ML | Start with heuristics, graduate to trained model with data |
| Data Store | SQLite + encryption | Local, fast, no server dependency |
| Frontend | React (web) | Consistent with Code Companion, web-first approach |
| Game Engine | HTML5 Canvas or Phaser.js | Lightweight, runs in browser, good mobile support |
| Backend | Node.js/Express (optional) | Only needed if adding cloud sync or therapist dashboard later |

### Privacy Architecture

```
┌──────────────────────────────────────────────┐
│              PATIENT'S DEVICE                │
│                                              │
│  ┌─────────┐  ┌──────────┐  ┌────────────┐  │
│  │ Game UI │──│ AI Coach │──│ Ollama LLM │  │
│  └────┬────┘  └────┬─────┘  └────────────┘  │
│       │            │                         │
│  ┌────┴────────────┴─────┐                   │
│  │   Local Encrypted DB  │                   │
│  │  - Mood logs          │                   │
│  │  - Flashback diary    │                   │
│  │  - Game metrics       │                   │
│  │  - Journal entries    │                   │
│  └───────────────────────┘                   │
│                                              │
│  ★ Nothing leaves the device by default ★    │
└──────────────────────────────────────────────┘
```

**Why local-first matters:** Mental health data is among the most sensitive personal information. Patients with PTSD may be especially wary of data collection. By running AI locally via Ollama and storing all data on-device, we eliminate trust barriers and comply with health data regulations by default.

### Data Model (Local)

```
Patient Profile
├── preferences (speed, coaching level, theme)
├── onboarding_completed (boolean)
├── sessions_count (integer)
└── memory_recall_unlocked (boolean, after 3 sessions)

Session Record
├── timestamp
├── duration_seconds
├── pre_mood (1-4 scale)
├── post_mood (1-4 scale)
├── flashback_count_since_last
├── blocks_placed
├── avg_placement_time_ms
├── rotations_per_block
├── lines_cleared
├── difficulty_curve (array of speed values over time)
├── coaching_prompts_shown (count)
├── memory_recall_used (boolean)
└── journal_entry (encrypted text, optional)

Progress Metrics (computed)
├── weekly_flashback_trend
├── avg_session_duration_trend
├── mood_improvement_trend
├── rotation_skill_progression
└── consistency_score (sessions per week)
```

---

## 9. Implementation Phases

### Phase 1: Core Game + Adaptive Difficulty (Weeks 1–4)
**Goal:** A playable therapeutic Tetris with AI-driven difficulty adjustment

- [ ] Tetris game engine (slow default speed, calming aesthetics)
- [ ] Adaptive difficulty system (rule-based heuristics v1)
- [ ] Basic session tracking (duration, blocks, speed curve)
- [ ] Zen mode (no game-over state)
- [ ] Calming visual theme (soft colors, gentle animations)
- [ ] 20-minute default session timer with gentle prompts

**AI involvement:** Adaptive engine only (no LLM needed yet)

### Phase 2: Mental Rotation Coach (Weeks 5–7)
**Goal:** Guided mental rotation technique integrated into gameplay

- [ ] Onboarding tutorial with visual rotation demonstrations
- [ ] AI coaching prompts during gameplay (Ollama integration)
- [ ] Coaching fade system (tracks skill progression)
- [ ] Rotation detection (does player pause + rotate before placing?)
- [ ] Player profile to track coaching phase

**AI involvement:** Ollama LLM for contextual coaching messages

### Phase 3: Pre-Session Check-In (Weeks 8–10)
**Goal:** Safe, gentle AI companion for mood check-in and optional memory recall

- [ ] Mood selection UI (emoji-based, low friction)
- [ ] Conversational AI check-in flow (Ollama)
- [ ] Crisis keyword detection and resource surfacing
- [ ] Optional memory recall step (gated behind 3+ sessions)
- [ ] Breathing exercise / grounding module
- [ ] Clinical disclaimer and onboarding consent flow
- [ ] Safety review with mental health professional (external)

**AI involvement:** Ollama LLM for conversational check-in

### Phase 4: Smart Journaling & Insights (Weeks 11–14)
**Goal:** Post-session journaling with AI-generated prompts and progress tracking

- [ ] Post-session mood check and flashback count entry
- [ ] AI-generated journal prompts (personalized to session data)
- [ ] Flashback diary with weekly trend visualization
- [ ] Progress insights dashboard
- [ ] AI-generated weekly summary messages
- [ ] Data export for patient records (optional)

**AI involvement:** Ollama LLM for prompt/insight generation + simple stats

### Phase 5: Polish & Safety (Weeks 15–18)
**Goal:** Production-ready with accessibility, safety review, and final polish

- [ ] Accessibility audit (one-handed play, colorblind mode, screen reader)
- [ ] Professional safety review of AI check-in scripts
- [ ] Penetration testing on local data encryption
- [ ] Onboarding flow refinement
- [ ] Performance optimization (smooth 60fps gameplay)
- [ ] Mobile-responsive layout (preparing for native mobile later)
- [ ] User testing with target populations

---

## 10. Risks and Mitigations

| Risk | Severity | Mitigation |
|------|----------|------------|
| AI gives harmful advice during check-in | **Critical** | Strict system prompt constraints, crisis detection, regular review of AI outputs, professional safety audit in Phase 5 |
| Memory recall step causes distress | **High** | Gated behind 3+ sessions, always optional, immediate exit to gameplay, crisis resources surfaced proactively |
| Patient over-relies on app instead of therapy | **High** | Clear disclaimers, periodic prompts to consider professional support, never claims to be treatment |
| Ollama model quality inconsistent | **Medium** | Test multiple models, provide fallback scripted responses if model output fails quality check |
| Patient data breach (device theft/compromise) | **Medium** | AES-256 encryption at rest, no cloud sync by default, biometric/PIN lock option |
| Adaptive difficulty doesn't feel right | **Medium** | Start with conservative heuristics, extensive playtesting, patient feedback loop |
| Regulatory/medical device classification | **Medium** | Position as wellness tool, not medical device. Avoid diagnostic claims. Legal review recommended. |
| Ollama too resource-heavy for some devices | **Low** | Offer "lite mode" with scripted responses instead of LLM, or optional cloud API fallback |

---

## 11. Competitive Differentiation

| Feature | Tetris Effect | Standard Tetris | **Th3rdAI Slow Tetris** |
|---------|--------------|----------------|------------------------|
| Meditative aesthetics | ✅ | ❌ | ✅ |
| Clinical protocol (ICTI) | ❌ | ❌ | ✅ |
| Mental rotation coaching | ❌ | ❌ | ✅ |
| Adaptive AI difficulty | ❌ | ❌ | ✅ |
| Memory recall guidance | ❌ | ❌ | ✅ |
| Flashback tracking | ❌ | ❌ | ✅ |
| AI companion | ❌ | ❌ | ✅ |
| Privacy-first (local AI) | N/A | N/A | ✅ |
| Free/accessible | ❌ ($40) | ✅ | ✅ (planned) |

**No existing app implements the clinical ICTI protocol.** This is the core differentiator.

---

## 12. Success Metrics

**Patient outcomes (self-reported):**
- Flashback frequency reduction over 4 weeks (target: 50%+ reduction, per clinical benchmarks)
- Pre/post session mood improvement rate
- Patient-reported usefulness score

**Engagement:**
- Average sessions per week (target: 4+, matching clinical study frequency)
- Average session duration (target: 15–20 minutes)
- Retention at 30 days
- Mental rotation coaching completion rate

**AI quality:**
- Coaching prompt relevance rating (patient feedback)
- Adaptive difficulty satisfaction (flow state self-report)
- Check-in completion rate (patients who engage vs. skip)
- Zero crisis misses (crisis keywords always surface resources)

---

## 13. Open Questions

1. **Regulatory positioning:** Should this be a wellness app or pursue FDA digital therapeutics classification? Wellness is faster to market; DTx opens insurance reimbursement but adds 12–24 months of regulatory process.

2. **Therapist dashboard (v2?):** The research supports hybrid models. Should a future version let therapists assign the app and monitor patient progress?

3. **Multiplayer/social:** Some patients might benefit from knowing others are on the same journey. Is there a safe way to add community features without compromising privacy?

4. **Insurance/payer integration:** If clinical outcomes are strong, could this become a covered digital therapeutic?

5. **Research partnership:** Would partnering with Emily Holmes' research group (the lead researcher behind the Lancet study) strengthen credibility and ensure clinical accuracy?

---

## 14. Key References

1. Holmes et al. (2026). *Lancet Psychiatry* — ICTI RCT, 90% flashback reduction
2. Holmes et al. (2024). *BMC Medicine* — Single guided session, 164 healthcare professionals
3. Butler, Kühn et al. (2020). *J Psychiatry Neurosci* — Hippocampal volume increase
4. Iyadurai, Holmes et al. (2017). *Molecular Psychiatry* — ER intervention within 6 hours
5. Sweeny et al. (2018). *Emotion* — Flow states and anxiety reduction
6. Skorka-Brown, Andrade, May (2015). *Addictive Behaviors* — 3-minute craving reduction
7. Multi-lab replication (2025). *Collabra: Psychology* — N=433 intrusion study

---

*Prepared by Th3rdAI — March 2026*
