Use this structure to deliver clear, executive-ready sentiment insights, technician coaching cues, and AI improvement opportunities.

---

## 1. 🩵 Overall Sentiment 
**Fetch the sentiment directly from the ticket.** 
*(Do not calculate — use the system-provided sentiment label.)*

---

## 2. 🔍 Key Indicators 
Break down the cues influencing the sentiment:

**Tone Cues:** 
*(e.g., polite, frustrated, enthusiastic, passive-aggressive)* 

**Language / Keywords:** 
*(e.g., “thank you,” “this is ridiculous,” “still not fixed,” “appreciate your help”)* 

**Phrasing Patterns:** 
*(e.g., repeated questions, abrupt sentences, CAPS use, excessive exclamation marks)* 

**Pacing & Response Gaps:** 
*(e.g., long silence between replies, sudden drop-off, rapid-fire responses)* 

---

## 3. 📈 Sentiment Trajectory 
**Did the client’s emotional tone improve, decline, or fluctuate?**

- ➕ **Improved** – Situation de-escalated; tone warmed. 
- ➖ **Worsened** – Client grew more frustrated or confused. 
- ➖➕ **Mixed** – Tone fluctuated or ended unresolved. 
- 🟰 **Flat** – Consistent tone throughout. 

**One-line summary:** 
*(Explain the emotional shift, if any, and what may have triggered it.)*

---

## 4. 🎯 Suggested Follow-Up Action 
Tailor response based on the sentiment outcome:

**If Negative:** 
- Send a personalized acknowledgment from a human agent. 
- Offer a quick call or direct touchpoint to rebuild rapport. 
- Flag to Customer Success or Account Manager if high-value or SLA-related.

**If Neutral:** 
- Reinforce closure with a short recap and friendly tone. 
- Optionally send a CSAT survey to gauge final satisfaction.

**If Positive:** 
- Highlight **BIG WINS** — note what went exceptionally well (speed, empathy, clarity, teamwork). 
- Tag and **recognize the technician(s) and team members** who delivered exceptional service. 
- Share kudos internally or post to company recognition channels. 

---

## 5. 🛠️ Opportunity Flag (Process Improvement) 
Identify structural or workflow issues impacting experience.

**⛳ Potential Process Issue:** 
*(e.g., confusion during escalation, unclear triage flow, excessive handoffs)* 

**⏳ Workflow Delay or Inefficiency:** 
*(e.g., multiple back-and-forths before resolution, missed automation or self-service options)* 

*(Provide a one-sentence note explaining what improvement could reduce friction.)*

---

## 6. 🧠 Coaching Tip

The AI should dynamically generate a **context-aware coaching note** that draws directly from the tone, pacing, and conversational behavior within the ticket. 

The goal is to produce **constructive, specific, and actionable feedback** that a technician or lead could immediately use to improve communication quality and customer experience.

### 🎯 AI Guidance:
- Reference **real phrasing** from the conversation to ground feedback (e.g., “When the client said ‘I’ve been waiting all morning,’...”).
- Focus on **communication habits** — tone, empathy, clarity, timing, acknowledgment.
- Use **positive reinforcement** when performance is strong, but include **growth opportunities** when applicable.
- Always keep feedback **supportive, not punitive** — aim for coaching, not criticism.

### 🧩 Expected Output Example:

**Coaching Tip:** 
> “When the client expressed frustration about waiting, you maintained professionalism and responded promptly. However, acknowledging their concern before providing updates (e.g., *‘I completely understand the delay has been frustrating’*) would have made the response feel more empathetic. Great job staying calm under pressure — this small shift will further elevate the client experience.”

---

### 🧠 Coaching Output Rules:
- Write in a **human coaching voice** (no robotic phrasing or bullet lists). 
- **Cite the behavior, explain the impact, and suggest the improvement** in 1–3 sentences. 
- If sentiment is **Positive or Exceptional**, focus on **replicating excellence** (e.g., communication style, speed, empathy). 
- If sentiment is **Negative or Critical**, focus on **tone repair and trust recovery** opportunities. 

---

### ✅ Output Example

**Overall Sentiment:** Negative 
**Key Indicators:** Frustrated tone, abrupt replies, “still not fixed,” repeated follow-ups after long gaps. 
**Sentiment Trajectory:** ➖ Worsened – tone shifted from polite to impatient after perceived delay. 
**Suggested Follow-Up Action:** Send acknowledgment and offer live call; flag to account manager. 
**Process Opportunity:** Review SLA monitoring alerts — client waited too long before update. 
**Coaching Tip:** Technician should set expectations early and provide interim updates even without progress. 
**Intent Opportunity:** Phrase “still spinning” not recognized — add rule to map to “System Performance Issue.”
 