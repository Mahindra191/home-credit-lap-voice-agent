# Home Credit Loan Against Property (LAP) AI Voice Agent

Production-grade, state-aware AI Voice Agent built for SalesAgents AI. The agent conducts preliminary qualification calls for existing customers regarding a pre-approved Loan Against Property (LAP) offer.

## 🛠️ Tech Stack & Platform
- **Voice Platform:** Bolna.ai (Leveraging ElevenLabs voice synthesis and Deepgram ASR)
- **Architecture:** State-machine conversational prompt design with sequential checklist execution and dynamic out-of-order memory management.

---

## 📋 Core Features & Business Logic Implemented

1. **Sequential 7-Point Eligibility Checklist:**
   - Property Type (Residential, Commercial, Industrial vs. Agricultural disqualification)
   - Ownership Status (Sole vs. Joint)
   - Document Availability (Original documents requirement)
   - Desired Loan Amount (Up to ₹75 Lakh cap with threshold handling)
   - Occupation & Income Mode (Salaried/Self-employed with mandatory Bank income verification)
   - Property Market Value (Estimated valuation capture)
   - Tenure (Strict 3–15 year repayment window)

2. **Advanced Conversational Edge Cases:**
   - **Out-of-Order Memory:** Captures multiple variables uttered out of sequence and skips already-answered questions using a "First Missing Question" algorithm.
   - **Immediate Disqualification:** Instantly halts the flow and gracefully terminates calls for invalid answers (e.g., cash income, agricultural property, short/long tenure).
   - **Loan-Transfer & EMI Reduction Branch:** Detects existing loans or EMI reduction intent and routes the user to a specialist flow.
   - **Diversion & RAG Handling:** Safely handles interruptions, pricing questions, and general queries without fabricating information.
   - **Busy Customer Flow:** Captures convenient callback times when customers are occupied.

---

## 🚀 System Prompt Architecture

The system prompt is structured into modular blocks within Bolna:
- **Identity & Demeanor:** Persona, gender alignment, and language settings.
- **Conversation Starter:** Verification gate and busy state management.
- **Conversation Flow:** Dynamic fact extraction and state tracking.
- **Closing Rules:** Conditional termination for disqualifications, transfers, and successful handoffs.

---

## 🧪 Testing & Validation Matrix

| Test Case | Scenario / Customer Input | Expected Behavior | Status |
| :--- | :--- | :--- | :--- |
| **1. Happy Path** | Residential, Sole owner, Original docs ready, ₹50L amount, Salaried via Bank, ₹1Cr value, 5-year tenure. | Progresses sequentially through all 7 steps $\rightarrow$ Successful Senior Loan Expert Handoff. | ✅ Passed |
| **2. Disqualification (Agri)** | "My property is agricultural land." | Politely informs customer they don't meet criteria $\rightarrow$ Ends call immediately. | ✅ Passed |
| **3. Disqualification (Cash)** | "I'm self-employed and get paid entirely in cash." | Immediately flags cash income rule violation $\rightarrow$ Terminates call. | ✅ Passed |
| **4. Loan Transfer** | "I already have an active loan on this property." | Bypasses fresh loan checklist $\rightarrow$ Routes to loan-transfer specialist message $\rightarrow$ Ends call. | ✅ Passed |
| **5. Out-of-Order Input** | "It's a residential apartment worth 90 lakhs, jointly owned with my spouse." | Captures property type, market value, and ownership simultaneously. Asks the *next* missing question (Original documents). | ✅ Passed |
| **6. Loan Amount Cap** | "I want a loan of ₹90 lakhs." | Explains the ₹75L limit, asks if they want to proceed with the maximum limit of ₹75L. | ✅ Passed |
| **7. Busy Customer** | "I'm in the middle of a meeting right now." | Acknowledges, asks for a preferred callback time, and terminates politely. | ✅ Passed |

---

## 📦 Submission Deliverables
- **System Prompt:** Fully configured in Bolna Agent Studio.
- **Call Logs & Transcripts:** Accessible via Bolna Dashboard Call History.
