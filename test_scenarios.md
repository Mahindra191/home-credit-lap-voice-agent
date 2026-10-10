# TEST SCENARIOS & EXECUTION MATRIX — HOME CREDIT LAP VOICE AGENT

This document details the test scenarios executed on Bolna.ai to validate the voice agent's state management, business rules, and edge-case handling.

---

## Detailed Test Cases

### Test Case 1: Happy Path (Fully Eligible Customer)
* **Customer Input Sequence:**
  1. *Verification:* "Yes, this is Mahi speaking."
  2. *Property Type:* "It is a residential flat."
  3. *Ownership:* "It's solely owned by me."
  4. *Documents:* "Yes, I have all the original property papers at home."
  5. *Loan Amount:* "I am looking for around ₹50 lakhs."
  6. *Occupation & Income:* "I am a salaried employee, and my salary is credited directly to my bank account."
  7. *Market Value:* "It is valued at approximately ₹1.2 crores."
  8. *Tenure:* "I want a repayment tenure of 5 years."
* **Expected Result:** Sequential data collection, successful validation of all 7 points, and final transition to the senior loan expert handoff[cite: 1, 3, 4].
* **Status:** ✅ **Passed**

---

### Test Case 2: Immediate Disqualification (Agricultural Property)
* **Customer Input Sequence:**
  1. *Verification:* "Speaking."
  2. *Property Type:* "It's an agricultural land plot."
* **Expected Result:** Immediate detection of agricultural property rule violation, polite termination message, and call termination without asking remaining questions[cite: 2, 3].
* **Status:** ✅ **Passed**

---

### Test Case 3: Immediate Disqualification (Cash Income)
* **Customer Input Sequence:**
  1. *Property Type:* "Commercial shop."
  2. *Ownership:* "Sole owner."
  3. *Documents:* "Originals are available."
  4. *Loan Amount:* "₹40 lakhs."
  5. *Occupation & Income:* "I run a small retail business, but customers pay me entirely in cash."
* **Expected Result:** Immediate flagging of cash income mode violation $\rightarrow$ Immediate call termination[cite: 3].
* **Status:** ✅ **Passed**

---

### Test Case 4: Loan Transfer / EMI Reduction Branch
* **Customer Input Sequence:**
  1. *Offer Response / Mid-Call:* "Actually, I already have an ongoing loan against this property and just want to know if I can reduce my current EMI."
* **Expected Result:** Bypasses fresh loan questionnaire entirely, informs the customer that a loan-transfer specialist will reach out, and terminates the call gracefully[cite: 3].
* **Status:** ✅ **Passed**

---

### Test Case 5: Out-of-Order Information Handling
* **Customer Input Sequence:**
  1. *Agent:* (Asks for property type)
  2. *Customer:* "It's a residential property, jointly owned with my wife, and worth about ₹90 lakhs."
* **Expected Result:** Agent internally updates state for `property_type = Residential`, `ownership_status = Joint`, and `market_value = ₹90L`. The agent skips asking about ownership or market value and proceeds directly to the next missing question (`documents_available`)[cite: 2, 4].
* **Status:** ✅ **Passed**

---

### Test Case 6: Loan Amount Exceeding Cap (> ₹75 Lakhs)
* **Customer Input Sequence:**
  1. *Loan Amount Question:* "I need a loan amount of ₹90 lakhs."
* **Expected Result:** Agent explains the ₹75 Lakh maximum limit and asks: *"Would you like to proceed with the maximum eligible amount of ₹75 lakhs?"* Proceeds if confirmed, terminates if declined[cite: 3].
* **Status:** ✅ **Passed**

---

### Test Case 7: Busy Customer Flow
* **Customer Input Sequence:**
  1. *Verification:* "I'm actually right in the middle of a client meeting."
* **Expected Result:** Agent acknowledges schedule constraints, asks for a preferred callback time, captures input, and ends call politely[cite: 2].
* **Status:** ✅ **Passed**
