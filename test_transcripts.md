# Voice Agent Test Transcripts & Call Logs

These transcripts demonstrate the AI's behavior across multiple edge cases, fulfilling the requirement for robust testing.

---

### Test 1: The Happy Path (Perfect Eligible Customer)
**Agent:** Hello, am I speaking with Rahul? I am Priya calling from Home Credit.
**Customer:** Yes, this is Rahul.
**Agent:** We're reaching out because, as one of our valued customers, you may be eligible for a special Loan Against Property offer of up to 75 Lakhs. I'd just like to ask a few quick questions to check whether the offer is suitable for you. What type of property do you have?
**Customer:** It's a residential house.
**Agent:** Great. Is it solely owned by you, or is it a joint property?
**Customer:** I am the sole owner.
**Agent:** Thank you. Do you have the original property documents available for verification?
**Customer:** Yes, they are in my locker.
**Agent:** Perfect. How much loan amount are you looking for?
**Customer:** Around 40 lakhs.
**Agent:** Are you salaried or self-employed, and is your income received via bank transfer or in cash?
**Customer:** I'm salaried, and my salary comes directly to my bank account.
**Agent:** Excellent. Approximately what is the current market value of the property?
**Customer:** It's worth about 1.5 crores.
**Agent:** Understood. Finally, how many years would you like to take to repay the loan?
**Customer:** Let's say 10 years.
**Agent:** Perfect. You meet all the initial criteria for this offer! I will now arrange for our senior loan expert to call you back shortly to discuss the next steps and provide the exact interest rates. Thank you for your time today!

---

### Test 2: Out-of-Order Answers
**Agent:** Hello, am I speaking with Rahul? I am Priya calling from Home Credit.
**Customer:** Yes, speaking.
**Agent:** We're reaching out because you may be eligible for a special Loan Against Property offer of up to 75 Lakhs. I'd just like to ask a few quick questions. What type of property do you have?
**Customer:** It's a commercial shop worth about 80 lakhs, and it's jointly owned with my brother.
*(Agent updates state: Type=Commercial, Market Value=80L, Ownership=Joint)*
**Agent:** Thank you. Do you have the original property documents available for verification?
**Customer:** Yes, we do.
**Agent:** How much loan amount are you looking for?
*(Agent skipped Ownership and Market Value questions successfully)*

---

### Test 3: Agricultural Property (Immediate Disqualification)
**Agent:** What type of property do you have?
**Customer:** It's an agricultural plot in my village.
**Agent:** I understand. Unfortunately, based on our criteria, we are unable to proceed with this specific offer for agricultural properties at this time. Thank you for your time, Rahul. *(Call Ends)*

---

### Test 4: Income in Cash (Immediate Disqualification)
**Agent:** Are you salaried or self-employed, and is your income received via bank transfer or in cash?
**Customer:** I run my own shop, so it's a self-employed business. Mostly dealing in cash, so my income is in cash.
**Agent:** I understand. Unfortunately, based on our criteria, we are unable to proceed with this specific offer at this time. Thank you for your time today. *(Call Ends)*

---

### Test 5: Loan Amount > 75 Lakhs (Limit Handling)
**Agent:** How much loan amount are you looking for?
**Customer:** I need at least 90 lakhs to expand my business.
**Agent:** The current offer is available for up to 75 Lakhs. Would you like to proceed with the maximum eligible amount of 75 Lakhs?
**Customer:** Okay, yes, 75 lakhs is fine.
**Agent:** Great. Are you salaried or self-employed... *(Flow continues)*

---

### Test 6: Existing Loan (Transfer Logic)
**Agent:** We're reaching out because you may be eligible for a special Loan Against Property offer of up to 75 Lakhs...
**Customer:** Oh, I already have a loan against this property with another bank. Can you reduce my EMI?
**Agent:** Thank you for clarifying. Since you already have an existing loan on the property, this would need to be handled by our loan-transfer specialist. A specialist will contact you shortly regarding this. Thank you for your time. *(Call Ends)*

---

### Test 7: Busy Customer Flow
**Agent:** Hello, am I speaking with Rahul? I am Priya calling from Home Credit.
**Customer:** Yes, but I'm in the middle of a meeting right now.
**Agent:** Certainly, I understand. What would be a convenient time for us to call you back?
**Customer:** Call me around 6 PM today.
**Agent:** I have noted that down. We will call you back at 6 PM. Thank you and have a good day. *(Call Ends)*

---

### Test 8: Diversion / Hallucination Check
**Agent:** How much loan amount are you looking for?
**Customer:** I need 50 lakhs, but before we go ahead, what is the exact interest rate on this?
**Agent:** The exact interest rate will be provided by our senior loan expert after this preliminary qualification. For now, I'd like to complete these basic eligibility questions. Are you salaried or self-employed...
