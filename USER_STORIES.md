# MyBank – DigiWallet User Stories

**Project:** MyBank Digital Wallet (web app, runs on `localhost:3000`)
**Scope:** Registration, Sign In, Loading money into the digital wallet, AI fraud check

> **Legend:** Criteria marked **[Screenshot]** are shown in the project screenshots.
> Criteria marked **[Business rule]** are additional rules defined for this project (not shown in the screenshots).

---

## Personas

| Persona | Description |
|---|---|
| **New User** | A person who does not yet have a MyBank account |
| **Registered User** | A person with a MyBank account who wants to use the digital wallet |

---

## Business Rules

**BR-01 – Username:** Usernames are case-insensitive (`priya`, `Priya` and `PRIYA` are the same user).

**BR-02 – Input validation (Wallet Id and Amount):**
- Wallet Id must be a whole number of 1 or more.
- Amount must be a number greater than 0.
- Zero, negative or non-numeric values are rejected with a validation message.
- There is no upper limit on either field.

**BR-03 – Fraud rule:** A transaction is **FRAUD** if Wallet Id is **500 or more** OR Amount is **100000 or more**. Otherwise it is **SAFE**.

| Wallet Id | Amount | Result |
|---|---|---|
| 1 | 1000 | SAFE |
| 499 | 99999 | SAFE |
| 1 | 100000 | FRAUD |
| 500 | 1000 | FRAUD |
| 5000 | 1000 | FRAUD |

**BR-04 – Repeated loads:** Small amounts can be added multiple times. There is no daily, cumulative or frequency limit. Each transaction is judged on its own.

---

## Epic 1: User Account Access

### US-01: Register a new account
**As a** new user,
**I want to** register with a username and password,
**so that** I can create my MyBank account and access the digital wallet.

**Acceptance Criteria**
- **[Screenshot]** **Given** I am on the MyBank screen, **when** I select the **Register** tab, **then** I see username and password fields and a **Register** button.
- **[Screenshot]** **Given** I enter a valid, unused username and a password, **when** I click **Register**, **then** my account is created and **"Registration successful!"** is displayed.
- **[Screenshot]** **Given** I am on the Register tab, **then** the password is masked and I can toggle visibility with the eye icon.
- **[Screenshot]** **Given** registration is successful, **then** I am taken to the **Sign In** tab.

**Priority:** High

---

### US-01A: See an error for an existing username during registration
**As a** new user,
**I want to** be told when the username I chose is already taken,
**so that** I can pick a different username.

**Acceptance Criteria**
- **[Business rule]** **Given** I am on the Register tab, **when** I enter a username that already exists (e.g., `priya`) and click **Register**, **then** I see a message such as **"Username already exists. Please select a new username."**
- **[Business rule]** **Given** the username `priya` exists, **when** I register as `Priya` or `PRIYA`, **then** I see the same message and no account is created (BR-01).
- **[Business rule]** **Given** the error is shown, **then** the existing account is unchanged and I stay on the Register tab with my entered values available to edit.
- **[Business rule]** **Given** I then enter a unique username and click **Register**, **then** registration succeeds and **"Registration successful!"** is displayed.

**Priority:** Medium

---

### US-02: Sign in to my account
**As a** registered user,
**I want to** sign in with my username and password,
**so that** I can securely access my digital wallet.

**Acceptance Criteria**
- **[Screenshot]** **Given** I am on the **Sign In** tab, **then** I see username and password fields and a **Sign In** button.
- **[Screenshot]** **Given** I enter correct credentials, **when** I click **Sign In**, **then** I see the alert **"Login successful!"** and I am taken to the Digital Wallet screen.
- **[Screenshot]** **Given** the password field, **then** the characters are masked and I can toggle visibility with the eye icon.
- **[Business rule]** **Given** I registered as `priya`, **when** I sign in as `Priya`, **then** login works (BR-01).

**Priority:** High

---

### US-03: See an error when the user is not found
**As a** user,
**I want to** be told when my username is not registered,
**so that** I know to correct it or register first.

**Acceptance Criteria**
- **[Screenshot]** **Given** I enter a username that is not registered (e.g., `kumar`), **when** I click **Sign In**, **then** I see the alert **"User not found"**.
- **[Screenshot]** **Given** the alert is shown, **when** I click **OK**, **then** the alert closes and I stay on the Sign In screen.
- **[Business rule]** **Given** login fails, **then** I am not taken to the Digital Wallet screen.

**Priority:** Medium

---

### US-03A: See an error for a wrong password
**As a** registered user,
**I want to** be told when my password is incorrect,
**so that** I can re-enter the correct password.

**Acceptance Criteria**
- **[Business rule]** **Given** I enter a registered username (e.g., `priya`) and a wrong password, **when** I click **Sign In**, **then** I see an error alert such as **"Invalid password"**.
- **[Business rule]** **Given** the alert is shown, **when** I click **OK**, **then** the alert closes and I stay on the Sign In screen.
- **[Business rule]** **Given** the password is wrong, **then** I am not taken to the Digital Wallet screen.

**Priority:** Medium

---

## Epic 2: Digital Wallet

### US-04: Load money into the digital wallet *(Story 1 from project brief)*
**As a** registered user,
**I want to** add money to my digital wallet by entering a Wallet Id and an amount,
**so that** I have funds available in my wallet.

**Acceptance Criteria**
- **[Screenshot]** **Given** I am logged in, **then** the **Digital Wallet** screen shows a **Wallet Id** field, an **Amount** field, an **Add Money** button, an **AI Fraud Check** button and a **Fraud Detection Result** label.
- **[Screenshot]** **Given** I enter Wallet Id `1` and Amount `1000`, **when** I click **Add Money**, **then** the money is added and I see the alert **"Money added"**.
- **[Business rule]** **Given** the Wallet Id or Amount is empty, **when** I click **Add Money**, **then** no money is added and I am prompted to fill in the missing field.
- **[Business rule]** **Given** the Wallet Id is zero, negative or non-numeric, **when** I click **Add Money**, **then** the request is rejected with a validation message (e.g., **"Please enter a valid Wallet Id"**) and no money is added.
- **[Business rule]** **Given** the Amount is zero, negative or non-numeric, **when** I click **Add Money**, **then** the request is rejected with a validation message (e.g., **"Please enter a valid amount"**) and no money is added.
- **[Business rule]** **Given** I add small amounts multiple times to a valid wallet (e.g., 1000 several times), **then** every transaction is accepted (BR-04).

**Priority:** High

---

### US-05: Check whether an amount is fraudulent using AI *(Story 2 from project brief)*
**As a** registered user,
**I want to** run an AI fraud check on a Wallet Id and amount,
**so that** I know whether the transaction is SAFE or FRAUD.

**Acceptance Criteria**
- **[Screenshot]** **Given** I have just opened the Digital Wallet screen, **then** the Fraud Detection Result area is blank until I click **AI Fraud Check**.
- **[Screenshot]** **Given** Wallet Id `1` and Amount `1000`, **when** I click **AI Fraud Check**, **then** the result shows **SAFE**.
- **[Screenshot]** **Given** Wallet Id `1` and Amount `100000`, **when** I click **AI Fraud Check**, **then** the result shows **FRAUD**.
- **[Business rule]** **Given** Wallet Id is 500 or more, or Amount is 100000 or more, **when** I click **AI Fraud Check**, **then** the result shows **FRAUD** (BR-03).
- **[Business rule]** **Given** Wallet Id is below 500 and Amount is below 100000, **when** I click **AI Fraud Check**, **then** the result shows **SAFE** (BR-03).
- **[Business rule]** **Given** the AI Fraud Check is run, **then** it only displays the result and **never adds money**, even when the result is SAFE.
- **[Business rule]** **Given** the Wallet Id or Amount is empty, zero, negative or non-numeric, **when** I click **AI Fraud Check**, **then** the check does not run and I see a validation message (BR-02).

**Priority:** High

---

### US-06: Block fraudulent transactions when adding money
**As a** registered user,
**I want** fraudulent transactions to be blocked when I click Add Money,
**so that** my wallet is protected from suspicious loads.

**Acceptance Criteria**
- **[Business rule]** **Given** I click **Add Money**, **then** the system applies the fraud rule internally, without requiring me to click **AI Fraud Check** first (BR-03).
- **[Business rule]** **Given** Wallet Id is 500 or more, or Amount is 100000 or more, **when** I click **Add Money**, **then** the money is **not** added and I see a message such as **"Transaction flagged as fraud. Money not added."**
- **[Business rule]** **Given** Wallet Id is below 500 and Amount is below 100000, **when** I click **Add Money**, **then** the money is added and **"Money added"** is shown.
- **[Business rule]** **Given** I add small amounts repeatedly, **then** each is judged on its own and none is blocked for being repeated (BR-04).

**Priority:** High

---

## Summary

| ID | Story | Priority |
|---|---|---|
| US-01 | Register a new account | High |
| US-01A | Existing username error (case-insensitive) | Medium |
| US-02 | Sign in to my account | High |
| US-03 | User not found error | Medium |
| US-03A | Wrong password error | Medium |
| US-04 | Load money into the digital wallet | High |
| US-05 | AI fraud check (SAFE / FRAUD) | High |
| US-06 | Block fraudulent transactions on Add Money | High |

## Screens Referenced
Sign In · Register · Registration successful · User not found alert · Login successful alert · Digital Wallet · Money added alert · Fraud Result: SAFE · Fraud Result: FRAUD
