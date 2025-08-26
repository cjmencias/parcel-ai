## Parcel.AI Domain

**Parcel.ai** is an on-demand parcel delivery platform that connects customers who need items delivered with nearby drivers. The platform provides real-time tracking, secure payments, and a seamless experience across mobile and web interfaces.

### Key Features
- **Instant Booking:** Customers can book deliveries with fare estimates in under 3 seconds
- **Smart Dispatch:** AI-powered driver matching with expanding radius and rating optimization
- **Real-time Tracking:** Live location updates and dynamic ETA calculations
- **Secure Payments:** Integrated payment processing with authorization/capture flow
- **Multi-role Platform:** Single app serving customers, drivers, and operators
- **Photo Proof:** Mandatory pickup and delivery confirmation photos with operator review
- **Support System:** In-app ticketing with photo attachments and operator communication
- **History & Receipts:** Paginated booking history with downloadable PDF receipts
- **Driver Earnings:** Real-time earnings tracking with detailed payout history
- **Operator Management:** Comprehensive search, driver management, and support ticket assignment within the app

---

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
**I want to** cancel a booking while it's pending or assigned and provide an explanation  
**So that** I'm not charged and drivers are informed

**Acceptance Criteria:**
- Cancel booking action is available while the job status is "pending" or "assigned"
- Tapping Cancel shows a short list of reasons plus an optional text box for extra details
- A confirmation pop-up asks the customer to confirm the cancellation
- After confirmation:
  - The job is marked Cancelled and the customer's card is not charged
  - The assigned driver (if any) and the support team receive a notification that the job was cancelled
  - The booking now appears in the customer's history with the label Cancelled and the chosen reason visible

### US-05: Authorize Payment on Booking

**As the** system  
**I want to** place a hold on the user's payment method when a booking status becomes "assigned"  
**So that** funds are guaranteed

**Acceptance Criteria:**
- A payment authorization occurs when booking status changes to "assigned"
- User sees success message; on failure, booking is cancelled with explanation

### US-06: Operator Invites Driver

**As an** operator  
**I want to** generate a unique invite link for a driver  
**So that** only vetted drivers can join the platform

**Acceptance Criteria:**
- Operator can generate a unique invite link with configurable expiration time
- Operator manually sends the invite link via email or SMS to the prospective driver
- Invite link expires after configurable time (default: 7 days)
- Driver status = "pending" until signup complete

### US-07: Driver Completes Signup

**As a** driver  
**I want to** submit my personal, vehicle, and payout details  
**So that** I can start receiving jobs

**Acceptance Criteria:**
- All mandatory fields validated (name, ID photo, vehicle plate, bank info)
- On save, driver status becomes "active"
- Incomplete signup blocks job acceptance

### US-08: Mobile-Optimized Field Operations

**As a** delivery driver  
**I want to** access and update job information on my mobile device with touch-friendly interactions  
**So that** I can efficiently manage deliveries while on the road without desktop dependency

**Acceptance Criteria:**
- Responsive design optimized for mobile screens
- Touch-friendly buttons and forms
- Fast loading performance for field use

### US-09: Receive & Accept Job (First-Acceptor-Wins)

**As a** driver  
**I want to** receive nearby job offers with payout info and accept the one I want  
**So that** I can earn money

**Acceptance Criteria:**
- Push notification arrives within 2s of dispatch
- Job card shows pickup, drop-off, payout
- First driver to tap "Accept" is assigned; others see "Job taken"

### US-10: Dispatch Fan-Out (System)

**As the** system  
**I want to** broadcast a new job to drivers in expanding radius with light rating bias  
**So that** it's claimed quickly and fairly

**Acceptance Criteria:**
- Nearest eligible drivers receive the job first
- Radius expands every n seconds until timeout
- Job is withdrawn after timeout if unclaimed

## US-11: Real-Time Status Tracking

**As an** operations manager
**I want** to track and update job status in real-time through the complete delivery lifecycle
**So that** I can maintain visibility into operations and provide accurate updates to customers

**Acceptance Criteria:**
- Visual progress indicators showing completion percentage
- Ability to mark jobs as cancelled from any status

### US-12: Live Map & ETA

**As a** user  
**I want to** see the driver's live location and ETA  
**So that** I know when to expect pickup and delivery

**Acceptance Criteria:**
- Initial ETA calculated when driver accepts the job
- Map updates every 15 seconds with driver's live location
- ETA recalculates dynamically every few seconds as driver moves
- User can share a read-only tracking link

### US-13: Photo Proof – Pickup & Drop-off

**As a** driver  
**I want to** take a photo of the parcel at pickup and drop-off  
**So that** everyone has evidence of hand-off

**Acceptance Criteria:**
- App requires pickup photo before driver can mark job as "in_transit"
- App requires delivery photo before driver can mark job as "delivered"
- Driver can retake photos as needed before confirming
- Photos are visible in booking detail for user & operator
- Status transitions are blocked until required photos are uploaded

### US-14: Capture Payment & Send Receipt

**As the** system  
**I want to** capture the held payment when the booking status changes to "delivered" and send a receipt  
**So that** the user is charged correctly and status becomes "completed"

**Acceptance Criteria:**
- Payment capture is triggered when delivery is confirmed and status changes to "delivered"
- Upon successful payment capture, status changes to "completed"
- On capture failure, booking marked "Payment failed" with retry option
- Email & in-app receipt sent within 1 min of successful capture

### US-15: Daily Driver Payout

**As the** system  
**I want to** aggregate each driver's earnings daily and initiate payout  
**So that** drivers are paid T+1

**Acceptance Criteria:**
- Cron runs daily at midnight system timezone
- Driver sees pending and paid amounts in Earnings screen
- Payouts processed on business days (weekends/holidays processed next business day)

### US-16: Rating Flow

**As a** user  
**I want to** rate the driver after delivery  
**So that** service quality stays high

**Acceptance Criteria:**
- 1-5 star modal appears after job completion
- Rating is optional but can't be edited after submission

### US-17: Booking History & Receipt Download

**As a** user  
**I want to** view my past bookings and download receipts  
**So that** I can track spending

**Acceptance Criteria:**
- History list paginated; each item opens detail view and receipt PDF link

### US-18: Driver Earnings & Job History

**As a** driver  
**I want to** see my completed jobs and total earnings  
**So that** I can track my income

**Acceptance Criteria:**
- Earnings summary (today, week, month) + list of jobs

### US-19: In-App Support Ticket

**As a** user  
**I want to** open a support ticket for my booking and see replies  
**So that** issues get resolved

**Acceptance Criteria:**
- New ticket form requires selecting a booking and accepts text + optional photo attachment
- Users can add photo attachments when creating tickets or in subsequent messages
- Threaded messages between user and operator
- Push notification on reply
- Photo upload retries automatically if failed

### US-20: Operator Management Interface

**As an** operator  
**I want to** search bookings, view driver list, and suspend/reactivate drivers  
**So that** I can manage daily operations

**Acceptance Criteria:**
- Search by booking ID, customer, driver
- Driver status toggle (active/suspended) reflects immediately in app

### US-21: Configure Commission & Tax Rates

**As an** operator  
**I want to** set commission percentages, applicable taxes, base fare parameters, and system currency  
**So that** the system can calculate user fares and driver payouts accurately

**Acceptance Criteria:**
- Operator can edit commission %, GST/VAT %, base fare, per-km, per-minute rates, and system currency
- Currency changes are not recommended after initial deployment due to operational complexity
- Changes apply only to new bookings created after the update
- Validation ensures numeric ranges and prevents negative values
- Currency changes affect all new transactions system-wide

### US-22: Operational Dashboard & Analytics

**As a** business owner  
**I want to** view job statistics and performance metrics through an interactive dashboard  
**So that** I can make data-driven decisions about my delivery operations

**Acceptance Criteria:**
- Animated counters showing job statistics by status
- Visual progress tracking across all active jobs
- Color-coded sections for different job types and statuses

---

## Business Rules & Logic

### Pricing Calculation
- **Base Calculation:** Base fare + (distance × per-km rate) + (duration × per-minute rate) = Driver Base Amount
- **Commission:** Platform commission = Driver Base Amount × commission %
- **Tax:** GST/VAT = (Driver Base Amount + Commission) × tax %
- **Customer Fare:** Driver Base Amount + Commission + Tax
- **Driver Receives:** Driver Base Amount (commission and tax are deducted from customer payment)
- **Platform Keeps:** Commission + Tax
- **Fare Display:** System must display customer fare ≤ 3 seconds after route details completed
- **Currency:** System uses single currency per deployment, typically configured during initial setup and rarely changed
- **Rate Configuration:** Only operators can modify commission %, tax %, base fare, per-km, per-minute rates, and system currency
- **Rate Application:** Changes apply only to new bookings created after the update

**Example Calculation (USD):**
- Base fare: $5.00
- Distance: 3 km × $1.50/km = $4.50
- Duration: 10 minutes × $0.25/minute = $2.50
- **Driver Base Amount:** $5.00 + $4.50 + $2.50 = $12.00
- **Commission (20%):** $12.00 × 0.20 = $2.40
- **Subtotal:** $12.00 + $2.40 = $14.40
- **Tax (10%):** $14.40 × 0.10 = $1.44
- **Customer Pays:** $14.40 + $1.44 = $15.84
- **Driver Receives:** $12.00
- **Platform Keeps:** $2.40 + $1.44 = $3.84

### Driver Management
- **Invitation Only:** Drivers can only join via operator-generated unique invite links
- **Invite Link Distribution:** System generates invite links; operators manually send them via external email or SMS to prospective drivers
- **Link Expiration:** Invite links expire after configurable time (default: 7 days)
- **Signup Completion:** Incomplete signup blocks job acceptance
- **Required Fields:** Name, ID photo, vehicle plate, bank info
- **Status Management:** pending → active ⟷ suspended (operators can directly reactivate suspended drivers)
- **Eligibility:** Only active drivers with complete signup receive job offers

### Job Dispatch Logic
- **First-Acceptor-Wins:** First driver to tap "Accept" gets the job
- **Proximity-Based:** Nearest eligible drivers receive jobs first
- **Expanding Radius:** Radius expands at configurable intervals (default: every 30 seconds) until timeout
- **Rating Bias:** Light preference for higher-rated drivers when distance is similar (small effect to maintain fairness for new drivers)
- **Timeout Handling:** Jobs withdrawn if unclaimed after timeout period (default: 5 minutes, operator configurable)
- **Notification Speed:** Push notifications must arrive within 2 seconds of dispatch

### Booking Lifecycle
- **Status Flow:** pending → assigned → picked_up → in_transit → delivered → completed
- **Cancellation Window:** Available while job status is "pending" or "assigned"
- **Photo Requirements:** Pickup photo required before status can progress from "picked_up" to "in_transit"; delivery photo required before status can progress from "in_transit" to "delivered"
- **Photo Visibility:** Photos accessible in booking details for customers and operators
- **Payment Authorization:** Hold placed on payment method when booking status changes from "pending" to "assigned"
- **Payment Capture:** Triggered when status changes to "delivered", then status changes to "completed" upon successful capture

### Cancellation & Refund Policy
- **Customer Cancellation:** Available while job is "pending" or "assigned", requires reason selection
- **No Charge Policy:** Cancelled bookings don't charge customer's payment method
- **Driver Notification:** Assigned driver and support team notified of cancellations
- **History Tracking:** Cancelled bookings appear in history with reason visible

### Payment Processing
- **Authorization Timing:** When booking status changes from "pending" to "assigned" (not during booking creation)
- **Capture Timing:** When booking status changes to "delivered"
- **Status Progression:** delivered → (payment capture) → completed
- **Failure Handling:** Failed captures mark booking as "payment failed" with retry option
- **Receipt Generation:** Email & in-app receipt sent within 1 minute of successful capture
- **Driver Payouts:** Daily aggregation at midnight local timezone with T+1 payout schedule (business days only)
- **Commission Structure:** Platform deducts commission and tax from customer payment; driver receives the base calculation (base fare + distance + time costs)

### Real-time Tracking
- **Initial ETA:** Calculated when driver accepts job based on current location and route
- **Update Frequency:** Driver location updates every 15 seconds
- **ETA Recalculation:** Dynamic recalculation every few seconds as driver moves
- **Sharing:** Users can generate read-only tracking links for third parties

### Rating System
- **Rating Window:** 1-5 star modal appears after job completion
- **Optional Rating:** Rating submission is optional and does not affect driver eligibility
- **Finality:** Ratings cannot be edited after submission
- **Driver Impact:** Ratings provide light preference in dispatch when drivers are at similar distances (small effect to maintain fairness for all drivers)

### Support & Communication
- **Ticket Linking:** Support tickets must be linked to a specific booking
- **Response Notifications:** Push notifications sent on operator replies
- **Media Support:** Tickets accept text + optional photo attachments during creation; additional attachments can be added in subsequent messages with automatic retry on upload failure
- **Threading:** Maintains conversation history between user and operator

### Operator Controls
- **Search Capabilities:** Search by booking ID, customer name, or driver name
- **Driver Management:** Real-time status toggle between active/suspended (no need to return to pending) reflects immediately in app
- **Rate Configuration:** Full control over pricing parameters and system currency with validation (currency changes not recommended post-deployment)
- **Invite Management:** Control driver onboarding through invitation system

### History & Receipt Management
- **Booking History:** Paginated list of past bookings with detail view access
- **Receipt Generation:** PDF receipts available for download from booking history
- **Data Retention:** Historical data maintained for regulatory and user access requirements
- **Receipt Timing:** Receipts generated within 1 minute of payment capture

### Driver Earnings & Tracking
- **Earnings Summary:** Real-time calculation of earnings (today, week, month)
- **Job History:** Complete list of completed jobs with payout details
- **Payout Transparency:** Clear breakdown of customer fare, commission deducted, tax deducted, and net payout per job
- **Earnings Aggregation:** Daily cron job at midnight system timezone aggregates earnings for T+1 payout processing
- **Business Days Only:** Payouts processed on business days (weekends/holidays deferred to next business day)

### Support System Enhancement
- **Ticket Creation:** Support tickets must be linked to specific bookings
- **Media Support:** Tickets accept text messages with optional photo attachments during ticket creation and in individual messages
- **Threading:** Maintains conversation history between user and operator
- **Notification System:** Push notifications sent on operator replies
- **Ticket Assignment:** Operators can be assigned to specific tickets for accountability

---

## Technology Stack

### Frontend
- Built with **React Native and Expo** for cross-platform support (iOS, Android, Web)
- Styled using **NativeWind**, enabling Tailwind-style utility classes and smooth animation support
- Uses **Axios** for API communication

### Backend
- Developed with **Node.js** and **Express**
- Written in **TypeScript** for strong typing and developer productivity
- Designed for rapid development and scalable API architecture

### Database
- **PostgreSQL** with **PostGIS extension** for advanced geospatial queries (e.g., delivery radius, driver proximity)

### Real-time
- **Socket.IO** for real-time communication: live tracking, push notifications, and job dispatch updates

### Infrastructure
- Targeted for deployment on **AWS Cloud**, leveraging **Kubernetes (EKS)** for container orchestration
- Designed for scalability, fault tolerance, and ease of deployment in cloud-native environments

### CI/CD
- **CI/CD**: Git-based automation using GitHub Actions or GitLab CI for testing, container builds, and deployments to AWS

## Technical Architecture

### Frontend Responsibilities (React Native + Expo)
- **User Interface:** Role-based views for Customer, Driver, and Operator
- **State Management:** Local app state, user sessions, cached data
- **Client-side Logic:** Form validation, navigation, UI interactions
- **Device Integration:** Camera for photo proof, GPS for location tracking
- **Real-time Updates:** Socket.IO client connections for live tracking and notifications
- **Offline Capabilities:** Cache critical data, queue actions when connectivity is poor
- **Push Notifications:** Handle incoming notifications and deep linking
- **Media Handling:** Photo capture, compression, and upload with progress indicators
- **Document Management:** PDF receipt viewing and download functionality
- **History Management:** Paginated booking and earnings history with local caching

### Backend Responsibilities (Express + TypeScript)
- **API Server:** RESTful endpoints for all business operations running as Kubernetes service
- **Authentication:** JWT-based auth with Google/Apple OAuth integration
- **Business Logic:** Booking lifecycle, dispatch algorithm, payment processing
- **Real-time Communication:** Socket.IO server for live tracking and notifications
- **Database Operations:** PostgreSQL queries with TypeORM/Prisma for type safety
- **File Management:** AWS S3
- **External Integrations:** Stripe payments, Google Maps, push notifications
- **PDF Generation:** Receipt generation using puppeteer
- **Container Management:** Dockerized service with health checks and resource limits

### Third-party Integrations (Development Mode)
- **Authentication:** Google Sign-In, Apple Sign-In (development credentials)
- **Maps & Geolocation:** Google Maps Platform (development API key)
- **Payments:** Stripe (test mode with test cards)
- **Push Notifications:** Expo Push Notifications (development tokens)
- **File Storage:** AWS S3 (development bucket for image uploads)
- **PDF Generation:** Local PDF generation using puppeteer

### Database Schema Overview
- **Core Entities:** Users, Drivers, Bookings, Payments, Support Tickets
- **Geospatial Data:** Driver locations, pickup/dropoff coordinates using PostGIS
- **Status Management:** Booking states, driver states, payment states
- **Audit Trail:** Booking history, payment transactions, driver earnings
- **Media Storage:** Photo proof URLs, receipt document references
- **Support Enhancement:** Ticket threading, operator assignments, message attachments

---

## Application Architecture Decision

### Single App with Role-Based Views
**Decision:** Build one React Native app that serves all user types (Customer, Driver, Operator) with different interfaces based on user role.

**Rationale:**
- **Shared Codebase:** Faster development and easier maintenance
- **Common Components:** Maps, payments, notifications, and support features reused across roles
- **Single Deployment:** One app to build, test, and deploy
- **Consistency:** Easier to maintain UI/UX consistency across user types
- **Cost Efficiency:** Lower development and maintenance costs
- **User Flexibility:** Users can potentially have multiple roles (customer + driver)

**App Structure:**
```
├── Authentication (shared login flow)
├── Customer Interface
│   ├── Booking creation and management
│   ├── Live tracking and ETA
│   ├── Payment methods and history
│   ├── Booking history with receipt download
│   └── Support tickets with photo attachments
├── Driver Interface
│   ├── Job acceptance and management
│   ├── Navigation and photo proof capture
│   ├── Earnings summary and job history
│   └── Profile management
├── Operator Interface
│   ├── Booking and driver search
│   ├── Driver management (invite/suspend)
│   ├── Rate configuration
│   ├── Support ticket assignment and management
│   └── Photo proof review and verification
└── Shared Components
    ├── Maps and location services
    ├── Payment processing
    ├── Push notifications
    └── Camera and media upload
```

---

## Design System & UI/UX

### Color Scheme
- **Primary:** Blue gradients (#0ea5e9 to #0369a1) - Main brand colors for primary actions and branding
- **Secondary:** Gray tones for neutral actions and secondary elements
- **Success:** Green gradient for completed states, successful operations, and positive feedback
- **Warning:** Yellow gradient for pending states, cautions, and intermediate status
- **Danger:** Red gradient for cancelled/error states, destructive actions, and alerts
- **Accent:** Purple gradients (#d946ef to #a21caf) - Highlight colors for special features and emphasis

### Typography
- **Font Family:** Inter from Google Fonts - Modern, clean, and highly readable
- **Font Weights:** 300 (Light), 400 (Regular), 500 (Medium), 600 (Semi-bold), 700 (Bold), 800 (Extra-bold)
- **Typography Effects:** Modern typography with gradient text effects for headings and emphasis
- **Hierarchy:** Clear typographic hierarchy supporting role-based interfaces and information density

### Design Features
- **Glassmorphism:** Beautiful glass effects with backdrop blur for modern, layered interface elements
- **Gradient Backgrounds:** Stunning color gradients throughout the interface for visual depth and brand consistency
- **Smooth Animations:** 
  - Fade-in effects for content loading
  - Slide-up animations for modals and sheets
  - Bounce effects for interactive feedback
- **Interactive Elements:** 
  - Hover effects for web interface
  - Scale animations on touch for mobile
  - Lift transitions for elevated components
- **Progress Visualization:** Animated progress bars showing job completion status and real-time updates
- **Loading States:** Beautiful loading spinners and skeleton screens for seamless user experience during data loading

### User Experience Principles
- **Visual Hierarchy:** Clear information architecture using gradients, typography, and spacing
- **Feedback Systems:** Immediate visual feedback for all user interactions through animations and color changes
- **Consistency:** Unified design language across all three user roles (Customer, Driver, Operator)
- **Accessibility:** High contrast ratios, readable font sizes, and touch-friendly interactive elements
- **Performance:** Optimized animations and effects that don't compromise app performance

### Role-Specific Design Considerations
- **Customer Interface:** 
  - Clean, consumer-friendly design with emphasis on booking flow and tracking
  - Prominent call-to-action buttons with gradient styling
  - Real-time status updates with animated progress indicators
- **Driver Interface:** 
  - Functional, efficiency-focused design for quick job management
  - Large, touch-friendly buttons for in-vehicle use
  - Clear visual hierarchy for job information and navigation
- **Operator Interface:** 
  - Data-dense design optimized for desktop/tablet use
  - Advanced filtering and search capabilities with clear visual feedback
  - Dashboard-style layout with comprehensive information display

### Component Standards
- **Buttons:** Gradient backgrounds with smooth hover/press animations
- **Cards:** Glassmorphism effects with subtle shadows and backdrop blur
- **Forms:** Clean input fields with gradient focus states and validation feedback
- **Navigation:** Smooth transitions between screens with consistent iconography
- **Status Indicators:** Color-coded with gradient styling and animated state changes
- **Maps:** Integrated with glassmorphism overlays and gradient markers

---

## Data Models & Relationships

### Core Entities

#### Users
```json
{
  "id": "uuid",
  "email": "string",
  "phone": "string",
  "name": "string",
  "role": "customer | driver | operator",
  "auth_provider": "google | apple",
  "auth_provider_id": "string",
  "profile_image_url": "string",
  "created_at": "timestamp",
  "updated_at": "timestamp",
  "is_active": "boolean"
}
```

#### Drivers (extends Users)
```json
{
  "user_id": "uuid (FK to users.id)",
  "status": "pending | active | suspended",
  "vehicle_plate": "string",
  "vehicle_type": "string",
  "id_photo_url": "string",
  "bank_account_info": "encrypted_json",
  "current_location": "point (PostGIS)",
  "rating_average": "decimal(2,1)",
  "total_jobs": "integer",
  "signup_completed": "boolean",
  "invited_by": "uuid (FK to users.id)",
  "invite_token": "string (unique token for invite URL)",
  "invite_expires_at": "timestamp"
}
```

#### Bookings
```json
{
  "id": "uuid",
  "customer_id": "uuid (FK to users.id)",
  "driver_id": "uuid (FK to users.id, nullable)",
  "pickup_location": "point (PostGIS)",
  "pickup_address": "string",
  "dropoff_location": "point (PostGIS)",
  "dropoff_address": "string",
  "parcel_size": "small | medium | large",
  "parcel_weight": "decimal",
  "status": "pending | assigned | picked_up | in_transit | delivered | completed | cancelled",
  "fare_amount": "decimal(10,2) (total amount customer pays)",
  "driver_base_amount": "decimal(10,2) (base fare + distance + time costs)",
  "commission_amount": "decimal(10,2) (platform commission deducted from customer payment)",
  "tax_amount": "decimal(10,2) (GST/VAT deducted from customer payment)",
  "estimated_distance_km": "decimal",
  "estimated_duration_minutes": "integer",
  "actual_distance_km": "decimal (nullable)",
  "actual_duration_minutes": "integer (nullable)",
  "pickup_eta": "timestamp (nullable)",
  "delivery_eta": "timestamp (nullable)",
  "pickup_photo_url": "string (nullable, required before status can progress from picked_up)",
  "delivery_photo_url": "string (nullable, required before status can progress from in_transit)",
  "cancellation_reason": "string (nullable)",
  "created_at": "timestamp",
  "updated_at": "timestamp",
  "completed_at": "timestamp (nullable)"
}
```

#### Payments
```json
{
  "id": "uuid",
  "booking_id": "uuid (FK to bookings.id)",
  "customer_id": "uuid (FK to users.id)",
  "payment_method_id": "string (Stripe PM ID)",
  "stripe_payment_intent_id": "string",
  "amount": "decimal(10,2)",
  "currency": "string (ISO 4217 currency code, e.g., USD, EUR, GBP)",
  "status": "authorized | captured | failed | refunded",
  "authorization_date": "timestamp",
  "capture_date": "timestamp (nullable)",
  "failure_reason": "string (nullable)",
  "receipt_url": "string (nullable)",
  "created_at": "timestamp",
  "updated_at": "timestamp"
}
```

#### Payment Methods
```json
{
  "id": "uuid",
  "user_id": "uuid (FK to users.id)",
  "stripe_payment_method_id": "string",
  "type": "card | apple_pay | google_pay",
  "last_four": "string",
  "brand": "string",
  "is_default": "boolean",
  "created_at": "timestamp"
}
```

#### Driver Locations (Real-time tracking)
```json
{
  "id": "uuid",
  "driver_id": "uuid (FK to users.id)",
  "booking_id": "uuid (FK to bookings.id, nullable)",
  "location": "point (PostGIS)",
  "heading": "decimal (0-360 degrees)",
  "speed": "decimal (km/h)",
  "accuracy": "decimal (meters)",
  "timestamp": "timestamp",
  "created_at": "timestamp"
}
```

#### Ratings
```json
{
  "id": "uuid",
  "booking_id": "uuid (FK to bookings.id)",
  "customer_id": "uuid (FK to users.id)",
  "driver_id": "uuid (FK to users.id)",
  "rating": "integer (1-5)",
  "comment": "text (nullable)",
  "created_at": "timestamp"
}
```

#### Support Tickets
```json
{
  "id": "uuid",
  "booking_id": "uuid (FK to bookings.id)",
  "user_id": "uuid (FK to users.id)",
  "assigned_operator_id": "uuid (FK to users.id, nullable)",
  "subject": "string",
  "initial_message": "text",
  "initial_attachment_url": "string (nullable, photo attached during ticket creation)",
  "initial_attachment_type": "photo | document (nullable)",
  "status": "open | in_progress | resolved | closed",
  "priority": "low | medium | high",
  "created_at": "timestamp",
  "updated_at": "timestamp",
  "resolved_at": "timestamp (nullable)"
}
```

#### Support Messages
```json
{
  "id": "uuid",
  "ticket_id": "uuid (FK to support_tickets.id)",
  "sender_id": "uuid (FK to users.id)",
  "message": "text",
  "attachment_url": "string (nullable)",
  "attachment_type": "photo | document (nullable)",
  "is_internal": "boolean",
  "created_at": "timestamp"
}
```

#### Driver Earnings
```json
{
  "id": "uuid",
  "driver_id": "uuid (FK to users.id)",
  "booking_id": "uuid (FK to bookings.id)",
  "customer_fare_amount": "decimal(10,2) (total amount customer paid)",
  "driver_base_amount": "decimal(10,2) (base fare + distance + time costs)",
  "commission_amount": "decimal(10,2) (platform commission deducted)",
  "tax_amount": "decimal(10,2) (tax amount deducted)",
  "net_amount": "decimal(10,2) (what driver actually receives: driver_base_amount)",
  "payout_date": "date (nullable)",
  "payout_status": "pending | paid | failed",
  "payout_reference": "string (nullable)",
  "created_at": "timestamp"
}
```

#### System Configuration
```json
{
  "id": "uuid",
  "key": "string (unique)",
  "value": "json",
  "description": "string",
  "updated_by": "uuid (FK to users.id)",
  "updated_at": "timestamp"
}
```

**Example Configuration Keys:**
- `dispatch_timeout_minutes`: 5 (default job timeout)
- `dispatch_radius_expansion_seconds`: 30 (radius expansion interval)
- `location_update_interval_seconds`: 15 (driver location update frequency)
- `eta_recalculation_interval_seconds`: 5 (ETA recalculation frequency)
- `payout_cron_time`: "00:00" (daily payout processing time in system timezone)
- `system_timezone`: "UTC" (system timezone for all cron jobs and scheduling)
- `system_currency`: "USD" (single currency for entire deployment, configured once during setup)
- `currency_symbol`: "$" (currency symbol for display, matches system_currency)
- `commission_percentage`: 20.0 (platform commission rate deducted from customer payment)
- `tax_percentage`: 10.0 (GST/VAT rate deducted from customer payment)
- `base_fare_amount`: 5.00 (base fare in local currency)
- `per_km_rate`: 1.50 (rate per kilometer)
- `per_minute_rate`: 0.25 (rate per minute)

### Status Enums & Transitions

#### Booking Status Flow
```
pending → assigned → picked_up → (pickup photo required) → in_transit → (delivery photo required) → delivered → (payment capture) → completed
   ↓         ↓
cancelled ←──┘
```

#### Payment Status Flow
```
authorized → captured
    ↓
  failed (retry possible)
    ↓
  refunded (for cancellations)
```

#### Driver Status Flow
```
pending → active ⟷ suspended
          ↑         ↑
          └─────────┘
(operators can toggle between active and suspended)
```

### Key Relationships

- **Users** (1) → (many) **Bookings** (as customer)
- **Users** (1) → (many) **Bookings** (as driver)
- **Bookings** (1) → (1) **Payments**
- **Bookings** (1) → (many) **Driver Locations**
- **Bookings** (1) → (1) **Ratings** (optional)
- **Bookings** (1) → (many) **Support Tickets**
- **Users** (1) → (many) **Payment Methods**
- **Users** (1) → (many) **Driver Earnings** (for drivers)
- **Support Tickets** (1) → (many) **Support Messages**

### Indexes & Performance Considerations

#### Critical Indexes
- `bookings.status` - For filtering active bookings
- `bookings.customer_id, bookings.created_at` - Customer history
- `bookings.driver_id, bookings.created_at` - Driver history
- `driver_locations.driver_id, driver_locations.timestamp` - Real-time tracking
- `drivers.current_location` - Spatial index for nearby driver queries
- `payments.stripe_payment_intent_id` - Webhook processing
- `support_tickets.user_id, support_tickets.status` - User support queries

#### Geospatial Indexes (PostGIS)
- `bookings.pickup_location` - Pickup location queries
- `bookings.dropoff_location` - Delivery location queries
- `drivers.current_location` - Driver proximity searches
- `driver_locations.location` - Historical location tracking

---

## API Specifications

### Authentication
- **Base URL:** `https://api.parcelai.com/v1`
- **Authentication:** Bearer JWT tokens in Authorization header
- **Token Refresh:** Automatic refresh using refresh tokens

### REST API Endpoints

#### Authentication Service
```
POST /auth/login
POST /auth/refresh
POST /auth/logout
GET  /auth/me
```

#### User Management
```
GET    /users/profile
PUT    /users/profile
POST   /users/payment-methods
GET    /users/payment-methods
DELETE /users/payment-methods/{id}
PUT    /users/payment-methods/{id}/default
GET    /users/bookings/history             # Paginated booking history
GET    /users/bookings/{id}/receipt        # Download receipt PDF
```

#### Driver Management
```
POST /drivers/invite                       # Generate unique invite link (Operator only)
GET  /drivers                              # Operator only
PUT  /drivers/{id}/status                  # Toggle between active/suspended (Operator only)
GET  /drivers/{id}/profile
PUT  /drivers/{id}/profile
GET  /drivers/{id}/earnings                # Earnings summary (today, week, month)
GET  /drivers/{id}/earnings/history        # Detailed earnings history
GET  /drivers/{id}/jobs/history            # Completed jobs list
POST /drivers/{id}/location                # Real-time location update
```

#### Booking Service
```
POST /bookings                             # Create booking (payment authorized when status becomes "assigned")
GET  /bookings                             # List user's bookings
GET  /bookings/{id}                        # Get booking details
PUT  /bookings/{id}/cancel                 # Cancel booking
GET  /bookings/{id}/tracking               # Get tracking link
POST /bookings/fare-estimate               # Get fare estimate
```

#### Driver Job Management
```
GET  /jobs/available                       # Get available jobs for driver
POST /jobs/{id}/accept                     # Accept job
PUT  /jobs/{id}/status                     # Update job status (validates photo requirements)
POST /jobs/{id}/photos/pickup              # Upload pickup photo
POST /jobs/{id}/photos/delivery            # Upload delivery photo
```

#### Payment Service
```
POST /payments/authorize                   # Authorize payment
POST /payments/capture                     # Capture payment
POST /payments/refund                      # Process refund
GET  /payments/{id}/receipt                # Get receipt
```

#### Support Service
```
POST /support/tickets                      # Create support ticket
GET  /support/tickets                      # List user's tickets
GET  /support/tickets/{id}                 # Get ticket details
POST /support/tickets/{id}/messages        # Add message to ticket
POST /support/tickets/{id}/attachments     # Upload photo attachment
```

#### Rating Service
```
POST /bookings/{id}/rating                 # Submit rating
GET  /drivers/{id}/ratings                 # Get driver ratings
```

#### Operator Management
```
GET  /admin/bookings                       # Search bookings
GET  /admin/drivers                        # List all drivers
PUT  /admin/drivers/{id}/status            # Toggle between active/suspended
GET  /admin/support/tickets                # List support tickets
PUT  /admin/support/tickets/{id}/assign    # Assign ticket to operator
POST /admin/support/tickets/{id}/messages  # Reply to ticket
GET  /admin/bookings/{id}/photos           # Review pickup/delivery photos
GET  /admin/config                         # Get system configuration
PUT  /admin/config                         # Update rates and settings
```

### Detailed Endpoint Examples

#### Generate Driver Invite Link
```http
POST /drivers/invite
Authorization: Bearer {operator_jwt_token}
Content-Type: application/json

{
  "expiration_hours": 168
}

Response 201:
{
  "invite_token": "inv_abc123def456",
  "invite_url": "https://app.parcelai.com/driver/signup?token=inv_abc123def456",
  "expires_at": "2024-01-22T14:30:00Z",
  "created_by": "operator_789"
}
```

#### Create Booking
```http
POST /bookings
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "pickup_location": {
    "lat": -33.8688,
    "lng": 151.2093,
    "address": "Sydney Opera House, Sydney NSW"
  },
  "dropoff_location": {
    "lat": -33.8567,
    "lng": 151.2153,
    "address": "Circular Quay, Sydney NSW"
  },
  "parcel_size": "medium",
  "parcel_weight": 2.5,
  "payment_method_id": "pm_1234567890"
}

Response 201:
{
  "id": "booking_123",
  "status": "pending",
  "fare_amount": 15.50,
  "estimated_pickup_time": "2024-01-15T14:30:00Z",
  "estimated_delivery_time": "2024-01-15T15:15:00Z",
  "tracking_url": "https://track.parcelai.com/booking_123"
}
```

#### Get Available Jobs (Driver)
```http
GET /jobs/available
Authorization: Bearer {driver_jwt_token}

Response 200:
{
  "jobs": [
    {
      "id": "job_456",
      "pickup_location": {
        "lat": -33.8688,
        "lng": 151.2093,
        "address": "Sydney Opera House"
      },
      "dropoff_location": {
        "lat": -33.8567,
        "lng": 151.2153,
        "address": "Circular Quay"
      },
      "distance_km": 2.1,
      "estimated_duration": 15,
      "payout_amount": 12.50,
      "expires_at": "2024-01-15T14:35:00Z"
    }
  ]
}
```

#### Upload Pickup Photo
```http
POST /jobs/{id}/photos/pickup
Authorization: Bearer {driver_jwt_token}
Content-Type: multipart/form-data

photo: [binary image data]
timestamp: "2024-01-15T14:32:00Z"

Response 200:
{
  "success": true,
  "photo_url": "https://s3.amazonaws.com/photos/pickup_123.jpg",
  "status_transition_allowed": true,
  "next_available_status": "in_transit"
}
```

#### Get Driver Earnings Summary
```http
GET /drivers/{id}/earnings
Authorization: Bearer {driver_jwt_token}

Response 200:
{
  "today": {
    "customer_fare_total": 188.25,
    "driver_base_total": 125.50,
    "commission_deducted": 25.10,
    "tax_deducted": 18.83,
    "net_amount": 125.50,
    "jobs_completed": 8
  },
  "week": {
    "customer_fare_total": 1125.38,
    "driver_base_total": 750.25,
    "commission_deducted": 150.05,
    "tax_deducted": 112.55,
    "net_amount": 750.25,
    "jobs_completed": 45
  },
  "month": {
    "customer_fare_total": 4801.13,
    "driver_base_total": 3200.75,
    "commission_deducted": 640.15,
    "tax_deducted": 480.11,
    "net_amount": 3200.75,
    "jobs_completed": 180
  }
}
```

#### Toggle Driver Status
```http
PUT /admin/drivers/{id}/status
Authorization: Bearer {operator_jwt_token}
Content-Type: application/json

{
  "status": "active"
}

Response 200:
{
  "id": "driver_123",
  "status": "active",
  "previous_status": "suspended",
  "updated_at": "2024-01-15T14:30:00Z",
  "updated_by": "operator_456"
}
```

#### Download Receipt PDF
```http
GET /users/bookings/{id}/receipt
Authorization: Bearer {jwt_token}

Response 200:
{
  "receipt_url": "https://s3.amazonaws.com/receipts/booking_123.pdf",
  "expires_at": "2024-01-15T18:00:00Z"
}
```

#### Create Support Ticket with Photo
```http
POST /support/tickets
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "booking_id": "booking_123",
  "subject": "Delivery issue",
  "initial_message": "Package was damaged during delivery",
  "initial_attachment_url": "https://s3.amazonaws.com/attachments/damage_photo.jpg",
  "initial_attachment_type": "photo"
}

Response 201:
{
  "id": "ticket_456",
  "status": "open",
  "created_at": "2024-01-15T14:30:00Z",
  "initial_message_id": "msg_789"
}
```

#### Add Message with Photo Attachment
```http
POST /support/tickets/{id}/messages
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "message": "Here's another photo showing the damage from a different angle",
  "attachment_url": "https://s3.amazonaws.com/attachments/damage_photo_2.jpg",
  "attachment_type": "photo"
}

Response 201:
{
  "id": "msg_890",
  "ticket_id": "ticket_456",
  "created_at": "2024-01-15T14:35:00Z"
}
```

### WebSocket Events

#### Connection
```
wss://api.parcelai.com/ws
Authorization: Bearer {jwt_token}
```

#### Customer Events
```javascript
// Incoming events (server → client)
{
  "event": "booking_assigned",
  "data": {
    "booking_id": "booking_123",
    "driver": {
      "id": "driver_456",
      "name": "John Doe",
      "rating": 4.8,
      "vehicle_plate": "ABC123"
    },
    "estimated_pickup": "2024-01-15T14:30:00Z"
  }
}

{
  "event": "location_update",
  "data": {
    "booking_id": "booking_123",
    "driver_location": {
      "lat": -33.8688,
      "lng": 151.2093
    },
    "pickup_eta": "2024-01-15T14:28:00Z",
    "delivery_eta": "2024-01-15T15:12:00Z"
  }
}

{
  "event": "status_update",
  "data": {
    "booking_id": "booking_123",
    "status": "picked_up",
    "timestamp": "2024-01-15T14:32:00Z",
    "photo_url": "https://s3.amazonaws.com/photos/pickup_123.jpg"
  }
}
```

#### Driver Events
```javascript
// Incoming events (server → client)
{
  "event": "new_job_offer",
  "data": {
    "job_id": "job_789",
    "pickup_location": {
      "lat": -33.8688,
      "lng": 151.2093,
      "address": "Sydney Opera House"
    },
    "dropoff_location": {
      "lat": -33.8567,
      "lng": 151.2153,
      "address": "Circular Quay"
    },
    "payout_amount": 12.50,
    "expires_at": "2024-01-15T14:35:00Z"
  }
}

{
  "event": "job_taken",
  "data": {
    "job_id": "job_789",
    "message": "This job has been taken by another driver"
  }
}

// Outgoing events (client → server)
{
  "event": "location_update",
  "data": {
    "lat": -33.8688,
    "lng": 151.2093,
    "heading": 45,
    "speed": 25.5,
    "timestamp": "2024-01-15T14:30:00Z"
  }
}
```

### Error Response Format
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid pickup location",
    "details": {
      "field": "pickup_location.lat",
      "reason": "Latitude must be between -90 and 90"
    },
    "request_id": "req_123456789"
  }
}
```

### Standard HTTP Status Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request (validation errors)
- `401` - Unauthorized (invalid/expired token)
- `403` - Forbidden (insufficient permissions)
- `404` - Not Found
- `409` - Conflict (e.g., job already taken)
- `422` - Unprocessable Entity (business logic errors)
- `429` - Too Many Requests (rate limiting)
- `500` - Internal Server Error

### Rate Limiting
- **General API:** 1000 requests per hour per user
- **Location Updates:** 240 requests per hour (every 15 seconds)
- **Job Acceptance:** 10 requests per minute per driver
- **Headers:** `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`

### Pagination
```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 150,
    "total_pages": 8,
    "has_next": true,
    "has_prev": false
  }
}
```

