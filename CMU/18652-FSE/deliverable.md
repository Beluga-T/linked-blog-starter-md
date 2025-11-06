# Step A - prototype
![[stepa.jpg]]
# Step B – Use-Case Specification : Select Payment Method

**Participating Actors**  
- Employee ( actor)  
- Bank Validation System  
- Address Validation System  

**Brief Description**  
An employee chooses how to receive their salary: pick up at HR office, receive by mail, or direct deposit into a bank account. All information entered must be validated by external systems before the change takes effect in the next pay period.

### Basic Flow (Direct Deposit)

1. Employee logs in and opens **Select Payment Method**.  
2. System displays three options: HR pickup, mail, direct deposit.  
3. Employee selects **Direct Deposit** and clicks **Continue**.  
4. System asks for Bank Name, Routing Number, Account Number.  
5. Employee enters details and submits.  
6. System validates information with the Bank Validation System.  
7. If valid, system confirms payment will arrive in 3–10 business days.  
8. System saves the method and applies it for the next pay cycle.  

### Alternative Flows

| A1  | Invalid bank data → fields highlighted red; employee must correct.            |
| --- | ----------------------------------------------------------------------------- |
| A2  | Mail option selected → system requests address and validates it.              |
| A3  | Invalid address → system shows “Address not found.”                           |
| A4  | HR pickup selected → system shows “Please go to HR office to get your money.” |
| A5  | External API timeout → system shows “Validation temporarily unavailable.”     |


### Rules
1. Only one active payment method per employee.  
2. Routing number = 9 digits; account number = 12 digits.  
3. Address must pass external validation.  
4. Changes apply next pay period.  
5. All updates logged with timestamp and user ID.  

---

# Step C – User Story & Acceptance Criteria

**User Story Motivation**  
As an employee, I want to choose how to receive my pay so that I get it in the most convenient way.

### Acceptance Criteria (Gherkin notation)

**Scenario 1 – Display Options**  
Given an employee is logged in  
When they open Select Payment Method  
Then the system shows HR pickup, mail, and direct deposit options.  

**Scenario 2 – Choose Direct Deposit**  
Given options are shown  
When employee selects Direct Deposit and clicks Continue  
Then the system requests bank details.  

**Scenario 3 – Validate Bank Info**  
Given bank details are entered  
When system calls Bank Validation API  
Then it confirms valid information and shows success message.  

**Scenario 4 – Invalid Bank Info**  
Given incorrect data  
When API returns error  
Then system marks fields red and asks for correction.  

**Scenario 5 – Mail Option**  
Given employee selects Mail  
When address is entered  
Then system validates via Address Validation API.  

**Scenario 6 – Invalid Address**  
Given validation fails  
When error is returned  
Then system prompts to fix address.  

**Scenario 7 – HR Pickup**  
Given employee chooses HR pickup  
When confirmed  
Then system shows instruction to collect check at HR.  

**Scenario 8 – API Timeout**  
Given any API is unavailable  
When timeout occurs  
Then system shows “Please try again later.”  

### Rules (reused)
Same as above (use-case rules).

---

# Step D – Consistency Check
all good, i guess

---
# Step E – INVEST Breakdown
Can split into smaller stories:  
1. Display Payment Options  
2. Validate Bank Info  
3. Validate Mail Address  
4. Confirm Payment Change  
Each is Independent, Negotiable, Valuable, Estimable, Small, Testable.

---

# Step F – Reflection
- Use cases give clearer structure and alternative flows.  
- User stories are simpler to implement and test.  
- Both complement each other for complete requirements.  
- Prototype first helps visualize validation and user flow before writing requirements.
