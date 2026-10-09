# Voice Agent Design Approach: Home Credit LAP

## 1. Core Architecture
The solution was designed using a **5-layer architecture** to ensure robust conversational handling and strict business rule adherence:
1. **Conversation Layer:** Handles greetings, natural language parsing, and conversational turns, ensuring the bot never sounds like a rigid questionnaire.
2. **Business Logic Layer:** Implements strict sequential questioning while managing out-of-order data.
3. **State Memory Layer:** An internal `QUALIFICATION_STATE` tracker (e.g., `property_type`, `income_mode`) to prevent repeating questions and remember context provided earlier in the call.
4. **Diversion/Routing Layer:** Actively listens for "Existing Loan" or "EMI reduction" intents and branches off to the transfer flow immediately.
5. **Decision Engine:** Immediately evaluates answers against strict eligibility rules, firing either the Disqualification Flow, Handoff Flow, or continuing the Checklist.

## 2. The "First Missing Question" Algorithm
To handle the requirement of dynamic, out-of-order conversations, the prompt instructs the LLM to follow this processing loop on every customer utterance:
- **Extract:** Pull any valid data points from the user's speech.
- **Validate:** Check against hard rules (e.g., Cash = Reject).
- **Update:** Save the valid data to the internal state.
- **Ask:** Scan the checklist (1 to 7) and ask the *first* question whose state is still `UNKNOWN`.

This ensures the bot behaves naturally. If a user says, *"I need 50 lakhs for my residential home,"* the bot updates `loan_amount` and `property_type` simultaneously, completely skipping those questions later in the call.

## 3. Strict Boundary Enforcement
- **Immediate Disqualification:** Rules like "Agricultural Property" or "Cash Income" trigger an immediate, polite call termination. The bot is explicitly instructed not to ask further irrelevant questions once a disqualifying condition is met.
- **Limit Handling:** The edge case of a customer wanting >75L is handled gracefully by offering the maximum cap of 75L, rather than an outright rejection.
- **Handoff Gate:** The prompt enforces a strict rule: *Never qualify the lead until all 7 variables are explicitly filled and validated.*
- **Hallucination Prevention:** The prompt relies on `additional_context_from_rag` for product questions. If asked about interest rates, it defers to the Senior Loan Expert rather than inventing a number.
