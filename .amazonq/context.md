## User Stories

### US-01: Social Sign-In

**As a** new user  
**I want to** sign up or log in with Google or Apple  
**So that** I can start booking without creating a password

**Acceptance Criteria:**
- Sign-in with Google/Apple button is visible on the welcome screen
- Upon successful OAuth, the user lands on the Home screen already authenticated
- If OAuth fails, an error message explains the issue

### US-02: Add Payment Method

**As a** user  
**I want to** add a payment method (card / Apple Pay / Google Pay)  
**So that** the app can auto-charge me after delivery

**Acceptance Criteria:**
- User can add, view, and delete at least one payment method
- Invalid cards are rejected with explanatory message
- Payment method is stored and selectable during booking

### US-03: Create Booking with Fare Estimate

**As a** user  
**I want to** input parcel & route details and see the fare before I confirm  
**So that** I know the cost upfront

**Acceptance Criteria:**
- Mandatory fields: pickup, destination, parcel size/weight
- System displays fare ≤ 3 seconds after fields completed
- "Book" button is disabled until all required fields are valid

### US-04: Cancel Booking (with reason)

**As a** customer  
**I want to** cancel a booking before pickup and provide an explanation  
**So that** I'm not charged and drivers are informed

**Acceptance Criteria:**
- Cancel booking action is available only while the job is still waiting for pickup
- Tapping Cancel shows a short list of reasons plus an optional text box for extra details
- A confirmation pop-up asks the customer to confirm the cancellation
- After confirmation:
  - The job is marked Cancelled and the customer's card is not charged
  - The assigned driver and the support team receive a notification that the job was cancelled
  - The booking now appears in the customer's history with the label Cancelled and the chosen reason visible

### US-05: Authorize Payment on Booking

**As the** system  
**I want to** place a hold on the user's payment method when a booking is created  
**So that** funds are guaranteed

**Acceptance Criteria:**
- A payment authorization occurs immediately after booking
- User sees success message; on failure, booking is cancelled with explanation

### US-06: Operator Invites Driver

**As an** operator  
**I want to** invite a driver  
**So that** only vetted drivers enter the platform

**Acceptance Criteria:**
- Operator can send an invite via email/phone
- Invite link expires after configurable time
- Driver status = "Pending" until signup complete

### US-07: Driver Completes Profile

**As a** driver  
**I want to** submit my personal, vehicle, and payout details  
**So that** I can start receiving jobs

**Acceptance Criteria:**
- All mandatory fields validated (name, ID photo placeholder, vehicle plate, bank info)
- On save, profile status becomes "Active"
- Incomplete profile blocks job acceptance

### US-08: Receive & Accept Job (First-Acceptor-Wins)

**As a** driver  
**I want to** receive nearby job offers with payout info and accept the one I want  
**So that** I can earn money

**Acceptance Criteria:**
- Push notification arrives within 2s of dispatch
- Job card shows pickup, drop-off, payout
- First driver to tap "Accept" is assigned; others see "Job taken"

### US-09: Dispatch Fan-Out (System)

**As the** system  
**I want to** broadcast a new job to drivers in expanding radius with light rating bias  
**So that** it's claimed quickly and fairly

**Acceptance Criteria:**
- Nearest eligible drivers receive the job first
- Radius expands every n seconds until timeout
- Job is withdrawn after timeout if unclaimed

### US-10: Live Map & ETA

**As a** user  
**I want to** see the driver's live location and ETA  
**So that** I know when to expect pickup and delivery

**Acceptance Criteria:**
- Map updates every ≤ 5 s
- ETA recalculates dynamically if driver route changes
- User can share a read-only tracking link

### US-12: Capture Payment & Send Receipt

**As the** system  
**I want to** capture the held payment when the driver completes delivery and send a receipt  
**So that** the user is charged correctly

**Acceptance Criteria:**
- Capture succeeds or booking marked "Payment failed" with retry option
- Email & in-app receipt sent within 1 min of capture

### US-13: Daily Driver Payout

**As the** system  
**I want to** aggregate each driver's earnings daily and initiate payout  
**So that** drivers are paid T+1

**Acceptance Criteria:**
- Cron runs once per 24 hours
- Driver sees pending and paid amounts in Earnings screen

### US-14: Rating Flow

**As a** user  
**I want to** rate the driver after delivery  
**So that** service quality stays high

**Acceptance Criteria:**
- 1-5 star modal appears after job completion
- Rating is optional but can't be edited after submission

### US-19: Configure Commission & Tax Rates

**As an** operator  
**I want to** set commission percentages, applicable taxes, and base fare parameters  
**So that** the system can calculate user fares and driver payouts accurately

**Acceptance Criteria:**
- Operator can edit commission %, GST/VAT %, base fare, per-km, and per-minute rates
- Changes apply only to new bookings created after the update
- Validation ensures numeric ranges and prevents negative values

---
